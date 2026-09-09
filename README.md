# Job Radar

Personal job-search automation for Hadar Wolf — finds Bachelor's-level student positions in software, hardware, and AI/ML across Israeli tech hubs, and tracks them in a live dashboard.

## How it works

1. **Sourcing** — reads LinkedIn saved-search alert emails (never scrapes linkedin.com directly) plus public web search for company/startup career pages. No login automation, no bot detection bypass.
2. **Filtering** — matches against [`profile.md`](profile.md) (skills, target roles, locations) and enforces a hard filter for Bachelor's-level eligibility, excluding Master's/PhD-only postings.
3. **Tracking** — new leads are appended to [`state.json`](state.json), which is the source of truth for the dashboard.
4. **Dashboard** — [`dashboard.html`](dashboard.html) is a static, self-contained page (published live at the URL in [`dashboard_url.txt`](dashboard_url.txt)) listing every lead with a status pipeline (New → Reviewing → Seeking referral → Applied / Not a fit), a suggested pitch per posting, and a one-click "find people at this company" link for chasing a referral before applying. Status marks are saved in the viewer's browser only.
5. **Automation** — [`scheduled-task.md`](scheduled-task.md) is the prompt driving the recurring job (runs hourly 7am–8pm Israel time, archives processed alert emails out of the inbox, updates `state.json` and republishes the dashboard).

## Files

| File | Purpose |
|---|---|
| `profile.md` | Candidate profile, target roles/locations, and the degree-level hard filter |
| `state.json` | Current job leads (source of truth) |
| `dashboard.html` | The dashboard UI — the `JOBS` array between `JOBS_START`/`JOBS_END` markers is regenerated from `state.json` each run |
| `dashboard_url.txt` | The published dashboard's live URL |
| `scheduled-task.md` | The automation's full prompt/instructions |

## Notes

- Never auto-applies to anything — every lead gets a human read before any application goes out.
- LinkedIn alert emails are archived (not deleted) after processing, tagged `LinkedIn Alerts (Processed)`, to keep the inbox clean.
- The scheduled task only runs while the Claude Code app is open, so it isn't a fully independent server-side cron — expect gaps if the app hasn't been running.
