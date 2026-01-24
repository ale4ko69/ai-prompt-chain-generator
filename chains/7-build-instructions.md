# Step 7: Build Final Instructions

**Purpose:** Combine all analyzed data from Steps 0-6 with base templates to generate the final instruction file.

**Parameters:**
- `{output_folder}` - Where analysis results are stored (e.g., `.results/`)
- `{final_output_file}` - Target file (e.g., `.github/copilot-instructions.md`)
- `{workspace_root}` - Root directory of the workspace

**Dependencies:**
- ALL previous steps (0-6) must be completed

---

## Instructions

### 1. Read All Previous Analysis

Gather data from all completed steps:

```
Read files:
- {output_folder}/0-detect-setup.md
- {output_folder}/1-determine-techstack.md
- {output_folder}/2-categorize-files.md
- {output_folder}/3-identify-architecture.md
- {output_folder}/4-domain-deep-dive.md
- {output_folder}/5-styleguide-generation.md
- {output_folder}/6-dependency-audit.md
```

Extract key information:
- Project mode (single vs multi-project)
- Primary language
- Framework(s)
- Architecture pattern
- Key entities and features
- Coding style patterns
- Dependency health status and recommendations

### 2. Load Base Templates

Read universal templates:

```
Required:
- templates/base/universal-rules.md
- templates/base/spec-driven-development.md

Language-specific (based on Step 1):
- templates/base/languages/{detected_language}-patterns.md
  Examples:
  - templates/base/languages/nodejs-patterns.md
  - templates/base/languages/python-patterns.md
  - templates/base/languages/csharp-patterns.md
  - templates/base/languages/bash-patterns.md

Fallback (if specific language not found):
- templates/base/languages/generic-patterns.md
```

### 3. Load Multi-Project Templates (if applicable)

If Step 0 detected multi-project mode:

```
Read:
- templates/multi-project/sync-workflow.md
- templates/multi-project/adaptation-patterns.md
- templates/multi-project/sync-status-tracker.md
```

### 4. Determine Output Strategy

Based on project mode from Step 0:

#### **Single-Project Mode**

**Output:** `{final_output_file}` (single comprehensive file)

**Structure:**
```markdown
# {Project Name} - AI Coding Instructions

## Project Overview
{From Steps 1-4}

## Tech Stack
{From Step 1}

## Architecture
{From Step 3}

## Domain
{From Step 4}

## Coding Style
{From Step 5}

## Universal Rules
{Content from templates/base/universal-rules.md}

## Language-Specific Patterns
{Content from templates/base/languages/{language}-patterns.md}

## Spec-Driven Development
{Content from templates/base/spec-driven-development.md}

## File Organization
{From Step 2}

## Key Features
{From Step 4}

## Testing Guidelines
{From Steps 3, 5}

## Development Workflow
{How to build, run, test - from README/package.json}
```

#### **Multi-Project Mode**

**Output 1:** `~/.shared-docs/global-copilot-instructions.md` (shared rules)

**Structure:**
```markdown
# Multi-Project Global Instructions

## Shared Documentation
This file contains rules applicable to ALL projects in the workspace.

## Universal Rules
{Content from templates/base/universal-rules.md}

## Spec-Driven Development
{Content from templates/base/spec-driven-development.md}

## Cross-Project Sync Workflow
{Content from templates/multi-project/sync-workflow.md}

## Adaptation Patterns
{Content from templates/multi-project/adaptation-patterns.md}

## Shared Tech Stack
{Common frameworks, libraries from Step 1}

## Shared Packages
{List from Step 1}

## General Coding Style
{Common patterns from Step 5}
```

**Output 2:** `{project_1}/.github/copilot-instructions.md` (project-specific)

**Structure:**
```markdown
# {Project 1 Name} - Project-Specific Instructions

## Shared Documentation
⚠️ **Read global instructions first:** `~/.shared-docs/global-copilot-instructions.md`

## Project Overview
{From Steps 1-4, project-specific}

## Tech Stack
{From Step 1, project-specific libraries}

## Architecture
{From Step 3, this project only}

## Domain
{From Step 4, this project's entities/features}

## Coding Style
{From Step 5, project-specific patterns}

## Language-Specific Patterns
{Content from templates/base/languages/{language}-patterns.md}

## File Structure
{From Step 2, this project only}

## Development Workflow
{Build, run, test commands for this project}
```

**Output 3:** `{project_2}/.github/copilot-instructions.md` (repeat for each project)

### 5. Backup Existing Files

Before writing, check if files exist:

```bash
if [ -f "{final_output_file}" ]; then
  timestamp=$(date +%Y-%m-%d-%H%M%S)
  cp {final_output_file} {final_output_file}.backup-$timestamp.md
  echo "✅ Backed up existing file to {final_output_file}.backup-$timestamp.md"
fi
```

### 6. Build Final Content

**Combine sections in this order:**

1. **Header:** Project name, description, last updated
2. **Quick Start:** How to use these instructions
3. **Project Overview:** Tech stack, architecture, domain summary
4. **Universal Rules:** From templates/base/universal-rules.md
5. **Language Patterns:** From templates/base/languages/{language}-patterns.md
6. **Architecture Details:** From Step 3
7. **Domain Knowledge:** From Step 4 (entities, features, workflows)
8. **Coding Style:** From Step 5 (actual patterns found)
9. **File Organization:** From Step 2
10. **Spec-Driven Development:** From templates/base/spec-driven-development.md
11. **Multi-Project (if applicable):** From templates/multi-project/*
12. **Development Workflow:** Build, run, test, deploy commands
13. **Dependency Maintenance:** From Step 6 (maintenance schedule + current health)
14. **Appendix:** Links to key files, resources

### 7. Write Output File(s)

**Single-Project:**
```bash
Write to: {final_output_file}
```

**Multi-Project:**
```bash
Write to: ~/.shared-docs/global-copilot-instructions.md
Write to: {project_1}/.github/copilot-instructions.md
Write to: {project_2}/.github/copilot-instructions.md
```

### 8. Create Version Tracking File

Create `.ai-prompt-chain-generator.json` to track the version and configuration:

**Single-Project:**
```bash
Write to: {workspace_root}/.ai-prompt-chain-generator.json
```

**Multi-Project:**
```bash
Write to: ~/.shared-docs/.ai-prompt-chain-generator.json
Write to: {project_1}/.ai-prompt-chain-generator.json
Write to: {project_2}/.ai-prompt-chain-generator.json
```

**JSON Structure:**

```json
{
  "version": "0.2.0",
  "generator": "AI Prompt Chain Generator",
  "repository": "https://github.com/ale4ko69/ai-prompt-chain-generator",
  "generatedAt": "{ISO 8601 timestamp}",
  "projectMode": "single",
  "configuration": {
    "outputFolder": "{output_folder}",
    "finalOutputFile": "{final_output_file}",
    "projectFolders": ["{project_folders}"]
  },
  "components": {
    "steps": ["0-detect-setup", "1-determine-techstack", "2-categorize-files", "3-identify-architecture", "4-domain-deep-dive", "5-styleguide-generation", "6-dependency-audit", "7-build-instructions"],
    "templates": {
      "base": ["universal-rules", "spec-driven-development"],
      "language": "{detected_language}-patterns",
      "multiProject": []
    }
  },
  "techStack": {
    "primaryLanguage": "{from Step 1}",
    "framework": "{from Step 1}",
    "packageManager": "{from Step 1}"
  },
  "lastUpdatedBy": "AI Prompt Chain Generator v0.2.0",
  "customModifications": false,
  "notes": "Track local changes in this field if you customize the generated instructions"
}
```

**For Multi-Project, add:**
```json
{
  "projectMode": "multi",
  "projects": [
    {
      "name": "{project_1_name}",
      "path": "{project_1}",
      "language": "{language}",
      "instructionsFile": "{project_1}/.github/copilot-instructions.md"
    },
    {
      "name": "{project_2_name}",
      "path": "{project_2}",
      "language": "{language}",
      "instructionsFile": "{project_2}/.github/copilot-instructions.md"
    }
  ],
  "sharedDocsPath": "{shared_docs_path}"
}
```

**Important Notes:**
- This file helps track which version of PCG generated the instructions
- Enables automatic update detection in future versions
- Set `customModifications: true` if user manually edits generated instructions
- Do NOT commit this file if it contains sensitive paths

### 9. Verification

After writing, verify:

```
✅ File(s) created successfully
✅ Version tracking file (.ai-prompt-chain-generator.json) created
✅ No syntax errors (valid Markdown)
✅ All sections included
✅ Code examples properly formatted
✅ Links to templates/files are correct
✅ Backup created if file existed
```

### 10. Summary Report

Generate summary:

```markdown
# Instruction Generation Complete

## Files Created/Updated

### Single-Project:
- ✅ {final_output_file} ({file_size} KB)

### Multi-Project:
- ✅ ~/.shared-docs/global-copilot-instructions.md ({file_size} KB)
- ✅ {project_1}/.github/copilot-instructions.md ({file_size} KB)
- ✅ {project_2}/.github/copilot-instructions.md ({file_size} KB)

## Backups Created
- {list backup files if any}

## Templates Included
- ✅ templates/base/universal-rules.md
- ✅ templates/base/spec-driven-development.md
- ✅ templates/base/languages/{language}-patterns.md
- ✅ templates/multi-project/* (if applicable)

## Analysis Used
- ✅ Step 0: Project setup detection
- ✅ Step 1: Tech stack ({primary_language}, {framework})
- ✅ Step 2: File categorization ({file_count} files analyzed)
- ✅ Step 3: Architecture ({architecture_pattern})
- ✅ Step 4: Domain ({entity_count} entities, {feature_count} features)
- ✅ Step 5: Code style (extracted from {sample_size} files)
- ✅ Step 6: Dependency audit ({critical_count} critical, {recommended_count} recommended updates)

## Next Steps
1. Review generated instruction file(s)
2. Test with AI coding assistant (GitHub Copilot, Cursor, etc.)
3. Refine based on usage
4. Update as project evolves
5. Archive analysis files or clean up:
   - Keep: {final_output_file}
   - Archive: {output_folder}/* (optional, for reference)
```

---

## Output File Template (Single-Project)

```markdown
# {Project Name} - AI Coding Instructions

**Generated:** {date}
**Primary Language:** {from Step 1}
**Framework:** {from Step 1}
**Architecture:** {from Step 3}

---

## How to Use This File

This file provides comprehensive instructions for AI coding assistants working on this project.

**For AI Agents:**
- Read this entire file before starting work
- Follow all rules strictly
- Refer to specific sections as needed
- Ask for clarification if requirements conflict

**For Developers:**
- Keep this file updated as project evolves
- Add project-specific rules as needed
- Review AI-generated code against these guidelines

---

## Project Overview

### Tech Stack
{Paste from Step 1}

### Architecture
{Summary from Step 3}

### Domain
{Summary from Step 4}

---

## Universal Rules

{Content from templates/base/universal-rules.md}

---

## {Language} Coding Patterns

{Content from templates/base/languages/{language}-patterns.md}

---

## Architecture Details

{Detailed content from Step 3}

---

## Domain Knowledge

### Core Entities
{From Step 4}

### Key Features
{From Step 4}

### Critical Workflows
{From Step 4}

---

## Coding Style (Project-Specific)

{Content from Step 5}

---

## File Organization

{Content from Step 2}

---

## Spec-Driven Development

{Content from templates/base/spec-driven-development.md}

---

## Development Workflow

### Setup
```bash
{Installation commands from README or package.json}
```

### Build
```bash
{Build commands}
```

### Run
```bash
{Run commands}
```

### Test
```bash
{Test commands}
```

---

## Dependency Maintenance

{Content from Step 6: Maintenance Schedule section}

### Regular Checks
- [ ] **Weekly:** Review security advisories
- [ ] **Monthly:** Run full dependency audit
- [ ] **Quarterly:** Major dependency review and planning
- [ ] **Before major features:** Full dependency scan
- [ ] **After 3+ months inactivity:** Complete security audit

### Commands for This Stack

{Language-specific commands from Step 6}

### Current Dependency Health

**Last Audit:** {date from Step 6}

{Summary from Step 6 dependency-audit.md:}
- 🔴 Critical issues: {count}
- ⚠️ Recommended updates: {count}
- ℹ️ Optional updates: {count}

**See full report:** `.results/6-dependency-audit.md`

---

## Key Files Reference

**Configuration:**
- {list key config files}

**Entry Points:**
- {list entry point files}

**Core Logic:**
- {list critical files}

---

## External Resources

**Documentation:**
- Project README: {link}
- API docs: {link}
- Architecture diagrams: {link}

**Dependencies:**
- {Framework} docs: {link}
- {Library} docs: {link}

---

**Last Updated:** {date}
**Maintained by:** {team/person}
```

---

## Output File Template (Multi-Project Global)

```markdown
# Multi-Project Global Instructions

**Generated:** {date}
**Projects:** {list project names}
**Mode:** Multi-Project

---

## Purpose

This file contains rules and workflows applicable to ALL projects in this workspace.

**Project-specific instructions** are in each project's `.github/copilot-instructions.md`.

---

## Universal Rules

{Content from templates/base/universal-rules.md}

---

## Spec-Driven Development

{Content from templates/base/spec-driven-development.md}

---

## Cross-Project Sync Workflow

{Content from templates/multi-project/sync-workflow.md}

---

## Adaptation Patterns

{Content from templates/multi-project/adaptation-patterns.md}

---

## Shared Packages

{From Step 1, list all shared packages}

---

## Shared Coding Patterns

{Common patterns from Step 5 across all projects}

---

**Last Updated:** {date}
```

---

## Cleanup (Optional)

After successful generation, optionally clean up analysis files:

```bash
# Keep analysis for reference
mv {output_folder} {output_folder}.archive-{date}

# Or delete if not needed
# rm -rf {output_folder}
```

---

## Success Criteria

✅ Final instruction file(s) created
✅ All templates included
✅ Project-specific data integrated
✅ Proper Markdown formatting
✅ Code examples properly formatted
✅ Backups created (if applicable)
✅ Summary report generated

---

## Troubleshooting

**Issue:** Language template not found
**Solution:** Use `templates/base/languages/generic-patterns.md` as fallback

**Issue:** Previous analysis incomplete
**Solution:** Re-run missing steps (1-5)

**Issue:** File write permission denied
**Solution:** Check directory permissions, create `.github/` if needed

---

**Execution Complete! 🎉**

Your AI instruction file(s) are ready to use with GitHub Copilot, Cursor, Windsurf, or other AI coding assistants.
