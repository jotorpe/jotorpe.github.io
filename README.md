# jotorpe.github.io

Personal academic + industry website of Jorge Torres-Pérez, built with
[Hugo Blox](https://hugoblox.com) (Academic CV template) and deployed to GitHub Pages
by GitHub Actions.

## Run locally

Requirements: Hugo extended, Go, Node.js and pnpm (`brew install hugo go node pnpm`).

```bash
pnpm install
hugo server        # http://localhost:1313
```

## Where things live

| What | Where |
|------|-------|
| Profile, experience, education, skills, training | `data/authors/me.yaml` |
| Home page sections | `content/_index.md` |
| Industry profile page | `content/industry.md` |
| Publications (one folder each, with `cite.bib`) | `content/publications/` |
| Talks | `content/events/` |
| Projects (software, start-up, funded) | `content/projects/` |
| Notes & snippets (articles, code) | `content/blog/` |
| Menu | `config/_default/menus.yaml` |
| Site settings, colours | `config/_default/params.yaml` |
| Local view overrides (`talk-list`, `compact-card`) | `layouts/_partials/views/` |

Theme files are pulled as Hugo modules and are not edited here.
