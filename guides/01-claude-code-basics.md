# 01 — Claude Code Basics

> **Goal:** Get comfortable navigating Claude Code like a power user, not just someone typing prompts.

## What Is Claude Code?

Claude Code is Anthropic's official CLI tool. It's not a chatbot — it's an **agent**. It can read and write files, run terminal commands, search the web, control browsers, manage git repos, and automate workflows. Think of it as a senior developer sitting in your terminal.

## Key Shortcuts

Learn these first. They'll save you hours:

| Shortcut | What It Does |
|----------|-------------|
| `Enter` | Send message |
| `Shift+Enter` | New line (multi-line prompts) |
| `Shift+Tab` | Cycle permission modes |
| `Escape` | Clear current input |
| `Escape, Escape` | Rollback menu (undo actions) |
| `/` | Open slash command menu |

## Permission Modes

Claude has four permission levels. Cycle through them with `Shift+Tab`:

### 1. Normal Mode (default)
Claude asks permission before running commands, editing files, etc. Good for learning — you see everything before it happens.

### 2. Auto-Accept Mode
Auto-approves safe operations (reading files, running tests). Still asks for destructive stuff. Good daily driver once you trust the workflow.

### 3. Plan Mode
Read-only. Claude can look at your code but can't change anything. **Use this for learning** — ask Claude to explain code, plan architecture, or review your work without risk of changes.

### 4. YOLO Mode (`claude --dangerously-skip-permissions`)
Fully autonomous. Claude does whatever it needs without asking. Only use this when:
- You're in a sandboxed environment
- You're running a well-understood task (like a build script)
- You trust the outcome

> **Why this matters:** Permission modes let you dial the trust level. Start in Normal, graduate to Auto-Accept as you learn what Claude does with each tool. Plan Mode is your safe space for exploration.

## Conversation Management

### Rollback / Rewind
Made a mistake? Hit `Escape, Escape` to open the rollback menu. You can undo any action Claude took — file edits, commands, all of it.

### Branching
Use `/branch` to fork a conversation. Try an experimental approach without losing your current progress. If it doesn't work, switch back.

### Resume
`claude --resume` picks up your last conversation. Context is preserved — Claude remembers what you were working on.

### Compact
Conversations get long. `/compact` summarizes the history to free up context window space without losing the thread.

## Slash Commands

Type `/` to see available commands. Key ones:

- `/help` — Get help
- `/memory` — View/manage Claude's memory about you
- `/compact` — Compress conversation history
- `/branch` — Fork the conversation
- `/commit` — Smart git commit with analysis

## Tips for Getting the Most Out of Claude

1. **Be specific.** "Fix the login bug" is vague. "The login form on /auth/login returns a 500 when the email field is empty — fix the validation" is actionable.

2. **Use multi-line prompts.** `Shift+Enter` for new lines. Give Claude context, then the task:
   ```
   I'm building a dashboard with Next.js and Tailwind.
   The sidebar component is at src/components/Sidebar.tsx.
   Add a collapsible section for "Settings" with three sub-items.
   ```

3. **Let Claude read before writing.** Don't describe your code — let Claude read the actual files. "Read src/app/page.tsx and then add a loading spinner" is better than explaining the component structure yourself.

4. **Course-correct early.** If Claude starts going in the wrong direction, interrupt. Don't wait for it to finish a wrong approach.

5. **Use Plan Mode for learning.** Switch to Plan Mode and ask "explain how this codebase works" or "what would you change about this architecture?" Zero risk, maximum learning.

## What's Next

Now that you know the controls, let's give Claude some permanent instructions about who you are and how you work → [02 — Your First CLAUDE.md](02-claude-md.md)
