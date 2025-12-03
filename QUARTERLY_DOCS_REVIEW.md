# Quarterly Documentation Review Checklist

> **Purpose**: Prevent documentation from going stale by conducting systematic reviews every quarter.
>
> **When**: First week of Q1, Q2, Q3, Q4
>
> **Who**: Engineering lead + 1-2 developers (rotate quarterly)
>
> **Time**: 2-3 hours per quarter

---

## 📅 Schedule

| Quarter | Months | Recommended Week |
|---------|--------|------------------|
| Q1 | Jan, Feb, Mar | First week of January |
| Q2 | Apr, May, Jun | First week of April |
| Q3 | Jul, Aug, Sep | First week of July |
| Q4 | Oct, Nov, Dec | First week of October |

**Add to team calendar** with 2-week reminder.

---

## ✅ Review Checklist

### 1. Technology Stack Changes (30 min)

**Check for**:
- [ ] Package manager changes (pnpm → something else?)
- [ ] Runtime changes (Node.js version updates)
- [ ] Framework updates (major versions)
- [ ] Build tool changes (Vite, Turbo, etc.)
- [ ] Database version updates
- [ ] New services added (Redis, Doppler, etc.)

**Action**:
```bash
# Run the CI validation workflow
gh workflow run validate-docs.yml

# Check for failures
gh run list --workflow=validate-docs.yml --limit 1
```

**Update**:
- READMEs with new prerequisites
- SETUP.md with new versions
- CLAUDE.md files with tech stack changes

---

### 2. Command Validation (30 min)

**Test all documented commands still work**:

#### Backend

```bash
cd backend

# Extract commands from docs
grep -rh "pnpm " docs/ README.md CLAUDE.md | \
  grep -E "^pnpm (run |install|test)" | \
  sort -u > /tmp/backend-commands.txt

# Test each command exists
while read cmd; do
  script=$(echo "$cmd" | sed 's/pnpm run //')
  if [[ "$cmd" =~ "pnpm run" ]]; then
    if ! grep -q "\"$script\":" package.json; then
      echo "❌ Missing: $cmd"
    fi
  fi
done < /tmp/backend-commands.txt
```

#### Frontend

```bash
cd front-end

# Similar process for npm commands
grep -rh "npm " docs/ README.md | \
  grep -E "^npm (run |install|test)" | \
  sort -u > /tmp/frontend-commands.txt

# Verify scripts exist
while read cmd; do
  script=$(echo "$cmd" | sed 's/npm run //')
  if [[ "$cmd" =~ "npm run" ]]; then
    if ! grep -q "\"$script\":" package.json; then
      echo "❌ Missing: $cmd"
    fi
  fi
done < /tmp/frontend-commands.txt
```

#### SDK

```bash
cd gacha-sdk

# Check SDK commands
grep -rh "pnpm " README.md CLAUDE.md examples/ | \
  grep -E "^pnpm (run |install|test)" | \
  sort -u > /tmp/sdk-commands.txt

# Verify
while read cmd; do
  script=$(echo "$cmd" | sed 's/pnpm run //')
  if [[ "$cmd" =~ "pnpm run" ]]; then
    if ! grep -q "\"$script\":" package.json; then
      echo "❌ Missing: $cmd"
    fi
  fi
done < /tmp/sdk-commands.txt
```

**Action**: Remove or update any commands that no longer exist.

---

### 3. Link Validation (15 min)

**Check internal links**:

```bash
# Run link checker
cd /Users/nelsonmelina/Repositories/altworth-markets

# Find all markdown files and check relative links
find . -name "*.md" -not -path "./node_modules/*" -not -path "./.git/*" | \
while read file; do
  echo "Checking: $file"

  # Extract relative links
  grep -oP '\[([^\]]+)\]\(\./[^\)]+\)' "$file" 2>/dev/null | \
  grep -oP '\(\./\K[^\)]+' | \
  while read link; do
    dir=$(dirname "$file")
    target="$dir/$link"

    if [ ! -f "$target" ]; then
      echo "  ❌ Broken: $link"
    fi
  done
done
```

**Action**: Fix or remove broken links.

---

### 4. Setup Path Validation (30 min)

**Test the new developer experience**:

#### Backend Setup

```bash
# Test Doppler path
cd backend
doppler run -- pnpm run quick-start
# Expected: No errors, database seeded

# Test .env path
cp .env.example .env.test
# Edit .env.test with test values
DATABASE_URL=postgresql://... pnpm dev
# Expected: Server starts successfully
```

#### Frontend Setup

```bash
cd front-end
npm install
npm run dev:local
# Expected: App loads on localhost:3001
```

#### Full-Stack

Follow [SETUP.md](./SETUP.md) instructions as a new developer would.

**Track time**: Should complete in < 15 minutes.

**Action**: Update setup guides if > 15 minutes or confusing.

---

### 5. Code-as-Documentation Audit (20 min)

**Check for documentation drift**:

```bash
# Find READMEs with many hardcoded commands
for readme in backend/README.md front-end/README.md gacha-sdk/README.md; do
  if [ -f "$readme" ]; then
    count=$(grep -c "^pnpm \|^npm " "$readme" 2>/dev/null || echo 0)
    reference=$(grep -c "pnpm run.*# See all" "$readme" 2>/dev/null || echo 0)

    echo "$readme: $count commands, $reference references to package.json"

    if [ "$count" -gt 15 ] && [ "$reference" -eq 0 ]; then
      echo "  ⚠️ Consider adding reference to 'pnpm run' or 'npm run'"
    fi
  fi
done
```

**Principle check**:
- [ ] READMEs refer to `pnpm run` / `npm run` as source of truth?
- [ ] Quick-start scripts exist and work?
- [ ] Scripts have helpful error messages?

**Action**: Convert text docs to executable scripts where possible.

---

### 6. Version Badge Updates (10 min)

**Check version badges match reality**:

```bash
# Backend
cat backend/package.json | grep '"version"'
cat backend/README.md | grep "badge.*version"

# Frontend
cat front-end/package.json | grep '"version"'
cat front-end/README.md | grep "badge.*version"

# Contracts
cat contracts/package.json | grep '"version"'
cat contracts/README.md | grep "badge.*version"

# SDK
cat gacha-sdk/package.json | grep '"version"'
cat gacha-sdk/README.md | grep "badge.*version"
```

**Action**: Update stale version badges.

---

### 7. New Developer Feedback (15 min)

**Review recent onboarding issues**:

```bash
# Check GitHub issues labeled "documentation"
gh issue list --label documentation --state all --limit 20 --json number,title,body

# Check for common pain points
gh issue list --search "setup OR install OR configure" --limit 20 --json number,title,body
```

**Questions to ask new developers**:
- What was confusing during setup?
- What took longer than expected?
- What documentation was outdated?
- What was missing?

**Action**: Create issues for recurring problems.

---

### 8. Stale Content Removal (20 min)

**Identify candidates for deletion**:

```bash
# Find old documentation files not updated in 6+ months
find . -name "*.md" -not -path "./node_modules/*" -not -path "./.git/*" \
  -type f -mtime +180 | \
while read file; do
  echo "🕰️ Not updated in 6+ months: $file"
done
```

**Review criteria**:
- [ ] Is this still relevant?
- [ ] Is there a better place for this info?
- [ ] Can this be converted to code?

**Action**:
- Archive outdated docs to `docs/archive/`
- Delete truly obsolete content
- Consolidate overlapping docs

---

### 9. Cross-Repository Consistency (15 min)

**Check for inconsistencies across repos**:

| Check | Backend | Frontend | Contracts | SDK |
|-------|---------|----------|-----------|-----|
| README structure | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Quick start section | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Package manager section | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Troubleshooting section | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Code-as-docs reference | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |

**Action**: Standardize structure across all repos.

---

### 10. CI Workflow Health (10 min)

**Check documentation validation workflow**:

```bash
# View recent workflow runs
gh run list --workflow=validate-docs.yml --limit 10

# Check for failures
gh run list --workflow=validate-docs.yml --status failure --limit 5 --json conclusion,name,createdAt
```

**Action**: Fix any recurring failures in CI checks.

---

## 📊 Review Output

### Create Review Report

```markdown
# Q[X] 202[Y] Documentation Review

**Date**: YYYY-MM-DD
**Reviewers**: [Names]
**Time Spent**: [X] hours

## Summary

- ✅ [N] issues found and fixed
- ⚠️ [N] issues identified for future work
- 📝 [N] action items created

## Issues Found

1. [Description] - Fixed in commit [hash]
2. [Description] - Issue created: #[number]
3. ...

## Action Items

- [ ] [Task] - Owner: [Name] - Due: [Date]
- [ ] [Task] - Owner: [Name] - Due: [Date]

## Metrics

- Setup time (new developer): [X] minutes
- Broken links found: [N]
- Stale commands removed: [N]
- Files archived: [N]

## Next Quarter Focus

[What should we prioritize in next quarter's review?]
```

**Save to**: `.github/docs-reviews/Q[X]-202[Y]-review.md`

---

## 🚀 Quick Actions

### Before Review

```bash
# 1. Create review issue
gh issue create --title "Q[X] 202[Y] Documentation Review" \
  --body "See .github/QUARTERLY_DOCS_REVIEW.md for checklist" \
  --label documentation

# 2. Schedule meeting
# Add to team calendar: "Documentation Review - 2-3 hours"

# 3. Assign reviewers
gh issue edit [number] --add-assignee @reviewer1,@reviewer2
```

### After Review

```bash
# 1. Commit fixes made during review
git add .
git commit -m "docs: quarterly review fixes (Q[X] 202[Y])"

# 2. Create issues for deferred work
gh issue create --title "[Description]" --label documentation

# 3. Update checklist with findings
# Add notes to .github/QUARTERLY_DOCS_REVIEW.md if process improvements found

# 4. Close review issue
gh issue close [number] --comment "Review complete. See .github/docs-reviews/Q[X]-202[Y]-review.md"
```

---

## 🎯 Success Metrics

**Good documentation health indicators**:
- ✅ All setup paths work in < 15 minutes
- ✅ Zero "this command doesn't work" issues
- ✅ CI validation passes consistently
- ✅ New developers complete setup without asking questions
- ✅ < 5% of links are broken

**Red flags**:
- ❌ Setup takes > 30 minutes
- ❌ >3 "documentation is wrong" issues per quarter
- ❌ CI validation fails frequently
- ❌ Multiple conflicting setup guides exist

---

## 📚 Resources

- **Audit report**: [DOCUMENTATION-AUDIT-2025-12-02.md](./DOCUMENTATION-AUDIT-2025-12-02.md)
- **Fix plan**: [DOCUMENTATION-FIX-PLAN.md](./DOCUMENTATION-FIX-PLAN.md)
- **Philosophy**: [CODE-AS-DOCUMENTATION.md](./CODE-AS-DOCUMENTATION.md)
- **CI workflow**: [.github/workflows/validate-docs.yml](../workflows/validate-docs.yml)

---

**Last Updated**: 2025-12-03
**Next Review**: [Add date of next quarter]
**Template Version**: 1.0.0
