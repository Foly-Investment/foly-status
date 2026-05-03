# Foly Status

Status page for Foly services, powered by [Upptime](https://upptime.js.org).

Live at: https://status.foly.app

## Services monitored

- **Foly API** — https://api.foly.app/healthz
- **Foly App** — https://app.foly.app
- **Foly Marketing** — https://foly.app

## Setup (human steps required)

1. Create the GitHub repo: `gh repo create Foly-Investment/foly-status --public`
2. Push this directory to the remote
3. Install the [Upptime GitHub App](https://github.com/apps/upptime) on the `Foly-Investment` organization
4. Add the Upptime app credentials as repository variables/secrets (see workflow file)
5. Enable GitHub Pages on the `gh-pages` branch
6. Set Cloudflare CNAME: `status` → `foly-investment.github.io` (proxy: DNS only)
