[![actions](https://github.com/gomatic/docs.go-domainsec/actions/workflows/actions.yml/badge.svg)](https://github.com/gomatic/docs.go-domainsec/actions/workflows/actions.yml) [![docs](https://github.com/gomatic/docs.go-domainsec/actions/workflows/docs.yml/badge.svg)](https://github.com/gomatic/docs.go-domainsec/actions/workflows/docs.yml) [![pages](https://github.com/gomatic/docs.go-domainsec/actions/workflows/pages.yml/badge.svg)](https://github.com/gomatic/docs.go-domainsec/actions/workflows/pages.yml)

# docs.go-domainsec

The **public documentation** site for [`gomatic/go-domainsec`](https://github.com/gomatic/go-domainsec). Built from [`nicerobot/template.repo-docs`](https://github.com/nicerobot/template.repo-docs) as a self-contained [Hugo](https://gohugo.io) site, published to GitHub Pages.

## Layout

| Path | Purpose |
| --- | --- |
| [`content/`](content/) | The documentation — Hugo site content. |
| [`layouts/`](layouts/) | Hugo templates. |
| [`hugo.json`](hugo.json) | Hugo configuration. |
| [`.github/workflows/pages.yml`](.github/workflows/) | The GitHub Pages build workflow. |
| [`Makefile`](Makefile) | Local preview and build. Run `make` for help. |

## Preview locally

```bash
make serve    # http://localhost:1313
```

Everything here is **public** — it exists to be published. Anything private (ideas, tasks, specs) belongs in the source project, never here.
