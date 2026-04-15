---
name: interview-prep
description: Generate a full new-grad interview prep kit for any company. Creates 4 tailored markdown files — DSA, Behavioral, Follow-Up Questions, and Company Stack — from a job description and resume. Use when the user wants to prepare for a software engineering interview.
tools: Read, Write, WebSearch, Bash, Glob
---

# Interview Prep Kit Generator

Generate a complete, tailored interview prep kit for a new-grad software engineering role.

## Phase 0: Interactive Input Collection

**Always run this phase first, before any research or file generation.**

### Step 1 — Ask for company and role
If the user did not provide a company name in the slash command args, ask:

> "Which company and role are you interviewing for? (e.g. Google — Software Engineer, New Grad)"

Wait for the user's reply before continuing.

### Step 2 — Ask for the Job Description
Once you have the company name, ask:

> "Paste the job description below (or type `skip` to use general prep for this company):"

Wait for the user's reply. Accept plain pasted text. If they type `skip`, note that no JD was provided and continue with company-level defaults.

### Step 3 — Ask for the Resume
After receiving the JD (or skip), ask:

> "Share your resume — you can:
> - **Attach a file directly in this chat** (PDF or DOCX recommended; PNG works but text extraction is less reliable)
> - **Paste the file path** if it's already on disk (e.g. `/Users/you/resume.pdf`)
> - **Paste the text** of your resume directly here
> - Type `skip` to generate generic behavioral questions
>
> Note: PDF and DOCX give the best parsing results. PNG/image resumes are read via vision and may miss formatting details."

Wait for the user's reply. Handle each case:
- **File attached in chat** (PDF/DOCX/PNG): Claude receives it as a document/image — read and extract content directly. No tool call needed.
- **File path provided**: Use the Read tool to read it. Supports `.pdf`, `.docx`, `.txt`, `.md`.
- **Text pasted**: Use as-is.
- **PNG/image attached**: Read via vision. Warn the user: "I'll do my best with the image, but PDF or DOCX gives more reliable extraction."
- `skip` — generate behavioral questions without resume context

### Step 4 — Confirm and proceed
Summarize what you collected in one short block, then ask for confirmation:

> "Got it! Here's what I'll use:
> - **Company**: <Company>
> - **Role**: <Role>
> - **JD**: <"provided" or "skipped">
> - **Resume**: <"provided" or "skipped">
>
> Generating your prep kit now..."

Do not proceed to Phase 1 until the user confirms or gives the go-ahead.

---

## Phase 1: Extract Context from Inputs

### 1a. Parse provided JD (if not skipped)
Extract from the JD:
- Required and preferred skills
- Tech stack signals
- Key responsibilities
- Role level and team description

### 1b. Parse provided resume (if not skipped)
Extract from the resume:
- Technologies used
- Projects (names, tech, outcomes)
- Past internships or roles
- Notable accomplishments

### 1b. Research the company (web search)
Search for:
- `"<Company> software engineer interview process <current year>"`
- `"<Company> engineering tech stack"`
- `"<Company> new grad SWE interview leetcode"`
- `"<Company> engineering blog"`

Capture: interview rounds, known focus areas (e.g., system design weight, OA format), languages/frameworks used in production, engineering values.

If web search is unavailable, use your training knowledge about the company and note that details may be outdated.

## Phase 2: Determine Output Directory

Create files under:
```
<current-working-directory>/<CompanyName>/
```

Use `Bash` to create the directory:
```bash
mkdir -p "<cwd>/<CompanyName>"
```

## Phase 3: Generate All Four Files

Generate all four files in parallel (use Write tool for each). All files should be comprehensive — err on the side of more content. Each file gets its own section below.

---

### File 1: `<CompanyName>/DSA.md`

**Title**: `# DSA Prep — <Company> (<Role>)`

Include:

#### Company-Specific Focus
A short paragraph on what this company is known for in interviews (e.g., "Meta heavily tests arrays, strings, and BFS/DFS. Expect 2 LC-medium problems per round.").

#### Topic Priority Matrix
A table ranking DSA topics by importance for this company/role:

| Priority | Topic | Why It Matters for <Company> |
|----------|-------|-------------------------------|
| P0 | ... | ... |
| P1 | ... | ... |
| P2 | ... | ... |

Cover at minimum: Arrays/Strings, Linked Lists, Trees/Graphs, Dynamic Programming, Recursion/Backtracking, Sorting/Searching, Heaps/Priority Queues, Hash Maps, Sliding Window/Two Pointers, BFS/DFS, System Design (if applicable for role level).

#### Curated Problem List
For each P0 and P1 topic, list 4–6 specific LeetCode problems (by name and number). Format:

```
### Arrays & Strings
- [ ] Two Sum (LC 1) — classic hashmap
- [ ] Best Time to Buy and Sell Stock (LC 121) — sliding window variant
- [ ] Container With Most Water (LC 11) — two pointers
```

Include a mix of Easy (warm-up), Medium (core), and 1–2 Hard (stretch).

#### Patterns Cheat Sheet
A section listing the top 8–10 problem-solving patterns with a one-line description and a trigger phrase:

```
**Sliding Window** — use when: contiguous subarray/substring with a size or condition constraint
**Two Pointers** — use when: sorted array, searching pairs, or in-place modification
```

#### Time/Space Reference
A quick table of common data structure operation complexities.

---

### File 2: `<CompanyName>/Behavioral.md`

**Title**: `# Behavioral Prep — <Company> (<Role>)`

Include:

#### Company Values Alignment
List the company's stated engineering/cultural values (from research or training knowledge). For each value, write 1 sentence on how to frame answers to align with it.

#### STAR Template
A quick STAR reminder (2–3 lines max).

#### Core Question Bank
15–20 behavioral questions, grouped by theme. For each, add a `[Resume Hook]` note where relevant — a specific bullet from the user's resume that could anchor the answer. If no resume was provided, leave generic hooks.

Themes to cover:
- **Ownership & Impact** (3–4 questions)
- **Conflict & Collaboration** (3–4 questions)
- **Learning & Growth** (2–3 questions)
- **Problem Solving Under Pressure** (2–3 questions)
- **Leadership & Initiative** (2–3 questions)
- **Company-Specific** (2–3 questions tailored to this company's culture, e.g. "Tell me about a time you moved fast and broke something" for Meta)

Format:
```
### Ownership & Impact

**Q: Tell me about a project you owned end-to-end.**
[Resume Hook]: Your <project name> project — led backend architecture, shipped to production
[Prep Note]: Emphasize decision-making authority, not just execution.
```

#### Story Bank Template
A table for the user to fill in their top 5–6 stories (one story can answer multiple questions):

| Story | Situation | Task | Action | Result | Questions It Covers |
|-------|-----------|------|--------|--------|---------------------|
| Story 1 | | | | | |

---

### File 3: `<CompanyName>/FollowUpQuestions.md`

**Title**: `# Questions to Ask — <Company> (<Role>)`

Include:

#### Why These Matter
A 3-sentence intro: asking good questions signals preparation, genuine interest, and seniority of thinking.

#### By Interview Round
Organize questions by who the user is likely talking to:

**Recruiter / HR Screen**
5–6 questions (process, timeline, team culture, remote/hybrid, compensation range)

**Technical Interviewer (Engineer)**
6–8 questions (codebase health, on-call, tech debt, how the team makes technical decisions, recent eng challenges)

**Hiring Manager**
6–8 questions (team growth, success metrics for this role in 6 months, biggest technical challenge the team faces, how feedback is given)

**Senior Engineer / Peer**
4–5 questions (what they wish they knew before joining, what makes a new grad successful here)

#### Avoid These
3–5 example bad questions and why they backfire (e.g., "What does <Company> do?" — signals no research).

#### Personalize These
2–3 questions with `[FILL IN]` placeholders where the user should insert specifics from their research before the interview.

---

### File 4: `<CompanyName>/CompanyStack.md`

**Title**: `# Company Stack & Technical Context — <Company>`

Include:

#### Overview
2–3 sentences: what the company builds, scale, and what kind of engineering problems dominate.

#### Tech Stack
What's known or inferred about:
- **Languages**: primary + secondary
- **Frontend**: frameworks, build tools
- **Backend**: services, frameworks
- **Data/ML**: pipelines, warehouses, ML infra (if relevant)
- **Infrastructure**: cloud provider, container orchestration, CI/CD
- **Databases**: SQL/NoSQL, caching layer

Mark inferred items with *(inferred)* and known items with *(confirmed)*.

#### System Design Focus
For this company/role, which system design topics are most likely to come up? List 3–5 with a brief note on why.

Example:
```
- **News Feed / Timeline**: Meta's core product — expect fan-out on write vs. read tradeoffs
- **Rate Limiter**: common warm-up design question across all FAANG
```

#### Things to Brush Up On
A checklist of specific technologies or concepts the user should review before the interview, derived from the JD and stack:

```
- [ ] <Technology from JD> — <why it's relevant>
- [ ] REST vs GraphQL — <Company> uses GraphQL extensively
```

#### Interview Process Summary
What to expect: number of rounds, format (phone screen / virtual onsite / OA), typical timeline, and any known quirks (e.g., "Google has a separate Googleyness round").

#### Useful Links
3–5 links to company engineering blog, Glassdoor interview reports, Blind threads, or official careers page. Use real URLs only if found via web search; otherwise omit.

---

### File 5: `<CompanyName>/START_HERE.md`

**Title**: `# Interview Prep Master Guide — <Company> (<Role>)`

This is the entry point file. Generate it last, after all four content files exist. It should give the user a complete, opinionated game plan for using the kit.

Include:

#### What's in This Kit
A one-paragraph overview of the four files and what each one is for. Written in second person ("You have...").

#### How to Use This Kit — Day by Day

A concrete study schedule broken into phases. Tailor the timeline based on how much lead time is typical for this company's process (use what you found in Phase 1 research). Default to a 2-week plan if unknown.

Format:

```
### Phase 1 — Orient (Day 1)
**Goal**: Understand the battlefield before you practice.

- [ ] Read `CompanyStack.md` end-to-end — understand what they build, how they interview, what to brush up on
- [ ] Skim `DSA.md` Topic Priority Matrix — note your P0 weak spots
- [ ] Read `Behavioral.md` Company Values section — internalize what they care about

**Time estimate**: ~1–2 hours

---

### Phase 2 — DSA Foundation (Days 2–7)
**Goal**: Get P0 topics solid. Don't move to P1 until P0 feels comfortable.

- [ ] Work through all P0 problems in `DSA.md` (aim for 3–4 problems/day)
- [ ] For each problem: solve it, check the pattern it belongs to in the Patterns Cheat Sheet, write a one-line note on what made it click
- [ ] Time yourself — aim for 20–25 min per medium, 10 min per easy
- [ ] Re-attempt any problem you couldn't solve cleanly without hints

**Time estimate**: ~2–3 hours/day

---

### Phase 3 — Behavioral Prep (Days 3–8, parallel with DSA)
**Goal**: Have 5–6 polished STAR stories ready that cover all themes.

- [ ] Open `Behavioral.md` → fill in the Story Bank table with your real experiences
- [ ] Write out each story in full STAR format (bullet points, not paragraphs)
- [ ] Practice saying each story out loud — aim for 90–120 seconds
- [ ] Map each story to the questions it can answer in the Core Question Bank

**Time estimate**: 30–45 min/day

---

### Phase 4 — Stack Brush-Up (Days 5–10)
**Goal**: Be fluent enough in the company's tech to ask and answer questions confidently.

- [ ] Work through the "Things to Brush Up On" checklist in `CompanyStack.md`
- [ ] For each item: read the docs or a blog post, then write one sentence in your own words explaining it
- [ ] Review the System Design Focus section — sketch rough architectures for each topic

**Time estimate**: 1–2 hours/day

---

### Phase 5 — Mock & Finalize (Days 9–14)
**Goal**: Simulate real interview conditions before the actual interview.

- [ ] Do 2–3 timed mock DSA sessions (pick random P0/P1 problems, 45 min each)
- [ ] Do 1–2 mock behavioral interviews — say your answers out loud to a friend or record yourself
- [ ] Finalize `FollowUpQuestions.md` — fill in all `[FILL IN]` placeholders
- [ ] Choose 2–3 questions per interviewer type to actually ask
- [ ] Re-read `CompanyStack.md` interview process summary the day before

**Time estimate**: 2–3 hours/day
```

#### File Cross-Reference Map

A table showing which files to use for which interview activity:

| Situation | File to Open |
|-----------|-------------|
| Preparing for the OA / coding screen | `DSA.md` |
| Practicing behavioral answers | `Behavioral.md` |
| Day before the interview | `CompanyStack.md` + `FollowUpQuestions.md` |
| During / right before each round | `FollowUpQuestions.md` (pick questions for that interviewer type) |
| After the interview (debrief) | `Behavioral.md` Story Bank (note what worked) |

#### What to Do If You're Short on Time

A prioritized survival guide for cramming (1–3 days notice):

```
**48 hours out:**
1. Read CompanyStack.md fully (1 hr)
2. Do 5 P0 DSA problems you know well — confidence, not new material (1.5 hr)
3. Write out 3 strong STAR stories from Behavioral.md (1 hr)
4. Pick 2 follow-up questions per interviewer type (30 min)

**24 hours out:**
1. Skim DSA Patterns Cheat Sheet
2. Say your 3 STAR stories out loud twice
3. Sleep

**Day of:**
1. Reread Company Values in Behavioral.md
2. Open FollowUpQuestions.md — pick your questions for today's rounds
```

#### Common Mistakes to Avoid

5–7 specific, actionable anti-patterns. Make them company-relevant where possible. Examples:
- "Jumping to code before clarifying constraints — <Company> interviewers explicitly look for this"
- "Memorizing story scripts word-for-word — sounds rehearsed, kills authenticity"
- "Asking no follow-up questions — signals low interest"
- "Practicing only Easy problems — mediums are the bar"

#### After the Interview

A short checklist for post-interview actions:
- [ ] Send a thank-you note to the recruiter within 24 hours
- [ ] Write down every question you were asked (while fresh) — add to this folder as `Interview_Notes.md`
- [ ] Note what went well and what didn't in your Story Bank
- [ ] If rejected: ask the recruiter for feedback and note it

---

#### Resources — Articles & Videos

This section is built from web search results gathered during Phase 1. It must be populated with real, verified URLs only — never fabricate links. If a search returns no usable results for a subcategory, omit that subcategory rather than leaving placeholder links.

Perform these additional searches during Phase 1 research and collect results for this section:

**Interview process articles** — search:
- `"<Company> software engineer interview experience <current year> site:leetcode.com OR site:glassdoor.com OR site:levels.fyi"`
- `"<Company> new grad interview process blog"`
- `"<Company> engineering blog interview"`

**Tech gap articles** — for each item in the "Things to Brush Up On" checklist generated for `CompanyStack.md`, search:
- `"<technology> explained simply blog article"`
- `"<technology> crash course tutorial"`
Prefer official docs, well-known engineering blogs (Cloudflare, Netflix Tech Blog, Meta Engineering, Google Developers, Stripe Blog, AWS Blog), or high-quality tutorials (CSS Tricks, freeCodeCamp, Baeldung, Martin Fowler's blog).

**YouTube videos** — search:
- `"<Company> interview experience youtube"`
- `"<technology gap item> tutorial youtube"`
- `"<P0 DSA topic> explained youtube"`
Prefer channels: NeetCode, TechLead, Clément Mihailescu (AlgoExpert), Abdul Bari, Back To Back SWE, Kevin Naughton Jr., ByteByteGo (system design), Gaurav Sen (system design).

Format the section as:

```markdown
---

## Resources

### Interview Process — Articles & Reports
Real experiences and breakdowns of the <Company> interview pipeline.

| Resource | What It Covers | Link |
|----------|---------------|------|
| <Title from search result> | <1-line description> | [Read](<url>) |
...

*(Only include URLs confirmed via web search. Mark any training-knowledge suggestions with ⚠️ verify link)*

---

### Technology Gap — Articles
Targeted reading for the gaps identified in `CompanyStack.md`.

#### <Tech Topic 1> *(P0 — brush up before interview)*
- [<Article title>](<url>) — <one-line description of what it covers>
- [<Article title>](<url>) — <one-line description>

#### <Tech Topic 2> *(P1)*
- [<Article title>](<url>) — <one-line description>

---

### YouTube — Top Priority Watches

Ordered by priority. Watch P0 videos before anything else.

#### P0 — Watch Before the Interview

| Topic | Video | Channel | Why Watch |
|-------|-------|---------|-----------|
| <DSA/Tech topic> | [<Video title>](<url>) | NeetCode / ByteByteGo / etc. | <one-line reason> |
...

#### P1 — Watch If You Have Time

| Topic | Video | Channel | Why Watch |
|-------|-------|---------|-----------|
...

#### Bonus — <Company>-Specific
Actual interview experience videos or tech talks from <Company> engineers.

| Title | Channel | Link |
|-------|---------|------|
| <Video title> | <Channel name> | [Watch](<url>) |
...
```

**Rules for this section:**
- Only include URLs that were returned by web search in Phase 1. Do not invent URLs.
- If a YouTube search returns a video title and channel but no direct URL, write the title and channel with a note `(search on YouTube)` instead of a broken link.
- Mark all links that come from training knowledge (not live search) with ⚠️ so the user knows to verify.
- Priority order for videos: P0 DSA topics first, then system design topics, then company-specific content, then P1 topics.
- Cap the list at 15 videos total — curate, don't dump.

---

## Phase 4: Summary Output

After writing all five files, print a short summary to the user:

```
## Interview Prep Kit Created — <Company> (<Role>)

Files created in `./<CompanyName>/`:

| File | What's Inside |
|------|---------------|
| START_HERE.md | Master guide — day-by-day study plan, cross-reference map, survival guide |
| DSA.md | Topic priority matrix, 30+ problems, patterns cheat sheet |
| Behavioral.md | 15–20 questions with resume hooks, story bank template |
| FollowUpQuestions.md | 25+ questions organized by interviewer type |
| CompanyStack.md | Tech stack, system design focus, process summary |

**Start with `START_HERE.md`** — it tells you exactly how to use everything else.
```

## Quality Rules

- **Be specific, not generic.** A generic answer that applies to any company is less useful than a tailored one. Use company name, real product names, real tech stack details throughout.
- **If resume was provided**, every section of `Behavioral.md` should reference actual projects or roles from that resume.
- **If JD was provided**, every checklist item in `CompanyStack.md` should trace back to a JD requirement.
- **Do not fabricate LeetCode problem numbers.** If unsure, use problem name only and skip the number.
- **Mark uncertainty clearly.** Use *(inferred)*, *(unconfirmed)*, or *(check Glassdoor)* rather than stating uncertain things as facts.
