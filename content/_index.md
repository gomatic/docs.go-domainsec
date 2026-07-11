---
title: go-domainsec
---

`go-domainsec` is the gomatic ecosystem's **sender-domain security assessment** library (package `domainsec`). It resolves a domain's email-authentication records (SPF, DKIM, DMARC), derives a security level and report, and decides whether the domain meets the security floor consumers gate on (e.g. alias creation): **DKIM present and DMARC at `quarantine` or `reject`**.

- **Source:** [`gomatic/go-domainsec`](https://github.com/gomatic/go-domainsec)
- **API reference:** [pkg.go.dev/github.com/gomatic/go-domainsec](https://pkg.go.dev/github.com/gomatic/go-domainsec)

## Install

```sh
go get github.com/gomatic/go-domainsec
```

## Assessing a domain

`Service` assesses sender-domain security, optionally caching reports. Build it with a `CacheStore` (or `nil` for no caching) and a `Resolver` (`nil` uses the production default resolver):

```go
svc := domainsec.NewService(cache, nil)

report, _ := svc.CheckDomain(ctx, "newsletter.com")
if report.MeetsSecurityFloor() {
	// DKIM present and DMARC at quarantine or reject
}
```

`CheckDomain` serves a cached report when one is available. Its error return is part of the contract callers depend on, but assessment itself never fails — a failed lookup is treated as an absent record — so the returned error is always `nil`. The package accordingly declares **no error sentinels**.

## The report

`DomainSecurityReport` captures the assessment: `HasSPF`/`SPFPolicy`, `HasDKIM`/`DKIMSelector`, `HasDMARC`/`DMARCPolicy` (the raw `p=` value: `none`, `quarantine`, `reject`), the derived `Level`, `IsRecommended`, and the `AssessedAt`/`ExpiresAt` cache window. `MeetsSecurityFloor()` is the single security-floor decision consumers gate on — for example, whether an alias may be created for the domain.

`SecurityLevel` is a three-value scale:

| Level | Meaning |
| --- | --- |
| `SecurityStrict` | Strong authentication posture. |
| `SecurityModerate` | Partial posture. |
| `SecurityWeak` | Little or no authentication. |

## Injection seams

- `Resolver` — the DNS lookup surface the package depends on; its single method matches [`net.Resolver.LookupTXT`](https://pkg.go.dev/net#Resolver.LookupTXT), so production passes the real resolver and tests inject a fake.
- `CacheStore` — persists assessed reports between lookups (`Get`/`Set` keyed by `Domain`); the production implementation lives with the consumer's database layer.

## The monitor

`Monitor` (built with `NewMonitor(service, logger)`) periodically re-assesses sender domains as their cached reports expire, flagging aliases bound to degraded domains. `Run(ctx, interval)` checks at the given interval until the context is cancelled; a consumer's degradation checker can then disable aliases whose domain dropped below the floor.

## Design

- **Value types, injected collaborators** — `Service` and `Monitor` are immutable values; the resolver and cache arrive at construction, which is what makes the package fully testable (100% coverage under the shared gate).
- **Stdlib only** — the package depends on nothing beyond the standard library (testify for tests).
- **Fail-open assessment, fail-closed decision** — lookups never error (absent records simply weaken the report), while `MeetsSecurityFloor` applies the hard DKIM+DMARC floor.

## Provenance

Extracted from `xto-email/go-domainsec` (originally `xto-email/xto`'s `internal/domainsec`) on the move to gomatic, where `AllowsAliasCreation` was renamed to the domain-neutral `MeetsSecurityFloor`.
