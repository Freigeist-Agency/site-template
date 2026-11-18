# Freigeist website template

Start every Freigeist website repo from this template. It comes with a staging site and a protected prod.

## Starting a new site

1. On this repo, press **Use this template** → **Create a new repository**.
   - Owner: **Freigeist-Agency**.
   - Name: end it in `Site` (e.g. `MuellerSite`). The org rule that protects `main` matches `*Site`.
   - Tick **Include all branches**. Otherwise the new repo gets no `main` and can't publish.
2. Push your first change to `staging`. The `staging-deploy` workflow creates the Cloudflare Pages project and puts the site on `https://staging.<repo name in lower case>.pages.dev`.
3. When it's ready for prod: Settings → Pages → Deploy from a branch → `main` / root. Add a `CNAME` file and set up DNS if it has its own domain.

## Day to day

- Work on `staging`, or on a branch off it. Every branch except `main` gets its own link, `https://<branch>.<repo>.pages.dev`, which Cloudflare keeps out of Google (`X-Robots-Tag: noindex`).
- **Publish:** open a pull request `staging` → `main` and merge it with a merge commit. GitHub Pages puts it live about a minute later.
- Never edit `main` directly. After an urgent fix there, merge `main` back into `staging` the same day.
- Files over 25 MiB can't go on Cloudflare. Staging redirects them to the same path on the prod domain from `CNAME`.
- Not uploaded to staging: `tests/`, `docs/`, `tools/`, `scripts/`, dotfiles and this README.

The Cloudflare token is the org secret `CLOUDFLARE_API_TOKEN`, so there's nothing to set up per repo.
