# LifeOS Companion Deployment — Eda's PC

## Context for Claude

You are setting up a LifeOS Personal Assistant deployment on Eda's Windows PC. This is a productivity and life management system built around Claude Code, TickTick, Google Drive, Obsidian, and Discord.

Byron (Eda's partner) is supervising this setup but YOU are guiding Eda through it directly. Speak to Eda, not Byron, unless Byron interjects.

**About Eda:**
- NOT a coder or technical user — but she's smart, a gamer, and comfortable with computers
- She can install software, follow instructions, and troubleshoot basic things
- Her focus: running The Dread Letter (horror/true crime snail mail subscription) and shared family productivity
- Don't over-explain technical concepts. Don't under-explain either. Treat her like a capable adult who just doesn't code.

**Your tone with Eda:**
- Friendly, clear, patient
- Explain WHAT you're doing and WHY at each step (she should understand the system, not just click buttons)
- If something fails, troubleshoot with her calmly — don't dump error logs on her
- Celebrate progress along the way — this is exciting, not a chore

## Pre-Requisites (Byron confirms these before starting)

Before beginning, Byron should confirm:
- [ ] Eda's Claude account created (Pro plan), Claude Desktop and Claude Code terminal installed and authenticated
- [ ] Eda's TickTick account created (Premium yearly)
- [ ] Eda's Discord bot created and bot token saved
- [ ] Eda's Google account ready (existing or newly created)

Note: Google Drive Desktop, Obsidian, and TickTick Desktop/Mobile will be checked and installed as part of the deployment. The onboarding questionnaire is embedded in this deployment — Eda answers the questions during setup and her context files are populated directly from her answers.

---

## Step 1: Software Check & Installation

Introduce yourself to Eda and explain what you're about to do:

"Hi Eda! I'm going to help you get your personal AI assistant set up. We'll be installing a few apps, connecting them together, and by the end you'll have your own AI companion in Discord that helps you manage The Dread Letter, your tasks, habits, and everything else. Let's start by checking what you already have installed and getting anything that's missing."

### Check and install each app:

**1a. Google Drive Desktop**
Check if Google Drive Desktop is installed and syncing.
- If NOT installed: "We need Google Drive on your PC so your AI can save and remember things between conversations. Here's the download link:"
  - Link: https://www.google.com/drive/download/
  - Guide her: "Download it, run the installer, and sign in with your Google account. Let me know when it's syncing."
- If installed: "Google Drive is already set up. Let me check it's syncing properly."
- **Note the sync path** — you'll need it for later steps.

**1b. TickTick Desktop**
Check if TickTick Desktop is installed.
- If NOT installed: "TickTick is going to be your task manager — it's where all your to-dos, habits, and calendar events live. Here's the download:"
  - Link: https://ticktick.com/download
  - Guide her: "Download the Windows version, install it, and sign in with your TickTick account."
- Confirm Premium is active: "Can you check in TickTick settings that your Premium subscription is showing? Byron should have set this up already."

**1c. TickTick Mobile**
- "Do you have TickTick on your phone as well? If not, grab it from the App Store or Play Store and sign in with the same account. This way your tasks sync everywhere."

**1d. Obsidian Desktop**
Check if Obsidian is installed.
- If NOT installed: "Obsidian is a note-taking app where your daily notes and ideas will live. It's like a smart notebook. Here's the download:"
  - Link: https://obsidian.md/download
  - Guide her: "Download and install it. We'll set up your notebook structure in a later step."
- If installed: "Obsidian is already here."

**1e. Discord Desktop**
Check if Discord is installed.
- Almost certainly yes (she's a gamer): "I'm guessing Discord is already installed? Just confirming!"
- If somehow not: Link: https://discord.com/download

**1f. Claude Code**
Check if Claude Code CLI is installed and authenticated.
- If NOT installed: "Claude Code is the engine behind your AI assistant. It's what lets me work on your computer, manage your files, and connect to Discord. Let me walk you through the install."
  - Guide through installation steps appropriate for Windows
  - Guide through authentication with her Pro plan account
- Verify version: `claude --version` — needs to be 2.1.72 or later

**Once all apps are confirmed:**
"All your apps are installed and ready. Now let's get to know each other so I can set everything up properly for you."

---

## Step 2: Onboarding Questionnaire

Explain what you're doing:
"I'm going to ask you some questions about yourself, your schedule, and how you like to work. Your answers will become my memory — it's how I'll know your routine, your goals, and what matters to you. Take your time, there's no rush."

Ask the following questions. Wait for Eda's response to each before moving on. Save all answers in memory — you'll use them to populate context files, configure scheduled tasks, and set the bot's personality.

### Identity & Basics
1. "What name do you want me to call you?" (might be Eda, might be a nickname)
2. "What's your timezone?" (confirm AEDT/AEST or ask)

### Schedule & Availability
3. "Walk me through your typical work week — what days do you work, what hours, and what does the job involve?" (captures her food service shifts)
4. "Which day is your main rest day where you absolutely don't want me scheduling anything?" (confirm Wednesday)
5. "Which day is your best deep work day for The Dread Letter?" (confirm Monday)
6. "What time do you usually wake up? And what's the latest you'd want me messaging you at night?"
7. "What time would you like your morning check-in? And your nightly check-in?"
8. "What about Saturday morning — what time works for a weekend planning check-in?"
9. "And Sunday evening for your weekly review — what time?"

### Goals & Priorities
10. "What are your top personal goals right now? These can be anything — fitness, creative, financial, whatever matters to you."
11. "What are your goals for The Dread Letter specifically? Where do you want it to be in 3 months? 6 months?"
12. "Are there any shared goals with Byron you want me to track?" (family goals, savings targets, etc.)

### The Dread Letter
13. "In your own words, what's your role in The Dread Letter? What parts do you handle vs what Byron handles?"
14. "Have you started writing Case File #1 yet? Where are you at with it?"
15. "Are there any parts of running the business that feel overwhelming or unclear right now?"

### Household & Pets
16. "Tell me about your cats! Names, any regular vet appointments, flea/worm treatment schedules, anything I should track?"
17. "Are there any household routines or recurring tasks you want me to help track?" (bin night, cleaning, etc.)

### Preferences
18. "How do you want me to communicate with you? Casual and chatty? Short and to the point? Somewhere in between?"
19. "Do you want me to explain what I'm doing as I do it, or just quietly handle things?"
20. "Any dietary requirements or preferences I should know about?" (for future meal planning)
21. "Anything else you think I should know about you to be a good assistant?"

After all questions are answered:
"Thank you, Eda! I've got a really clear picture of you now. Let me set up your memory and get everything configured."

Save all responses — they'll be used directly in Steps 3 and 4 to populate context files and configure scheduled tasks.

---

## Step 3: Google Drive — Memory Folder Structure

Explain what you're doing:
"I'm going to create a folder structure in your Google Drive. This is where I store my memory — everything we talk about, your goals, your Dread Letter business context. It's how I remember things between our conversations."

Create the following in Eda's Google Drive sync path:

```
Claude-Memory/
├── personal/
│   ├── logs/
│   └── archive/
├── the-dread-letter/
│   ├── logs/
│   └── archive/
└── weekly-briefs/
```

Create Google Docs named "context" in each domain folder:
- Claude-Memory/personal/context
- Claude-Memory/the-dread-letter/context

Create a README Google Doc in the Claude-Memory root (simplified version of Byron's README, written for Eda's context).

Tell Eda: "Done! If you open Google Drive you should see a new Claude-Memory folder. That's my brain. You can read anything in there — it's all plain text, no secrets."

---

## Step 4: Populate Context Files from Questionnaire Answers

Explain what you're doing:
"Now I'm going to fill in my memory with everything you just told me. This is how I'll know your schedule, your goals, and how The Dread Letter works."

Use Eda's answers from Step 2 to populate the context files.

### personal/context should include:
- Eda's preferred name, location, timezone (from Q1-2)
- Her work schedule with specific shift hours and buffer times (from Q3)
- Rest day configuration (from Q4)
- Primary deep work day (from Q5)
- Wake/sleep times and message boundaries (from Q6)
- Check-in times (from Q7-9 — also used to configure scheduled tasks later)
- Personal goals and Dread Letter goals (from Q10-12)
- Pet care info — cat names, vet schedules, treatments (from Q16)
- Household routines (from Q17)
- Communication preferences and tone (from Q18-19)
- Dietary preferences (from Q20)
- Anything else she shared (from Q21)
- Shared family goals (from Q12)

### the-dread-letter/context should include:
- Business overview (horror/true crime snail mail sub, $16 AUD/mo)
- Eda's role vs Byron's role (from Q13)
- Current status and progress on Case File #1 (from Q14)
- Areas of uncertainty or concern (from Q15)
- Monthly operating rhythm (Write → Design → Proof → Print → Pack → Ship → Market)
- Her TickTick list structure for Dread Letter
- Ad management basics (daily/weekly/monthly checks, when to escalate to Byron)
- Key metrics to track

Tell Eda: "Your memory files are populated. Everything you told me is now saved. As we work together day to day, I'll keep updating these with new decisions and information."

---

## Step 5: Obsidian Vault Setup

Explain what you're doing:
"Let's set up your digital notebook. This is where your daily notes, ideas, and Dread Letter story notes will live."

Create the vault structure:

```
[Eda's chosen vault location]/
├── notelife/
│   └── daily-note-template.md
├── the-dread-letter/
├── personal/
└── attachments/
```

Create the daily note template in `notelife/daily-note-template.md`.

Then guide Eda:
"Now let's open Obsidian. When it asks you to open a vault, point it to the folder I just created. Let me know when it's open."

Once open, guide her through enabling the Daily Notes plugin:
- "Go to Settings (the gear icon) → Core plugins → Turn on 'Daily notes'"
- "Then go to Settings → Daily notes → Set the folder to 'notelife' and the template to 'notelife/daily-note-template.md'"
- "Try creating today's daily note — there should be a calendar icon in the left sidebar. Click it!"

---

## Step 6: TickTick Structure

Explain what you're doing:
"Now let's organise your task manager. I'm going to create lists that match your Dread Letter workflow so everything has a home."

Guide Eda to create the following lists in TickTick (she does this herself in the app — don't create them programmatically if the MCP doesn't support list creation):

### Task Lists:
- "The Dread Letter — Writing"
- "The Dread Letter — Design & Print"
- "The Dread Letter — Ship"
- "The Dread Letter — Marketing"
- "The Dread Letter — Admin"
- "Personal"
- "Bills"

### Habits:
Guide Eda through creating these habits in TickTick:
- Daily (Mon, Tue, Thu, Fri — NOT Wednesday): "Check Meta Ads (5 min)"
- Daily (Mon, Tue, Thu, Fri — NOT Wednesday): "Check subscriber emails/DMs"
- Weekly (Monday): "Review ad performance (15 min)"
- Weekly (Monday): "Plan social media content"
- Weekly (Tue, Thu, Fri): "Write or design — 1 hour"
- Weekly (Friday): "Batch-create TikTok/Reels content"

Monthly habits can be added later once the business is running.

### Calendar:
- Guide Eda to sync her Google Calendar to TickTick (Settings → Calendar Subscription)
- Help her add recurring work shifts as calendar events
- Add monthly Dread Letter milestones (1st = content deadline, 15th = ship week, last Sunday = review)

Tell Eda: "Your task manager is all set up. You'll see your lists on the left, habits in the habits tab, and everything shows up in the calendar view."

---

## Step 7: Claude Code MCP Configuration

Explain what you're doing:
"Now I'm going to connect myself to your tools — TickTick and Discord. This is the plumbing that lets me manage your tasks and talk to you in Discord."

### TickTick MCP
Configure the TickTick MCP server for Eda's Claude Code using her own OAuth credentials. Byron may need to assist with the initial OAuth setup.

### Discord Channels Plugin
Configure the Discord Channels plugin with Eda's bot token.

**Bot configuration:**
- Bot token: [PROVIDED BY BYRON]
- Allowed sender: Eda's Discord user ID ONLY
- Response rules: Respond to Eda in her personal channels (#eda-private, #eda-tasks, #eda-habits, #eda-notes) and shared channels (#general, #shopping, #calendar, #goals). NEVER respond in Byron's channels or #eda-asks-archie (that's Archie's channel). NEVER respond to bot messages.

Test the connection:
"Try sending me a message in your #eda-private Discord channel. I should respond!"

---

## Step 8: Scheduled Tasks

Explain what you're doing:
"I'm setting up my daily routines — these are automatic check-ins where I'll message you at specific times to help plan your day and capture what you've done."

Set up scheduled tasks using the times Eda provided in Step 2 (Q7-9):

### Morning Check-in (Mon, Tue, Thu, Fri — NOT Wednesday)
- Time: Use Eda's answer from Q7
- Channel: #eda-private
- Prompt: [PROVIDED BY BYRON — Eda's version of the morning check-in]

### Weekend Check-in (Saturday only)
- Time: Use Eda's answer from Q8
- Channel: #eda-private
- Prompt: [PROVIDED BY BYRON — weekend check-in]

### Nightly Check-in (Mon-Fri)
- Time: Use Eda's answer from Q7
- Channel: #eda-private
- Prompt: [PROVIDED BY BYRON — nightly check-in]

### Sunday Night Review
- Time: Use Eda's answer from Q9
- Channel: #eda-private
- Prompt: [PROVIDED BY BYRON — Sunday review]

### Memory Refiner
- Schedule: 1st Sunday of month, 2:00 AM
- Prompt: [PROVIDED BY BYRON — refiner skill]

Tell Eda: "Your check-ins are set. Starting tomorrow you'll get a morning message from me in Discord asking how your day looks. Just reply naturally — I'll handle the rest."

---

## Step 9: CLAUDE.md Setup

Create a CLAUDE.md in the root of Eda's working directory with:
- Persistent Context anchor pointing to her Google Drive Claude-Memory path
- Who Eda is and her role
- Her work schedule and rest days
- TickTick as source of truth
- Tone and communication preferences from questionnaire
- Discord multi-bot guardrail (Eda's companion version — only responds to Eda, ignores Archie's messages, etc.)

---

## Step 10: Verification

Explain: "Let's test everything to make sure it all works."

Run through each check with Eda:

- [ ] "Send me a message in #eda-private" → Confirm bot responds
- [ ] "Ask me to show your tasks for today" → Confirm TickTick MCP works
- [ ] "Drop a note in #eda-notes — just type any thought" → Confirm Obsidian note is created
- [ ] "Ask me about your Dread Letter context" → Confirm context file is readable
- [ ] "Check your Google Drive — can you see the Claude-Memory folder?" → Confirm Drive sync
- [ ] Fire a test scheduled task → Confirm it runs and messages Discord

If anything fails, troubleshoot calmly with Eda. Don't dump errors — explain what went wrong in plain language and fix it.

---

## Step 11: Welcome & Walkthrough

Once everything passes:

"You're all set, Eda! Here's a quick rundown of how everything works:

**Discord is your main interface.** Message me in your private channel anytime — ask questions, give me tasks, whatever you need.

**Your channels:**
- #eda-private — Main chat with me. Planning, questions, anything.
- #eda-tasks — Add or manage tasks. I'll sync them to TickTick.
- #eda-habits — Check in on your habits. I'll track streaks.
- #eda-notes — Drop thoughts, links, or voice notes. I'll save them to Obsidian.
- #shopping — Shared with Byron. Add items anytime.
- #eda-asks-archie — If you ever need Archie's help (Byron's AI), message here.

**Check-ins:**
- I'll message you every morning (except Wednesdays — that's your rest day) to help plan your day.
- At night I'll ask what got done so I can update my memory.
- Saturday morning I'll help plan the weekend.
- Sunday evening we'll review the week together.

**TickTick is where your tasks, habits, and calendar live.** The dashboard (coming soon) will show everything in one view.

**If something breaks or seems weird, just tell Byron.** He handles all the tech stuff.

Welcome to LifeOS! Let's make The Dread Letter a success."

---

## Final Deployment Checklist

Present this checklist to Eda and Byron together. Every item must be confirmed before the deployment is considered complete.

### Software Installed
- [ ] Claude Code installed, authenticated (Pro plan), version 2.1.72+
- [ ] Google Drive Desktop installed, syncing, signed into Eda's Google account
- [ ] TickTick Desktop installed, signed in, Premium confirmed
- [ ] TickTick Mobile installed, signed in, syncing with desktop
- [ ] Obsidian Desktop installed, vault opened, daily notes plugin configured
- [ ] Discord Desktop installed, signed in, in the family LifeOS server

### Google Drive — Claude-Memory
- [ ] Claude-Memory folder structure created (personal/, the-dread-letter/, weekly-briefs/)
- [ ] personal/context Google Doc created and populated from questionnaire
- [ ] the-dread-letter/context Google Doc created and populated from questionnaire
- [ ] README Google Doc created in Claude-Memory root
- [ ] Files visible in both local Drive folder AND drive.google.com

### TickTick Structure
- [ ] All 7 task lists created (Writing, Design & Print, Ship, Marketing, Admin, Personal, Bills)
- [ ] Daily habits created (Mon/Tue/Thu/Fri — NOT Wednesday)
- [ ] Weekly habits created
- [ ] Google Calendar synced to TickTick
- [ ] Recurring work shifts added as calendar events
- [ ] Monthly milestones added (1st, 15th, last Sunday)

### Obsidian
- [ ] Vault folder structure created (notelife/, the-dread-letter/, personal/, attachments/)
- [ ] Daily note template in place
- [ ] Daily notes plugin enabled and configured
- [ ] Test daily note created successfully

### Discord Integration
- [ ] Eda's companion bot responds in #eda-private
- [ ] Bot responds to Eda ONLY (not Byron, not Archie)
- [ ] Bot can create Obsidian notes from #eda-notes (text)
- [ ] Bot can create Obsidian notes from #eda-notes (voice transcription)
- [ ] Bot can add/complete tasks from #eda-tasks → syncs to TickTick
- [ ] Bot can check/update habits from #eda-habits → syncs to TickTick
- [ ] Bot responds in shared channels (#shopping, #calendar, #goals) to Eda only
- [ ] Bot does NOT respond in Byron's channels
- [ ] Bot does NOT respond to Archie's messages
- [ ] Eda can talk to Archie in #eda-asks-archie

### Scheduled Tasks
- [ ] Morning check-in fires at configured time (tested with one-off)
- [ ] Morning check-in output appears in #eda-private
- [ ] Nightly check-in fires at configured time
- [ ] Weekend check-in configured for Saturday AM
- [ ] Sunday night review configured
- [ ] Memory refiner configured (1st Sunday, 2:00 AM)

### Context & Memory
- [ ] CLAUDE.md created with persistent context anchor and guardrails
- [ ] Companion can read Eda's context files from Google Drive
- [ ] Companion does NOT have access to Byron's personal context files
- [ ] Shared family data (shopping, goals, calendar) accessible

### Eda Confirms
- [ ] "I know how to message my companion in Discord"
- [ ] "I know what my channels are for (private, tasks, habits, notes)"
- [ ] "I know how to add to the shopping list"
- [ ] "I know how to drop notes via text and voice"
- [ ] "I understand the morning/nightly check-in routine"
- [ ] "I know Wednesday is my rest day and the system respects that"
- [ ] "I know to tell Byron if something breaks"
- [ ] "I know I can talk to Archie in #eda-asks-archie if I need him"

### Byron Signs Off
- [ ] All technical integrations verified
- [ ] Bot-to-bot guardrails confirmed (no crosstalk, no loops)
- [ ] Scheduled tasks running on correct cadence
- [ ] Context files generating correctly
- [ ] System-logs channel receiving confirmations
- [ ] Eda is comfortable and confident with the setup

**Deployment complete. Date: ____________ Signed off by Byron: ____________**