# Vycount on the web

Every Vycount website lives in this repo, one folder per address:

| Folder | Address | Company |
|---|---|---|
| `vycount.com/` | https://vycount.com | Vycount (the main page linking to each company) |
| `softworks.vycount.com/` | https://softworks.vycount.com | Vycount Softworks Ltd |
| `estates.vycount.com/` | https://estates.vycount.com | Vycount Estates and Spaces Ltd |
| `manufacturing.vycount.com/` | https://manufacturing.vycount.com | Vycount Manufacturing Ltd |
| `media.vycount.com/` | https://media.vycount.com | Vycount Media |

Each site is a single hand-written `index.html` with no build step. Edit it, push to `main`, and the sites update within a couple of minutes.

## How publishing works

GitHub Pages serves one web address per repo. So:

- **vycount.com** is served straight from this repo (Settings → Pages → Source: GitHub Actions).
- **Each company site** is copied by the `Publish sites` workflow into its own publish repo, `vycount/<address>` (for example `vycount/estates.vycount.com`), which Pages serves from its `main` branch. Don't edit the publish repos by hand; the next push here overwrites them.

The workflow needs one repo secret, `PUBLISH_TOKEN`: a fine-grained personal access token for the `vycount` account, limited to the publish repos, with **Contents: Read and write**. When it expires, create a new one and replace the secret.

## Files that must stay

- Each folder's `CNAME` file holds its custom domain.
- `softworks.vycount.com/google8fe995ea39e5bdb2.html` proves ownership to Google Search Console. Don't delete it.

## Adding a site for a new company

1. Add a folder named after its address, with an `index.html` and a `CNAME` file containing the address.
2. Create an empty public repo `vycount/<address>` with a README, and turn on Pages (Deploy from a branch, `main`, root).
3. Add the address to the `site:` list in `.github/workflows/publish.yml` and give `PUBLISH_TOKEN` access to the new repo.
4. At Porkbun, add a CNAME record for the subdomain pointing to `vycount.github.io`.
5. Add a card for it on `vycount.com/index.html`.

## DNS (Porkbun, vycount.com)

- `vycount.com`: ALIAS `vycount.github.io`
- `www`, `softworks`, `estates`, `manufacturing`, `media`: CNAME `vycount.github.io`
- `_github-pages-challenge-vycount`: TXT record that verifies vycount.com for the `vycount` GitHub account. Don't delete it.
- Email records (Zoho: MX, SPF, DKIM, DMARC) are separate and unaffected.
