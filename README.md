```
 ██████╗ ██████╗ ██████╗
██╔════╝██╔════╝██╔═══██╗
██║     ██║     ██║   ██║
██║     ██║     ██║   ██║
╚██████╗╚██████╗╚██████╔╝
 ╚═════╝ ╚═════╝ ╚═════╝
   stackshift — claude code optimizer
```

# Stackshift — Claude Code Optimizer

> **Codename:** stackshift &middot; **Version:** 0.2 &middot; MIT &middot; [github](https://github.com/ronchestermusic/claude-code-optimizer)

A Claude Code skill that learns how **you** use Claude, audits your full stack (plugins, skills, MCP servers, marketplaces — broken ones flagged), hunts new tools across GitHub, X, Reddit, Hacker News, and the official Anthropic channels, asks before changing anything, and prints a personalized dated PDF guide to your Desktop.

**Run it once a month. Keep your stack sharp. Get a report written for your tech level.**

---

## ⌘ &nbsp; Install

```bash
mkdir -p ~/.claude/skills/claude-code-optimizer
curl -o ~/.claude/skills/claude-code-optimizer/SKILL.md \
  https://raw.githubusercontent.com/ronchestermusic/claude-code-optimizer/main/SKILL.md
```

Then in any Claude Code session:

```
/claude-code-optimizer
```

Or just say *"optimize my Claude Code setup"* — the skill auto-triggers.

---

## ⚡ &nbsp; What it does

```
┌─ STEP 0 ─────────────────────────────────────────┐
│  PROFILE   →  use case, tech level, PDF style    │
├─ STEP 1 ─────────────────────────────────────────┤
│  AUDIT     →  plugins + skills + MCPs + broken   │
├─ STEP 2 ─────────────────────────────────────────┤
│  RESEARCH  →  github + X + reddit + HN + docs    │
├─ STEP 3 ─────────────────────────────────────────┤
│  COMPARE   →  filter by fit, diff vs last run    │
├─ STEP 4 ─────────────────────────────────────────┤
│  ASK       →  pick by number, nothing auto       │
├─ STEP 5 ─────────────────────────────────────────┤
│  INSTALL   →  plugins, MCPs, and skills          │
├─ STEP 6 ─────────────────────────────────────────┤
│  REPORT    →  personalized PDF on your Desktop   │
└──────────────────────────────────────────────────┘
```

---

## 🧠 &nbsp; Personalized, not generic

Stackshift starts by asking who you are and how you use Claude Code:

- **Use case** — coding, writing, research, running a business, creative work, learning
- **Tech level** — brand new, basics, comfortable dev, power user
- **PDF style** — warm cream editorial, plain minimal, colorful
- **This session's goal** — what you actually want out of today's run

Every later step respects that. A novelist doesn't get Kubernetes recommendations. A power user doesn't get baby-talk explanations. The PDF language adapts to your level.

---

## 🔍 &nbsp; Multi-source research

Instead of scraping one awesome-list, Stackshift parallel-searches:

- Official Anthropic marketplace + releases + changelog
- Community awesome-lists (`awesome-claude-skills`, `awesome-claude-code`)
- MCP directories (`pulsemcp.com`, `mcp.so`)
- X (Twitter) — where most new plugins actually get announced
- Reddit (`r/ClaudeAI`, `r/Anthropic`)
- Hacker News (last 30 days)

Filtered by your profile, diffed against your last run, ranked by fit.

---

## 🧹 &nbsp; Broken-tool detection

Not just about adding — it finds what's already broken:

- MCP servers that fail to start
- Skills with missing or malformed `SKILL.md`
- Marketplaces that 404

Each gets a **FIX** or **REMOVE** recommendation you can approve or ignore.

---

## 📄 &nbsp; What's in the PDF

1. **Cover** — date, your profile summary, one-line headline of what changed
2. **What's new in your setup** — each installed tool in plain English with example prompts
3. **How to use each tool** — 2–4 usage examples per tool, matched to your use case
4. **Your full toolkit** — updated quick-reference table of your whole stack
5. **What we skipped** — and why
6. **Anything broken** — with repair steps
7. **Next check-in** — suggested cadence based on how active the ecosystem was

Saved as `claude-code-update-YYYY-MM-DD.pdf` on your Desktop. Language adapted to your tech level. Styling matched to your preference.

---

## 🛠 &nbsp; Requirements

| Need | Why |
|---|---|
| [Claude Code](https://claude.com/claude-code) | the host |
| macOS | PDF generation uses Chrome headless (Linux/Windows users can adapt the final bash block) |
| Google Chrome | at `/Applications/Google Chrome.app` |

---

## 🗺 &nbsp; Roadmap — Cross-tool compatibility

Today Stackshift runs inside Claude Code because that's where skills live. But most MCP servers already work in **Cursor, Windsurf, Zed, and any MCP-compatible terminal**. Planned:

- Detect which AI coding tools you have installed
- Tag every recommendation with "works in: Claude Code / Cursor / Windsurf / any MCP client"
- Emit equivalent install instructions per tool (e.g. `mcp.json` for Cursor)
- One unified PDF showing your full AI-assistant stack across every tool you use

**If you'd use this, ⭐ the repo and open an issue with the tools you want supported.**

---

## 💭 &nbsp; Why I built it

I'm an artist and studio owner in Toronto building tools for the music business. Claude Code is the spine of how I work — sketching releases, automating studio admin, prototyping ideas for other artists.

But new plugins ship weekly. New MCP servers ship weekly. Tracking all of it is a part-time job nobody asked for.

So I built a skill that does the tracking for me, on a schedule I control, with **receipts** (the PDF) so I remember what I added and why — and gave it a personality so the report reads like it was written for me specifically.

---

## 📜 &nbsp; License

MIT. Use it, fork it, ship your own version.

---

<sub>MIT · If this saved you an afternoon, ⭐ the repo.</sub>
