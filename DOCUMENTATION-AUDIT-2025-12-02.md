# Documentation Audit Report
**Date**: 2025-12-02
**Audited By**: Claude Code
**Scope**: All primary READMEs, setup guides, and developer documentation
**Repos**: backend, front-end, contracts, gacha-sdk, .github/profile

---

## Executive Summary

**Documentation Health Score**: **7/10** (Good, but needs urgent fixes)

| Metric | Count |
|--------|-------|
| **Total Documentation Files** | 18 primary files |
| **Repositories Audited** | 5 |
| **Critical Issues** | 5 |
| **Major Issues** | 8 |
| **Minor Issues** | 4 |
| **Quick Wins Available** | 5 (30 min total effort) |

**Key Finding**: The Bun → Node.js migration is incomplete in documentation. Many docs still reference `bun install` and `bun run` commands that no longer work.

---

## Critical Issues (Fix This Week)

### 1. Backend References Wrong Runtime
**Severity**: CRITICAL
**Location**: `backend/README.md` line 102
**Issue**: Says "Runtime: Bun 1.1+" but actually uses Node.js 24
**Fix**: Update to "Runtime: Node.js >= 24.11.1"
**Impact**: New developers install wrong runtime

### 2. Multiple Files Still Reference `bun`
**Severity**: CRITICAL
**Locations**:
- `backend/docs/guides/BACKEND_SETUP_GUIDE.md`
- `backend/docs/operations/ENVIRONMENT_SETUP.md`
- `frontend/README.md` lines 210-236

**Issue**: Commands say `bun install` but should be `pnpm install`
**Fix**: Find/replace `bun` → `pnpm` in these files
**Impact**: Setup fails if developers follow instructions

### 3. Broken Command in GET_STARTED.md
**Severity**: CRITICAL
**Location**: `backend/docs/guides/GET_STARTED.md` line 59
**Issue**: References `pnpm dev:local` which doesn't exist
**Fix**: Change to `pnpm dev` (verified in package.json)
**Impact**: Step 5 of quick start fails

### 4. Frontend README Shows Bun Setup for Backend
**Severity**: CRITICAL
**Location**: `frontend/README.md` lines 210-236
**Issue**: Shows how to install and run Bun for backend
**Fix**: Replace with: "See backend/README.md for setup"
**Impact**: Frontend devs confused about backend setup

### 5. Package Manager Not Clear
**Severity**: CRITICAL
**Locations**: Multiple READMEs
**Issue**: Some say "npm", some say "pnpm", inconsistent
**Fix**: Add package manager section to each README
**Impact**: Lock file conflicts, dependency issues

---

## Quick Wins (30 Minutes Total)

### 1. Fix `bun` References (10 min)
```bash
# In backend repo
find docs -name "*.md" -type f -exec sed -i '' 's/bun install/pnpm install/g' {} +
find docs -name "*.md" -type f -exec sed -i '' 's/bun run/pnpm run/g' {} +
```

### 2. Update GET_STARTED.md (5 min)
```diff
# backend/docs/guides/GET_STARTED.md line 59
- pnpm dev:local
+ pnpm dev
```

### 3. Fix Contracts Version Badge (2 min)
```diff
# contracts/README.md line 6
- Version: 5.0.2
+ Version: 6.0.5
```

### 4. Simplify Frontend Backend Instructions (10 min)
```diff
# frontend/README.md lines 210-236
- [All the Bun installation steps]
+ ## Backend Setup
+
+ See the [backend repository](https://github.com/altworth-markets/backend) for complete setup instructions.
+
+ Quick start: `cd ../backend && pnpm run quick-start`
```

### 5. Add Package Manager Sections (5 min)
Add to top of each README:
```markdown
## Package Manager

This repository uses **pnpm** (not npm).

```bash
npm install -g pnpm  # One-time global install
pnpm install         # Install dependencies
```
```

---

## By Repository Details

### Backend (Node.js 24 + pnpm)

**Files Analyzed**:
- README.md (776 lines)
- CLAUDE.md (285 lines)
- docs/guides/* (5 files)
- docs/operations/* (3 files)

**Critical Issues**: 3
- Runtime references (Bun vs Node.js)
- `bun install` in setup guides
- `dev:local` command doesn't exist

**Major Issues**: 2
- Doppler vs .env confusion (two conflicting paths)
- Missing decision tree for setup options

**Recommendations**:
- ✅ Keep: Architecture docs, monitoring guide, git workflow
- 🔧 Fix: Runtime references, command names
- 🗑️ Delete: Stale plan/TODO.md with old Bun references

---

### Frontend (Next.js 15 + pnpm)

**Files Analyzed**:
- README.md (895 lines)
- CLAUDE.md (70 lines)
- .internal/* (50+ files)

**Critical Issues**: 2
- Backend setup instructions (shows Bun, wrong repo)
- npm vs pnpm confusion

**Major Issues**: 2
- `npm run dev:local` vs `pnpm dev:local` inconsistency
- GitHub Packages auth has 3 different paths

**Recommendations**:
- ✅ Keep: Provider hierarchy, state management, architecture
- 🔧 Fix: Remove Bun references, clarify package manager
- 🎯 Improve: Simplify GitHub auth to single path

---

### Contracts (Solana Rust + pnpm)

**Files Analyzed**:
- README.md (262 lines)
- CLAUDE.md (180 lines)

**Critical Issues**: 0 ✅

**Major Issues**: 0 ✅

**Minor Issues**: 2
- Version badge outdated (5.0.2 vs 6.0.5)
- Deployment status unclear

**Recommendations**:
- ✅ Keep: Excellent architecture docs, error handling guide
- 🔧 Fix: Version badge
- 🎯 Improve: Add deployment status matrix

---

### Gacha SDK (TypeScript monorepo + pnpm)

**Files Analyzed**:
- README.md (100 lines)
- CLAUDE.md (150 lines)

**Critical Issues**: 0 ✅

**Major Issues**: 1
- Publishing guide referenced but not linked

**Minor Issues**: 1
- Incomplete troubleshooting section

**Recommendations**:
- ✅ Keep: Examples, API reference
- 🔧 Fix: Link to CLAUDE.md for publishing guide
- 🎯 Improve: Complete troubleshooting section

---

### Organization Profile (.github)

**Files Analyzed**:
- README.md (profile)
- SYSTEM_OVERVIEW.md (30 pages)
- USER_JOURNEYS.md (60 pages)
- CROSS_REPO_COORDINATION.md

**Assessment**: ✅ Excellent high-level documentation
**Action**: No changes needed (just updated with code-as-docs philosophy)

---

## Cross-Repo Issues

### 1. Package Manager Inconsistency
**Problem**: Some docs say npm, some say pnpm
**Impact**: Lock file conflicts
**Solution**: Add package manager section to every README

### 2. Runtime Technology Confusion
**Problem**: Old docs say "Bun", new reality is "Node.js 24"
**Impact**: Wrong runtime installed
**Solution**: Global find/replace + clarification in READMEs

### 3. Setup Guide Duplication
**Problem**: Multiple repos have similar setup guides
**Impact**: Maintenance burden, conflicting info
**Solution**: Create `/SETUP.md` in each repo as canonical entry point

---

## Files to Delete/Archive

| File | Reason | Safe? |
|------|--------|-------|
| `backend/plan/TODO.md` | Stale Bun references | Yes (backup first) |
| Multiple ENVIRONMENT_SETUP versions | Conflicting instructions | Consolidate to 1 |

---

## Conversion Candidates (Text → Scripts)

| Doc | Proposed Script | Status |
|-----|----------------|--------|
| backend/docs/guides/GET_STARTED.md | `scripts/quick-start.sh` | ⚠️ Partial |
| backend/docs/guides/ENVIRONMENT_SETUP.md | `scripts/dev-setup.sh` | ⚠️ Partial |
| frontend ENVIRONMENT_SETUP | `scripts/setup.sh` | ✅ Done |
| contracts setup instructions | `scripts/quick-start.sh` | ❌ Not done |

**Status**: Backend has scripts but docs don't reference them consistently

---

## Duplication/Conflict Detection

### Critical Conflicts

1. **Backend Setup Paths**
   - Path A (CLAUDE.md): Doppler only
   - Path B (README.md): .env alternative
   - **Fix**: Add decision tree: "Do you have Doppler access?"

2. **Frontend Dev Commands**
   - `npm run dev` documented as main
   - `npm run dev:local` documented as recommended
   - `npm run dev:staging` not documented
   - **Fix**: Create command decision matrix

3. **Package Manager**
   - Frontend mentions both npm and pnpm
   - Lock file is pnpm-lock.yaml
   - **Fix**: Standardize on pnpm everywhere

---

## Action Plan

### 🚨 Immediate (This Week) - 30 minutes

**Goal**: Fix critical setup blockers

1. Find/replace `bun install` → `pnpm install` (all docs)
2. Update GET_STARTED.md command
3. Remove Bun from frontend README
4. Update contracts version badge
5. Add package manager section to READMEs

**Owner**: Lead developer
**Verification**: Test setup on fresh machine

---

### 📅 Short-term (This Month) - 2 hours

**Goal**: Eliminate confusion

6. Create setup decision trees (Doppler vs .env)
7. Consolidate conflicting Doppler docs
8. Complete SDK troubleshooting section
9. Test all documented commands work

**Owner**: Lead dev + tech writer
**Verification**: New developer onboarding

---

### 🎯 Long-term (This Quarter) - 2 days

**Goal**: Prevent future staleness

10. Convert remaining setup guides to validation scripts
11. Create unified monorepo SETUP.md entry point
12. Implement quarterly documentation review
13. Add CI check: validate commands in docs exist

**Owner**: Tech writer
**Verification**: Automated testing in CI

---

## Verification Checklist

Before marking as complete:

- [ ] **Setup Testing**: Follow each README → get app running
  - [ ] backend: localhost:3000 works
  - [ ] frontend: localhost:3001 works
  - [ ] contracts: build succeeds
  - [ ] gacha-sdk: install succeeds

- [ ] **Command Verification**: Test every command documented
  - [ ] All `pnpm` commands work
  - [ ] All `npm run` scripts exist
  - [ ] No references to non-existent scripts

- [ ] **Link Verification**: All internal links work
  - [ ] No 404s
  - [ ] Cross-repo links correct

- [ ] **Version Verification**: Numbers match source of truth
  - [ ] Node versions match package.json
  - [ ] SDK versions match package.json
  - [ ] Badge versions current

---

## Success Metrics

Track these after fixes:

| Metric | Target |
|--------|--------|
| Setup success rate (first try) | > 90% |
| Time to first successful setup | < 5 min |
| "How do I..." questions | < 5/month |
| Documentation bug reports | < 1/month |

---

## Related Documents

- [CODE-AS-DOCUMENTATION.md](./.CODE-AS-DOCUMENTATION.md) - Philosophy guide
- [backend/CLAUDE.md](../backend/CLAUDE.md) - Backend context
- [frontend/CLAUDE.md](../front-end/CLAUDE.md) - Frontend context

---

## Conclusion

**TL;DR**: Documentation is mostly good, but the Bun → Node.js migration left many stale references. **30 minutes of fixes** will unblock new developers.

**Strengths**:
- ✅ Architecture and concept docs are excellent
- ✅ Most docs actively maintained
- ✅ Good examples and troubleshooting sections

**Weaknesses**:
- ❌ Runtime/tool migration incomplete
- ❌ Conflicting setup instructions
- ❌ Package manager inconsistency

**Recommendation**: Complete the quick wins this week, then schedule monthly documentation reviews to prevent future staleness.
