# Claude Code — Interview Prep Skill

A Claude Code skill that generates a **complete, tailored interview prep kit** for any software engineering role — new grad or experienced. Give it a company name, paste your resume and job description, and it produces four focused markdown files in under a minute.

---

## What It Generates

Run `/interview-prep` and Claude walks you through a short Q&A, then creates:

| File | Contents |
|------|----------|
| `DSA.md` | Company-specific topic priority matrix, 30+ curated LeetCode problems, patterns cheat sheet, time/space complexity reference |
| `Behavioral.md` | 15–20 behavioral questions with STAR hooks tied to *your* resume, story bank template to fill in |
| `FollowUpQuestions.md` | 25+ questions to ask interviewers, organized by round (recruiter, engineer, hiring manager, peer) |
| `CompanyStack.md` | Tech stack breakdown, system design focus areas, things to brush up on from the JD, interview process summary |

Everything is tailored to the company and role — not generic advice.

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

## Usage

Once installed, open any project in Claude Code and run:

```
/interview-prep
```

Claude will prompt you interactively:

```
Claude: Which company and role are you interviewing for?
You:    Google — Software Engineer, New Grad

Claude: Paste the job description below (or type `skip`):
You:    [paste JD]

Claude: Paste your resume or give me a file path (or type `skip`):
You:    /path/to/resume.md

Claude: Got it! Generating your prep kit...
```

Files are created in a `<CompanyName>/` folder in your current directory.

---

## Example Output Structure

```
Google/
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
