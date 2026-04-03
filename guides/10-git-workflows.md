# 10 — Git & GitHub Workflows

> **Goal:** Use git and GitHub effectively with Claude Code — branches, PRs, issues, and the `gh` CLI.

## Why Git Matters with Claude

Claude Code is deeply integrated with git. It reads your commit history for context, creates branches, writes commit messages, opens PRs, and reviews diffs. The better your git workflow, the more Claude can help.

## The `gh` CLI

GitHub's CLI tool. Claude uses it for everything GitHub-related:

```bash
# Install
brew install gh

# Authenticate
gh auth login
```

### Essential Commands

```bash
# Repos
gh repo create my-project --public
gh repo clone owner/repo

# Issues
gh issue list
gh issue create --title "Bug: login broken" --body "Steps to reproduce..."
gh issue view 42

# Pull Requests
gh pr create --title "Add dashboard" --body "Summary of changes"
gh pr list
gh pr view 15
gh pr merge 15

# Actions (CI/CD)
gh run list
gh run view 12345
```

## The Branch Workflow

Never commit directly to `main`. Always:

1. **Create a branch** for each feature/fix
2. **Make changes** on the branch
3. **Open a PR** for review
4. **Merge** when ready

```bash
# Create and switch to a new branch
git checkout -b feature/add-dashboard

# Work with Claude, make changes...

# Push the branch
git push -u origin feature/add-dashboard

# Create a PR
gh pr create --title "Add client dashboard" --body "Adds the main dashboard with KPIs and charts"
```

> **Why branches matter:** If something goes wrong, `main` is untouched. You can abandon a branch without consequences. And PRs create a clear history of what changed and why.

## Commit Messages

Good commit messages are documentation. Claude writes them well if you use the `/commit` skill:

**Good:**
```
Add revenue chart to dashboard

The client dashboard was missing monthly revenue visualization.
Added a line chart component using recharts that pulls data
from the invoices table.
```

**Bad:**
```
update stuff
```

### The `/commit` Workflow

```
> /commit
```

Claude will:
1. Run `git diff` to see all changes
2. Analyze what changed and why
3. Write a descriptive commit message
4. Stage relevant files and commit

## .gitignore

Always have a `.gitignore`. Critical entries:

```gitignore
# Dependencies
node_modules/

# Environment variables (NEVER commit these)
.env
.env.local
.env.production

# OS files
.DS_Store
Thumbs.db

# Build output
.next/
dist/
build/

# IDE
.vscode/settings.json
.idea/
```

> **The #1 rule:** Never commit `.env` files. They contain API keys, database passwords, and secrets. Once pushed to GitHub, they're in the git history forever — even if you delete the file later.

## Undoing Mistakes

```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Discard all uncommitted changes (CAREFUL)
git checkout .

# Revert a specific commit (safe — creates a new commit)
git revert abc1234
```

## What's Next

Let's lock down security → [11 — Security & Secrets](11-security.md)
