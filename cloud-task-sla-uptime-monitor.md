# Cloud Task Prompt: SLA & Uptime Monitor

Continuously checks your API and services for downtime, errors, or performance degradation. Catches outages at 2am so you find out in minutes, not hours.

---

## Prompt — Copy & Paste Into Claude Code

Monitor the health of our production services. Check the following:

1. **API availability** — hit all critical endpoints and confirm they return 200 status codes within acceptable response times
2. **Error rate** — review recent logs for any spike in 5xx errors or timeout patterns
3. **Database connections** — check for connection pool exhaustion or slow query warnings
4. **Third-party dependencies** — verify external APIs and services we depend on are responding

If any service is degraded or down:
- Post immediately to Slack #incidents with severity level (P1/P2/P3)
- Include: what's affected, when it started, and suggested next steps
- If possible, identify the root cause from recent commits or config changes

If everything is healthy, log a brief "all systems operational" confirmation.

---

## Recommended Schedule

| Setting | Value |
|---|---|
| Frequency | Every 30 minutes or every hour |
| Active Window | 24/7 — this is the whole point |

---

## Why Cloud Matters Here

The difference between a 20-minute outage and a 6-hour one. Your laptop being closed at 2am shouldn't mean your users discover the problem before you do.
