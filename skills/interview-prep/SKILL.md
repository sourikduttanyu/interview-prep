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

## Phase 4: Summary Output

After writing all four files, print a short summary to the user:

```
## Interview Prep Kit Created — <Company> (<Role>)

Files created in `./<CompanyName>/`:

| File | What's Inside |
|------|---------------|
| DSA.md | Topic priority matrix, 30+ problems, patterns cheat sheet |
| Behavioral.md | 18 questions with resume hooks, story bank template |
| FollowUpQuestions.md | 25+ questions organized by interviewer type |
| CompanyStack.md | Tech stack, system design focus, process summary |

**Suggested next steps:**
1. Read `CompanyStack.md` first to orient yourself
2. Fill in the Story Bank table in `Behavioral.md` with your own experiences
3. Work through DSA problems in priority order (P0 first)
4. Personalize the `[FILL IN]` sections in `FollowUpQuestions.md`
```

## Quality Rules

- **Be specific, not generic.** A generic answer that applies to any company is less useful than a tailored one. Use company name, real product names, real tech stack details throughout.
- **If resume was provided**, every section of `Behavioral.md` should reference actual projects or roles from that resume.
- **If JD was provided**, every checklist item in `CompanyStack.md` should trace back to a JD requirement.
- **Do not fabricate LeetCode problem numbers.** If unsure, use problem name only and skip the number.
- **Mark uncertainty clearly.** Use *(inferred)*, *(unconfirmed)*, or *(check Glassdoor)* rather than stating uncertain things as facts.
