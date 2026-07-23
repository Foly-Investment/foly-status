# CLAUDE.md — foly-status

Guidance for Claude Code when working in **foly-status**. Self-contained: ships in this repo.

## What this repo is

The **status page** for Foly (`status.foly.app`), powered by **Upptime**. There is **no
build and no application code** — monitoring runs entirely as a GitHub Actions cron that
probes endpoints, records results, and regenerates a static site published to GitHub Pages.

## How it works

- **Config:** [`.github/upptime.config.yml`](.github/upptime.config.yml) — owner
  `Foly-Investment`, repo `foly-status`, dark theme, CNAME `status.foly.app`, single locale
  (`en`), issues assigned to `eugenkhudoliiv`.
- **Workflow:** [`.github/workflows/uptime.yml`](.github/workflows/uptime.yml) — runs
  `upptime/uptime-monitor@v2` on `schedule: */5 * * * *` (every 5 min) + `workflow_dispatch`.
  Needs `contents: write`, `issues: write`, `pull-requests: write`.

### Monitored endpoints (current)

| Name | URL | Expect |
| --- | --- | --- |
| Foly API | `https://api.foly.app/healthz` | 200 + body contains `"status":"healthy"` |
| Foly App | `https://app.foly.app` | 200 |
| Foly Marketing | `https://foly.app` | 200 |

## Add / change a monitored endpoint

Edit the `sites:` list in [`.github/upptime.config.yml`](.github/upptime.config.yml):

```yaml
- name: Service Name
  url: https://example.com/healthz
  expectedStatusCodes: [200]   # optional; default accepts any 2xx/3xx
  method: GET           # optional, default GET
  # optional: timeout, icon, group, shouldNotify
```

Commit it — the cron picks it up within ~5 minutes. To change the polling cadence, edit the
cron in the workflow.

## Generated — do NOT hand-edit

The workflow regenerates these on every run; local edits are overwritten:
`history/`, `api/`, `graphs/`, `status-website/`, and the `README.md` badges/table.

## Notes & gotchas

- **No notification backends** are configured (no Slack/Discord/email/Telegram). Down
  endpoints open GitHub issues assigned to the configured assignee; add a `notifications`
  block to the config if alerts are needed.
- Upptime authenticates via a GitHub App; credentials are org-level
  variables/secrets (`UPPTIME_GH_APP_*`) — not in this repo.
- The published status page is **public**.
- Conventional Commits enforced if husky/commitlint is set up; otherwise just edit YAML → commit.

## Related

- App/API/marketing repos under [Foly-Investment](https://github.com/Foly-Investment).
- Product context: the [foly](https://github.com/Foly-Investment/foly) docs repo.
