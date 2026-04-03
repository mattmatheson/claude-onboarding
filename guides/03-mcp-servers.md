# 03 — MCP Servers

> **Goal:** Understand what MCP servers are, install the essential free ones, and know when to add more.

## What Are MCP Servers?

MCP (Model Context Protocol) servers are **plugins for Claude Code**. Out of the box, Claude can read files, run commands, and search the web. MCP servers add new capabilities — browser automation, live documentation lookup, persistent memory, email access, calendar management, and more.

Think of Claude Code as a smartphone. MCP servers are the apps you install.

## How They Work

MCP servers run as background processes that Claude communicates with. You configure them in `~/.claude/settings.json`. When Claude needs a capability (like "search Google Calendar"), it calls the appropriate MCP server.

```
You → Claude Code → MCP Server → External Service → Response back to Claude
```

## Where to Configure

All MCP servers go in `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@some/package"]
    }
  }
}
```

## The Essential Free Servers (Start Here)

These cost nothing and require no API keys. Install these first:

### 1. Context7 — Live Documentation

```json
"context7": {
  "command": "npx",
  "args": ["-y", "@upstash/context7-mcp"]
}
```

**What it does:** Fetches current documentation for any library, framework, or SDK. When you ask about React, Next.js, Tailwind, Prisma — Context7 pulls the latest docs instead of relying on Claude's training data (which may be outdated).

**Why you need it:** Libraries change fast. React 19, Next.js 15, new Tailwind classes — Claude's training data can't keep up. Context7 ensures you get current syntax, not last year's patterns.

**When it triggers:** Automatically when you ask about any library or framework, even well-known ones.

### 2. Sequential Thinking — Better Reasoning

```json
"sequential-thinking": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
}
```

**What it does:** Gives Claude a structured scratchpad for complex reasoning. Instead of rushing to an answer, it breaks problems into steps.

**Why you need it:** For debugging, architecture decisions, and multi-step planning, sequential thinking produces significantly better results. It's the difference between "let me just try something" and "let me think through the implications first."

**When to use:** Claude uses it automatically for non-trivial reasoning. You can also prompt it: "Think through this step by step."

### 3. Playwright — Browser Automation

```bash
# Install globally first
npm install -g @anthropic-ai/mcp-playwright
```

```json
"playwright": {
  "command": "npx",
  "args": ["@anthropic-ai/mcp-playwright"]
}
```

**What it does:** Claude can control a real browser — navigate pages, click buttons, fill forms, take screenshots, read page content.

**Why you need it:** Testing your web apps, scraping data, automating browser tasks, taking screenshots for debugging. Instead of manually checking if your dashboard looks right, tell Claude "open localhost:3000 and screenshot the dashboard."

### 4. Chrome DevTools — Debug Running Apps

```json
"chrome-devtools": {
  "command": "npx",
  "args": ["chrome-devtools-mcp@latest", "--isolated=true"]
}
```

**What it does:** Connects Claude to Chrome's DevTools protocol. Claude can inspect elements, read console logs, monitor network requests, and debug JavaScript in real time.

**Why you need it:** When your dashboard has a weird bug, Claude can open DevTools and debug it the same way you would — but faster.

### 5. Memory (Knowledge Graph) — Persistent Memory

```json
"memory": {
  "command": "npx",
  "args": ["-y", "mcp-knowledge-graph", "--memory-path", "~/.claude/memory/knowledge-graph.jsonl"]
}
```

**What it does:** Stores entities and relationships in a structured graph. Claude can save facts about your projects, preferences, and discoveries, then recall them later.

**Why you need it:** Claude Code has built-in auto-memory (markdown files), but the knowledge graph adds structured relationships. "Project X uses Supabase" + "Supabase is hosted at project-xyz.supabase.co" = Claude can connect the dots.

> **Built-in auto-memory vs. Knowledge Graph:**
> - Auto-memory: Automatic, human-readable markdown, per-project
> - Knowledge Graph: Manual/structured, entity-relationship model, global
> - Use both. Auto-memory for preferences and context. Knowledge graph for structured project data.

## Servers Worth Adding Later (Need API Keys)

Don't install these until you need them:

| Server | What It Does | When to Add |
|--------|-------------|-------------|
| Gmail MCP | Read/search/draft emails from Claude | When you want email automation |
| Google Calendar MCP | Create events, check availability | When you want scheduling from CLI |
| Notion MCP | Read/write Notion pages | If you use Notion |
| Slack MCP | Read/send Slack messages | If you use Slack |
| Firecrawl | Advanced web scraping | When you need to scrape websites |
| Exa | AI-powered web search | When built-in search isn't enough |

## Important: Don't Over-Install

Every MCP server:
- Adds to Claude's system prompt (burns context tokens)
- Runs as a background process
- Can slow down startup

**Start with the 5 free essentials.** Add more only when you have a specific need. You can always install later.

## Testing Your Setup

After adding servers to `settings.json`, restart Claude Code and verify:

```
> "What MCP servers do you have access to?"
```

Claude will list all connected servers and their tools.

## What's Next

Now let's create reusable commands with skills → [04 — Skills & Custom Commands](04-skills.md)
