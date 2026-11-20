# Freigeist website template

Start every Freigeist website repo from this template. It comes with a staging site and a protected prod.

## Starting a new site

1. On this repo, press **Use this template** → **Create a new repository**.
   - Owner: **Freigeist-Agency**.
   - Name: end it in `Site` (e.g. `MuellerSite`).
   - Tick **Include all branches**. Otherwise the new repo gets no `main` and can't publish.
2. Protect `main`: Settings → Rules → Rulesets → New ruleset → **Import a ruleset** → pick `.github/rulesets/main.json` from this repo (download it first) → Create. `main` then only changes through a pull request. (GitHub's free org plan has no org-wide rulesets, so this is per repo.)
3. Push your first change to `staging`. The `staging-deploy` workflow creates the Cloudflare Pages project and uploads the site. The staging link is in the workflow run's summary. It's `https://staging.<repo name in lower case>.pages.dev`, unless Cloudflare already has a project with that name anywhere, in which case it adds a short suffix (e.g. `staging.site-template-d0l.pages.dev`).
4. When it's ready for prod: Settings → Pages → Deploy from a branch → `main` / root. Add a `CNAME` file and set up DNS if it has its own domain.

## Day to day

- Work on `staging`, or on a branch off it. Every branch except `main` gets its own link, `https://<branch>.<repo>.pages.dev`, which Cloudflare keeps out of Google (`X-Robots-Tag: noindex`).
- **Publish:** open a pull request `staging` → `main` and merge it with a merge commit. GitHub Pages puts it live about a minute later.
- Never edit `main` directly. After an urgent fix there, merge `main` back into `staging` the same day.
- Files over 25 MiB can't go on Cloudflare. Staging redirects them to the same path on the prod domain from `CNAME`.
- **Clean addresses:** the live site never shows `.html` in the address bar. Link to `impressum`, not `impressum.html`, and to `./` (or `../`), not `index.html`. Canonical tags, share links (`og:url`), `sitemap.xml` and schema use the clean address too. Keep the small script at the top of `index.html`'s `<head>` on every page: it tidies old `.html` links in the address bar. GitHub Pages serves `page.html` at `/page`, and Cloudflare (staging) redirects `/page.html` to `/page`, so the page files keep their `.html` names. The `Clean addresses` check fails a push that breaks this; run it yourself with `node tools/check-clean-urls.mjs`.
- Not uploaded to staging: `tests/`, `docs/`, `tools/`, `scripts/`, dotfiles and this README.

The Cloudflare token is the org secret `CLOUDFLARE_API_TOKEN`, so there's nothing to set up per repo.
