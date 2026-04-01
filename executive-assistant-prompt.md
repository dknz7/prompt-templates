# Executive Assistant / Second Brain Setup Prompt

> **Usage:** Paste this prompt into a new Claude Code session inside the project folder you want to turn into your second brain. Claude will scaffold the structure, interview you, and build out your personalised context files.

---

I want you to set up this project folder as my personal executive assistant / second brain in Claude Code. You're going to do this in 3 phases. Complete each phase fully before moving to the next.

---

## Phase 1: Create the Folder Structure

First, initialize a git repo in this folder if one doesn't exist already.

Then create the following template structure. Don't put any content in the files yet — just create the skeleton with placeholder files where noted.

```
CLAUDE.md                           # Main brain file (we'll fill this in Phase 3)
CLAUDE.local.md                     # My personal overrides (git-ignored)
.gitignore                          # Ignore .env, CLAUDE.local.md, settings.local.json
.claude/
  settings.json                     # Empty JSON object for now: {}
  rules/                            # We'll add rule files in Phase 3
  skills/                           # Empty -- we'll build skills over time
context/
  me.md                             # About me (filled in Phase 3)
  work.md                           # My ventures and revenue streams (filled in Phase 3)
  current-priorities.md             # What I'm focused on right now (filled in Phase 3)
  goals.md                          # Quarterly goals and milestones (filled in Phase 3)
templates/
  session-summary.md                # Template for session closeout
references/
  sops/                             # Standard operating procedures (empty for now)
  examples/                         # Example outputs and style guides (empty for now)
projects/                           # Active workstreams (empty for now)
decisions/
  log.md                            # Decision log (append-only, empty for now)
archives/                           # Completed/outdated material (empty for now)
```

For empty directories, create a `.gitkeep` file inside them so git tracks them.

### Starter Files

**`templates/session-summary.md`**

```markdown
# Session Summary

**Date:**
**Focus:**

## What Got Done
-

## Decisions Made
-

## Open Items / Next Steps
-

## Memory Updates
- Preferences learned:
- Decisions to log:
```

**`decisions/log.md`**

```markdown
# Decision Log

Append-only. When a meaningful decision is made, log it here.

Format: [YYYY-MM-DD] DECISION: ... | REASONING: ... | CONTEXT: ...

---
```

**`.gitignore`**

```
.env
CLAUDE.local.md
.claude/settings.local.json
node_modules/
```

**`CLAUDE.local.md`**

```markdown
# Local Overrides

Personal preferences and overrides that don't get shared via git.
Add anything here that's specific to your local setup.
```

---

## Phase 2: Ask Me Onboarding Questions

Before filling in the context files, rules, and CLAUDE.md, interview me. Ask these questions **one section at a time**. Don't dump all questions at once — go section by section and wait for my answers before moving on.

### Section 1: About You

- What's your name?
- What's your role/title? (e.g., founder, freelancer, solo operator)
- What's your timezone?
- In one sentence, what do you do?
- What's your #1 priority — the thing everything else should support?

### Section 2: Your Ventures

- What are your businesses or ventures? (List each with a one-liner)
- What are your products, services, or revenue streams for each?
- Roughly how much revenue does each generate? (optional, but helps me prioritize)
- Does anyone else help with any of these? (e.g., a partner, contractor, VA)
- What tools do you use day-to-day? (ClickUp, Notion, Slack, Google Workspace, etc.)
- Do you have any MCP servers already connected to Claude Code? (If you don't know what this means, just say no)

### Section 3: Priorities, Goals & Projects

- What are the 3-5 things you're most focused on right now?
- Are there any deadlines or time-sensitive items I should know about?
- Do you have quarterly goals or milestones you're tracking? (If yes, list them. If you don't do formal goals, that's fine — your priorities above will work.)
- What active projects or workstreams are you managing right now? (These are specific initiatives, not ongoing responsibilities. E.g., "launching a new store", "building a client site", "running an SEO campaign")

### Section 4: Communication Preferences

- How do you like information presented? (bullet points, detailed paragraphs, etc.)
- Any writing pet peeves? (e.g., "never use emojis", "no em dashes", "keep it under 3 sentences")
- What tone do you want internally (casual, professional, etc.)?
- What tone do you want for external/public-facing content?

### Section 5: What Do You Want Help With?

- What are the recurring tasks that eat up your time? (List as many as you want)
- What would you hand off to an assistant first if you could?
- Are there any specific workflows you want to automate or templatize?
- Which of your ventures needs the most operational help right now?

---

## Phase 3: Build Out the Files

Based on my answers, fill in all the files:

### Context Files

- **`context/me.md`** — My profile based on Section 1 answers. Frame this as a solo operator running multiple ventures. Note any key partnerships (e.g., partner helping with a specific venture).
- **`context/work.md`** — My ventures and revenue streams based on Section 2 answers. Organize by venture with services, revenue, and tools listed under each.
- **`context/current-priorities.md`** — Priorities from Section 3, dated today.
- **`context/goals.md`** — Quarterly goals and milestones from Section 3. If no formal goals, use top priorities as informal goals. Date it with the current quarter (e.g., "Q2 2026"). Include a note at the top: "Update this file at the start of each quarter."

### Projects

If active projects were listed in Section 3, create a folder for each inside `projects/`. Each project folder gets a brief `README.md` with:

- One-line description of the project
- Which venture it belongs to
- Current status (active, planning, on hold)
- Key dates or deadlines mentioned

If no specific projects were mentioned, leave `projects/` empty.

### Rule Files in `.claude/rules/`

Create rule files based on communication preferences (Section 4). Suggested files:

- **`communication-style.md`** — Writing tone, formatting preferences, pet peeves
- Any other domain-specific rules that emerged from answers (e.g., tool conventions, content guidelines)

Keep each rule file focused on **one topic**. Don't create more than 3–4 rule files to start.

### `CLAUDE.md` (The Main Brain File)

This is the most important file. Keep it **under 150 lines**. It should contain:

1. **One-line identity** — Who you are to me (e.g., "You are [Name]'s executive assistant and operational right hand.")
2. **Solo operator context** — Note that I run multiple ventures solo and bandwidth is my scarcest resource. Every output should save me time, not create more work.
3. **Top priority** — The #1 thing everything supports
4. **Context imports** — Use `@context/me.md`, `@context/work.md`, `@context/goals.md`, etc. to reference files instead of repeating their content
5. **Tool integrations** — What tools are connected (ClickUp, etc.)
6. **Skills directory** — Point to `.claude/skills/` and explain how skills work
7. **Decision log** — Point to `decisions/log.md` and explain append-only convention
8. **Memory** — Add a section explaining how memory works:
   - "Claude Code maintains a persistent memory across conversations. As you work with your assistant, it automatically saves important patterns, preferences, and learnings. You don't need to configure this — it works out of the box."
   - "If you want your assistant to remember something specific, just say 'remember that I always want X' and it will save it."
   - "Memory + context files + decision log = your assistant gets smarter over time without you re-explaining things."
9. **Keeping context current** — Add a brief maintenance section:
   - Update `context/current-priorities.md` when your focus shifts
   - Update `context/goals.md` at the start of each quarter
   - Log important decisions in `decisions/log.md`
   - Add reference files as needed
   - Build skills when you notice you're repeating the same request
10. **Projects** — Point to `projects/` and explain that active workstreams live there
11. **Templates** — Point to `templates/`
12. **References** — Point to `references/`
13. **Archives rule** — Don't delete, archive

**Do NOT** put communication style rules in CLAUDE.md — those go in `.claude/rules/communication-style.md`.

**Do NOT** repeat context from the context files — just import them with `@`.

**Do NOT** include a "Session Start Protocol" that tells Claude to read a bunch of files — the import system and rules handle that automatically now.

### Skills Directory

Leave `.claude/skills/` empty. Don't create any skills yet. Just make sure CLAUDE.md mentions that skills live there and explains the pattern:

- Each skill gets a folder: `.claude/skills/skill-name/SKILL.md`
- Skills are built organically as recurring workflows emerge
- Include a note about Section 5 answers (what they want help with) as a "Skills to Build" backlog in CLAUDE.md or a separate file

---

## Final Step

After everything is created:

1. Show me a **tree view** of every file and folder you created
2. Show me a **brief summary** of what's in each file (one line per file)
3. List the **"Skills to Build" backlog** based on Section 5 answers — these are the workflows we'll turn into skills over time
4. Show me this maintenance cheat sheet:

> **Keeping Your Assistant Sharp**
>
> - **Weekly:** Nothing required. Auto-memory handles daily learnings for you.
> - **Monthly:** Glance at `context/current-priorities.md`. If your focus has shifted, update it.
> - **Quarterly:** Update `context/goals.md` with new goals and milestones.
> - **As needed:** Log decisions in `decisions/log.md`. Add reference files. Build new skills.
> - **Pro tip:** If you want your assistant to remember something permanently, just tell it: "Remember that I always prefer X." It will save it across all future conversations.

5. Create the first git commit with all the files
6. Ask me if I want to build any of those skills right now

---

## Important Rules for You (Claude)

- Do NOT create any skills yet. The skills directory should be empty after setup.
- Keep CLAUDE.md **under 150 lines**. If it's getting long, you're putting too much in it.
- Use `@` imports (e.g., `@context/me.md`) in CLAUDE.md instead of repeating information.
- One rule file = one topic. Max 3–4 rule files to start.
- Ask onboarding questions **one section at a time**. Wait for my response before asking the next section.
- If I say "skip" or "I'll fill that in later" for any section, create the file with a placeholder and move on.
- Frame everything for a solo operator. No team management overhead. Efficiency and leverage are king.
