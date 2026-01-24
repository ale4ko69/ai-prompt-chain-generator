# Step 0: Detect Project Setup

**Purpose:** Determine if this is a single-project or multi-project scenario and decide the execution path.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

---

## Instructions

### 1. Analyze Workspace Structure

Search for indicators of multi-project setup:

```
Check for:
- Multiple project folders in workspace root
- Existence of ~/.shared-docs/ directory
- Multiple package.json / requirements.txt / .csproj files at different levels
- Monorepo indicators (lerna.json, nx.json, pnpm-workspace.yaml)
- Docker compose with multiple services
```

### 2. Detect Shared Documentation

Look for existing cross-project documentation:

```
~/.shared-docs/
  ├── global-copilot-instructions.md
  ├── PROJECT-SYNC-WORKFLOW.md
  ├── ADAPTATION-PATTERNS.md
  └── other shared docs
```

If this directory exists and contains project documentation → **Multi-project scenario**

### 3. Count Active Projects

Analyze workspace folders:

```
Single project indicators:
- One main application folder
- One package.json/requirements.txt/solution file
- One .git repository

Multi-project indicators:
- Multiple related applications (e.g., backend + frontend + gateway)
- Multiple .git repositories
- Shared packages between projects
- Cross-project dependencies
```

### 4. Determine Execution Path

Based on analysis, decide:

**SINGLE PROJECT:**
```yaml
mode: single-project
execution_path: 1-6 (all-in-one)
output_file: .github/copilot-instructions.md
strategy: Generate comprehensive single file with all rules and patterns
```

**MULTI-PROJECT:**
```yaml
mode: multi-project
execution_path: 1-6 (shared + specific)
output_structure:
  - ~/.shared-docs/global-copilot-instructions.md (universal rules + cross-project workflows)
  - {project_1}/.github/copilot-instructions.md (project-specific overrides)
  - {project_2}/.github/copilot-instructions.md (project-specific overrides)
strategy: Shared base documentation + project-specific additions
```

---

## Output Format

Save to `{output_folder}/0-detect-setup.md`:

```markdown
# Project Setup Detection

## Mode
[single-project | multi-project]

## Workspace Structure
- Project 1: {path} ({description})
- Project 2: {path} ({description}) [if multi-project]

## Shared Documentation
[exists | not-exists]
Location: ~/.shared-docs/ [if exists]

## Execution Strategy
- Base templates: templates/base/universal-rules.md, templates/base/spec-driven-development.md
- Language templates: templates/base/languages/{detected-in-step-1}
- Multi-project templates: [yes/no]
  - templates/multi-project/sync-workflow.md [if yes]
  - templates/multi-project/adaptation-patterns.md [if yes]
  - templates/multi-project/sync-status-tracker.md [if yes]

## Output Targets
- Primary: {file_path}
- Secondary: {file_path} [if multi-project]

## Next Steps
Proceed to Step 1: Determine Tech Stack
```

---

## Decision Logic

```
IF ~/.shared-docs/ exists AND contains project docs:
  → Multi-project mode

ELIF workspace contains 2+ related projects:
  → Multi-project mode (suggest creating ~/.shared-docs/)

ELSE:
  → Single-project mode
```

---

**After completion, proceed to chains/1-determine-techstack.md**
