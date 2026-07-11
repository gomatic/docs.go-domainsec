[![actions](https://github.com/nicerobot/template.repo-docs/actions/workflows/actions.yml/badge.svg)](https://github.com/nicerobot/template.repo-docs/actions/workflows/actions.yml) [![docs](https://github.com/nicerobot/template.repo-docs/actions/workflows/docs.yml/badge.svg)](https://github.com/nicerobot/template.repo-docs/actions/workflows/docs.yml) [![pages](https://github.com/nicerobot/template.repo-docs/actions/workflows/pages.yml/badge.svg)](https://github.com/nicerobot/template.repo-docs/actions/workflows/pages.yml)

# template.repo-docs

Canonical template for a project's **public docs** repository (`<org>/docs.<project>`). A project's developer and user documentation — contributing, design, rules — lives here as a public [Hugo](https://gohugo.io) site published via GitHub Pages. Private content (ideas, tasks, specs) lives in the project's hub repo (`project.<project>`), never here.

## Layout

| Path | Purpose |
|------|---------|
| [`content/`](content/) | Markdown documentation — the Hugo site content. |
| [`layouts/`](layouts/) | Hugo templates. |
| [`hugo.json`](hugo.json) | Hugo configuration. |
| [`.github/workflows/pages.yml`](.github/workflows/pages.yml) | The GitHub Pages build workflow. Always present; it self-skips while the repo is private and deploys once the repo is public. |
| [`Makefile`](Makefile) | Local preview and build. Run `make` for help. |

## Public, with a self-gating workflow

Everything in this repo is **public** — it exists to be published, so there is no `public/`/`private/` split. Anything private (design notes, tasks, ideas, specs) belongs in the project's private hub repo (`project.<project>`).

GitHub Pages is unavailable on private repos in free orgs, so a docs repo ships ready to go public: the Pages workflow is always present but **self-skips while the repo is private**, deploying automatically once the repo is public and Pages is enabled — there is no file to rename.

## Going public

1. Make the repository public.
2. Enable Pages: **Settings → Pages → Source: GitHub Actions**.
3. Push (or re-run the **pages** workflow). It stops self-skipping and publishes the whole site.

## Creating a new docs repo from this template

```bash
gh repo create <org>/docs.<project> --public --template nicerobot/template.repo-docs
```
