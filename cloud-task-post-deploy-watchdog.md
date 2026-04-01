# Cloud Task Prompt: Post-Deploy Watchdog

Monitors your app after deployment while your laptop is closed. Checks error rates, build status, and API health on a recurring schedule overnight.

---

## Prompt — Copy & Paste Into Claude Code

Check the latest deployment on the main branch. Review the following:

1. **Error logs** — scan for any new errors, exceptions, or stack traces that appeared after the most recent deploy
2. **Build status** — confirm CI/CD pipeline is green and no tests are failing
3. **API health** — check response times and status codes for critical endpoints

If anything looks wrong:
- Summarise the issue in 2-3 sentences
- Identify the likely cause
- Suggest a fix or rollback strategy

If everything looks healthy, confirm with a short "all clear" summary.

Post the results to Slack in #deployments.

---

## Recommended Schedule

| Setting | Value |
|---|---|
| Frequency | Every 2 hours |
| Active Window | 6pm → 8am (overnight after deploy) |
| Alternative | Daily at 6am for a morning health report |

---

## Why Cloud Matters Here

You deploy at 5pm and close your laptop. Local tasks can't fire. Cloud runs at 6pm, 8pm, 10pm, midnight — catching issues before your users do.
