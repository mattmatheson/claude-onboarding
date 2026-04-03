# 07 — Building Dashboards & Websites

> **Goal:** Use Claude Code to build production-quality dashboards and websites — not generic AI slop, but things that actually look good.

## The Problem with AI-Generated UI

By default, AI tends to produce generic, template-looking websites. Same blue buttons, same card layouts, same "hero section with gradient." You can do way better with the right approach.

## The Stack

For dashboards and websites, this stack gives you the most flexibility with the least friction:

- **Next.js** (App Router) — React framework with server components, routing, API routes
- **Tailwind CSS** — Utility-first CSS, fast iteration, no fighting stylesheets
- **shadcn/ui** — Copy-paste component library (not a dependency — you own the code)
- **Vercel** — Deploy with `git push`, zero config

### Why This Stack?

**Next.js App Router** — Server components mean less JavaScript shipped to the browser. API routes mean your backend lives next to your frontend. One framework, full stack.

**Tailwind** — Claude is exceptionally good at Tailwind. It can iterate on designs fast because every style is inline — no jumping between CSS files. What you see in the JSX is what you get.

**shadcn/ui** — Unlike component libraries that hide code behind `node_modules`, shadcn copies components into your project. You can customize everything. Claude can read and modify the actual component code.

**Vercel** — The fastest path from code to URL. Connect your GitHub repo, every push auto-deploys. Free tier is generous.

## Starting a New Project

```bash
npx create-next-app@latest my-dashboard --typescript --tailwind --app --src-dir
cd my-dashboard

# Add shadcn/ui
npx shadcn@latest init

# Add components as needed
npx shadcn@latest add button card table chart
```

## Working with Claude on UI

### The Interview Approach

Don't just say "build me a dashboard." Have Claude interview you:

```
I want to build a dashboard for [purpose]. Before writing code:
1. Ask me what data/metrics I want to display
2. Ask about my brand aesthetic (colors, vibe, reference sites)
3. Suggest a layout with sections
4. Get my approval before writing code
```

### Design Principles to Enforce

Add these to your CLAUDE.md or project CLAUDE.md:

```markdown
## Design Rules
- No generic AI aesthetics — no gradient hero sections, no stock illustration style
- Typography hierarchy matters — clear H1 > H2 > body > caption sizing
- Whitespace is a feature, not wasted space
- Responsive by default — test at mobile, tablet, desktop
- Real data over placeholder "Lorem ipsum" where possible
- Dark mode support from the start
- Subtle animations (hover states, transitions) — nothing flashy
```

### Using Design Skills

If you've installed design-focused skills (see Guide 04), invoke them:

```
/frontend-design Build a client invoicing dashboard with sidebar nav, 
invoice list, and monthly revenue chart
```

Skills like `frontend-design`, `high-end-visual-design`, and `design-taste-frontend` encode specific design rules that push Claude beyond generic output.

## Dashboard Patterns

### Data Dashboard
```
Components: Stat cards (KPIs), charts (line/bar/pie), data tables, filters
Layout: Sidebar nav + main content area
Data: API routes fetching from Supabase or local JSON
```

### Client Portal
```
Components: Project list, status badges, file uploads, messaging
Layout: Top nav + card grid
Auth: NextAuth.js or Supabase Auth
```

### Personal Command Center
```
Components: Calendar widget, task list, quick links, recent activity
Layout: Grid-based, widget style
Data: Pull from multiple APIs (calendar, email, tasks)
```

## Deploying to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy (first time — follow prompts)
vercel

# After that, every git push auto-deploys
git push origin main
```

### Environment Variables on Vercel
Never commit `.env` files. Add secrets through the Vercel dashboard:
- Project Settings → Environment Variables
- Add each key/value pair
- Redeploy for changes to take effect

## Iteration Workflow

The best workflow for building with Claude:

1. **Describe what you want** (layout, data, aesthetic)
2. **Claude builds v1** (working but rough)
3. **You review** (open localhost:3000, look at it)
4. **Give feedback** ("the chart is too wide," "make the sidebar collapsible," "the cards need more padding")
5. **Claude iterates** (targeted changes, not rewrites)
6. **Repeat** until it's right

> **Pro tip:** Take screenshots and share them with Claude if you're struggling to describe what's wrong. Playwright can also screenshot for you: "take a screenshot of localhost:3000 and tell me what you think."

## What's Next

Dashboards need data. Let's set up databases → [08 — Databases](08-databases.md)
