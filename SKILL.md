---
name: claude-code-optimizer
description: Stackshift — Claude Code Optimizer. Profiles how you actually use Claude Code, audits your installed plugins/skills/MCP servers/marketplaces (flagging broken ones), researches new tools across GitHub, X, Reddit, Hacker News, and official Anthropic channels in parallel, and generates a personalized dated PDF guide whose language and depth match your tech level. Use when the user wants to optimize their Claude Code setup, do a periodic maintenance check, or figure out which new tools fit their workflow.
---

# Stackshift — Claude Code Optimizer

A personalized, periodic optimization pass for Claude Code. Seven steps:

0. Profile the user
1. Audit current setup (deep)
2. Research what's new (parallel, multi-source)
3. Filter, diff, and compare
4. Ask by number
5. Install approved tools
6. Generate the personalized PDF guide

Never change anything without explicit numbered confirmation. Always adapt language to the user's tech level.

---

## Step 0 — Profile the User

Before touching anything, build a short profile. If CLAUDE.md or prior memory already answers these, skip the question — otherwise ask them all in **one** message, conversationally:

- **What do you mainly use Claude Code for?** (coding / writing / running a business / research / creative work / learning / other)
- **Tech level?** (brand new / know some basics / comfortable dev / power user) — controls PDF language
- **Anything specific you want from this session?** (e.g. "just show me what's new", "faster PDFs", "cut my prompting in half", "automate my posting")
- **PDF style preference?** (warm cream editorial — default / plain minimal / colorful / don't care)

Save the answers as a working profile block. Everything downstream uses it: research is filtered by use case, recommendations are ranked by fit, PDF language + depth match the tech level, PDF visuals match the style preference.

**Also check for prior runs:** if a `~/Desktop/claude-code-update-*.pdf` exists, note the most recent date. Step 3 uses it to only surface what's new since then.

---

## Step 1 — Audit Current Setup (Deep)

Run these in parallel and capture everything. Don't only look at plugins — a real optimizer audits the whole surface.

```bash
# Plugins + hooks + permissions
cat ~/.claude/settings.json

# Active plugin marketplaces
claude plugin marketplace list

# MCP servers (flag any that error)
claude mcp list

# Installed skills (user-level)
ls -la ~/.claude/skills/ 2>/dev/null

# Installed skills (project-level, if inside a repo)
ls -la ./.claude/skills/ 2>/dev/null

# Claude Code version
claude --version
```

**Broken-tool detection:**
- MCP servers that error out → candidate for **FIX** or **REMOVE**
- Skill folders without a valid `SKILL.md` or with broken frontmatter → candidate for **FIX** or **REMOVE**
- Marketplaces that 404 → candidate for **REMOVE**

Print a one-line snapshot: *"You have N plugins, M skills, K MCP servers across L marketplaces. Claude Code version X.Y.Z."*

---

## Step 2 — Research What's New (Parallel, Multi-Source)

Fire these in parallel. Cast a wide net — filter hard in Step 3. Prefer `WebFetch` and `WebSearch` over a single source.

**GitHub:**
- `github.com/anthropics/claude-plugins-official` → `plugins/` directory for new entries
- `github.com/travisvn/awesome-claude-skills`
- `github.com/hesreallyhim/awesome-claude-code`
- GitHub search queries: `claude code plugin`, `claude code skill`, `claude mcp server` — sort by recently updated + stars

**MCP directories:**
- `pulsemcp.com`
- `mcp.so`

**Social / community (this is where most shipping gets announced first):**
- **X (Twitter)** — `WebSearch` for `"claude code" (plugin OR skill OR mcp)` and recent posts tagging `@AnthropicAI`
- **Reddit** — `site:reddit.com/r/ClaudeAI claude code`, `site:reddit.com/r/Anthropic`
- **Hacker News** — `hn.algolia.com "claude code"` filtered to last 30 days

**Official Anthropic:**
- `docs.claude.com/en/docs/claude-code` — feature/changelog pages
- `github.com/anthropics/claude-code/releases` — version notes
- `anthropic.com/news` — ecosystem announcements

For each candidate capture: name, type (plugin / skill / MCP), one-line purpose, install URL or command, last updated date, adoption signal (stars / mentions / comments), and **whether it's Claude-Code-exclusive or cross-tool** (some MCP servers work in Cursor, Windsurf, Zed, and custom terminals too — relevant for Step 3 ranking if the user cares).

---

## Step 3 — Filter, Diff, and Build the Comparison

**Filter by the profile from Step 0.** A non-technical writer does not need a Kubernetes MCP. A Rails dev may not need a writing assistant skill. Drop obvious mismatches, rank the rest by fit.

**Diff against the last run.** If a previous PDF exists, only surface tools that are genuinely new or meaningfully updated since that date. Don't re-pitch what the user already saw.

**Present as a numbered table:**

| # | Tool | Type | What it does (plain English, matched to tech level) | Recommendation | Reason |
|---|------|------|-----------------------------------------------------|----------------|--------|
| 1 | name | plugin / skill / mcp | one line | **INSTALL** / **REPLACE [X]** / **FIX** / **REMOVE** / **SKIP** | why |

Recommendations:
- **INSTALL** — new, fits this user
- **REPLACE [existing]** — meaningfully better than something they have
- **FIX** — currently broken, worth repairing
- **REMOVE** — broken and not worth fixing, or clearly unused
- **SKIP** — reviewed, not a fit (one-line reason)

Close with a one-line summary: *"Found N candidates, M broken, K already installed and healthy."*

---

## Step 4 — Ask by Number

Show the table, then ask:

> "Which ones? Reply with numbers (`1, 3, 5`), `all install`, `all fixes`, or `skip` to do nothing. I won't touch anything until you confirm."

Wait for an explicit answer. Never install before confirmation.

---

## Step 5 — Install Approved Tools

Handle each type:

**Plugins — official marketplace:**
```bash
claude plugin install <name>@claude-plugins-official
```

**Plugins — third-party marketplace:**
```bash
claude plugin marketplace add <github-url>
claude plugin install <name>@<marketplace>
```

**MCP servers (stdio):**
```bash
claude mcp add --transport stdio <name> -- npx -y <package-name>
```

**Skills (git-based):**
```bash
git clone <repo-url> ~/.claude/skills/<skill-name>
# then verify ~/.claude/skills/<skill-name>/SKILL.md has valid frontmatter
```

**Skills inside a skill-marketplace plugin:** document how the user invokes them via the hosting plugin (no separate install needed).

After any plugin or MCP install, tell the user to run `/reload-plugins` or restart Claude Code.

If any install fails, capture the error and surface it in the PDF under "Anything broken" rather than silently continuing.

---

## Step 6 — Generate the Personalized PDF Guide

The PDF is both a change report **and** a usage guide — something the user can open next month and still learn from.

**Language rule (non-negotiable):** write the whole document in language matched to the tech level from Step 0.
- *power user* — "MCP server over stdio transport, registered with `claude mcp add`"
- *comfortable dev* — "MCP server — a little program Claude can call for extra tools"
- *basics* — "a helper that plugs extra abilities into Claude. Here's what it does for you…"
- *brand new* — no jargon at all. Describe outcomes, not mechanisms.

**Section order:**
1. **Cover** — date, one-line profile summary, headline of what changed this run
2. **What's new in your setup** — for each installed tool: plain-language description, concrete example prompts, what to expect
3. **How to use each tool** — 2–4 usage examples per tool, matched to the user's stated use case
4. **Your full toolkit** — updated quick-reference table of ALL tools (old + new), grouped by type
5. **What we skipped and why** — one short paragraph per skipped tool
6. **Anything broken** — FIX / REMOVE items with repair steps
7. **Next check-in** — suggested interval (2 weeks if the ecosystem was active, 4–6 weeks if quiet)

**Styling preset from Step 0:**
- *Warm cream editorial* (default) — `#faf7f1` bg, `#c47b2b` amber accent, Playfair Display + Source Serif 4 + IBM Plex Mono
- *Plain minimal* — white bg, black text, system sans, no accent fonts, generous whitespace
- *Colorful* — saturated accent palette (blue `#4F8FFF`, warm orange `#ffb070`), keep serif headers
- *Don't care* — default to warm cream

**Shared CSS rules:**
- Google Fonts via CDN (Chrome headless loads them fine)
- `page-break-inside: avoid` on every card
- `-webkit-print-color-adjust: exact` on `body`
- Page width: 210mm, padding: `52px 56px 128px` (room for a tips bar)

**Save to:** `~/Desktop/claude-code-update-[YYYY-MM-DD].pdf`

**Generation pattern:**

```bash
cat > ~/Desktop/claude-code-update-<date>.html << 'HTML'
<!-- full HTML with inline styles, Google Fonts CDN, print CSS -->
HTML

/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --headless --disable-gpu \
  --print-to-pdf=$HOME/Desktop/claude-code-update-<date>.pdf \
  --print-to-pdf-no-header \
  $HOME/Desktop/claude-code-update-<date>.html
```

---

## Hard Rules

- **Never install without explicit numbered confirmation.**
- **Never remove or disable anything without permission** — FIX / REMOVE items are still suggestions.
- **Tech level controls language everywhere** — table descriptions, PDF body, error messages, question wording.
- **Filter by profile.** A non-dev using Claude Code for writing shouldn't get Docker MCP recommendations.
- **Diff against the last PDF.** Don't re-pitch tools the user already saw.
- **Date every PDF.** The user keeps a versioned history of their setup.

---

## Scope & Roadmap

**Current scope:** Claude Code only. Skills only run inside Claude Code today, and install commands (`claude plugin install`, `claude mcp add`) are Claude-Code-specific.

**Cross-tool direction (planned):** many MCP servers already work across Cursor, Windsurf, Zed, and custom AI terminals. A future version will:
- Detect which AI coding tools the user has installed
- Flag recommendations as "works in: Claude Code / Cursor / Windsurf / any MCP client"
- Emit equivalent install instructions for each tool's config file (e.g., Cursor's `mcp.json`)
- Generate a portable PDF that shows the user's stack across all their AI tools, not just Claude Code

Until then, note cross-tool compatibility in Step 2 research output so the user knows which tools they could carry over if they switch or use multiple assistants.
