# Automated Meeting Prep Dashboard Generator
> A complete pipeline using NotebookLM to generate deep research, artifacts, and a glassmorphic HTML dashboard for meeting prep. Takes a company/meeting context and builds a local presentation website.

---

## Overview

Operate as an elite intelligence analyst and engineer, automatically building a comprehensive web dashboard full of deep-researched artifacts for any upcoming meeting or company target, leveraging NotebookLM MCP tools.

### The Goal

Take a raw input (company name, domain, and/or meeting context) and transform it into a ready-to-view "Meeting Prep Dashboard" folder containing:

1. A suite of AI-generated Markdown documents (briefing, competitive intel, research report, quiz, flashcards)
2. Downloaded media artifacts (audio podcast MP3, market infographic PNG)
3. A beautiful, offline-ready HTML dashboard unifying everything for the executive

---

## Trigger Details

Input can come from any of the following sources:

| Source | Description |
|---|---|
| **Zapier / Make** | Watching Google Calendar for new "Discovery Call" events, firing a payload with the company name |
| **Email / CRM** | A new lead or meeting request arriving in an inbox or CRM system |
| **Manual prompt** | User types a company name and meeting context directly |

**Minimum required payload:** `company_name`

**Optional fields:** `company_domain` · `meeting_date` · `meeting_type` · `contact_name` · `contact_title`

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

### Step 0.1 — Website Scrape

Use web scraping tools (Firecrawl, Exa, Tavily, or Apify via MCP) to read the company's official website. Target:

- About / Company page
- Products / Services / Platform page
- Pricing page (if public)
- Team / Leadership page
- Blog / News / Press page
- Careers page (reveals growth stage and tech stack)

### Step 0.2 — External Enrichment

Search the broader web for:

- LinkedIn company page (employee count, recent posts, hiring activity)
- Crunchbase or PitchBook data (funding rounds, investors, valuation)
- Recent news articles, press releases, or founder interviews (last 6 months)
- Wikipedia page (if it exists)
- Glassdoor or similar (company culture signals)
- YouTube videos featuring the company or its founders

### Step 0.3 — Synthesize Company Profile

Combine all scraped data into a single distilled "Company Profile" document including:

- Company name, HQ location, founding year
- CEO/founder names and backgrounds
- Employee count range
- Funding history (rounds, amounts, lead investors)
- Revenue model (SaaS, services, hybrid, marketplace)
- Core products/services with brief descriptions
- Key clients or case studies mentioned on their site
- Direct competitors (named on site or obvious from market position)
- Recent news highlights (last 2–3 significant events)
- Meeting context (why are we meeting? what do they want from us?)

> [!tip] This profile becomes the foundational "seed" for NotebookLM — ensuring the AI never hallucinates because it starts with verified facts.

---

## Phase 1 — Notebook Preparation & Initial Ingestion

### Step 1.1 — Create Notebook

```
mcp: notebook_create
title: "Meeting Prep - [Company Name]"
```

### Step 1.2 — Inject Seed Data as Text Source

```
mcp: source_add
notebook_id: [from step 1.1]
source_type: "text"
title: "[Company Name] — Company Profile"
text: [the synthesized Company Profile from Phase 0.3]
wait: true
```

### Step 1.3 — Add Scraped URLs as Sources

For each high-quality URL found during Phase 0 (company website, Wikipedia, key news articles):

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
query: "[Company Name] competitive landscape market trends [industry] [region] 2025 2026"
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

### Step 3.1 — Executive Briefing (`01_briefing_doc.md`)

Query prompt:
```
"Create a comprehensive executive pre-meeting briefing document with these exact sections:
1) Company Overview (background, size, leadership, financials, business model, core products, key clients, recent developments, meeting context)
2) Competitive Landscape (name each competitor, their approach, and the client's advantage)
3) Market Opportunity (specific dollar figures, growth rates, and government initiatives)
4) Key Talking Points (numbered, actionable conversation starters)
5) Handling Objections (table format with Objection and Response columns)
6) Recommended Next Steps (3 concrete follow-up actions)"
```

### Step 3.2 — Deep Research Report (`02_deep_research_report.md`)

```
mcp: notebook_query
notebook_id: [from step 1.1]
query: "Write a deep research report summarizing the macro trends affecting this company's
industry over the next 2 years. Include a table of the top 10 most important sources
discovered, with columns for Source Name and Why It Matters. Then summarize the key
themes assessed."
```

### Step 3.3 — Competitive Intelligence (`03_competitive_intel.md`)

```
mcp: notebook_query
query: "Create a rapid competitive intelligence cheat sheet formatted as:
Top 3 Things to Know (each with a bold headline, 3-4 bullet points of evidence,
and a 'Your angle' recommendation), followed by a 'Market Numbers to Drop in
Conversation' section listing 7-10 specific statistics with dollar signs and percentages."
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
output_path: "[prep_folder]/04_market_infographic.png"
```

### Step 3.5 — Audio Briefing Podcast (`audio_briefing.mp3`)

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
output_path: "[prep_folder]/audio_briefing.mp3"
```

### Step 3.6 — Knowledge Quiz (`06_pre_call_quiz.md`)

```
mcp: studio_create
notebook_id: [from step 1.1]
artifact_type: "quiz"
question_count: 8
difficulty: "medium"
confirm: true
```

Poll `studio_status`, then:

```
mcp: download_artifact
artifact_type: "quiz"
output_path: "[prep_folder]/06_pre_call_quiz.md"
output_format: "markdown"
```

### Step 3.7 — Flashcards (`07_flashcards.md`)

```
mcp: studio_create
notebook_id: [from step 1.1]
artifact_type: "flashcards"
difficulty: "medium"
confirm: true
```

Poll `studio_status`, then:

```
mcp: download_artifact
artifact_type: "flashcards"
output_path: "[prep_folder]/07_flashcards.md"
output_format: "markdown"
```

> [!tip] Fallback for thin flashcards
> If the output contains fewer than 8 Q&A pairs, run a supplementary `notebook_query`:
> ```
> "Generate 10 flashcard-style Q&A pairs covering the most important facts someone
> should memorize before meeting with [Company Name].
> Format each as 'Q: [question]' and 'A: [answer]'."
> ```
> Append the output to the flashcards file.

### Step 3.8 — Slide Deck (`08_slide_deck.pdf`) — Optional

```
mcp: studio_create
notebook_id: [from step 1.1]
artifact_type: "slide_deck"
slide_format: "detailed_deck"
confirm: true
```

Download as PDF when complete.

---

## Phase 4 — Index File Generation

Create `00_INDEX.md` as the table of contents for the prep folder:

```markdown
# Meeting Prep Package: [Company Name]
**Generated:** [timestamp]
**Meeting Date:** [date]
**Meeting Type:** [type]

## Downloaded Files
| # | File | Description |
|---|------|-------------|
| 1 | 01_briefing_doc.md | Executive pre-meeting briefing |
| 2 | 02_deep_research_report.md | Deep research summary with source table |
| 3 | 03_competitive_intel.md | Competitive intelligence cheat sheet |
| 4 | 04_market_infographic.png | Visual market landscape infographic |
| 5 | 06_pre_call_quiz.md | Knowledge test (8 questions) |
| 6 | 07_flashcards.md | Rapid-review flashcards |
| 7 | audio_briefing.mp3 | AI podcast briefing |
| 8 | index.html | Interactive dashboard |

## Cloud Resources
- **NotebookLM Notebook:** [link to notebook]
- **Research Sources:** [number] web sources analysed

## How to Use
1. Open `index.html` in your browser for the full interactive experience
2. Or run `python3 -m http.server 8888` in this folder and visit localhost:8888
3. Listen to `audio_briefing.mp3` on your commute
4. Review `01_briefing_doc.md` for a 3-minute text summary
```

---

## Phase 5 — Premium HTML Dashboard Construction

### Step 5.1 — Create Output Folder

Create a folder named `Meeting Prep - [Company Name]` in the current working directory.

### Step 5.2 — Build the Dashboard (`index.html`)

The dashboard must include all of the following:

| Component | Specification |
|---|---|
| **Navbar** | "Antigravity OS" branding, meeting date badge, Export PDF button |
| **Header** | Company name, subtitle, embedded audio player with animated visualizer bars |
| **Sidebar** | 6 tabs: Executive Briefing, Competitive Intel, Deep Research, Knowledge Test, Flashcards, Market Infographic |
| **Content area** | Dynamically renders markdown content for the first 3 tabs using marked.js |

### Critical Implementation Details

**Markdown tabs (Briefing, Intel, Research)**
Store raw markdown inside `<script type="text/markdown" id="md-[tabname]">` blocks. Use marked.js to parse and render on tab click.

**Quiz tab** — do NOT render via markdown. Build an interactive quiz with:
- Question cards with clickable radio-button options
- Green ✓ / Red ✗ visual feedback on answer selection
- Score counter appearing after all questions are answered
- Questions sourced from the downloaded quiz file

**Flashcards tab** — do NOT render via markdown. Build an interactive flashcard component with:
- 3D flip animation (CSS `perspective` + `rotateY(180deg)`)
- Purple gradient front (Question), gold accent back (Answer)
- Left/right arrow navigation with card counter
- Progress dots below the card
- Content sourced from the downloaded flashcards file

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

### Step 5.3 — Start Local Server (Optional)

```bash
cd "Meeting Prep - [Company Name]"
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
| Flashcards fewer than 8 cards | Auto-regenerate via `notebook_query` with explicit prompt for 10 Q&A pairs, append to file |
| Infographic generation fails | Skip it, note in INDEX file — dashboard must still function without it |
| Audio still processing after 5 minutes | Move on, note as "generating" in INDEX |

### Aesthetics Are Non-Negotiable

The HTML dashboard must feel premium — dark mode, glassmorphism, smooth transitions, crisp typography, animated elements. It should look like a product, not a prototype. Colour preferences: **blue + gold**.

### Data Isolation

Each client/meeting gets its own NotebookLM notebook. Never reuse notebooks across clients. This prevents data contamination and hallucination.

### Security

- Never leak API keys, MCP tokens, or authentication credentials into the HTML dashboard
- All servers are local only (localhost)
- Generated content stays on the user's machine unless explicitly shared

---

## File Naming Convention

```
Meeting Prep - [Company Name]/
├── 00_INDEX.md
├── 01_briefing_doc.md
├── 02_deep_research_report.md
├── 03_competitive_intel.md
├── 04_market_infographic.png
├── 06_pre_call_quiz.md
├── 07_flashcards.md
├── 08_slide_deck.pdf
├── audio_briefing.mp3
└── index.html
```

---

*Tags: #automation #meeting-prep #notebooklm #mcp #workflow*