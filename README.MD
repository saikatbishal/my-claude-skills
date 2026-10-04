# DOCUMENTATION AND TABLE OF CONTENTS

This is a repository containing all the skills that I have created between Jan-2026 and now. This repository is always a work in progress. To add your own skills to this, create PR to the `add-skill-staging` branch long with a short amrkdown documentation(not mandatory but good to have). It will be added after the review.

The list of skills are segregated in 2 folders, tech and non-tech skills. Might divide it further when the volume increases.

## TABLE OF CONTENTS

### TECH

- [System-design-interviewer](#system-design-interviewer)
- [UI-interview](#ui-interview)

---

## TECH SKILLS

### System-design-interviewer

📁 [`tech/system-design-interviewer`](tech/system-design-interviewer)

A realistic 45-minute mock system design (HLD) interview. Claude plays a senior engineer interviewer: it picks a problem (URL shortener, chat, news feed, rate limiter, etc.) or uses the one you name, then walks you through clarifying questions → estimation → requirements → high-level design → deep dive → bottlenecks. It asks one question at a time and pushes back instead of designing the system for you.

- **Candidate commands:** `hint`, `pause` / `eli5 <topic>`, `harder`, `go easier`, `skip to feedback`, `restart`
- **Ends with:** a scored debrief — hire/no-hire verdict, per-area scores, technical and non-technical gaps, and a study list.
- **Trigger:** "interview me on system design", "mock HLD round", "be my interviewer — design Twitter", or `/system-design-interviewer`.

### UI-interview

📁 [`tech/ui-interview`](tech/ui-interview)

A front-end UI component coding-interview round. You pick a level (**Easy / Medium / Hard**) and optionally a component or a technique to practice (e.g. drag and drop, keyboard nav). Claude then:

1. Writes a behavioral spec (inputs, states, interactions, edge cases, non-goals).
2. Publishes a visual reference artifact with the target states and design tokens (colors, type scale, spacing) — never the solution code.
3. Gives you realistic sample data to paste into your project.
4. Answers clarifying questions like a real interviewer.
5. Judges your submitted TSX + CSS + screenshot on **Code clarity** and **UI match** (each out of 5), with specific strengths and improvements.

You build the component yourself in your own editor — Claude won't write or fix your code during the round.

- **Trigger:** `/ui-interview`
- **Note:** needs an environment that can publish Artifacts (claude.ai or Claude Code with Artifacts enabled) for the visual reference.

---

## HOW TO INSTALL AND USE A SKILL

Each skill folder has two files:

| File | What it is |
| --- | --- |
| `<skill-name>.skill` | Packaged skill (a zip containing `<skill-name>/SKILL.md`) — use this to install. |
| `<skill-name>.md` | Readable copy of the same `SKILL.md`, for browsing on GitHub. |

### Option 1 — Claude.ai (web / desktop app)

1. Download the `.skill` file of the skill you want (e.g. `tech/ui-interview/ui-interview.skill`).
2. In Claude, go to **Settings → Capabilities → Skills** (make sure Skills are enabled).
3. Click **Upload skill** and select the `.skill` file.
4. Toggle the skill on. Start a new chat and use its trigger (see each skill above).

### Option 2 — Claude Code (CLI)

Skills live in a folder named after the skill, containing a `SKILL.md` file:

- **Personal (all projects):** `~/.claude/skills/<skill-name>/SKILL.md`
- **Project only:** `<your-project>/.claude/skills/<skill-name>/SKILL.md`

Easiest way — unzip the `.skill` file straight into your skills folder:

```bash
git clone https://github.com/saikatbishal/my-claude-skills.git
cd my-claude-skills

# macOS / Linux / Git Bash
mkdir -p ~/.claude/skills
unzip tech/system-design-interviewer/system-design-interviewer.skill -d ~/.claude/skills/
unzip tech/ui-interview/ui-interview.skill -d ~/.claude/skills/
```

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item tech\ui-interview\ui-interview.skill "$env:TEMP\ui-interview.zip"
Expand-Archive "$env:TEMP\ui-interview.zip" -DestinationPath "$HOME\.claude\skills" -Force
```

Alternatively, copy the `.md` file manually and rename it to `SKILL.md`:

```bash
mkdir -p ~/.claude/skills/ui-interview
cp tech/ui-interview/ui-interview.md ~/.claude/skills/ui-interview/SKILL.md
```

Then start (or restart) Claude Code. The skill is picked up automatically.

### Using a skill

- **By slash command:** type `/<skill-name>` (e.g. `/ui-interview`).
- **By asking naturally:** Claude loads a skill on its own when your request matches its description — e.g. _"Give me a mock system design interview for a chat app."_
- To check it's installed, ask Claude _"What skills do you have?"_ or type `/` in Claude Code and look for the skill name.
