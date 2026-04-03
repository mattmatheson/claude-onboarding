# 11 — Security & Secrets

> **Goal:** Never leak an API key, never expose credentials, and understand how to vet the tools you install. This is the guide you'll be glad you read before it was too late.

## The Stakes

One leaked API key can:
- Run up thousands of dollars on your cloud accounts
- Expose your users' data
- Give attackers access to your email, databases, and services
- Get your accounts suspended

GitHub has bots that scan every public commit for API keys. Within **minutes** of pushing a key, someone will try to use it. This is not hypothetical — it happens constantly.

## Rule #1: Never Hardcode Secrets

**Wrong:**
```typescript
const supabase = createClient(
  'https://abc123.supabase.co',
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'  // EXPOSED
)
```

**Right:**
```typescript
const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)
```

Secrets go in `.env` files that are **never committed to git**.

## Setting Up .env Files

### Create the file
```bash
# In your project root
touch .env.local
```

### Add your secrets
```bash
# .env.local
NEXT_PUBLIC_SUPABASE_URL=https://abc123.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGci...
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
OPENAI_API_KEY=sk-...
```

### Make sure it's gitignored
Your `.gitignore` MUST include:
```gitignore
.env
.env.local
.env.production
.env*.local
```

### Verify it's not tracked
```bash
git status
# .env.local should NOT appear in the output
# If it does, you need to untrack it:
git rm --cached .env.local
```

## macOS Keychain — System-Level Security

For secrets you use across multiple projects (API keys, passwords), macOS Keychain is more secure than `.env` files scattered across your filesystem.

### Storing a Secret
```bash
# Add a secret to Keychain
security add-generic-password -a "$USER" -s "OPENAI_API_KEY" -w "sk-your-key-here"
```

### Retrieving a Secret
```bash
# Get it back
security find-generic-password -a "$USER" -s "OPENAI_API_KEY" -w
```

### Using in Scripts
```bash
export OPENAI_API_KEY=$(security find-generic-password -a "$USER" -s "OPENAI_API_KEY" -w)
```

### Using in .env Files (Reference Pattern)
Instead of putting the actual key in `.env`, reference the Keychain:
```bash
# In a startup script or shell profile
export SUPABASE_KEY=$(security find-generic-password -a "$USER" -s "SUPABASE_KEY" -w)
```

> **Why Keychain over .env?** A `.env` file is a plain text file on disk. Anyone (or any malware) with file access can read it. Keychain is encrypted, requires your login password, and integrates with macOS security. For your most sensitive keys (production database, payment APIs), use Keychain.

## 1Password CLI (Advanced)

If you use 1Password, the CLI gives you even more:

```bash
# Install
brew install --cask 1password-cli

# Reference secrets without exposing them
export DB_PASSWORD="op://vault-name/database/password"

# Run a command with secrets injected
op run -- npm start
```

The `op://` reference pattern means the actual secret never touches your filesystem.

## Vetting MCP Servers

MCP servers are npm packages that run on your machine with access to your files, network, and potentially your secrets. **Not all of them are safe.**

### Before Installing an MCP Server, Check:

1. **Source code** — Is it open source? Can you read what it does?
2. **Author** — Is it from a known organization (Anthropic, Vercel) or a random GitHub user?
3. **Stars/downloads** — Has the community vetted it?
4. **Permissions** — What does it access? Does a "time" server need network access?
5. **Dependencies** — Run `npm audit` after installing

### Red Flags
- Obfuscated/minified source code
- Requests permissions beyond its purpose
- No README or documentation
- Very few downloads or stars
- Makes network requests to unknown servers

### Safe Defaults
These are well-vetted:
- `@modelcontextprotocol/server-*` (official Anthropic)
- `@anthropic-ai/*` (official Anthropic)
- `@upstash/context7-mcp` (Upstash — known company)
- `chrome-devtools-mcp` (active community)

## Vetting Skills

Skills are markdown files — they can't execute code directly. But they **instruct Claude to execute code**. A malicious skill could tell Claude to:
- Read and exfiltrate your `.env` files
- Install compromised packages
- Modify your code to include backdoors

### Before Using Someone Else's Skill:
1. **Read the entire file** — it's just markdown, takes 30 seconds
2. **Check for suspicious instructions** — "read all .env files," "curl to external URL," "download and run this script"
3. **Understand what it tells Claude to do** — if it's more than what you'd do manually, question it

## HTTPS Everywhere

When your dashboards or apps talk to APIs:
- Always use `https://`, never `http://`
- Verify SSL certificates aren't disabled in your code
- Never trust user input — sanitize everything

## Environment Variables: Public vs Private

In Next.js:
- `NEXT_PUBLIC_*` — Exposed to the browser (anyone can see these)
- Everything else — Server-side only (hidden from users)

```bash
# SAFE to be public (it's just an identifier)
NEXT_PUBLIC_SUPABASE_URL=https://abc.supabase.co

# MUST be private (grants access)
SUPABASE_SERVICE_ROLE_KEY=eyJhbGci...
DATABASE_URL=postgresql://...
OPENAI_API_KEY=sk-...
```

> **The rule:** If a key grants write access, admin access, or costs money to use, it's PRIVATE. Server-side only. Never prefix with `NEXT_PUBLIC_`.

## Production Secrets on Vercel

When deploying, add secrets through the Vercel dashboard:
1. Project Settings → Environment Variables
2. Add each key/value pair
3. Set scope (Production, Preview, Development)
4. Redeploy for changes to take effect

**Never** put production secrets in your repo, even in a "deploy script."

## Security Checklist

Before every deploy, verify:
- [ ] `.env` files are in `.gitignore`
- [ ] No secrets in git history (`git log --all --full-history -- .env*`)
- [ ] `NEXT_PUBLIC_` prefix only on truly public values
- [ ] No hardcoded API keys in source code
- [ ] MCP servers are from trusted sources
- [ ] `npm audit` shows no critical vulnerabilities
- [ ] Supabase Row Level Security is enabled (if using Supabase)

## If You Accidentally Commit a Secret

**Act immediately:**

1. **Rotate the key** — Generate a new key in the service dashboard. The old one is compromised.
2. **Remove from git history:**
   ```bash
   # Remove file from all history
   git filter-branch --force --index-filter \
     'git rm --cached --ignore-unmatch .env' \
     --prune-empty --tag-name-filter cat -- --all
   git push --force
   ```
3. **Check for unauthorized usage** — Review the service's access logs
4. **Update your `.gitignore`** — Make sure it can't happen again

> **Important:** Simply deleting the file and making a new commit does NOT remove it from history. The secret is still in older commits. You must rewrite history or use a tool like `git-filter-repo`.

## What's Next

Let's teach Claude to remember you → [12 — Memory & Context](12-memory.md)
