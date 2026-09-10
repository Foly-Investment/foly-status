# CLAUDE.md — foly-status

Guidance for Claude Code when working in **foly-status**. Self-contained: ships in this repo.

## What this repo is

The **status page** for Foly (`status.foly.app`), powered by **Upptime**. There is **no
build and no application code** — monitoring runs entirely as a GitHub Actions cron that
probes endpoints, records results, and regenerates a static site published to GitHub Pages.

## How it works

- **Config:** [`.upptimerc.yml`](.upptimerc.yml) — owner
  `Foly-Investment`, repo `foly-status`, dark theme, CNAME `status.foly.app`, issues assigned to `shikigami12`.
  **Upptime only reads this path** — the config sat at `.github/upptime.config.yml`
  until 2026-09-10, which is why every run failed and the workflow was disabled.
- **Workflows:** the eight files under [`.github/workflows/`](.github/workflows/) are the
  upstream Upptime template (v1.44) verbatim — Setup, Uptime (every 5 min), Response Time,
  Graphs, Static Site (builds and deploys to `gh-pages`), Summary (rewrites `README.md`),
  Update Template, Updates. **Setup CI regenerates them from `.upptimerc.yml`**; never edit
  them by hand.

### Monitored endpoints (current)

| Name | URL | Expect |
| --- | --- | --- |
| Foly API | `https://api.foly.app/healthz` | 200 + body contains `"status":"healthy"` |
| Foly App | `https://app.foly.app/en-US/` | 200 |
| Foly Marketing | `https://foly.app` | 200 |

## Add / change a monitored endpoint

Edit the `sites:` list in [`.upptimerc.yml`](.upptimerc.yml):

```yaml
- name: Service Name
  url: https://example.com/healthz
  expectedStatusCodes: [200]   # optional; default accepts any 2xx/3xx
  method: GET           # optional, default GET
  # optional: timeout, icon, group, shouldNotify
```

Commit it — Setup CI regenerates the workflows and the next Uptime CI run picks it up.
Change the cadence with `workflowSchedule` in the config, not in a workflow file.

## Generated — do NOT hand-edit

The workflows regenerate these; local edits are overwritten: `history/`, `api/`, `graphs/`,
`status-website/`, `README.md` (Summary CI) and `.github/workflows/*.yml` (Setup CI).

## Notes & gotchas

- **No notification backends** are configured (no Slack/Discord/email/Telegram). Down
  endpoints open GitHub issues assigned to the configured assignee; add a `notifications`
  block to the config if alerts are needed.
- **Auth:** every workflow uses `secrets.GH_PAT` (a classic PAT with `repo` + `workflow`
  scopes) or a GitHub App (`vars.GH_APP_ID` + `secrets.GH_APP_PRIVATE_KEY`), falling back to
  `GITHUB_TOKEN` — which cannot regenerate workflows (Setup CI) or trigger downstream runs,
  so one of the two must be set on the repo.
- **Publishing:** Static Site CI pushes `gh-pages`; GitHub Pages must be enabled on that
  branch with custom domain `status.foly.app`, and Cloudflare DNS needs
  `CNAME status → foly-investment.github.io` (DNS-only until the certificate is issued).
- The published status page is **public**.
- Conventional Commits enforced if husky/commitlint is set up; otherwise just edit YAML → commit.

## Related

- App/API/marketing repos under [Foly-Investment](https://github.com/Foly-Investment).
- Product context: the [foly](https://github.com/Foly-Investment/foly) docs repo.
