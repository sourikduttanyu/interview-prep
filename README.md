# Claude Code — Interview Prep Skill

A Claude Code skill that generates a **complete, tailored interview prep kit** for any software engineering role — new grad or experienced. Give it a company name, paste your resume and job description, and it produces five focused markdown files including a master guide with a day-by-day study plan, curated articles, and a prioritized YouTube watch list.

---

## What It Generates

Run `/interview-prep` and Claude walks you through a short Q&A, then creates a `<CompanyName>/` folder with:

| File | Contents |
|------|----------|
| `START_HERE.md` | **Master guide** — day-by-day study plan, file cross-reference map, short-on-time survival guide, common mistakes, post-interview checklist, curated articles & YouTube videos |
| `DSA.md` | Company-specific topic priority matrix, 30+ curated LeetCode problems, patterns cheat sheet, time/space complexity reference |
| `Behavioral.md` | 15–20 behavioral questions with STAR hooks tied to *your* resume, story bank template to fill in |
| `FollowUpQuestions.md` | 25+ questions to ask interviewers, organized by round (recruiter, engineer, hiring manager, peer) |
| `CompanyStack.md` | Tech stack breakdown, system design focus areas, things to brush up on from the JD, interview process summary |

Everything is tailored to the company and role — not generic advice. Start with `START_HERE.md`.

---

## Interactive Input

The skill prompts you in chat — no flags or file paths required upfront:

```
Claude: Which company and role are you interviewing for?
You:    Google — Software Engineer, New Grad

Claude: Paste the job description below (or type `skip`):
You:    [paste JD text]

Claude: Share your resume — you can attach a file, paste a path, paste the text, or type `skip`.
You:    [attach resume.pdf directly in chat]

Claude: Got it! Generating your prep kit now...
```

### Resume Input Options

| Method | How |
|--------|-----|
| Attach in chat | Drag-and-drop or attach a PDF / DOCX / PNG directly into the Claude Code chat |
| File path | Type `/path/to/resume.pdf` |
| Paste text | Paste resume content directly |
| Skip | Type `skip` for generic behavioral questions |

> PDF and DOCX give the best parsing results. PNG is read via vision and may miss formatting details.

---

## What's Inside `START_HERE.md`

The master guide is generated last and ties everything together:

- **Day-by-day study plan** across 5 phases (Orient → DSA → Behavioral → Stack Brush-Up → Mock), timeline tailored to the company's known process
- **File cross-reference map** — which file to open for each situation (OA prep, day-before, during rounds, post-interview debrief)
- **Short on time?** Survival checklists for 48 hours, 24 hours, and day-of
- **Common mistakes to avoid** — company-specific anti-patterns
- **Post-interview checklist** — thank-you note, notes log, story bank debrief
- **Resources section** — web-searched articles and YouTube videos:
  - Interview process reports from Glassdoor, Levels.fyi, LeetCode discuss, engineering blogs
  - Targeted reading for each tech gap identified in `CompanyStack.md`
  - Prioritized YouTube watch list (P0 topics first, capped at 15 videos) — prefers NeetCode, ByteByteGo, Gaurav Sen, Abdul Bari, Back To Back SWE

> Only verified URLs from web search are included. Training-knowledge links are flagged with ⚠️. Broken YouTube links are written as `(search on YouTube)` instead.

---

## Installation

### Option A — Global (use from any project)

```bash
mkdir -p ~/.claude/skills/interview-prep
curl -o ~/.claude/skills/interview-prep/SKILL.md \
  https://raw.githubusercontent.com/sourikduttanyu/interview-prep/main/skills/interview-prep/SKILL.md
```

### Option B — Local (scoped to one project)

```bash
mkdir -p .claude/skills/interview-prep
curl -o .claude/skills/interview-prep/SKILL.md \
  https://raw.githubusercontent.com/sourikduttanyu/interview-prep/main/skills/interview-prep/SKILL.md
```

### Option C — Clone and symlink

```bash
git clone https://github.com/sourikduttanyu/interview-prep.git
mkdir -p ~/.claude/skills
ln -s $(pwd)/interview-prep/skills/interview-prep ~/.claude/skills/interview-prep
```

---

## Example Output Structure

```
Google/
├── START_HERE.md          ← open this first
├── DSA.md
├── Behavioral.md
├── FollowUpQuestions.md
└── CompanyStack.md
```

---

## Requirements

- [Claude Code](https://claude.ai/code) CLI installed
- A Claude account (free tier works)

---

## Why This Exists

Generic interview prep resources don't know your resume, the specific JD, or the company's actual interview style. This skill combines all three and produces material you can use the same day — not a 200-page guide you'll never finish.

---

## Contributing

PRs welcome. If you have corrections to company-specific interview details, improvements to the DSA problem lists, or new behavioral question themes, open an issue or send a PR.

---

## License

MIT
