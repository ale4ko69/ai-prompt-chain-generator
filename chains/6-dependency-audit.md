# Step 6: Dependency Audit

**Purpose:** Analyze project dependencies for security vulnerabilities, outdated packages, and maintenance recommendations.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)
- `{project_folders}` - Project folder(s) from Step 0

**Dependencies:**
- Step 0 (detect-setup) - project mode
- Step 1 (determine-techstack) - detected languages and frameworks

---

## Instructions

### 1. Read Previous Analysis

Load results from:
```
{output_folder}/0-detect-setup.md
{output_folder}/1-determine-techstack.md
```

Extract:
- Project mode (single vs multi-project)
- Detected languages and package managers
- Project folder paths

---

### 2. Detect Monorepo Structure

**Search for multiple package manifests at different directory levels:**

```bash
# Node.js / JavaScript / TypeScript
find {project_folder} -name "package.json" -type f

# Python
find {project_folder} -name "requirements.txt" -o -name "pyproject.toml" -o -name "Pipfile" -type f

# .NET / C#
find {project_folder} -name "*.csproj" -o -name "*.sln" -type f

# PHP
find {project_folder} -name "composer.json" -type f

# Go
find {project_folder} -name "go.mod" -type f

# Rust
find {project_folder} -name "Cargo.toml" -type f

# Java
find {project_folder} -name "pom.xml" -o -name "build.gradle" -type f

# Ruby
find {project_folder} -name "Gemfile" -type f
```

**Categorize structure:**

```
IF manifests found in > 1 subdirectory:
  Structure = MONOREPO
  List workspaces with their paths and languages

ELSE:
  Structure = SIMPLE (single manifest)
```

**Output monorepo detection:**

```markdown
## Project Structure Analysis

Type: [MONOREPO / SIMPLE]

Workspaces detected:
1. ui/ - Node.js (package.json)
2. server/ - Python (requirements.txt)
3. gateway/ - Go (go.mod)

Total: 3 workspaces
```

---

### 3. User Interaction - Scan Method

**Ask user how to proceed:**

```
🔍 DEPENDENCY AUDIT SETUP

Project structure: [MONOREPO with 3 workspaces / SIMPLE project]

How would you like to check dependencies?
1. Automatic - Run commands now (requires tools installed)
2. Manual - Provide commands for you to run
3. Skip - Skip dependency audit for now

Choose [1/2/3]:
```

**Wait for user response.**

If `3` (Skip):
- Document that audit was skipped
- Recommend running manually later
- Proceed to Step 7

---

### 4. Monorepo Scan Strategy (if applicable)

**For monorepo structures, ask:**

```
🏗️ MONOREPO SCAN OPTIONS

Detected workspaces:
- ui/ (Node.js)
- server/ (Python)
- gateway/ (Go)

How to scan?
1. All workspaces separately (detailed per workspace)
2. Combined report (all dependencies together)
3. Choose specific workspaces

Recommendation: [1] for first audit, [2] for regular checks

Choose [1/2/3]:
```

If `3`, ask which workspaces to scan.

---

### 5. Dependency Check Strategy

**For each workspace/project, ask:**

```
📦 DEPENDENCY DEPTH

Include transitive dependencies?
- Direct only (packages you directly depend on) [recommended]
- All dependencies (including dependencies of dependencies)

Note: 'All' may result in hundreds of packages

Choose [direct/all]:
```

---

### 6. Execute Checks (Automatic Mode)

**For each detected package manager, try running audit commands:**

### Node.js / npm

```bash
cd {workspace_path}

# Security audit
npm audit --json > audit.json 2>&1

# Outdated packages
npm outdated --json > outdated.json 2>&1

# If npm audit fails, try basic check
if [ $? -ne 0 ]; then
  npm outdated --long
fi
```

**If command fails:**
```
⚠️ Tool 'npm audit' failed or not found.

Options:
1. Try alternative command (npm outdated only)
2. Skip this check
3. Show manual instructions

Choose [1/2/3]:
```

### Python / pip

```bash
cd {workspace_path}

# Outdated packages
pip list --outdated --format=json > outdated.json

# Security check (if safety installed)
safety check --json > safety.json 2>&1

# If safety not found
if [ $? -ne 0 ]; then
  echo "⚠️ 'safety' not installed"
  # Offer to install or skip
fi
```

**If safety not found:**
```
⚠️ Tool 'safety' not found for Python security check.

Options:
1. Install now: pip install safety
2. Skip security check (only outdated packages)
3. Show manual instructions

Choose [1/2/3]:
```

### .NET / NuGet

```bash
cd {workspace_path}

# Outdated packages
dotnet list package --outdated --format json > outdated.json

# Vulnerable packages
dotnet list package --vulnerable --format json > vulnerable.json
```

### PHP / Composer

```bash
cd {workspace_path}

# Outdated packages
composer outdated --format=json > outdated.json

# Security audit
composer audit --format=json > audit.json
```

### Go

```bash
cd {workspace_path}

# Check for updates
go list -u -m all > updates.txt

# Vulnerability check (requires govulncheck)
govulncheck ./... > vulncheck.txt 2>&1
```

### Rust / Cargo

```bash
cd {workspace_path}

# Outdated packages
cargo outdated --format json > outdated.json

# Security audit
cargo audit --json > audit.json
```

### Java / Maven

```bash
cd {workspace_path}

# Check for updates
mvn versions:display-dependency-updates -DoutputFile=updates.txt

# Vulnerability check (OWASP)
mvn dependency-check:check
```

### Ruby / Bundler

```bash
cd {workspace_path}

# Outdated gems
bundle outdated --parseable > outdated.txt

# Security audit
bundle audit check > audit.txt
```

---

### 7. Timeout Handling

**For long-running commands:**

```
Running npm audit... (timeout: 60s)
⏱️ Taking longer than expected (30s elapsed)...

Options:
1. Wait (continue)
2. Skip this check
3. Run manually later

Choose [1/2/3]:
```

---

### 8. Parse Results & Categorize

**Analyze command outputs and categorize issues:**

### 🔴 CRITICAL - Update Immediately

Criteria:
- Security vulnerabilities (High/Critical severity)
- Deprecated packages (no longer maintained)
- Known exploits (CVE numbers)
- Major version lag (>2 major versions behind)

### ⚠️ RECOMMENDED - Update Soon

Criteria:
- Security vulnerabilities (Medium severity)
- Major version updates (1-2 major versions behind)
- Unmaintained packages (>2 years without updates)
- Breaking changes available

### ℹ️ OPTIONAL - Consider Update

Criteria:
- Minor/patch updates
- Performance improvements
- New features available
- Code quality improvements

### ❓ UNABLE TO CHECK

Reasons:
- Private packages (no access to registry)
- Git dependencies (no version in registry)
- Local/workspace packages (part of monorepo)
- Registry unavailable/timeout
- Invalid version format

---

### 9. Generate Report

**Create comprehensive markdown report:**

```markdown
# Dependency Audit Report

**Date:** {current_date}
**Project:** {project_name}
**Structure:** [MONOREPO with N workspaces / SIMPLE project]

---

## Executive Summary

| Category | Count | Status |
|----------|-------|--------|
| 🔴 Critical | X | Immediate action required |
| ⚠️ Recommended | Y | Update soon |
| ℹ️ Optional | Z | Low priority |
| ✅ Up to date | N | No action needed |
| ❓ Unable to check | M | Manual review needed |

**Total packages analyzed:** {total}

---

{IF MONOREPO}
## Workspace: ui/ (Node.js)

### Package Manager: npm
**Direct dependencies:** 25 packages
**Total dependencies:** 150 packages

{END IF}

## 🔴 CRITICAL - Update Immediately

| Package | Current | Latest | Issue | Severity | CVE |
|---------|---------|--------|-------|----------|-----|
| lodash | 4.17.15 | 4.17.21 | Prototype Pollution | High | CVE-2021-23337 |
| express | 3.0.0 | 4.18.2 | Deprecated, multiple vulnerabilities | Critical | Multiple |

**Recommended Actions:**
1. Update lodash: `npm install lodash@latest`
2. Migrate express 3 → 4: Breaking changes expected
   - [Migration Guide](https://expressjs.com/en/guide/migrating-4.html)
   - Test thoroughly before production

---

## ⚠️ RECOMMENDED - Update Soon

| Package | Current | Latest | Reason | Impact |
|---------|---------|--------|--------|--------|
| react | 16.8.0 | 18.2.0 | 2 major versions behind | Breaking changes |
| axios | 0.21.0 | 1.6.0 | Security fixes, new features | Minor breaking changes |

**Recommended Actions:**
1. Plan React 16 → 18 migration (this month)
   - Review breaking changes
   - Update hooks usage
   - Test concurrent features
2. Update axios (this week)
   - Check axios 1.x migration guide
   - Test API calls

---

## ℹ️ OPTIONAL - Consider Update

| Package | Current | Latest | Type | Notes |
|---------|---------|--------|------|-------|
| jest | 28.0.0 | 29.5.0 | Minor | New test features |
| eslint | 8.40.0 | 8.55.0 | Patch | Bug fixes |

**Optional Actions (backlog):**
- Jest update when convenient
- ESLint update during maintenance window

---

## ❓ UNABLE TO CHECK

| Package | Reason | Recommendation |
|---------|--------|----------------|
| @company/internal-lib | Private registry | Check internal docs or contact @team |
| custom-fork-lib | Git dependency | Review upstream: https://github.com/user/repo |
| @workspace/shared | Workspace package | N/A - local package |

**Manual Review Needed:**
- [ ] @company/internal-lib - Check internal registry or GitHub releases
- [ ] custom-fork-lib - Compare with upstream, check for updates
- [ ] Consider publishing workspace packages to registry

---

{IF MONOREPO - REPEAT SECTIONS FOR EACH WORKSPACE}

---

## Summary Across All Workspaces

| Workspace | Language | Critical | Recommended | Optional | Total |
|-----------|----------|----------|-------------|----------|-------|
| ui | Node.js | 2 | 5 | 8 | 15 |
| server | Python | 1 | 3 | 4 | 8 |
| gateway | Go | 0 | 0 | 1 | 1 |
| **TOTAL** | | **3** | **8** | **13** | **24** |

---

{END IF}

## Alternative Packages to Consider

💡 **Deprecated/Unmaintained Packages:**

| Current | Status | Recommended Alternative | Reason |
|---------|--------|------------------------|--------|
| moment.js | Deprecated | date-fns or dayjs | Smaller bundle, tree-shakeable |
| request | Unmaintained | axios or got | Active maintenance, modern API |

---

## Version Policy Detected

{IF found}
✅ **Automated dependency management detected:**
- Dependabot configuration found (.github/dependabot.yml)
- Renovate configuration found (renovate.json)
- Lock files present: package-lock.json, yarn.lock

{ELSE}
⚠️ **No automated dependency management detected**

Recommendation: Set up Dependabot or Renovate for automated updates
{END IF}

---

## Maintenance Schedule

### Regular Checks
- [ ] **Weekly:** Review security advisories
- [ ] **Monthly:** Run full dependency audit
- [ ] **Quarterly:** Major dependency review and planning
- [ ] **Before major features:** Full dependency scan
- [ ] **After 3+ months inactivity:** Complete security audit

### Commands by Detected Stack

#### Node.js / npm
```bash
# Quick check
npm audit
npm outdated

# Detailed analysis
npm audit --json
npx npm-check-updates

# Fix vulnerabilities
npm audit fix
npm audit fix --force  # May introduce breaking changes
```

#### Python / pip
```bash
# Check outdated
pip list --outdated

# Security check
pip install safety
safety check

# Update all (careful!)
pip list --outdated --format=json | jq -r '.[] | .name' | xargs -n1 pip install -U
```

#### .NET / NuGet
```bash
# Check outdated
dotnet list package --outdated

# Check vulnerabilities
dotnet list package --vulnerable

# Update package
dotnet add package {PackageName}
```

#### Go
```bash
# Update dependencies
go get -u ./...
go mod tidy

# Vulnerability check
go install golang.org/x/vuln/cmd/govulncheck@latest
govulncheck ./...
```

---

## Breaking Changes Guide

### React 16 → 18
- Automatic batching changes
- Strict Mode double-invocation
- useEffect cleanup timing
- [Official Migration Guide](https://react.dev/blog/2022/03/08/react-18-upgrade-guide)

### Express 3 → 4
- req.params signature changed
- res.json/send behavior
- middleware changes
- [Official Migration Guide](https://expressjs.com/en/guide/migrating-4.html)

---

## Next Steps

**Immediate (this week):**
1. Fix critical security vulnerabilities
2. Review deprecated packages
3. Plan major version migrations

**Short-term (this month):**
1. Update recommended packages
2. Test breaking changes in dev environment
3. Update documentation

**Long-term (this quarter):**
1. Set up automated dependency management
2. Establish update policy
3. Schedule regular audits

---

**Report generated:** {timestamp}
**Next audit recommended:** {next_date}
```

---

### 10. Manual Mode - Provide Instructions

**If user chose Manual mode (option 2):**

```markdown
# Dependency Audit - Manual Instructions

Run these commands in your project directory:

## Node.js / npm
```bash
cd {workspace_path}
npm audit
npm outdated
```

## Python / pip
```bash
cd {workspace_path}
pip list --outdated
pip install safety && safety check
```

## .NET
```bash
cd {workspace_path}
dotnet list package --outdated
dotnet list package --vulnerable
```

{... similar for other detected package managers ...}

After running commands, share results for analysis.
```

---

### 11. Save Results

**Save report to:**
```
{output_folder}/6-dependency-audit.md
```

**Include in output:**
- Full report with all sections
- Summary statistics
- Recommended actions prioritized
- Manual review items
- Maintenance schedule
- Commands for detected stack

---

### 12. Next Step

Proceed to **Step 7: Build Instructions**

The dependency audit results will be referenced in Step 7 to:
- Include maintenance schedule in final instructions
- Add dependency health section
- Provide ongoing monitoring commands

---

## Error Handling

### Command Not Found
- Try fallback commands
- Ask user to install or skip
- Provide manual alternative

### Timeout
- Give user option to wait/skip/manual
- Default timeout: 60 seconds per command

### Parse Errors
- Log warning
- Include raw output in report
- Mark as "Unable to check"

### Network Issues
- Retry once
- Fall back to local checks only
- Document in report

---

## Output Format

Save to: `{output_folder}/6-dependency-audit.md`

Include:
- Full markdown report
- Tables for all categories
- Actionable recommendations
- Commands for detected technologies
- Maintenance schedule
- Links to migration guides

---

## Notes

- Always ask before running commands that modify files
- Respect user's choice (Auto/Manual/Skip)
- Handle missing tools gracefully
- Provide clear, actionable recommendations
- Include timeframes (immediate/short-term/long-term)
- Link to official documentation when available
- Consider project age (new vs legacy)
- Prioritize security over features
