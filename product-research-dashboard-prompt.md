# Automated Product Research Dashboard Generator
> A complete pipeline using NotebookLM to generate deep research, artifacts, and a glassmorphic HTML dashboard for SaaS/software product evaluation. Takes a product name and evaluation context and builds a local research website.

---

## Overview

Operate as an elite product analyst and engineer, automatically building a comprehensive web dashboard full of deep-researched artifacts for any software product or SaaS tool under evaluation, leveraging NotebookLM MCP tools.

### The Goal

Take a raw input (product name, vendor, and/or evaluation context) and transform it into a ready-to-view "Product Research Dashboard" folder containing:

1. A suite of AI-generated Markdown documents (product evaluation, competitive analysis, market report, risk assessment, pros & cons, pricing breakdown)
2. Downloaded media artifacts (audio podcast MP3, market infographic PNG)
3. A beautiful, offline-ready HTML dashboard unifying everything for the decision-maker

---

## Trigger Details

Input can come from any of the following sources:

| Source | Description |
|---|---|
| **Zapier / Make** | Watching a Slack channel, Jira board, or procurement queue for new product evaluation requests |
| **Email / CRM** | A vendor demo follow-up or internal tooling request arriving in an inbox |
| **Manual prompt** | User types a product name and evaluation context directly |

**Minimum required payload:** `product_name`

**Optional fields:** `vendor_name` · `product_url` · `product_category` · `evaluation_purpose` · `current_tool` (what it would replace) · `budget_range` · `team_size` (how many users/seats needed) · `must_have_integrations`

---

## Execution Pipeline

```
Phase 0 → Agent Pre-Research (Data Enrichment)
Phase 1 → Notebook Preparation & Initial Ingestion
Phase 2 → Autonomous Deep Research
Phase 3 → Artifact Generation & Extraction
Phase 4 → Index File Generation
Phase 5 → HTML Dashboard Construction
```

---

## Phase 0 — Agent Pre-Research
*Data Enrichment — before relying on NotebookLM*

> [!important] Seed data quality determines output quality
> NotebookLM's outputs are only as good as the sources you feed it — garbage in, garbage out.

### Step 0.1 — Product Website Scrape

Use web scraping tools (Firecrawl, Exa, Tavily, or Apify via MCP) to read the product's official website. Target:

- Homepage / Product overview page
- Features / Capabilities page
- Pricing page (tiers, limits, enterprise options)
- Integrations / Marketplace / API documentation page
- Security / Compliance / Trust page (SOC 2, GDPR, uptime SLA)
- Changelog / Release notes page (reveals development velocity)
- Documentation / Knowledge base (reveals product maturity and depth)
- Status page (historical uptime)

### Step 0.2 — External Enrichment

Search the broader web for:

- G2, Capterra, and TrustRadius reviews (star ratings, review volume, sentiment trends)
- Reddit and Hacker News discussions (unfiltered user sentiment, complaints, praise)
- Crunchbase or PitchBook data on the vendor (funding rounds, investors, valuation, runway)
- Recent news articles, press releases, or founder interviews (last 6 months)
- YouTube product demos, walkthroughs, and review videos
- Competitor comparison pages (both from this product and from rivals)
- Stack Overflow, GitHub issues, or community forums (developer experience signals)
- Job postings (reveals tech stack, scaling challenges, and product direction)

### Step 0.3 — Synthesize Product Profile

Combine all scraped data into a single distilled "Product Profile" document including:

- Product name, vendor name, HQ location, founding year
- CEO/founder names and backgrounds
- Employee count range and recent hiring trends
- Funding history (rounds, amounts, lead investors, estimated runway)
- Revenue model (subscription tiers, usage-based, freemium, hybrid)
- Core capabilities with brief descriptions of each
- Target audience / ideal customer profile (who is this built for?)
- Publicly named customers or case studies
- Published integrations and API availability
- Security posture (certifications, compliance, SSO, data residency options)
- Direct competitors (named on comparison pages or obvious from market position)
- Review sentiment summary (average ratings across G2/Capterra/TrustRadius, common praise, common complaints)
- Recent notable updates (last 2–3 significant releases or announcements)
- Evaluation context (why are we evaluating this? what would it replace? what problem does it solve for us?)

> [!tip] This profile becomes the foundational "seed" for NotebookLM — ensuring the AI never hallucinates because it starts with verified facts.

---

## Phase 1 — Notebook Preparation & Initial Ingestion

### Step 1.1 — Create Notebook

```
mcp: notebook_create
title: "Product Research - [Product Name]"
```

### Step 1.2 — Inject Seed Data as Text Source

```
mcp: source_add
notebook_id: [from step 1.1]
source_type: "text"
title: "[Product Name] — Product Profile"
text: [the synthesized Product Profile from Phase 0.3]
wait: true
```

### Step 1.3 — Add Scraped URLs as Sources

For each high-quality URL found during Phase 0 (product website, review aggregators, key articles, documentation):

```
mcp: source_add
notebook_id: [from step 1.1]
source_type: "url"
url: "[each URL]"
wait: true
```

> [!tip] Add 3–8 of the best URLs. Don't exceed 10 here — deep research will find more.

---

## Phase 2 — Autonomous Deep Research
*NotebookLM Web Search*

### Step 2.1 — Start Deep Research

```
mcp: research_start
notebook_id: [from step 1.1]
query: "[Product Name] vs alternatives review comparison [product category] features pricing limitations 2025 2026"
source: "web"
mode: "deep"
```

| Mode | Speed | Sources Found |
|---|---|---|
| `deep` | ~5 minutes | 40–100+ sources |
| `fast` | ~30 seconds | ~10 sources |

### Step 2.2 — Poll Until Complete

```
mcp: research_status
notebook_id: [from step 1.1]
max_wait: 300
```

Wait for `status: "completed"` before proceeding.

### Step 2.3 — Batch Import Sources

> [!warning] Critical — never bulk-import all sources at once
> Importing 40–110+ sources simultaneously **will** cause an MCP timeout. Always batch in chunks of 20.

```
# Import sources 0–19
mcp: research_import
notebook_id: [from step 1.1]
task_id: [from step 2.1]
source_indices: [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19]

# Import sources 20–39
mcp: research_import
source_indices: [20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39]

# Continue in chunks of 20 until all sources imported...
```

Wait 2–3 seconds between each batch to let NotebookLM process incoming sources.

> [!tip] If using `mode="fast"`, you can safely omit `source_indices` to import all ~10 sources at once.

---

## Phase 3 — Artifact Generation & Extraction

### Step 3.1 — Product Evaluation Report (`01_product_evaluation.md`)

Query prompt:
```
"Create a comprehensive product evaluation report with these exact sections:
1) Product Overview (what it is, who it's for, core value proposition, founding story, vendor maturity signals)
2) Feature Deep Dive (categorised feature breakdown — list each major capability area with sub-features, noting standout strengths and notable gaps)
3) User Experience & Onboarding (setup complexity, learning curve, documentation quality, support channels and responsiveness)
4) Security & Compliance (certifications held, SSO/SAML support, data residency, encryption, audit logging, GDPR/SOC2/ISO status)
5) Integration Ecosystem (native integrations, API quality and documentation, webhook support, marketplace/plugin availability, iPaaS compatibility)
6) Verdict Summary (a 3-sentence executive summary: what it does well, where it falls short, and who should consider it)"
```

### Step 3.2 — Market & Ecosystem Report (`02_market_ecosystem_report.md`)

```
mcp: notebook_query
notebook_id: [from step 1.1]
query: "Write a market and ecosystem research report covering: the overall market this product
operates in (TAM, growth trajectory, key trends for the next 2 years), the vendor's position
within that market (leader, challenger, niche), ecosystem health signals (partner count,
community activity, third-party integrations, developer adoption), and a table of the
top 10 most important sources discovered with columns for Source Name, Type (review,
article, documentation, forum), and Key Takeaway."
```

### Step 3.3 — Competitive Landscape & Alternatives (`03_competitive_landscape.md`)

```
mcp: notebook_query
query: "Create a detailed competitive analysis formatted as:
1) Feature Comparison Matrix — a table with the evaluated product and its top 4–5 competitors
as columns, and 15–20 key feature/capability areas as rows. Use ✅ (full support),
⚠️ (partial/limited), ❌ (not available) indicators in each cell.
2) Pricing Comparison — a table showing each competitor's pricing tiers, per-seat cost
(or usage-based equivalent), free tier availability, and enterprise pricing transparency.
3) Competitor Spotlight — for each of the top 3 alternatives, provide: a 2-sentence summary
of their positioning, their key advantage over the evaluated product, and their key
disadvantage relative to the evaluated product.
4) Migration & Switching Considerations — any known lock-in risks, data export capabilities,
or migration tooling available."
```

### Step 3.4 — Market Infographic (`04_market_infographic.png`)

```
mcp: studio_create
notebook_id: [from step 1.1]
artifact_type: "infographic"
orientation: "portrait"
detail_level: "detailed"
confirm: true
```

Poll `studio_status` until complete, then:

```
mcp: download_artifact
notebook_id: [from step 1.1]
artifact_type: "infographic"
output_path: "[research_folder]/04_market_infographic.png"
```

### Step 3.5 — Audio Research Briefing (`audio_briefing.mp3`)

```
mcp: studio_create
notebook_id: [from step 1.1]
artifact_type: "audio"
audio_format: "brief"
audio_length: "short"
confirm: true
```

Poll `studio_status` until complete, then:

```
mcp: download_artifact
notebook_id: [from step 1.1]
artifact_type: "audio"
output_path: "[research_folder]/audio_briefing.mp3"
```

### Step 3.6 — Risk Assessment & Red Flags (`05_risk_assessment.md`)

```
mcp: notebook_query
notebook_id: [from step 1.1]
query: "Create a product risk assessment with these sections:
1) Vendor Viability — funding runway, profitability signals, acquisition risk, leadership
stability. Rate as LOW / MEDIUM / HIGH risk with evidence.
2) Product Maturity — how long has it been in market, release cadence, roadmap transparency,
history of breaking changes. Rate as LOW / MEDIUM / HIGH risk with evidence.
3) Lock-in & Portability — data export options, proprietary formats, API completeness for
migration, contractual lock-in clauses. Rate as LOW / MEDIUM / HIGH risk with evidence.
4) Security & Privacy Concerns — any known breaches, CVEs, missing certifications, dependency
on third-party sub-processors. Rate as LOW / MEDIUM / HIGH risk with evidence.
5) Community & Support Risk — support tier quality, response times, community health (active
forums, GitHub stars/issues), documentation freshness. Rate as LOW / MEDIUM / HIGH risk with evidence.
6) Overall Risk Matrix — a summary table with columns: Risk Category, Rating (LOW/MEDIUM/HIGH),
Key Evidence, Mitigation Strategy."
```

### Step 3.7 — Pros & Cons Analysis (`06_pros_and_cons.md`)

```
mcp: notebook_query
notebook_id: [from step 1.1]
query: "Create a thorough pros and cons analysis of [Product Name]. Format the output as
clearly separated PRO and CON items. For each item provide:
- A bold headline (e.g., 'Exceptional API documentation')
- 2–3 sentences of supporting evidence drawn from reviews, documentation, or research
- A 'Weight' tag indicating impact: CRITICAL / HIGH / MEDIUM / LOW

Aim for at least 8 Pros and 8 Cons. Draw from real user reviews, feature analysis,
pricing evaluation, support experiences, and competitive positioning. Be candid and
balanced — do not sugarcoat weaknesses or overstate strengths."
```

> [!tip] Fallback for thin Pros & Cons
> If the output contains fewer than 6 items on either side, run a supplementary `notebook_query`:
> ```
> "Generate 10 additional pro/con pairs for [Product Name] covering areas like:
> pricing value, onboarding ease, scalability, mobile experience, reporting,
> customer support, API quality, and admin controls.
> Format each as 'PRO: [headline] — [evidence]' or 'CON: [headline] — [evidence]'
> with a Weight tag (CRITICAL/HIGH/MEDIUM/LOW)."
> ```
> Append the output to the pros and cons file.

### Step 3.8 — Pricing & Licensing Breakdown (`07_pricing_breakdown.md`)

```
mcp: notebook_query
notebook_id: [from step 1.1]
query: "Create a detailed pricing and licensing analysis with these sections:
1) Tier Comparison Table — columns for each pricing tier (including free/trial if available),
with rows for: monthly price, annual price, per-seat cost, included features, usage limits,
storage limits, support level, and SSO/SAML availability.
2) Hidden Costs & Gotchas — any known overages, add-on charges, implementation fees, training
costs, premium support surcharges, or costs that only surface after purchase.
3) Contract & Commitment — minimum terms, cancellation policy, refund policy, auto-renewal
clauses, and price lock/escalation terms.
4) Value Assessment — a brief analysis of price-to-value ratio compared to the top 2–3
alternatives, and a recommendation on which tier represents the best fit for teams of
[team_size] users.
5) Total Cost of Ownership (TCO) Estimate — estimated first-year and three-year cost
including seats, implementation, training, and integration effort."
```

---

## Phase 4 — Index File Generation

Create `00_INDEX.md` as the table of contents for the research folder:

```markdown
# Product Research Package: [Product Name]
**Generated:** [timestamp]
**Evaluation Date:** [date]
**Category:** [product_category]
**Evaluation Purpose:** [evaluation_purpose]
**Would Replace:** [current_tool]

## Downloaded Files
| # | File | Description |
|---|------|-------------|
| 1 | 01_product_evaluation.md | Comprehensive product evaluation report |
| 2 | 02_market_ecosystem_report.md | Market trends and ecosystem health analysis |
| 3 | 03_competitive_landscape.md | Feature matrix, pricing comparison, and alternatives |
| 4 | 04_market_infographic.png | Visual market landscape infographic |
| 5 | 05_risk_assessment.md | Vendor and product risk analysis with ratings |
| 6 | 06_pros_and_cons.md | Weighted pros and cons with evidence |
| 7 | 07_pricing_breakdown.md | Tier comparison, hidden costs, and TCO estimate |
| 8 | audio_briefing.mp3 | AI podcast research briefing |
| 9 | index.html | Interactive research dashboard |

## Cloud Resources
- **NotebookLM Notebook:** [link to notebook]
- **Research Sources:** [number] web sources analysed

## How to Use
1. Open `index.html` in your browser for the full interactive experience
2. Or run `python3 -m http.server 8888` in this folder and visit localhost:8888
3. Listen to `audio_briefing.mp3` on your commute for a hands-free summary
4. Review `01_product_evaluation.md` for a quick-read text overview
5. Jump to `05_risk_assessment.md` if you need to present risk factors to leadership
```

---

## Phase 5 — Premium HTML Dashboard Construction

### Step 5.1 — Create Output Folder

Create a folder named `Product Research - [Product Name]` in the current working directory.

### Step 5.2 — Build the Dashboard (`index.html`)

The dashboard must include all of the following:

| Component | Specification |
|---|---|
| **Navbar** | "Antigravity OS" branding, evaluation date badge, Export PDF button |
| **Header** | Product name, vendor name, product category subtitle, embedded audio player with animated visualizer bars |
| **Sidebar** | 7 tabs: Product Evaluation, Competitive Landscape, Market & Ecosystem, Risk Assessment, Pros & Cons, Pricing Breakdown, Market Infographic |
| **Content area** | Dynamically renders markdown content for applicable tabs using marked.js |

### Critical Implementation Details

**Markdown tabs (Product Evaluation, Competitive Landscape, Market & Ecosystem, Pricing Breakdown)**
Store raw markdown inside `<script type="text/markdown" id="md-[tabname]">` blocks. Use marked.js to parse and render on tab click.

**Risk Assessment tab** — do NOT render via markdown. Build an interactive risk dashboard with:
- Risk category cards displaying the category name, risk rating (LOW/MEDIUM/HIGH), and key evidence
- Colour-coded severity indicators: green for LOW, amber/orange for MEDIUM, red for HIGH
- Expandable sections for mitigation strategies (click to reveal)
- A summary risk matrix at the top showing all categories at a glance (coloured grid)
- Content sourced from the downloaded risk assessment file

**Pros & Cons tab** — do NOT render via markdown. Build an interactive comparison component with:
- Two-column layout: Pros (left, green accent) and Cons (right, red accent)
- Each item as a card with the bold headline, evidence text, and a weight badge (CRITICAL/HIGH/MEDIUM/LOW)
- Weight badges colour-coded: CRITICAL = red, HIGH = orange, MEDIUM = blue, LOW = grey
- 3D flip animation (CSS `perspective` + `rotateY(180deg)`) — front shows the headline and weight, back reveals the full evidence
- Optional toggle: "Sort by Weight" to reorder items by impact severity
- Content sourced from the downloaded pros and cons file

**Market Infographic tab** — do NOT render via markdown (the `<img>` tag will get escaped). Render natively with a JS function returning HTML with `<img>` pointing to `04_market_infographic.png`.

**Audio player** — `<audio>` element pointing to `audio_briefing.mp3` with play/pause toggle and animated visualizer bars.

**Styling requirements:**

| Element | Specification |
|---|---|
| Dark mode | Background `#08080c` with purple/blue radial gradients |
| Glassmorphism | `backdrop-filter: blur(10px)`, transparent white backgrounds, subtle borders |
| Font | Inter from Google Fonts |
| Icons | Font Awesome 6 |
| CSS framework | Tailwind CSS via CDN |
| Markdown styling | Custom CSS for h1–h3, p, ul, li, strong, a, table, th, td within `.markdown-content` |
| Accent colours | Green for Pros/LOW risk, Red for Cons/HIGH risk, Amber for MEDIUM risk, Blue + Gold for branding and neutral elements |

### Step 5.3 — Start Local Server (Optional)

```bash
cd "Product Research - [Product Name]"
python3 -m http.server 8888
# Then open http://localhost:8888/index.html
```

---

## Guiding Principles & Constraints

### Batch Imports — Non-Negotiable

> [!danger] Never bulk-import 100+ sources at once
> Always iterate in chunks of 20 using `source_indices`. Wait 2–3 seconds between batches.

### Fail Gracefully

| Failure | Recovery |
|---|---|
| Pros & Cons fewer than 6 per side | Auto-regenerate via `notebook_query` with explicit prompt for 10 additional pro/con pairs, append to file |
| Infographic generation fails | Skip it, note in INDEX file — dashboard must still function without it |
| Audio still processing after 5 minutes | Move on, note as "generating" in INDEX |
| Risk assessment missing categories | Re-query with explicit category list (Vendor Viability, Product Maturity, Lock-in, Security, Community) |
| Pricing data unavailable (private pricing) | Note "Contact vendor for pricing" in relevant sections, populate what is publicly known |

### Aesthetics Are Non-Negotiable

The HTML dashboard must feel premium — dark mode, glassmorphism, smooth transitions, crisp typography, animated elements. It should look like a product, not a prototype. Colour preferences: **blue + gold** for branding, **green/amber/red** for risk and pro/con indicators.

### Data Isolation

Each product evaluation gets its own NotebookLM notebook. Never reuse notebooks across products. This prevents data contamination and hallucination.

### Security

- Never leak API keys, MCP tokens, or authentication credentials into the HTML dashboard
- All servers are local only (localhost)
- Generated content stays on the user's machine unless explicitly shared

---

## File Naming Convention

```
Product Research - [Product Name]/
├── 00_INDEX.md
├── 01_product_evaluation.md
├── 02_market_ecosystem_report.md
├── 03_competitive_landscape.md
├── 04_market_infographic.png
├── 05_risk_assessment.md
├── 06_pros_and_cons.md
├── 07_pricing_breakdown.md
├── audio_briefing.mp3
└── index.html
```

---

*Tags: #automation #product-research #saas-evaluation #notebooklm #mcp #workflow*