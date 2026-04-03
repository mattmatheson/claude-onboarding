# 13 — Productivity Workflows

> **Goal:** See how all the pieces come together for daily work — email, calendar, web scraping, file management, and automation.

## The Daily Driver Setup

At this point you've got:
- Claude Code with MCP servers (browser, docs, memory)
- CLAUDE.md with your preferences
- Obsidian vault for context
- Skills for common tasks
- Git workflows for code

Now let's use all of it together.

## Email Integration

### Setup
Add the Gmail MCP server (requires Google OAuth):
```json
"gmail": {
  "command": "npx",
  "args": ["-y", "@anthropic-ai/mcp-gmail"]
}
```

### What You Can Do
```
> "Search my email for invoices from last month"
> "Draft a reply to the latest email from [client]"
> "Show me unread emails from today"
```

> **Why this matters:** You're in the terminal, deep in a coding session. A client emails. Instead of context-switching to Gmail, you handle it in Claude and get back to work.

## Calendar Integration

### What You Can Do
```
> "What's on my calendar tomorrow?"
> "Schedule a meeting with [name] for Friday at 2pm"
> "Find a free 1-hour slot this week"
```

> **The compound effect:** "Read the email from [client] about the project kickoff, then create a calendar event for the meeting time they mentioned, and add a note in my Obsidian vault under projects/[client]."
> 
> One prompt. Three systems. Zero context switching.

## Web Scraping with Playwright

With the Playwright MCP server, Claude can browse the web for you:

```
> "Go to [competitor's website] and screenshot their pricing page"
> "Scrape the documentation from [API docs URL] and save it to my vault"
> "Fill out the form on localhost:3000/contact with test data"
```

### Practical Uses
- **Research** — Scrape competitor features, pricing, design patterns
- **Testing** — Automate form fills, click through flows, catch visual bugs
- **Data collection** — Pull structured data from web pages into JSON

## File Management

Claude can organize your files:

```
> "Find all PDF invoices in ~/Downloads and move them to ~/Documents/Invoices/2024/"
> "List all files over 100MB on my desktop"
> "Rename all screenshots in ~/Desktop to include today's date"
```

## Building Automations

### Cron Jobs + Claude

Schedule recurring tasks:
```bash
# Add to crontab (crontab -e)
# Every morning at 8am, generate a daily brief
0 8 * * * cd ~/projects && claude --print "Give me a summary of yesterday's git commits across all my repos" > ~/Vault/daily/$(date +\%Y-\%m-\%d)-morning.md
```

### Claude as a Pipeline

Chain operations:
```
> "Read the CSV at ~/data/clients.csv, create a Supabase table with the right columns, and import all the data"
```

```
> "Take a screenshot of my dashboard at localhost:3000, analyze the layout, and suggest three specific improvements"
```

```
> "Read my Obsidian daily notes from this week, summarize what I accomplished, and draft a weekly update email"
```

## Workflow Patterns

### The Morning Routine
```
> "Check my calendar for today, check email for anything urgent, 
   and update my Obsidian daily note with the plan for today"
```

### The Code Review
```
> /review
> "Also check the PR comments on GitHub and address them"
```

### The End-of-Day
```
> "Summarize what we did today, commit any uncommitted work, 
   and write a session summary to my Obsidian vault"
```

### The Client Delivery
```
> "Build the project, deploy to Vercel, take a screenshot of the live site, 
   and draft an email to [client] with the deployment URL"
```

## The Meta-Skill: Teaching Claude Your Workflows

The most powerful thing you can do is **teach Claude your patterns.** When you find a workflow you repeat:

1. Do it manually once with Claude
2. Tell Claude: "Turn what we just did into a skill"
3. Claude creates a skill file encoding the workflow
4. Next time, one slash command does the whole thing

This is how a setup evolves from "I use Claude for coding" to "Claude runs half my business."

## What's Next

You've completed the guide. Here's what to do now:

### Immediate (Today)
- [ ] Set up CLAUDE.md with your preferences
- [ ] Install the 5 free MCP servers
- [ ] Create 2-3 starter skills (`/commit`, `/review`, `/yolo`)
- [ ] Install Obsidian and create your vault

### This Week
- [ ] Build your first dashboard with Claude
- [ ] Connect Supabase for data
- [ ] Get comfortable with git workflows

### This Month
- [ ] Set up Tailscale if you have multiple machines
- [ ] Add email/calendar MCP servers
- [ ] Build up your Obsidian vault with project notes
- [ ] Create custom skills for your repeated workflows

### Ongoing
- [ ] Review and clean up Claude's memory monthly
- [ ] Update CLAUDE.md as preferences evolve
- [ ] Archive unused skills to reduce token burn
- [ ] Share skills with your team/friends

---

**Welcome to the club. You're not just using AI — you're building a system that gets smarter every day.**
