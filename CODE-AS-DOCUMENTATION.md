# Code as Documentation: The Impossible-to-Stale Approach

## The Problem

Traditional documentation suffers from **documentation rot**:

```markdown
<!-- This goes stale quickly -->
## Getting Started

1. Run `bun install`
2. Run `bun run db:up && bun run db:wait && bun run db:migrate`
3. Run `bun run dev`
```

**Why this fails:**
- ❌ Commands change but docs don't get updated
- ❌ New developers follow outdated instructions
- ❌ Troubleshooting takes hours
- ❌ README says "bun" but codebase uses "pnpm"
- ❌ No way to validate that instructions still work

## The Solution

**Make code the documentation:**

```bash
# This can NEVER go stale
pnpm run quick-start
```

**Why this works:**
- ✅ Script is executable - it either works or fails immediately
- ✅ `package.json` is the single source of truth
- ✅ `pnpm run` always shows current commands
- ✅ Scripts can validate prerequisites and give helpful errors
- ✅ Refactoring commands automatically updates "documentation"

## Implementation

### 1. Scripts as Documentation

**Instead of:**
```markdown
## Setup
1. Install dependencies
2. Configure environment
3. Start database
4. Run migrations
```

**Do this:**
```json
{
  "scripts": {
    "quick-start": "bash scripts/quick-start.sh",
    "dev:setup": "bash scripts/dev-setup.sh",
    "dev:fresh": "bash scripts/dev-setup.sh --force-reseed"
  }
}
```

**Then reference the scripts:**
```markdown
## Setup

```bash
pnpm run quick-start  # Automated 3-minute setup
```

See all commands: `pnpm run`
```

### 2. Self-Documenting Commands

Use `pnpm run` or `npm run` to show all available commands:

```bash
$ pnpm run

Commands available via "pnpm run":
  quick-start          bash scripts/quick-start.sh
  dev                  doppler run --config dev -- tsx watch src/index.ts
  dev:setup            bash scripts/dev-setup.sh
  dev:fresh            bash scripts/dev-setup.sh --force-reseed
  test                 pnpm type-check && vitest run
  db:up                docker compose up -d db redis
  db:down              docker compose down -v
  db:migrate           drizzle-kit migrate
```

**This list can NEVER be wrong** - it's generated from the actual package.json.

### 3. Validation at Runtime

Scripts should validate prerequisites and give actionable errors:

```bash
#!/bin/bash

# Check Node.js
if ! command -v node &> /dev/null; then
    echo "❌ Node.js is not installed"
    echo "   Install from: https://nodejs.org/"
    exit 1
fi

# Check Docker
if ! docker info &> /dev/null 2>&1; then
    echo "❌ Docker is not running"
    echo "   Start Docker Desktop"
    exit 1
fi

# Check port availability
if lsof -Pi :3000 -sTCP:LISTEN -t >/dev/null 2>&1; then
    echo "❌ Port 3000 is already in use"
    echo "   Kill the process with: kill $(lsof -Pi :3000 -sTCP:LISTEN -t)"
    exit 1
fi
```

### 4. Help Built-In

Scripts should have `--help` flags:

```bash
$ pnpm run quick-start -- --help

🚀 Altworth Backend Quick Start

USAGE:
    ./scripts/quick-start.sh [OPTIONS]

OPTIONS:
    -y, --yes           Skip all confirmations
    --wizard            Interactive configuration wizard
    --tutorial          Learn as you setup (explains everything)
    --troubleshoot      Diagnose issues
    --browser           Open browser automatically when done
    -h, --help          Show this help
```

## Documentation Hierarchy

### Tier 1: Executable Code (Never Stale)
- **package.json scripts** - Single source of truth
- **Validation scripts** - Check prerequisites
- **Setup scripts** - Automate onboarding

### Tier 2: Reference to Code (Rarely Stale)
- README that says "Run `pnpm run`"
- Links to script files
- Examples using script names

### Tier 3: Context (Can Go Stale, But That's OK)
- Architecture explanations
- Design decisions
- Tutorials and walkthroughs
- Why, not how

## Anti-Patterns to Avoid

### ❌ Anti-Pattern #1: Manual Command Documentation

**Bad:**
```markdown
## Database Setup

1. Start PostgreSQL:
   ```bash
   docker compose up -d
   ```

2. Wait for readiness:
   ```bash
   until pg_isready -U postgres; do sleep 1; done
   ```

3. Run migrations:
   ```bash
   DATABASE_URL=postgresql://postgres:postgres@localhost:5432/altworth_tcg pnpm db:migrate
   ```
```

**Good:**
```markdown
## Database Setup

```bash
pnpm run db:up && pnpm run db:wait && pnpm run db:migrate
```

Or use the automated setup: `pnpm run quick-start`
```

### ❌ Anti-Pattern #2: Hardcoded URLs/Ports

**Bad:**
```markdown
Visit http://localhost:3000
```

**Good:**
```bash
# In the setup script:
echo "✅ Backend started at: http://localhost:${SERVER_PORT}"
echo "   Run 'pnpm run dev:status' to see connection info"
```

### ❌ Anti-Pattern #3: Copy-Paste Command Blocks

**Bad:**
```markdown
Run these commands:
```bash
export DATABASE_URL=postgresql://postgres:postgres@localhost:5432/altworth_tcg
export NODE_ENV=development
export LOG_LEVEL=info
pnpm dev
```
```

**Good:**
```markdown
```bash
pnpm run dev  # Loads config from Doppler automatically
```

See `.env.example` for local development without Doppler.
```

## When to Use Text Documentation

Text docs are still useful for:

### ✅ Architecture & Design
```markdown
## Why We Use Zustand

We chose Zustand over Redux because:
1. Smaller bundle size (3KB vs 20KB)
2. Less boilerplate
3. Better TypeScript support
4. Hooks-first API
```

### ✅ Concepts & Context
```markdown
## Provider Hierarchy

The order matters because SDKProvider needs wallet context:

QueryProvider → ThemeProvider → WalletProvider → SDKProvider
```

### ✅ Troubleshooting Patterns
```markdown
## Common Issues

### Hydration Mismatch Errors

**Symptom:** Error about server/client HTML mismatch

**Cause:** Wallet component rendering differently on server vs client

**Solution:**
```tsx
<HydrationGate>
  <WalletButton />
</HydrationGate>
```
```

## Adoption Strategy

### Phase 1: Identify Pain Points
1. Watch for issues in onboarding
2. Track questions in Slack/issues
3. Find commands that fail frequently

### Phase 2: Extract to Scripts
1. Create script for common task
2. Add to package.json
3. Update one README as pilot

### Phase 3: Standardize
1. Template for new repos
2. Update all major READMEs
3. Add to onboarding docs

### Phase 4: Evangelize
1. Show new devs: "Just run `pnpm run`"
2. Track time-to-first-success
3. Iterate based on feedback

## Success Metrics

Track these to measure effectiveness:

| Metric | Target |
|--------|--------|
| **Time to first successful setup** | < 5 minutes |
| **Setup success rate (first try)** | > 90% |
| **Documentation bug reports** | < 1 per month |
| **"How do I..." questions** | Answered by `pnpm run` |

## Examples from Altworth

### Backend Quick Start
- **Script:** `scripts/quick-start.sh` (1200+ lines)
- **Features:** Interactive wizard, auto-fix, prerequisite checking
- **Result:** 3-minute setup for new developers

### Frontend Dev Modes
- **Scripts:** `dev:local`, `dev:staging`, `dev:status`
- **Features:** Automated environment switching
- **Result:** No more "which backend am I using?"

## Conclusion

**The best documentation is code that:**
1. ✅ Validates prerequisites
2. ✅ Provides helpful errors
3. ✅ Can be discovered via `pnpm run`
4. ✅ Has `--help` flags
5. ✅ Is tested in CI

**Text documentation should:**
1. Reference scripts, not duplicate them
2. Explain *why*, not *how*
3. Provide context and architecture
4. Link to code as source of truth

**Remember:**
- Scripts = Source of Truth (Tier 1)
- README = Pointer to Scripts (Tier 2)
- Guides = Context & Architecture (Tier 3)

---

**Related:**
- Backend: `scripts/quick-start.sh`
- Frontend: `scripts/quick-start.sh`
- This doc: `.github/CODE-AS-DOCUMENTATION.md`
