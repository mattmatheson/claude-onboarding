# 06 — Obsidian as Your Brain

> **Goal:** Set up Obsidian as a local knowledge base that Claude can read and write to — your shared second brain.

## Why Obsidian?

Here's the key insight: **Obsidian stores everything as plain markdown files on your computer.** No proprietary database. No cloud lock-in. No API keys needed.

This means Claude Code can read and write your notes directly — no MCP server required, no authentication, no API limits. Your Obsidian vault is just a folder of `.md` files, and Claude already knows how to work with files.

Compare that to Notion (needs API key + MCP server), Google Docs (needs OAuth), or Evernote (needs scraping). Obsidian is zero-friction for AI integration.

## The "Second Brain" Concept

Your vault becomes a shared workspace between you and Claude:

- **You** take notes during meetings, capture ideas, journal
- **Claude** reads those notes for context, writes summaries, updates project trackers
- **Both** benefit from the accumulated knowledge

Over time, your vault becomes a rich context store. Instead of re-explaining your business, clients, or project history to Claude every conversation, it just reads your vault.

## Setting Up Your Vault

### Install Obsidian
```bash
brew install --cask obsidian
```
Or download from [obsidian.md](https://obsidian.md)

### Create Your Vault
```bash
mkdir -p ~/Vault
```

Open Obsidian → "Open folder as vault" → select `~/Vault`

### Recommended Structure

```
~/Vault/
├── daily/              # Daily notes, standup logs
├── projects/           # One folder per active project
│   ├── dashboard-v2/
│   └── client-website/
├── knowledge/          # Reference material
│   ├── tools/          # Notes on tools you use
│   ├── apis/           # API docs, integration notes
│   └── recipes/        # Reusable code patterns
├── meetings/           # Meeting notes
├── templates/          # Reusable note templates
├── capture/            # Quick dump — sort later
└── CLAUDE.md           # Instructions for Claude about this vault
```

> **Why this structure:** It mirrors how your brain organizes information — by project, by type, by time. Claude navigates it the same way you do.

### The Vault CLAUDE.md

Create `~/Vault/CLAUDE.md` to tell Claude how to interact with your vault:

```markdown
# Vault Instructions

This is my Obsidian knowledge vault.

## Rules
- Always use [[double brackets]] for internal links
- New daily notes go in daily/ with format YYYY-MM-DD.md
- Project notes go in projects/{project-name}/
- Never delete the capture/ folder
- Use frontmatter (---) at the top of notes for metadata

## Frontmatter Template
---
title: Note Title
date: YYYY-MM-DD
tags: [tag1, tag2]
status: active | complete | archived
---
```

## Key Obsidian Features to Learn

### Internal Links
`[[Note Name]]` creates a link to another note. This is Obsidian's killer feature — it builds a web of connected knowledge.

```markdown
Working on [[Dashboard V2]] today. 
Need to integrate the [[Supabase]] backend.
Discussed requirements in [[2024-01-15 Client Meeting]].
```

### Graph View
Obsidian visualizes all your note connections as an interactive graph. As your vault grows, you'll see clusters of related knowledge emerge.

### Daily Notes
Enable the Daily Notes core plugin. One note per day, auto-created from a template. Great for:
- Morning planning
- End-of-day summaries
- Quick capture throughout the day

### Templates
Create template files in `templates/` and insert them into new notes. Example daily template:

```markdown
# {{date}}

## Plan
- [ ] 

## Notes


## End of Day
### What got done

### What's next
```

### Community Plugins (Install These)
- **Dataview** — Query your notes like a database (`TABLE` queries across vault)
- **Templater** — Advanced templates with variables and logic
- **Tasks** — Track to-dos across all notes
- **Calendar** — Visual calendar linked to daily notes
- **Git** — Auto-backup vault to GitHub

## How Claude Uses Your Vault

Tell Claude about your vault in your global CLAUDE.md:

```markdown
## Obsidian Vault
My knowledge vault is at ~/Vault/. Check it for context before asking me questions.
Write session summaries to daily/ when we do significant work.
```

Now Claude can:
- **Read project notes** before starting work on a project
- **Check meeting notes** for context on client requests
- **Write daily summaries** of what you accomplished
- **Update project trackers** as work progresses
- **Search your vault** for relevant past decisions

## Syncing Your Vault

### Option 1: Git (Best for Claude Integration)
```bash
cd ~/Vault
git init
git remote add origin git@github.com:you/vault.git
```

Use the Obsidian Git plugin for auto-commits. Access from any machine.

### Option 2: iCloud Drive (Easiest for Mac/iOS)
Move vault to `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Vault`

### Option 3: Syncthing (Best for Multi-Computer, see Guide 09)
Peer-to-peer sync between machines. No cloud, near-instant, works over LAN.

## What's Next

Let's build some dashboards and websites → [07 — Dashboards & Websites](07-dashboards-and-websites.md)
