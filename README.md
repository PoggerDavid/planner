# Today — AI Planner

A personal AI planner that syncs across devices. Powered by Claude via OpenRouter.

## Setup

### 1. Create a Supabase project
- Go to [supabase.com](https://supabase.com) and create a free account
- Create a new project

### 2. Create the database tables
Go to the **SQL Editor** in your Supabase dashboard and run this:

```sql
-- Config table (stores your API key and settings)
CREATE TABLE config (
  id TEXT PRIMARY KEY DEFAULT 'main',
  pin_hash TEXT,
  api_key TEXT,
  model TEXT DEFAULT 'anthropic/claude-haiku-4.5',
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- Sessions table (stores your chat history and plans)
CREATE TABLE sessions (
  id TEXT PRIMARY KEY,
  title TEXT,
  type TEXT,
  date_label TEXT,
  messages JSONB DEFAULT '[]'::jsonb,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- Allow public access (since this is a personal app with PIN protection)
ALTER TABLE config ENABLE ROW LEVEL SECURITY;
ALTER TABLE sessions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Allow all on config" ON config FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "Allow all on sessions" ON sessions FOR ALL USING (true) WITH CHECK (true);
```

### 3. Get your Supabase credentials
Go to **Settings → API** in your Supabase dashboard and copy:
- **Project URL** (e.g. `https://xxxxx.supabase.co`)
- **anon public key** (starts with `eyJ...`)

### 4. Get an OpenRouter API key
- Go to [openrouter.ai](https://openrouter.ai)
- Create an account and add credits
- Generate an API key

### 5. Deploy to GitHub Pages
1. Create a new GitHub repository
2. Push these files to the repo
3. Go to **Settings → Pages** in the repo
4. Set source to "Deploy from a branch" → select `main` → root folder
5. Your site will be live at `https://yourusername.github.io/reponame`

### 6. Auto-deploy
Every time you push changes to GitHub, the site auto-updates within a minute.

## Quick deploy commands

```bash
# First time
git init
git add .
git commit -m "initial"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/YOURREPO.git
git push -u origin main

# Future updates
git add .
git commit -m "update"
git push
```

## Features
- PIN login (no account needed)
- Chat with AI to create day/week/month plans
- Visual plan cards with time blocks
- Plans dashboard to see all your plans
- Syncs across phone and laptop via Supabase
- Smart memory: compresses old messages to save tokens
- Model switching: Haiku ($1/MTok), Sonnet ($3/MTok), Opus ($5/MTok)
- Mobile-friendly with bottom nav
- Works as a PWA (add to home screen on iPhone)
