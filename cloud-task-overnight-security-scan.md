# Cloud Task Prompt: Overnight Security Scan

Scans your codebase every night for exposed secrets, vulnerable dependencies, and new CVEs. Report is waiting for you every morning without fail.

---

## Prompt — Copy & Paste Into Claude Code

Run a comprehensive security audit of this repository. Check the following:

1. **Exposed secrets** — scan all files for hardcoded API keys, tokens, passwords, private keys, or credentials that should be in environment variables
2. **Dependency vulnerabilities** — check `package.json` / `requirements.txt` / `Cargo.toml` for packages with known CVEs or security advisories
3. **Outdated dependencies** — flag any critical dependencies more than 2 major versions behind
4. **Permission issues** — check for overly permissive file permissions, open CORS policies, or misconfigured auth middleware
5. **Recent commits** — review commits from the last 24 hours for any changes that introduce security risks

Generate a report with:
- **CRITICAL** — must fix immediately (exposed secrets, known exploits)
- **WARNING** — should fix soon (outdated deps, weak patterns)
- **INFO** — worth noting (minor improvements, best practice suggestions)

If any Critical issues are found, post to Slack #security immediately. Otherwise, post the daily summary to Slack #security-daily.

---

## Recommended Schedule

| Setting | Value |
|---|---|
| Frequency | Daily at 3am |
| Why 3am | Runs after most commits are pushed for the day, report ready by morning |

---

## Why Cloud Matters Here

Security scans need to run every single day without gaps. Not "whenever you open your laptop." A missed day could mean an exposed API key sits in your repo for 48 hours instead of 8.
