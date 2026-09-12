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
