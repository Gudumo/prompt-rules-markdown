# Project Setup

> This file is used **once** when initializing a new project. After setup, the LLM reads `PROMPT_RULES.md` and (for frontend steps) `FRONTEND_RULES.md` instead.

## Tech Stack

> **Instructions:** Delete every option you are NOT using. Keep only your actual stack. Check the box for your selection.

### Framework

<!-- Keep one, delete the rest -->
- [ ] **React** (CRA / Vite)
- [ ] **Next.js** (App Router)
- [ ] **Next.js** (Pages Router)
- [ ] **Remix**
- [ ] **Astro**
- [ ] **Svelte / SvelteKit**
- [ ] **Vue 3** (Vite)
- [ ] **Nuxt 3**
- [ ] **Angular**
- [ ] **Solid.js / SolidStart**
- [ ] **Qwik / Qwik City**
- [ ] **Gatsby**
- [ ] **Eleventy (11ty)**
- [ ] **Plain HTML / Vanilla JS**

### Language

<!-- Keep one, delete the rest -->
- [ ] **TypeScript** (strict mode)
- [ ] **TypeScript** (loose / no strict)
- [ ] **JavaScript** (ESM)
- [ ] **JavaScript** (CommonJS)

### Styling

<!-- Keep one or combine, delete the rest -->
- [ ] **Tailwind CSS**
- [ ] **CSS Modules**
- [ ] **Styled Components**
- [ ] **Emotion**
- [ ] **Sass / SCSS**
- [ ] **Vanilla CSS**
- [ ] **UnoCSS**
- [ ] **Panda CSS**

### UI Component Library

<!-- Keep one or none, delete the rest -->
- [ ] **shadcn/ui**
- [ ] **Radix UI** (unstyled primitives)
- [ ] **Headless UI**
- [ ] **MUI (Material UI)**
- [ ] **Ant Design**
- [ ] **Chakra UI**
- [ ] **Mantine**
- [ ] **DaisyUI**
- [ ] **PrimeReact / PrimeVue**
- [ ] **None — custom components only**

### State Management

<!-- Keep one or none, delete the rest -->
- [ ] **React Context + useReducer**
- [ ] **Zustand**
- [ ] **Redux Toolkit**
- [ ] **Jotai**
- [ ] **Recoil**
- [ ] **MobX**
- [ ] **Pinia** (Vue)
- [ ] **Svelte stores** (Svelte)
- [ ] **Signals** (Solid / Angular / Preact)
- [ ] **None — props/server state only**

### Data Fetching

<!-- Keep one or combine, delete the rest -->
- [ ] **TanStack Query (React Query)**
- [ ] **SWR**
- [ ] **tRPC**
- [ ] **Apollo Client** (GraphQL)
- [ ] **urql** (GraphQL)
- [ ] **Fetch / Axios** (manual)
- [ ] **Server Actions** (Next.js / Remix)

### Testing (Frontend)

<!-- Keep one or combine, delete the rest -->
- [ ] **Vitest**
- [ ] **Jest**
- [ ] **Testing Library** (React / Vue / Svelte)
- [ ] **Playwright** (E2E)
- [ ] **Cypress** (E2E)
- [ ] **Storybook** (visual / interaction tests)
- [ ] **None — backend tests only**

### Linting & Formatting

<!-- Keep what applies, delete the rest -->
- [ ] **ESLint**
- [ ] **Prettier**
- [ ] **Biome**
- [ ] **Stylelint**
- [ ] **oxlint**

### Package Manager

<!-- Keep one, delete the rest -->
- [ ] **npm**
- [ ] **pnpm**
- [ ] **yarn**
- [ ] **bun**

### Deployment Target

<!-- Keep one or combine, delete the rest -->
- [ ] **Vercel**
- [ ] **Netlify**
- [ ] **Cloudflare Pages / Workers**
- [ ] **AWS (Amplify / S3+CloudFront / Lambda)**
- [ ] **Docker / self-hosted**
- [ ] **GitHub Pages**
- [ ] **Railway / Fly.io**
- [ ] **Firebase Hosting**

### Backend / API

<!-- Keep one or none, delete the rest -->
- [ ] **Node.js (Express)**
- [ ] **Node.js (Fastify)**
- [ ] **Node.js (Hono)**
- [ ] **Next.js API Routes / Route Handlers**
- [ ] **Remix Loaders / Actions**
- [ ] **Deno**
- [ ] **Bun server**
- [ ] **Python (FastAPI)**
- [ ] **Python (Django)**
- [ ] **Python (Flask)**
- [ ] **Go (net/http / Gin / Echo)**
- [ ] **Rust (Actix / Axum)**
- [ ] **Ruby on Rails**
- [ ] **PHP (Laravel)**
- [ ] **Java (Spring Boot)**
- [ ] **C# (.NET / ASP.NET)**
- [ ] **Elixir (Phoenix)**
- [ ] **External API only (no backend)**

### Database

<!-- Keep one or none, delete the rest -->
- [ ] **PostgreSQL**
- [ ] **MySQL / MariaDB**
- [ ] **SQLite**
- [ ] **MongoDB**
- [ ] **Redis**
- [ ] **Supabase**
- [ ] **Firebase / Firestore**
- [ ] **PlanetScale**
- [ ] **Turso (libSQL)**
- [ ] **DynamoDB**
- [ ] **None**

### ORM / Database Client

<!-- Keep one or none, delete the rest -->
- [ ] **Prisma**
- [ ] **Drizzle**
- [ ] **Kysely**
- [ ] **TypeORM**
- [ ] **Sequelize**
- [ ] **Mongoose** (MongoDB)
- [ ] **SQLAlchemy** (Python)
- [ ] **Django ORM** (Python)
- [ ] **GORM** (Go)
- [ ] **Raw SQL / query builder**
- [ ] **None**

### Auth

<!-- Keep one or none, delete the rest -->
- [ ] **NextAuth.js / Auth.js**
- [ ] **Clerk**
- [ ] **Supabase Auth**
- [ ] **Firebase Auth**
- [ ] **Lucia**
- [ ] **Passport.js**
- [ ] **Auth0**
- [ ] **Custom JWT**
- [ ] **None**

---

## Environment Setup

### Required global tools

<!-- Fill in your versions, delete lines that don't apply -->

| Tool | Version | Install |
|------|---------|---------|
| Node.js | `<!-- e.g. 20.x -->` | `.nvmrc` or `nvm install` |
| npm | `<!-- e.g. 10.x -->` | ships with Node |
| pnpm | `<!-- e.g. 9.x -->` | `npm i -g pnpm` |
| yarn | `<!-- e.g. 4.x -->` | `corepack enable` |
| bun | `<!-- e.g. 1.x -->` | `curl -fsSL https://bun.sh/install \| bash` |
| git | `2.x+` | system install |
| tsx | `latest` | `npm i -g tsx` |
| playwright | `latest` | `npx playwright install` (for visual QA screenshots) |

### Node version pinning

Create `.nvmrc` at the repo root:

```
<!-- e.g. 20.18.0 -->
```

And in `package.json`:

```json
{
  "engines": {
    "node": ">=<!-- e.g. 20.0.0 -->"
  }
}
```

The LLM must **never** change the Node version or install a different major version without asking.

### `.env` and secrets

1. The repo MUST include a `.env.example` with all required keys and placeholder values:
   ```
   # API keys
   LLM_API_KEY=your-key-here
   LLM_BASE_URL=http://localhost:1234/v1

   # Server
   PORT=3737

   # Database (if applicable)
   DATABASE_URL=postgresql://user:password@localhost:5432/dbname
   ```
2. `.env` is in `.gitignore` — never committed.
3. `.env.example` IS committed — always kept in sync with actual `.env` keys.
4. When a step adds a new env var, the LLM MUST also add it to `.env.example` and note it in the COMPLETED block.

### `.gitignore` baseline

The repo `.gitignore` must include at minimum:

```
node_modules/
dist/
.env
.env.local
.env.*.local
*.log
.DS_Store
Thumbs.db
.dev-plan/design-reference/screenshots/
```

The LLM must check `.gitignore` before creating any file that could contain secrets or build artifacts.

### `.editorconfig`

Create `.editorconfig` at the repo root for consistent formatting across editors and LLMs:

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false

[Makefile]
indent_style = tab
```

### First-run bootstrap

The repo should include a setup script. The LLM can create this if it doesn't exist:

```bash
#!/usr/bin/env bash
# scripts/setup.sh — run once after cloning

set -e

echo "Checking Node version..."
node -v

echo "Installing dependencies..."
# Replace with your package manager:
npm install

echo "Setting up environment..."
if [ ! -f .env ]; then
  cp .env.example .env
  echo "Created .env from .env.example — fill in your values."
else
  echo ".env already exists, skipping."
fi

echo "Creating dev-plan folders..."
mkdir -p .dev-plan/design-reference/inspiration
mkdir -p .dev-plan/design-reference/screenshots

echo "Installing Playwright browsers (for visual QA)..."
npx playwright install chromium

echo "Done. Run 'npm test' to verify."
```

### Platform notes

<!-- Keep what applies, delete the rest -->
- [ ] **Windows** — use `netstat -ano | findstr :<port>` for port checks. Use forward slashes in paths for cross-compat. Git bash or WSL recommended.
- [ ] **macOS** — use `lsof -i :<port>` for port checks.
- [ ] **Linux** — use `lsof -i :<port>` or `ss -tlnp | grep :<port>` for port checks.

The LLM must use platform-appropriate commands. When writing scripts, prefer cross-platform syntax or include both variants.

### Git hooks

<!-- Keep one or none, delete the rest -->
- [ ] **Husky + lint-staged** — runs linter/formatter on staged files before commit
- [ ] **simple-git-hooks** — lightweight alternative to Husky
- [ ] **None** — no pre-commit hooks

If hooks are enabled, the LLM must never bypass them (`--no-verify`). If a hook fails, fix the underlying issue.

---

## README Generation

When setting up a new repo or when asked to create/update the README, the LLM must generate a `README.md` following this template. Keep it **low-key and practical** — no badges wall, no marketing speak, no emoji headers.

### Template

```markdown
# Project Name

One-sentence description of what this project does.

## Quick start

\`\`\`bash
git clone <repo-url>
cd <project-name>
./scripts/setup.sh    # or: npm install && cp .env.example .env
npm run dev
\`\`\`

Open `http://localhost:<port>` in your browser.

## Stack

<!-- Pulled from the Tech Stack section of PROJECT_SETUP.md -->
- **Framework**: [e.g. Next.js (App Router)]
- **Language**: [e.g. TypeScript]
- **Styling**: [e.g. Tailwind CSS]
- **Database**: [e.g. PostgreSQL + Prisma]
- **Auth**: [e.g. NextAuth.js]

## Project structure

\`\`\`
src/
  components/    — UI components
  pages/         — routes (or app/ for App Router)
  lib/           — utilities, API clients
  config/        — configuration and schemas
  __tests__/     — test files
.dev-plan/       — wave planning, status, design references
scripts/         — setup and utility scripts
\`\`\`

## Scripts

| Command | What it does |
|---------|-------------|
| `npm run dev` | Start dev server |
| `npm run build` | Production build |
| `npm test` | Run tests |
| `npm run lint` | Lint and format check |

## Environment variables

Copy `.env.example` to `.env` and fill in the values:

\`\`\`bash
cp .env.example .env
\`\`\`

See `.env.example` for all required keys and descriptions.

## Development workflow

This project uses a wave-based planning system. See `PROMPT_RULES.md` for the full process.

- Planning files: `.dev-plan/`
- Current status: `.dev-plan/DEV_STATUS.md`
- Design references: `.dev-plan/design-reference/inspiration/`

## License

[MIT / ISC / your choice]
```

### README rules for the LLM

1. **Generate on first setup** — when creating a new repo or running the bootstrap for the first time, create `README.md` if it doesn't exist.
2. **Keep it short** — the README should fit on one screen without scrolling. Link to other docs (PROMPT_RULES.md, DEV_STATUS.md) instead of duplicating content.
3. **Update the Stack section** when the tech stack checklist changes.
4. **Update the Scripts table** when new scripts are added to `package.json`.
5. **Never add badges** unless the user explicitly asks for them.
6. **Never add a "Contributing" section** unless the user explicitly asks — this is a personal/team project by default.
7. **Tone**: direct, factual, no filler. Write like a developer's personal notes, not a product landing page.
