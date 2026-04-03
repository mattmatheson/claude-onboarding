# 12 — Memory & Context

> **Goal:** Understand how Claude remembers you across conversations — auto-memory, knowledge graphs, and how to build a Claude that gets smarter over time.

## The Problem: Amnesia

Every Claude conversation starts fresh. Without memory, you're re-introducing yourself every time:
- "I use Next.js and Tailwind"
- "My project is at ~/projects/dashboard"
- "I prefer TypeScript"

Memory systems fix this. There are three layers:

## Layer 1: CLAUDE.md (Static Memory)

You already set this up in Guide 02. It's your **standing instructions** — always loaded, always followed. Think of it as long-term memory you manually curate.

**Best for:** Preferences, rules, tech stack, communication style — things that rarely change.

## Layer 2: Auto-Memory (Dynamic Memory)

Claude Code has a built-in memory system that automatically saves things it learns about you during conversations.

### How It Works

When Claude learns something important (your role, a project detail, a correction you made), it can save it to markdown files in `~/.claude/projects/`. These files are loaded into future conversations automatically.

### Types of Memories

- **User memories** — Who you are, your experience level, preferences
- **Feedback memories** — Corrections and confirmations ("don't do X", "yes, keep doing Y")
- **Project memories** — Ongoing work, deadlines, decisions
- **Reference memories** — Where to find things (which Linear project, which Slack channel)

### Managing Memory

```
> /memory
```

This shows what Claude has saved about you. You can:
- Tell Claude to remember something: "Remember that I prefer shadcn/ui over Material UI"
- Tell Claude to forget something: "Forget the memory about my old project"
- Review and clean up stale memories

### The Memory Index

Claude keeps a `MEMORY.md` index file with one-line pointers to individual memory files. Each memory has its own file with structured frontmatter:

```markdown
---
name: user_preferences
description: Braden's tech stack and coding preferences
type: user
---

Prefers Next.js App Router, TypeScript, Tailwind CSS, shadcn/ui.
Comfortable with CLI. Building dashboards and productivity tools.
```

> **Why this matters:** As you work with Claude over days, weeks, months — the memory accumulates. Claude stops asking questions it already knows the answer to. It remembers your past decisions and builds on them. It becomes YOUR Claude, not just A Claude.

## Layer 3: Knowledge Graph (Structured Memory)

The MCP knowledge graph (Guide 03) adds structured relationships:

```
Entity: "Dashboard V2" → uses → "Supabase"
Entity: "Supabase" → hosted at → "project-xyz.supabase.co"
Entity: "Dashboard V2" → deployed to → "Vercel"
```

Auto-memory is prose. The knowledge graph is structured data. Both are useful:
- **Auto-memory:** "Braden prefers dark mode and minimal UI"
- **Knowledge graph:** "Dashboard V2 uses Supabase at project-xyz.supabase.co and is deployed to dashboard-v2.vercel.app"

## Layer 4: Obsidian Vault (External Brain)

Your Obsidian vault (Guide 06) is the deepest layer. It contains:
- Project notes with full context
- Meeting transcripts
- Decision logs
- Research and references

Claude reads these when it needs deep context. The vault is the "source of truth" — memory and CLAUDE.md are summaries, the vault has the full picture.

## How the Layers Work Together

```
┌─────────────────────────────────────────┐
│  CLAUDE.md (always loaded, you control) │
├─────────────────────────────────────────┤
│  Auto-Memory (auto-saved, per-project)  │
├─────────────────────────────────────────┤
│  Knowledge Graph (structured, on-demand)│
├─────────────────────────────────────────┤
│  Obsidian Vault (deep context, on-demand│
└─────────────────────────────────────────┘
```

From top to bottom: always present → loaded when relevant → queried when needed → read on demand.

## Building Good Memory Habits

1. **Correct Claude and it sticks.** "Don't use yarn, use npm." Claude saves this as feedback memory.
2. **Confirm good approaches.** "Yes, exactly like that." Claude learns what works.
3. **Review periodically.** Run `/memory` every few weeks. Remove outdated memories.
4. **Use your vault.** For anything substantial (project plans, client details, meeting notes), put it in Obsidian. Memory is for quick context; the vault is for depth.

## What's Next

Let's tie it all together with daily productivity workflows → [13 — Productivity Workflows](13-workflows.md)
