---
name: claude-code-optimizer
description: Research new Claude Code plugins, skills, and MCP servers. Compare against what's installed. Ask user what to install or replace. Install approved tools. Generate a dated PDF summary. Use when user wants to optimize their Claude Code setup or do a periodic maintenance check.
---

# Claude Code Optimizer

You are running a periodic Claude Code optimization check. Your job is to research what's new, compare it against what's installed, ask before changing anything, install what's approved, and generate a PDF summary at the end.

## Step 1 — Audit Current Setup

Run these commands and record the results:

```bash
cat ~/.claude/settings.json | grep -A 50 "enabledPlugins"
claude mcp list
```

Also note the current date for the PDF filename.

## Step 2 — Research What's New

Search for recent additions and updates across these sources:

1. **Official marketplace** — `github.com/anthropics/claude-plugins-official` (check the plugins/ directory for anything not in the user's current list)
2. **Community skills** — `github.com/travisvn/awesome-claude-skills` and `github.com/hesreallyhim/awesome-claude-code`
3. **MCP servers** — search for "claude code MCP server [current year]" and check `pulsemcp.com`
4. **Top community repos** — search "claude code plugins skills [current year] github"

For each result, evaluate:
- Does it overlap with something already installed?
- Is it actively maintained (recent commits)?
- Is it genuinely useful for a developer/studio workflow?

## Step 3 — Build the Comparison

Present findings as a clean table:

| Tool | Type | What it does | Recommendation | Reason |
|------|------|-------------|----------------|--------|
| name | plugin/mcp/skill | one line | INSTALL / REPLACE [X] / SKIP | why |

- **INSTALL** — new tool the user doesn't have, worth adding
- **REPLACE [existing-tool]** — a newer/better alternative to something already installed
- **SKIP** — looked at it, not worth adding right now (explain why)

## Step 4 — Ask Before Doing Anything

Present the table and ask:

> "Here's what I found. Which ones do you want me to install? You can say 'install all', name specific ones, or say 'skip' for any. I won't touch anything until you confirm."

Wait for explicit confirmation. Do not install anything before the user responds.

## Step 5 — Install Approved Tools

For each approved item:

**Plugins** (from official marketplace):
```bash
claude plugin install [name]@claude-plugins-official
```

**Plugins** (from other marketplace):
```bash
claude plugin marketplace add [github-url]
claude plugin install [name]@[marketplace]
```

**MCP servers**:
```bash
claude mcp add --transport stdio [name] -- npx -y [package-name]
```

After all installs:
```bash
# remind user to run this in Claude Code
echo "Run /reload-plugins to activate new plugins"
```

## Step 6 — Generate the PDF

Build a dated HTML file and print it to PDF using Chrome headless.

The PDF should:
- Use the warm cream editorial style (Playfair Display headers, Source Serif 4 body, IBM Plex Mono for code)
- Have a cover with the date and what changed
- Include a section for each newly installed tool explaining what it does in plain language and how to use it
- Include a "what to do next" section with anything that was SKIPped and why
- Include the updated full quick-reference table with all tools (old + new)
- Have a tips bar at the bottom of each page

Save to Desktop as: `claude-code-update-[YYYY-MM-DD].pdf`

### PDF generation pattern:

```bash
# Write the HTML to Desktop
cat > /Users/[username]/Desktop/claude-code-update-[date].html << 'HTML'
[full HTML with inline styles, Google Fonts via CDN, print CSS]
HTML

# Print to PDF
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --headless --disable-gpu \
  --print-to-pdf=/Users/[username]/Desktop/claude-code-update-[date].pdf \
  --print-to-pdf-no-header \
  /Users/[username]/Desktop/claude-code-update-[date].html
```

**PDF design rules:**
- Background: #faf7f1 (warm cream)
- Accent: #c47b2b (amber)
- Fonts via Google Fonts CDN (loads fine in Chrome headless)
- `page-break-inside: avoid` on all cards
- `-webkit-print-color-adjust: exact` on body
- Page width: 210mm, padding: 52px 56px 128px (leaves room for tips bar)

## Important Rules

- **Never install anything without explicit user confirmation**
- **Never remove or disable existing tools** — only suggest replacements, user decides
- **Skip tools that clearly overlap** with existing setup unless they're significantly better
- **Always explain in plain English** — no jargon, no assuming technical knowledge
- **Date the PDF** — so the user has a versioned history of their setup over time
