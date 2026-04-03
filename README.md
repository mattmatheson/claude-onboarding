# Claude Code Power Setup

A guide to turning Claude Code from a basic CLI into a fully loaded productivity machine. Built by someone who runs this setup across multiple computers daily for web development, dashboards, automation, and creative work.

## Who This Is For

You've got Claude Code installed. Maybe you've built a Vercel dashboard or two. But you know there's more — MCP servers, skills, hooks, Obsidian as a brain, multi-machine sync. This repo walks you through all of it, explaining **why** each piece matters so you're not just copy-pasting configs.

## How to Use This Repo

**Point your Claude at this repo.** Clone it, then tell Claude:

> "Read through this repo. It's an onboarding guide — walk me through it step by step, explain things as we go, and help me set each piece up on my machine."

Claude will read the guides, understand your setup, and teach you as you build together. That's the whole point — you learn by doing, with Claude as your co-pilot.

## The Guides

| # | Guide | What You'll Learn |
|---|-------|-------------------|
| 01 | [Claude Code Basics](guides/01-claude-code-basics.md) | Shortcuts, permission modes, slash commands, conversation management |
| 02 | [Your First CLAUDE.md](guides/02-claude-md.md) | How to give Claude persistent instructions about you and your projects |
| 03 | [MCP Servers](guides/03-mcp-servers.md) | Plugins that give Claude superpowers — browser control, docs, memory |
| 04 | [Skills & Custom Commands](guides/04-skills.md) | Reusable prompts, slash commands, and how to build your own |
| 05 | [Hooks & Automation](guides/05-hooks.md) | Auto-format code, voice feedback, notifications — event-driven automation |
| 06 | [Obsidian as Your Brain](guides/06-obsidian.md) | Why local markdown files beat every other notes app for AI workflows |
| 07 | [Building Dashboards & Websites](guides/07-dashboards-and-websites.md) | Frontend skills, design systems, and shipping to Vercel |
| 08 | [Databases](guides/08-databases.md) | When to use Supabase vs local Postgres vs Neo4j |
| 09 | [Multi-Machine Setup](guides/09-multi-machine.md) | Tailscale VPN, Syncthing, running Claude on multiple computers |
| 10 | [Git & GitHub Workflows](guides/10-git-workflows.md) | PRs, branches, gh CLI, and how Claude handles version control |
| 11 | [Security & Secrets](guides/11-security.md) | Never hardcode keys — .env files, 1Password CLI, vetting MCP servers |
| 12 | [Memory & Context](guides/12-memory.md) | Auto-memory, knowledge graphs, and teaching Claude to remember you |
| 13 | [Productivity Workflows](guides/13-workflows.md) | Email, calendar, web scraping, file management — the daily driver stuff |

## Starter Configs

The [`configs/`](configs/) folder has ready-to-use templates:

- **`starter-settings.json`** — MCP servers config (drop into `~/.claude/settings.json`)
- **`starter-claude.md`** — CLAUDE.md template to customize for yourself
- **`get-to-know-you.md`** — Onboarding questionnaire for Claude to learn about you

## Quick Start (5 minutes)

```bash
# 1. Clone this repo
git clone https://github.com/mattmatheson/claude-onboarding.git
cd claude-onboarding

# 2. Open Claude Code in this directory
claude

# 3. Tell Claude to onboard you
> "Read the README and guides in this repo. Walk me through setting everything up on my machine, starting with guide 01."
```

## Philosophy

- **Explain the why, not just the how.** Every tool earns its place.
- **Start simple, add complexity when you need it.** Don't install 20 MCP servers on day one.
- **Your Claude should know you.** The memory system and CLAUDE.md exist so Claude gets better the more you use it.
- **Local-first, privacy-first.** Obsidian, local Postgres, markdown files — your data stays yours.
