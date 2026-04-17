# stackshift

**Claude Code optimizer.** A skill that audits your Claude Code setup, finds new plugins / skills / MCP servers worth adding, asks before changing anything, installs what you approve, and prints a dated PDF report to your Desktop.

Run it once a month. Keep your AI dev stack sharp.

---

## What it does

1. **Audits** your current plugins and MCP servers.
2. **Researches** the official marketplace, community awesome-lists, and pulsemcp.com for new tools.
3. **Compares** what's new against what you have — flags overlap, evaluates fit.
4. **Asks first.** Nothing installs without your explicit confirmation.
5. **Installs** approved tools via the right `claude` CLI commands.
6. **Generates a PDF** — warm cream editorial style, dated, saved to your Desktop. Versioned history of your setup over time.

---

## Install

Drop the skill into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/claude-code-optimizer
curl -o ~/.claude/skills/claude-code-optimizer/SKILL.md \
  https://raw.githubusercontent.com/ronchestermusic/stackshift/main/SKILL.md
```

Then in any Claude Code session:

```
/claude-code-optimizer
```

Or just say "optimize my Claude Code setup" — the skill auto-triggers.

---

## Why I built it

I'm a studio owner in Toronto who spends too much time in Claude Code. New plugins ship weekly. New MCP servers ship weekly. Nobody has time to track all of it.

So I built a skill that does the tracking for me, on a schedule I control, with receipts (the PDF) so I remember what I added and why.

---

## What you get in the PDF

- Cover page with the date and a summary of what changed
- One section per newly installed tool — what it does in plain English, how to use it
- A "skipped" section explaining what was looked at and why it didn't make the cut
- An updated quick-reference table of your full stack
- Tips bar at the bottom of every page

Saved as `claude-code-update-YYYY-MM-DD.pdf` on your Desktop.

---

## Requirements

- [Claude Code](https://claude.com/claude-code) installed
- macOS (the PDF generation uses Chrome headless — Linux/Windows users can adapt the path)
- Google Chrome at `/Applications/Google Chrome.app`

---

## License

MIT. Use it, fork it, ship your own version.

---

**Built by [Ron Chester](https://github.com/ronchestermusic) — Toronto.**
