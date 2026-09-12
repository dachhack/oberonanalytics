# Deploying oberonanalytics.ai on GitHub Pages

## One-time, on GitHub
1. Create the repository `dachhack/oberonanalytics`: public (Pages on a personal account requires public), no README, no license, no .gitignore.
2. After the first push: Settings > Pages > Build and deployment > Source: "Deploy from a branch", Branch: `main`, folder `/ (root)`. Save.
3. Settings > Pages > Custom domain: `oberonanalytics.ai` (the CNAME file in this repo sets it too). Tick "Enforce HTTPS" once the DNS check passes (can take up to an hour after DNS changes).

## DNS, in Squarespace (Domains > oberonanalytics.ai > DNS settings)
Delete the Squarespace parking records for `@` and `www`, then add:

| type | host | value |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | dachhack.github.io |

GitHub will serve both `oberonanalytics.ai` and `www.oberonanalytics.ai` and redirect one to the other. This also fixes the current apex certificate error (Squarespace parking only has a cert for www).

## Email on the domain
Squarespace DNS can hold MX records for Google Workspace, or move DNS to Cloudflare (free) and use Cloudflare Email Routing to forward `matt@oberonanalytics.ai` to Gmail at no cost. Either way, add the MX records before publishing the address on the site.

## What is in this repo
- `index.html`: the site. Single page, no build step, no external dependencies.
- `assets/`: images.
- `CNAME`, `.nojekyll`: GitHub Pages configuration.
The parody page is intentionally not in this repo; it lives in the private job-search repo.

## Email status (checked 2026-09-12)
The domain has no MX records and SPF is `v=spf1 -all`, so `matt@oberonanalytics.ai` does not exist and
mail to it bounces. The site links to mlporritt@gmail.com until one of these is done:
1. **Google Workspace via Squarespace** (paid, ~$7/user/month): Squarespace adds the MX/SPF/DKIM records
   itself. Simplest.
2. **Cloudflare Email Routing** (free): move nameservers to Cloudflare, add the routing rule
   `matt@oberonanalytics.ai -> mlporritt@gmail.com`, and set Gmail "Send mail as" to reply from the
   domain. The A/CNAME records above move to Cloudflare unchanged.
Whichever is chosen, replace `v=spf1 -all` with the provider's SPF record or outbound mail will be rejected.

## Which page is at the root

As of 2026-09-12 the **parody** is served at the root (`index.html`) and the real site lives at
`real/index.html` (https://oberonanalytics.ai/real/). `parody/` redirects to `/`. The parody keeps its
`noindex, nofollow` tag, so while it is at the root the home page is not indexed by search engines;
`/real/` is indexable.

To swap back: move `real/index.html` to `index.html` (asset path `assets/`), move the parody to
`parody/index.html` (asset path `../assets/`, banner link `/`), and delete the redirect.
