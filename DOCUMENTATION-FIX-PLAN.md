# Documentation Fix Plan
**Generated**: 2025-12-02
**Based On**: DOCUMENTATION-AUDIT-2025-12-02.md
**Estimated Total Effort**: 4 hours (30 min critical, 2 hours important, 1.5 hours optional)

---

## 🚨 Phase 1: Critical Fixes (This Week - 30 minutes)

**Impact**: Unblocks new developers immediately
**Risk**: High - developers cannot set up successfully without these

### Task 1.1: Fix All `bun` References (10 min)

**Files to Update**:
```
backend/README.md
backend/docs/guides/BACKEND_SETUP_GUIDE.md
backend/docs/operations/ENVIRONMENT_SETUP.md
frontend/README.md (backend setup section)
```

**Commands**:
```bash
# Backend docs
cd backend
find . -name "*.md" -type f -not -path "*/node_modules/*" -exec grep -l "bun install\|bun run" {} \;
# Review list, then:
find docs -name "*.md" -type f -exec sed -i '' 's/bun install/pnpm install/g' {} +
find docs -name "*.md" -type f -exec sed -i '' 's/bun run/pnpm run/g' {} +

# Also check README.md line 102
# Manually change: "Runtime: Bun 1.1+" → "Runtime: Node.js >= 24.11.1"
```

**Verification**:
```bash
# Should return no results:
grep -r "bun install" backend/docs/
grep -r "bun run" backend/docs/
```

---

### Task 1.2: Fix Broken Command in GET_STARTED (5 min)

**File**: `backend/docs/guides/GET_STARTED.md`
**Line**: ~59

**Change**:
```diff
- pnpm dev:local
+ pnpm dev
```

**Verification**:
```bash
cd backend
pnpm run  # Verify "dev" exists, "dev:local" does not
```

---

### Task 1.3: Simplify Frontend Backend Instructions (10 min)

**File**: `frontend/README.md`
**Lines**: 210-236 (Step 1: Backend Setup section)

**Before**:
```markdown
<details>
<summary><b>Step 1: Backend Setup</b></summary>

```bash
# Install Bun
curl -fsSL https://bun.sh/install | bash
# ... many more steps
```
</details>
```

**After**:
```markdown
<details>
<summary><b>Step 1: Backend Setup</b></summary>

The backend repository has its own setup instructions and scripts.

**Automated Setup (Recommended)**:
```bash
cd ../backend
pnpm run quick-start  # Interactive 3-minute setup
```

**For detailed instructions**, see the [backend README](https://github.com/altworth-markets/backend#-quick-start).

</details>
```

---

### Task 1.4: Update Contracts Version Badge (2 min)

**File**: `contracts/README.md`
**Line**: ~6

**Change**:
```diff
- Version: 5.0.2
+ Version: 6.0.5
```

**Verification**:
```bash
cd contracts
grep '"version"' package.json  # Should show 6.0.5
```

---

### Task 1.5: Add Package Manager Sections (5 min)

**Files**: All primary READMEs (backend, frontend, contracts, gacha-sdk)

**Add near top** (after title, before table of contents):
```markdown
## Package Manager

This repository uses **pnpm** (not npm or yarn).

```bash
# One-time global installation
npm install -g pnpm

# Install dependencies
pnpm install

# See all available commands
pnpm run
```

**Why pnpm?** Faster installs, better monorepo support, stricter dependency resolution.
```

---

### ✅ Phase 1 Completion Criteria

- [ ] No `bun install` or `bun run` in any docs (except CHANGELOG/archives)
- [ ] backend/docs/guides/GET_STARTED.md step 5 uses correct command
- [ ] frontend/README.md points to backend repo for backend setup
- [ ] contracts/README.md version badge matches package.json
- [ ] All 4 repos have package manager section

**Test**:
- [ ] Ask fresh developer to follow instructions
- [ ] They can set up backend in < 5 minutes
- [ ] They can set up frontend in < 5 minutes

---

## 📅 Phase 2: Important Improvements (This Month - 2 hours)

**Impact**: Reduces confusion and support questions
**Risk**: Medium - developers can work around these but with friction

### Task 2.1: Create Setup Decision Tree (30 min)

**Goal**: Clear guidance on Doppler vs .env setup

**Create**: `backend/docs/guides/SETUP_DECISION_TREE.md`

**Content**:
```markdown
# Backend Setup Decision Tree

## Step 1: Do you have Doppler access?

### ✅ YES - I have Doppler access
→ **Use Doppler Setup Path**

1. Install Doppler CLI: [Instructions](https://docs.doppler.com/docs/install-cli)
2. Authenticate: `doppler login`
3. Run quick start: `pnpm run quick-start`
4. Start server: `pnpm dev` (Doppler auto-injects secrets)

**Pros**:
- No .env file management
- Auto-syncs across team
- Centralized secret rotation

**Cons**:
- Requires Doppler account
- Internet connection needed

---

### ❌ NO - I don't have Doppler access
→ **Use Traditional .env Setup Path**

1. Copy template: `cp .env.example .env`
2. Get secrets from: [HOW_TO_GET_SECRETS.md](./HOW_TO_GET_SECRETS.md)
3. Run setup: `pnpm run dev:setup`
4. Start server: `NODE_ENV=development pnpm dev`

**Pros**:
- Works offline
- No external dependencies
- Simple for solo work

**Cons**:
- Manual .env file management
- No automatic team sync
- Must manually rotate secrets

---

## Step 2: Quick Start vs Manual?

### 🤖 Automated (Recommended)
```bash
pnpm run quick-start
```
Handles everything: dependencies, database, migrations, seeding

### 🛠️ Manual (For Control)
Follow: [MANUAL_SETUP.md](./MANUAL_SETUP.md)
Step-by-step commands with explanations

---

## Troubleshooting

**Doppler command not found?**
→ See [Doppler CLI installation](https://docs.doppler.com/docs/install-cli)

**.env file errors?**
→ See [Environment Setup Guide](../operations/ENVIRONMENT_SETUP.md)

**Still stuck?**
→ Run diagnostic: `pnpm run diagnose`
```

**Link from**: README.md, CLAUDE.md, GET_STARTED.md

---

### Task 2.2: Consolidate Doppler Documentation (30 min)

**Goal**: Single source of truth for Doppler setup

**Current State**: Instructions scattered across:
- backend/README.md
- backend/CLAUDE.md
- backend/docs/operations/ENVIRONMENT_SETUP.md

**Action**:
1. Create: `backend/docs/operations/DOPPLER_SETUP.md` (comprehensive guide)
2. Update other files to link to it
3. Keep only short quick-reference in README

**Template**:
```markdown
# Doppler Setup Guide

## What is Doppler?
[Explanation]

## Prerequisites
[Requirements]

## Setup Steps
[Detailed steps]

## Troubleshooting
[Common issues]

## Alternatives
See [SETUP_DECISION_TREE.md](../guides/SETUP_DECISION_TREE.md) for .env setup
```

---

### Task 2.3: Complete SDK Troubleshooting Section (10 min)

**File**: `gacha-sdk/README.md`
**Line**: ~102

**Current**: Incomplete section for "RPC rate limiting"

**Add**:
```markdown
### Issue 3: RPC Rate Limiting

**Symptom**: Errors like "429 Too Many Requests" or timeouts

**Cause**: Exceeded free tier RPC request limits

**Solution**:
```typescript
// Use the RPC proxy pattern (don't call Helius directly)
const connection = new Connection(
  process.env.NEXT_PUBLIC_RPC_URL, // Should be proxy URL
  'confirmed'
);

// Batch requests when possible
const accounts = await connection.getMultipleAccountsInfo([...]);

// Add retry logic with exponential backoff
import { retry } from '@altworth-markets/gacha-sdk-core';
await retry(() => connection.getLatestBlockhash(), { maxAttempts: 3 });
```

**Prevention**: Always use RPC proxy, never expose Helius API key to client
```

---

### Task 2.4: Test All Documented Commands (30 min)

**Goal**: Verify every command in docs actually works

**Script**: `scripts/validate-docs.sh`

```bash
#!/bin/bash
# Extracts all commands from markdown and tests them

DOCS_DIR="docs"
FAILURES=()

echo "🔍 Extracting commands from docs..."

# Find all code blocks in markdown files
find "$DOCS_DIR" -name "*.md" -type f | while read file; do
  echo "  Checking: $file"

  # Extract commands (look for npm/pnpm/git commands)
  grep -E '^\s*(npm|pnpm|git|docker) ' "$file" | while read cmd; do
    # Skip comments and explanations
    [[ "$cmd" =~ ^[[:space:]]*# ]] && continue

    # For pnpm run commands, verify script exists
    if [[ "$cmd" =~ pnpm[[:space:]]+run[[:space:]]+([a-z:-]+) ]]; then
      script="${BASH_REMATCH[1]}"
      if ! grep -q "\"$script\":" package.json; then
        echo "  ❌ Script not found: pnpm run $script (in $file)"
        FAILURES+=("$file: pnpm run $script")
      fi
    fi
  done
done

if [ ${#FAILURES[@]} -eq 0 ]; then
  echo "✅ All commands validated"
  exit 0
else
  echo "❌ Found ${#FAILURES[@]} invalid commands"
  printf '%s\n' "${FAILURES[@]}"
  exit 1
fi
```

**Run on**:
- backend/docs
- frontend/docs
- contracts/docs
- gacha-sdk/docs

---

### ✅ Phase 2 Completion Criteria

- [ ] SETUP_DECISION_TREE.md created and linked
- [ ] Doppler docs consolidated to single file
- [ ] SDK troubleshooting section complete
- [ ] All commands in docs verified to exist

**Test**:
- [ ] New developer can choose setup path in < 1 minute
- [ ] Zero "this command doesn't exist" issues reported

---

## 🎯 Phase 3: Long-term Quality (This Quarter - 1.5 hours)

**Impact**: Prevents future staleness
**Risk**: Low - these are process improvements

### Task 3.1: Create Monorepo Setup Entry Point (20 min)

**Create**: `/SETUP.md` in monorepo root

**Content**:
```markdown
# Altworth Markets - Developer Setup

**New to the platform?** Start here.

## Quick Start

```bash
# 1. Clone all repositories (this is a monorepo structure)
git clone https://github.com/altworth-markets/altworth-markets.git
cd altworth-markets

# 2. Choose what you're working on:
```

### I'm Working On...

| Role | Setup Guide | Time |
|------|------------|------|
| **Backend API** | [backend/README.md](./backend/README.md#quick-start) | 3 min |
| **Frontend UI** | [front-end/README.md](./front-end/README.md#quick-start) | 3 min |
| **Solana Contracts** | [contracts/README.md](./contracts/README.md#quick-start) | 5 min |
| **SDK Packages** | [gacha-sdk/README.md](./gacha-sdk/README.md#quick-start) | 2 min |

### I Want to Run Everything

**Full-stack local development:**

```bash
# Backend (terminal 1)
cd backend
pnpm run quick-start
pnpm dev

# Frontend (terminal 2)
cd front-end
npm run quick-start
npm run dev:local

# Visit: http://localhost:3001
```

## Prerequisites

All repositories require:
- **Node.js** >= 22.0.0 (backend requires 24+)
- **pnpm** >= 10.0 (package manager)
- **Docker Desktop** (for databases)

## Package Manager

This monorepo uses **pnpm**:
```bash
npm install -g pnpm
```

## Need Help?

- 📖 [Developer Onboarding](./front-end/docs/DEVELOPER_ONBOARDING.md) (comprehensive guide)
- 🏗️ [System Overview](./.github/profile/SYSTEM_OVERVIEW.md) (architecture)
- 🤝 [Cross-Repo Coordination](./.github/profile/CROSS_REPO_COORDINATION.md)
```

---

### Task 3.2: Add CI Documentation Validation (30 min)

**Goal**: Automated checking that docs don't go stale

**Create**: `.github/workflows/validate-docs.yml`

```yaml
name: Validate Documentation

on:
  pull_request:
    paths:
      - '**.md'
      - '**/package.json'

  workflow_dispatch:

jobs:
  check-commands:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Check for bun references
        run: |
          if grep -r "bun install\|bun run" docs/ README.md 2>/dev/null; then
            echo "❌ Found 'bun' references in documentation"
            echo "Use 'pnpm install' or 'pnpm run' instead"
            exit 1
          fi
          echo "✅ No stale 'bun' references"

      - name: Verify pnpm run scripts exist
        run: |
          # Extract all "pnpm run <script>" from markdown
          scripts=$(grep -rho "pnpm run [a-z:-]\+" docs/ README.md | \
                    sed 's/pnpm run //' | sort -u)

          missing=()
          for script in $scripts; do
            if ! grep -q "\"$script\":" package.json; then
              missing+=("$script")
            fi
          done

          if [ ${#missing[@]} -gt 0 ]; then
            echo "❌ Referenced scripts not in package.json:"
            printf '  - %s\n' "${missing[@]}"
            exit 1
          fi
          echo "✅ All documented scripts exist"

      - name: Check version badges match package.json
        run: |
          # Check if version in README badge matches package.json
          pkg_version=$(grep '"version"' package.json | sed 's/.*"version": "\(.*\)".*/\1/')
          if grep -q "Version.*$pkg_version" README.md; then
            echo "✅ Version badge matches package.json"
          else
            echo "⚠️  Version badge may be outdated (not critical)"
          fi
```

**Add to each repo**: backend, frontend, contracts, gacha-sdk

---

### Task 3.3: Implement Quarterly Documentation Review (20 min)

**Create**: `.github/DOCUMENTATION_REVIEW_CHECKLIST.md`

**Content**:
```markdown
# Quarterly Documentation Review Checklist

**Schedule**: First week of each quarter (Jan, Apr, Jul, Oct)
**Owner**: Tech Lead + Senior Developer
**Duration**: ~2 hours

## Review Process

### 1. Command Validation (30 min)
- [ ] Run `pnpm run` in each repo - verify all scripts work
- [ ] Test setup instructions on fresh machine/VM
- [ ] Check that quick-start scripts complete successfully

### 2. Link Validation (15 min)
- [ ] Use markdown link checker on all READMEs
- [ ] Test cross-repo links
- [ ] Verify external links (not broken)

### 3. Version Validation (15 min)
- [ ] Check version badges match package.json
- [ ] Verify Node.js version requirements current
- [ ] Update dependency version references

### 4. Content Review (30 min)
- [ ] Read "Getting Started" sections - still accurate?
- [ ] Check for new features not documented
- [ ] Remove deprecated feature docs

### 5. Code-as-Documentation Opportunities (30 min)
- [ ] Find command-heavy sections → candidates for scripts
- [ ] Identify repetitive troubleshooting → `diagnose` script?
- [ ] Look for manual checklists → validation scripts?

## Post-Review Actions

- Create issues for findings
- Update DOCUMENTATION-AUDIT report
- Schedule fixes for next sprint

## Metrics to Track

| Metric | Target | Actual |
|--------|--------|--------|
| Setup success rate | > 90% | ___ |
| Time to first setup | < 5 min | ___ |
| Doc bug reports/quarter | < 3 | ___ |
| Broken links found | 0 | ___ |
```

**Add to calendar**: Recurring quarterly review

---

### ✅ Phase 3 Completion Criteria

- [ ] Monorepo SETUP.md created
- [ ] CI workflow added to all repos
- [ ] Quarterly review checklist created and scheduled

**Test**:
- [ ] CI catches stale `bun` references
- [ ] CI catches non-existent script references
- [ ] Quarterly review completed once

---

## Success Metrics

**Track monthly**:

| Metric | Baseline | Target | 1 Month | 3 Months |
|--------|----------|--------|---------|----------|
| Setup success rate | ~60% | > 90% | ___ | ___ |
| Avg setup time | ~30 min | < 5 min | ___ | ___ |
| "Command not found" issues | 5/mo | < 1/mo | ___ | ___ |
| Doc bug reports | 3/mo | < 1/mo | ___ | ___ |

---

## Rollout Schedule

### Week 1 (Now)
- [ ] Phase 1: All critical fixes

### Week 2-3
- [ ] Phase 2: Important improvements
- [ ] Test with 2-3 new developers

### Week 4
- [ ] Phase 3: Process improvements
- [ ] First quarterly review

### Ongoing
- Monitor metrics
- Iterate based on feedback
- Quarterly reviews

---

## Related Documents

- [DOCUMENTATION-AUDIT-2025-12-02.md](./DOCUMENTATION-AUDIT-2025-12-02.md) - Full audit
- [CODE-AS-DOCUMENTATION.md](./CODE-AS-DOCUMENTATION.md) - Philosophy guide

---

## Questions?

Contact: Tech Lead or file issue at `.github` repo
