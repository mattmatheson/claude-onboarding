# 02 — Your First CLAUDE.md

> **Goal:** Create a file that tells Claude who you are, how you work, and what rules to follow — across every conversation.

## What Is CLAUDE.md?

`CLAUDE.md` is a markdown file that Claude reads at the start of every conversation. Think of it as your **standing instructions**. Instead of repeating "I prefer Tailwind over plain CSS" or "always use TypeScript" every time, you write it once in CLAUDE.md and Claude follows it forever.

## Why This Matters

Without CLAUDE.md, every conversation starts from zero. Claude doesn't know:
- What tech stack you prefer
- How you like code formatted
- What projects you're working on
- Your experience level
- Your safety rules

With CLAUDE.md, Claude shows up already knowing your preferences. It's the difference between onboarding a new contractor every morning vs. working with someone who knows your codebase.

## Where It Goes

There are two levels:

### Global (applies everywhere)
```
~/.claude/CLAUDE.md
```
This is YOUR file. It follows you across every project.

### Per-Project (applies to one repo)
```
your-project/CLAUDE.md
```
Drop this in any project root. Claude reads both global + project CLAUDE.md files, with project-level taking priority for conflicts.

## Building Your CLAUDE.md

Start with the template in [`configs/starter-claude.md`](../configs/starter-claude.md), then customize. Here's what to include:

### 1. About You
```markdown
## About Me
- Name: Braden
- Focus: Building dashboards, websites, and productivity tools
- Stack: Next.js, React, Tailwind, Vercel
- Experience: Comfortable with CLI, learning Claude Code workflows
```

> **Why:** Claude tailors its explanations to your level. A beginner gets step-by-step instructions. An experienced dev gets concise suggestions.

### 2. Code Preferences
```markdown
## Code Preferences
- TypeScript over JavaScript
- Tailwind CSS for styling
- Use App Router (not Pages Router) for Next.js
- Prefer server components unless client interactivity is needed
- Write clear commit messages that explain WHY, not just what
```

### 3. Safety Rules
```markdown
## Safety Rules
- Never hardcode API keys, tokens, or secrets in code
- Use .env files for all secrets
- Never force-push to main
- Never run rm -rf without asking first
- Never commit .env files or credentials
```

### 4. Tools & Conventions
```markdown
## Tools
- GitHub: Use `gh` CLI for all GitHub operations
- Package manager: npm (not yarn or pnpm unless specified)
- Always create feature branches for new work
- Run builds/tests before committing
```

### 5. Communication Style
```markdown
## Communication
- Be concise — skip the preamble
- Don't ask unnecessary questions — check the code first
- If something is unclear, make your best judgment and explain what you assumed
```

## The "Get to Know You" Conversation

Before writing your CLAUDE.md, have Claude interview you. Copy this into Claude:

```
Read configs/get-to-know-you.md and interview me. Use my answers to draft a personalized CLAUDE.md file.
```

This walks through questions about your background, goals, and working style, then generates a CLAUDE.md tailored to you.

## Evolving Your CLAUDE.md

Your CLAUDE.md isn't set-and-forget. Update it as you:
- Learn new preferences ("actually I prefer shadcn/ui over custom components")
- Start new projects (add project-specific context)
- Discover Claude habits you want to correct ("stop adding comments to every function")

> **Pro tip:** When Claude does something you love or hate, tell it: "Remember this for next time." If it's a pattern, add it to CLAUDE.md yourself.

## What's Next

Your Claude now knows who you are. Let's give it superpowers with MCP servers → [03 — MCP Servers](03-mcp-servers.md)
