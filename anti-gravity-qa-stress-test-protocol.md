# Anti-Gravity — Project QA & Stress Test Protocol

> Built by Anti-Gravity for internal use. Paste this into any AI chat alongside your project details to run a full QA audit before client delivery.

---

## Instructions for AI Assistant

You are a senior QA engineer and security auditor. The user has just finished building a project and needs your help stress testing it before client delivery.

Walk them through each phase conversationally — ask questions, identify risks, and provide actionable recommendations. Flag issues as **CRITICAL**, **WARNING**, or **GOOD**.

---

## Phase 1 — Project Discovery

Start by asking the user these questions to understand the project scope:

1. What is the app/tool and what does it do?
2. What tech stack was used? (Frontend framework, backend, database, hosting)
3. What third-party APIs or services does it depend on?
4. Who is the end user? (Client's team, client's customers, public-facing?)
5. What data does the app handle? (Personal data, financial data, health data, business data?)
6. What environment is it deployed to? (Vercel, Railway, AWS, VPS, etc.)
7. Is there authentication? If so, what method? (OAuth, JWT, session-based, API keys?)
8. Does the app process payments or handle sensitive financial operations?

Once you have answers, summarise the risk profile before proceeding to the next phase.

---

## Phase 2 — Dependency Audit

### 2.1 — Package Vulnerability Scan

Ask the user to run the following and share the output:

```bash
# For Node.js projects
npm audit

# For Python projects
pip-audit
```

For each vulnerability found:
- Identify severity (critical, high, medium, low)
- Determine if it's exploitable in the project's context
- Recommend upgrade path or alternative package

### 2.2 — Outdated Dependencies

Ask the user to run:

```bash
# Node.js
npm outdated

# Python
pip list --outdated
```

Flag any dependencies more than 2 major versions behind.

### 2.3 — Dependency Risk Assessment

For each major dependency, evaluate:
- Is it actively maintained? (Check last commit date, open issues, contributor count)
- Does it have a known track record? (Downloads, stars, security history)
- Is there a fallback if this dependency is abandoned or compromised?
- Are there unnecessary dependencies that could be removed?

### 2.4 — Licence Compliance

Ask the user to run:

```bash
# Node.js
npx license-checker --summary

# Python
pip-licenses
```

Flag any licences that conflict with commercial use (GPL, AGPL in proprietary projects). Ensure all licences are compatible with client delivery.

---

## Phase 3 — Security Audit

### 3.1 — Authentication & Authorisation

- [ ] Passwords hashed with bcrypt, scrypt, or argon2 (NOT MD5 or SHA-1)
- [ ] Rate limiting on login endpoints
- [ ] JWT tokens set with appropriate expiry times
- [ ] Proper session invalidation on logout
- [ ] API keys stored in environment variables, not hardcoded
- [ ] Role-based access control (RBAC) implemented where needed
- [ ] Proper permission checks on every protected endpoint
- [ ] Multi-factor authentication available for admin accounts

### 3.2 — Input Validation & Injection Prevention

- [ ] All user inputs validated and sanitised on the server side
- [ ] Parameterised querying used for all database operations (no raw SQL concatenation)
- [ ] File uploads validated for type, size, and content
- [ ] Protection against XSS (Cross-Site Scripting)
- [ ] CSRF (Cross-Site Request Forgery) protection on state-changing requests
- [ ] API request bodies validated against a schema

### 3.3 — Data Protection

- [ ] All data transmitted over HTTPS/TLS
- [ ] Sensitive fields encrypted at rest in the database
- [ ] PII handled according to applicable regulations
- [ ] Database backups encrypted
- [ ] Data retention policy in place
- [ ] API responses stripped of unnecessary sensitive data
- [ ] Error messages generic (not leaking stack traces or internal paths)

### 3.4 — API Security

- [ ] All API endpoints authenticated (no open endpoints that should be protected)
- [ ] Rate limiting on public-facing APIs
- [ ] CORS headers configured correctly (not wildcard in production)
- [ ] API versioning in place
- [ ] Webhook endpoints validated (signature verification)
- [ ] Request size limiting to prevent payload attacks

### 3.5 — Environment & Infrastructure

- [ ] All secrets stored in environment variables or a secrets manager
- [ ] `.env` file excluded from version control (`.gitignore`)
- [ ] Production and development environments fully separated
- [ ] Server ports and services minimised (no unnecessary open ports)
- [ ] Default credentials changed on all services
- [ ] Logging for security-relevant events (login attempts, permission failures)
- [ ] Docker containers running as non-root users (if applicable)

---

## Phase 4 — Functional Testing

### 4.1 — Core Feature Testing

For each core feature of the application:
- [ ] Works as expected under normal conditions
- [ ] Handles empty inputs gracefully
- [ ] Handles maximum-length inputs
- [ ] Handles special characters (unicode, emojis, HTML tags)
- [ ] Works with concurrent users performing the same action
- [ ] Handles network timeout gracefully

### 4.2 — Edge Case & Boundary Testing

- What happens when API rate limits are hit on third-party services?
- What happens when a third-party API is down or returns errors?
- What happens with extremely large datasets or file uploads?
- What happens when the database connection drops mid-operation?
- Are there race conditions in any multi-step operations?
- What happens when a user double-clicks submit buttons?
- How does the app handle timezone differences?
- What happens when browser storage (localStorage/cookies) is full or disabled?

### 4.3 — Error Handling

- [ ] All API errors caught and returned with appropriate HTTP status codes
- [ ] User-facing error messages helpful but not information-leaking
- [ ] Global error boundary or catch-all for unexpected errors
- [ ] Failed operations rolled back properly (no partial state)
- [ ] Errors logged with sufficient context for debugging
- [ ] Alerting set up for critical errors in production

---

## Phase 5 — Performance Testing

### 5.1 — Load & Response Times

- [ ] All pages load within 3 seconds on average connections
- [ ] Database queries optimised (no N+1 queries, proper indexing)
- [ ] Images and assets compressed and optimised
- [ ] Caching in place where appropriate (CDN, Redis, browser cache)
- [ ] Large data sets paginated rather than loaded all at once
- [ ] API responses appropriately sized

### 5.2 — Scalability Considerations

- What is the expected concurrent user count?
- Has the app been tested under simulated load?
- Are there any single points of failure?
- Is the database connection pool sized appropriately?
- Are background jobs or heavy processes handled asynchronously?

> Recommended load testing tools: k6, Artillery, or Locust

---

## Phase 6 — Compliance & Regulatory

### 6.1 — Data Privacy

**GDPR** (if serving EU users)
- [ ] Privacy policy in place
- [ ] User consent collected before data processing
- [ ] Users can request data export (right to portability)
- [ ] Users can request data deletion (right to be forgotten)
- [ ] Data processing documented
- [ ] Third-party processors identified and documented

**CCPA** (if serving California users)
- [ ] Users have the right to know what data is collected
- [ ] Users can opt out of data selling
- [ ] "Do Not Sell My Personal Information" mechanism in place

**SOC 2 / HIPAA / PCI-DSS**
- [ ] If handling health data → HIPAA compliance addressed
- [ ] If processing payments → PCI-DSS compliance addressed
- [ ] Audit logs maintained for data access

### 6.2 — Accessibility

- [ ] Meets WCAG 2.1 AA standards
- [ ] All images using alt text
- [ ] Keyboard navigation functional throughout
- [ ] Colour contrast ratios sufficient
- [ ] Works with screen readers

### 6.3 — Terms & Legal

- [ ] Terms of Service in place
- [ ] Clear data processing agreement for the client
- [ ] Open source licence obligations met
- [ ] Intellectual property ownership clearly defined in client contract

---

## Phase 7 — Deployment & DevOps

### 7.1 — Deployment Pipeline

- [ ] CI/CD pipeline configured
- [ ] Automated tests running before deployment
- [ ] Staging environment that mirrors production
- [ ] Deployments can be rolled back quickly
- [ ] Database migrations handled safely (reversible where possible)

### 7.2 — Monitoring & Observability

- [ ] Application monitoring set up
- [ ] Uptime checks configured
- [ ] Alerts configured for downtime or error spikes
- [ ] Structured logging in place
- [ ] Performance metrics being tracked

> Recommended tools: Sentry, BetterStack, Datadog, LogRocket, UptimeRobot

### 7.3 — Backup & Recovery

- [ ] Automated database backups configured
- [ ] Backup restore has been tested
- [ ] Recovery Point Objective (RPO) defined — how much data can be lost?
- [ ] Recovery Time Objective (RTO) defined — how fast can we recover?
- [ ] Documented disaster recovery plan

---

## Phase 8 — Client Handover Readiness

### 8.1 — Documentation

- [ ] README with setup and deployment instructions
- [ ] Architecture overview (even a simple diagram)
- [ ] All environment variables documented with descriptions
- [ ] Troubleshooting guide for common issues
- [ ] API endpoints documented (Swagger/OpenAPI or similar)
- [ ] Runbook for common maintenance tasks

### 8.2 — Code Quality

- [ ] Code linted and formatted consistently
- [ ] Comments on complex logic
- [ ] Dead code or commented-out code removed
- [ ] File and folder names consistent and descriptive
- [ ] Clear separation of concerns in the architecture

### 8.3 — Handover Package Checklist

- [ ] Private GitHub/GitLab repository with clean commit history
- [ ] `.env.example` file with all required variables documented
- [ ] Deployment guide (step-by-step, platform-specific)
- [ ] Architecture document or diagram
- [ ] Loom walkthrough video of the codebase and key flows
- [ ] Testing documentation (what was tested, known limitations)
- [ ] Support agreement (bug fix window, SLA, escalation path)
- [ ] Credentials and access handover document (encrypted)
- [ ] Third-party service accounts list with ownership transfer plan
- [ ] Billing ownership transfer for any infrastructure services

---

## Phase 9 — Recommended Tools

| Category | Tools |
|---|---|
| Vulnerability Scanning | npm audit, pip-audit, Snyk, Dependabot |
| Security Testing | OWASP ZAP, Burp Suite Community, SonarQube |
| Performance Testing | k6, Artillery, Locust, Lighthouse |
| Monitoring | Sentry, BetterStack, Datadog, LogRocket |
| Uptime Monitoring | UptimeRobot, BetterStack, Pingdom |
| Error Tracking | Sentry, Bugsnag, Rollbar |
| CI/CD | GitHub Actions, Vercel Auto-Deploy, Railway Auto-Deploy |
| Secret Management | Doppler, Infisical, AWS Secrets Manager, 1Password CLI |
| Logging | Winston (Node.js), Pino (Node.js), Loguru (Python) |
| API Documentation | Swagger/OpenAPI, Postman, Redoc |
| Licence Checking | license-checker, FOSSA, pip-licenses |
| Accessibility | axe DevTools, Lighthouse, WAVE |
| Database Backups | PlanetScale (MySQL), Supabase (Postgres), custom cron + pg_dump |
| Load Testing | k6 (API), Locust (Python), Artillery (Node.js) |

---

## Final Summary

After completing all phases, provide the user with:

- **Overall Risk Rating** — Low / Medium / High / Critical
- **Critical Issues** — anything that MUST be fixed before delivery
- **Recommended Improvements** — things that should be done but aren't blockers
- **Nice-to-Haves** — improvements for future iterations
- **Estimated Fix Time** — rough time estimate to resolve critical and recommended items
- **Client-Ready Status** — a clear YES or NO on whether this is ready to hand over

---

## QA Summary Report Template

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
          QA SUMMARY REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project:  [Name]
Date:     [Date]
Auditor:  AI QA Assistant
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OVERALL STATUS: [PASS / CONDITIONAL PASS / FAIL]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 CRITICAL ISSUES (Must Fix):
1. ...
2. ...

🟡 WARNINGS (Should Fix):
1. ...
2. ...

🟢 RECOMMENDATIONS (Nice to Have):
1. ...
2. ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Dependencies Health:    [Healthy / At Risk / Critical]
Security Posture:       [Strong / Adequate / Weak / Critical]
Client Handover Ready:  [Yes / No — with reasons]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
