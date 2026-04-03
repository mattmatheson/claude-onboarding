# 04 — Skills & Custom Commands

> **Goal:** Create reusable slash commands that encode your best workflows into one-word triggers.

## What Are Skills?

Skills are markdown files that become slash commands. Instead of typing a long prompt every time, you create a skill file and invoke it with `/skill-name`.

Example: Instead of typing "analyze all staged and unstaged git changes, write a clear commit message that explains why not what, and commit" every time — you just type `/commit`.

## Why Skills Matter

1. **Consistency** — Same quality output every time, not dependent on how well you prompt
2. **Speed** — One word instead of a paragraph
3. **Knowledge capture** — Your best prompts are preserved, not lost in conversation history
4. **Shareability** — Drop a skill file in a repo and your whole team gets it

## Where Skills Live

### Global Skills (available everywhere)
```
~/.claude/commands/skill-name.md
```

### Project Skills (available in one repo)
```
your-project/.claude/commands/skill-name.md
```

## Anatomy of a Skill

A skill is just a markdown file with instructions. The filename (minus `.md`) becomes the slash command.

**Example: `~/.claude/commands/commit.md`**
```markdown
Analyze all staged and unstaged changes using git diff and git status.
Write a clear, concise commit message that:
- Summarizes the nature of the changes (new feature, bug fix, refactor, etc.)
- Focuses on WHY, not just what changed
- Is 1-2 sentences max

Stage relevant files and create the commit. Never commit .env files or credentials.
```

Now `/commit` does all of that automatically.

## Essential Starter Skills

### `/yolo` — Auto-Approve Mode
```markdown
# ~/.claude/commands/yolo.md
Auto-approve all safe operations. Do not ask for permission for:
- Reading files
- Writing/editing code files
- Running build commands, tests, linters
- Git operations (add, commit, branch, checkout)
- Installing npm packages

Still ask before: deleting files, force-pushing, running destructive commands.
```

### `/safe` — Maximum Caution
```markdown
# ~/.claude/commands/safe.md
Ask permission before every action — file edits, shell commands, git operations, everything.
Explain what you're about to do and why before each action.
```

### `/review` — Code Review
```markdown
# ~/.claude/commands/review.md
Review the current git diff (staged + unstaged changes) for:
1. Bugs or logic errors
2. Security issues (hardcoded secrets, injection vectors, XSS)
3. Performance concerns
4. Code style inconsistencies
5. Missing error handling at system boundaries

Be direct. If it looks good, say so. If there are issues, list them with file:line references.
```

### `/explain` — Codebase Explorer
```markdown
# ~/.claude/commands/explain.md
Switch to read-only mode. Do not make any changes.

The user wants to understand this codebase. Start by:
1. Reading the project structure (package.json, key directories)
2. Identifying the tech stack
3. Explaining the architecture in plain terms
4. Offering to deep-dive into any specific area

Explain like a senior dev onboarding a new team member.
```

### `/dashboard` — Build a Dashboard
```markdown
# ~/.claude/commands/dashboard.md
Help build or improve a dashboard. Before writing code:
1. Ask what data/metrics to display
2. Ask about the tech stack (default: Next.js + Tailwind + shadcn/ui)
3. Suggest a layout with sections

Design principles:
- Clean, minimal UI — no clutter
- Responsive by default
- Use proper typography hierarchy
- Real data, not placeholder text where possible
- Dark mode support
```

## Creating Your Own Skills

Think about tasks you repeat. Good candidates for skills:

- **Deployment:** Check build, run tests, deploy to Vercel
- **New component:** Create a component with proper types, tests, and Storybook
- **Debug:** Read error logs, trace the issue, suggest fixes
- **Write tests:** Generate tests for the current file
- **Standup:** Summarize yesterday's git commits into a standup update

### Skill Writing Tips

1. **Be specific.** Vague skills produce vague results.
2. **Include constraints.** "Never add comments unless the logic is non-obvious."
3. **Define the output.** "Create the file at X" or "Output a markdown summary."
4. **Include the why.** Claude makes better decisions when it understands your reasoning.

## Skill Management

Over time you'll accumulate skills. Some tips:

- **Archive rarely-used skills.** Every skill in `~/.claude/commands/` loads into every conversation, burning context tokens. Move seasonal or niche skills to a `~/.claude/skills-archive/` folder and load them on demand.
- **Version control your skills.** Keep them in a git repo. They're code — treat them like it.
- **Share with your team.** Drop project-level skills in `.claude/commands/` and commit them.

## What's Next

Skills are manual triggers. Hooks are automatic triggers → [05 — Hooks & Automation](05-hooks.md)
