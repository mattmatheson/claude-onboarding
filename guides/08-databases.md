# 08 — Databases

> **Goal:** Know when to use Supabase, local Postgres, or a graph database — and set up the right one for your project.

## Why You Need a Database

JSON files and local state work fine for prototypes. But once you need:
- Data that persists between sessions
- Multiple users accessing the same data
- Search, filtering, sorting at scale
- Real-time updates
- Authentication

...you need a database.

## The Three Options

### Option 1: Supabase (Start Here)

**What it is:** Hosted PostgreSQL with a dashboard, authentication, file storage, and real-time subscriptions built in. Think "Firebase but with a real database."

**When to use it:**
- Web apps other people will use
- You want auth without building it yourself
- You want a visual dashboard to see your data
- You don't want to manage servers
- You need file storage (images, documents)

**Setup:**
1. Sign up at [supabase.com](https://supabase.com) (free tier: 500MB database + 1GB file storage)
2. Create a new project
3. Grab your project URL and anon key from Settings → API
4. Add to your `.env.local`:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGci...
   ```

**What you get for free:**
- PostgreSQL database with SQL editor
- Row-level security (per-user data access)
- Authentication (email, Google, GitHub login)
- File storage (images, PDFs, etc.)
- Real-time subscriptions (live updates)
- REST API auto-generated from your tables
- Dashboard to manage everything visually

> **Why Supabase over Firebase:** Supabase uses PostgreSQL — a real relational database with SQL. Firebase uses a document store that gets weird fast with complex queries. Supabase also lets you do full SQL — JOINs, aggregations, views — which matters for dashboards.

### Option 2: Local PostgreSQL

**What it is:** PostgreSQL running on your own machine.

**When to use it:**
- Large datasets (10GB+) where cloud costs add up
- AI/vector search with pgvector
- Development/testing before deploying to Supabase
- You want full control

**Setup:**
```bash
# Install
brew install postgresql@16

# Start the service
brew services start postgresql@16

# Create a database
createdb my_project

# Connect
psql my_project
```

**pgvector (AI embeddings):**
```bash
brew install pgvector
```
```sql
CREATE EXTENSION vector;
CREATE TABLE documents (
  id serial PRIMARY KEY,
  content text,
  embedding vector(1536)  -- OpenAI embedding dimension
);
```

> **Why pgvector matters:** It lets you store AI embeddings and do semantic search. "Find documents similar to this query" instead of exact keyword matching. Essential for RAG (Retrieval Augmented Generation) systems.

### Option 3: Neo4j (Graph Database)

**What it is:** A database that stores nodes (things) and relationships (connections between things).

**When to use it:**
- Relationship-heavy data (who knows who, what depends on what)
- Knowledge graphs
- Network analysis
- Recommendation engines

**Setup:**
```bash
brew install neo4j
neo4j start
# Dashboard at http://localhost:7474
```

**Example query (Cypher, not SQL):**
```cypher
CREATE (matt:Person {name: 'Matt'})
CREATE (braden:Person {name: 'Braden'})
CREATE (matt)-[:KNOWS]->(braden)
CREATE (matt)-[:USES]->(claude:Tool {name: 'Claude Code'})
CREATE (braden)-[:USES]->(claude)
RETURN matt, braden, claude
```

> **When SQL isn't enough:** SQL is great for tables. But "find all people who know someone who worked on a project that uses the same tech stack" requires multiple JOINs in SQL and is one line in Cypher. If your data is about connections, use a graph database.

## Decision Matrix

| Scenario | Use This |
|----------|----------|
| First project, want easy setup | Supabase |
| Web app with user login | Supabase |
| Dashboard pulling business data | Supabase |
| AI/vector search, embeddings | Local Postgres + pgvector |
| Big dataset, no budget for cloud | Local Postgres |
| Mapping relationships/networks | Neo4j |
| Just experimenting | Supabase (free tier) |

## Connecting to Your Dashboard

### Supabase + Next.js
```bash
npm install @supabase/supabase-js
```
```typescript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)

// Fetch data
const { data, error } = await supabase
  .from('invoices')
  .select('*')
  .order('created_at', { ascending: false })
```

### Local Postgres + Next.js
```bash
npm install pg
```
Use in API routes (server-side only — never expose database credentials to the browser).

## What's Next

Got multiple computers? Let's connect them → [09 — Multi-Machine Setup](09-multi-machine.md)
