# AI Project Prompts

**Status:** ✅ Ready for Testing
**Version:** 0.2.0
**Date:** January 24, 2026
**License:** MIT

---

## Purpose

Automated prompt chain system for generating project-specific AI instructions:
- **Single projects** - BASE templates + project analysis → `.github/copilot-instructions.md`
- **Multi-project setups** - Shared docs + cross-project workflows + sync automation

---

## Current Structure

```
new-project-ai-prompts/
├── README.md                         ← This file
├── chains/                           ← ✅ COMPLETED
│   ├── 0-detect-setup.md             ← Detect single/multi-project
│   ├── 1-determine-techstack.md      ← Analyze tech stack
│   ├── 2-categorize-files.md         ← Categorize by purpose (adaptive)
│   ├── 3-identify-architecture.md    ← Identify patterns (universal)
│   ├── 4-domain-deep-dive.md         ← Domain analysis (language-agnostic)
│   ├── 5-styleguide-generation.md    ← Extract code style (descriptive)
│   └── 6-build-instructions.md       ← Build final instructions
├── templates/                        ← ✅ COMPLETED
│   ├── base/
│   │   ├── universal-rules.md        ← Core rules for ALL projects
│   │   ├── spec-driven-development.md ← Feature spec workflow
│   │   └── languages/                ← Language-specific patterns
│   │       ├── nodejs-patterns.md    ← Node.js/JavaScript/TypeScript
│   │       ├── python-patterns.md    ← Python/Flask/Django/FastAPI
│   │       ├── csharp-patterns.md    ← C#/.NET/ASP.NET Core
│   │       ├── bash-patterns.md      ← Bash/Shell scripts
│   │       └── generic-patterns.md   ← Fallback for other languages
│   ├── multi-project/
│   │   ├── sync-workflow.md          ← Cross-project sync process
│   │   ├── adaptation-patterns.md    ← COPY/ADAPT/SKIP patterns
│   │   └── sync-status-tracker.md    ← Sync progress tracking
│   └── specs/
│       ├── feature-spec-single.md    ← Single project feature template
│       └── cross-project-spec.md     ← Multi-project sync template
```

---

## Templates Overview

### BASE Templates (Universal)

**`universal-rules.md`** (~500 lines)
- Language & Communication (English responses, English code)
- UI/UX Policy (ALWAYS discuss before implementation)
- Using Existing Code (NEVER invent APIs)
- Decision Making (USER DECIDES, AI EXECUTES)
- File Organization (`.results/` for docs)
- Terminal Best Practices
- Deletion Policy
- Error Handling Pattern
- Clean Code Principles
- Code Style (double quotes, English comments)
- Task Complexity Categories

**`spec-driven-development.md`** (~400 lines)
- When to use specs (4+ files, architecture changes)
- Spec workflow (Create → Approve → Implement → Archive)
- Full spec template with TODO lists
- Progress reporting examples
- Change Log format
- Daily summaries

---

### Multi-Project Templates

**`sync-workflow.md`** (~300 lines)
- USER DECIDES, AI EXECUTES principle
- 6-step sync process (Read → Map → Approve → Implement → Update)
- File Mapping Table template
- COPY/ADAPT/SKIP actions
- Confidence levels (🟢🟡🔴)

**`adaptation-patterns.md`** (~250 lines)
- Pattern documentation template
- Quick reference table
- Detection decision tree
- Confidence level guidelines
- Common pitfalls catalog

**`sync-status-tracker.md`** (~150 lines)
- Active/Planned/Completed sync tables
- Feature detail sections
- Complexity levels (Low/Medium/High)
- Progress tracking format

---

### Spec Templates

**`feature-spec-single.md`** (~150 lines)
- Overview, Requirements, Technical Design
- TODO List, Acceptance Criteria
- Change Log with daily summaries
- For single-project features

**`cross-project-spec.md`** (~250 lines)
- Project mapping
- File Mapping Table (COPY/ADAPT/SKIP)
- Source → Target adaptations
- Dependency verification
- For cross-project sync operations

---

## Usage

### Main Prompt (Copy & Paste to AI Agent)

> **⚠️ IMPORTANT:** Edit these values BEFORE running the prompt:
> - **Choose ONE scenario:** Keep only single-project OR multi-project line (delete the other)
> - Replace `["~/my-project"]` with your actual project path
>   - For current directory: `["."]`
>   - For absolute path: `["~/projects/app"]` or `["D:/MyProjects/app"]`
>   - For relative path: `["../other-project"]`
> - For multi-project: Replace `["~/project-a", "~/project-b"]` with your actual project paths
> - Adjust `{output_folder}` if needed (default: `.results`)
> - Set `{shared_docs_path}` for multi-project setups

```
{output_folder} = .results
{final_output_file} = .github/copilot-instructions.md
{project_folders} = ["~/my-project"]  # Single project - REPLACE with your path!
# OR
{project_folders} = ["~/project-a", "~/project-b"]  # Multi-project - REPLACE with your paths!
{shared_docs_path} = ~/.shared-docs  # For multi-project only

You are assisting with generating AI coding instructions using a multi-step prompt chain.

1. Read all prompt files from GitHub:
   https://github.com/ale4ko69/new-project-ai-prompts/tree/main/chains/

2. Review all prompt files (0-6) in the chains/ folder.

3. Review all prompt files (0-6) WITHOUT executing them.
   - This will help you understand the full scope of the prompt chain.

4. Confirm you have a full understanding of the chain sequence.

5. Once familiar with the flow, execute prompts in numerical order:
   - 0-detect-setup.md          (Detect single vs multi-project)
   - 1-determine-techstack.md    (Analyze tech stack)
   - 2-categorize-files.md       (Categorize files by purpose)
   - 3-identify-architecture.md  (Identify patterns)
   - 4-domain-deep-dive.md       (Understand domain logic)
   - 5-styleguide-generation.md  (Extract code style)
   - 6-build-instructions.md     (Generate final file)

6. For each step, output results to `{output_folder}/`:
   - Example: 1-determine-techstack.md → {output_folder}/1-techstack.md

7. Use BASE templates from `templates/` folder:
   - templates/base/universal-rules.md
   - templates/base/spec-driven-development.md
   - templates/base/languages/{detected-language}-patterns.md (from Step 1)
   - templates/multi-project/* (if multi-project detected)

8. Final output location:
   - Single project: {final_output_file}
   - Multi-project: {shared_docs_path}/*.md + per-project {final_output_file}

9. IMPORTANT: If {final_output_file} exists, create backup first:
   - Backup: {final_output_file}.backup-YYYY-MM-DD-HHmmss.md
   - Notify user about backup creation

Stop ONLY when:
- All steps (0-6) are complete
- All BASE templates are integrated
- Full {final_output_file} is generated
- User is notified of completion
```

---

## How It Works

### Single Project Mode

```
Input: {project_folder}

Step 0: Detect single-project setup
Steps 1-5: Analyze project (tech stack, architecture, domain, style)
Step 6: Combine BASE templates + analysis → .github/copilot-instructions.md

Backup: If file exists → .github/copilot-instructions.backup-YYYY-MM-DD-HHmmss.md
```

---

### Multi-Project Mode

```
Input: {project_folders} (multiple), {shared_docs_path}

Step 0: Detect multi-project setup

Create shared docs:
  ~/.shared-docs/global-rules.md (from universal-rules.md)
  ~/.shared-docs/using-existing-code.md (from CRITICAL template)
  ~/.shared-docs/sync-workflow.md (from sync-workflow.md)
  ~/.shared-docs/adaptation-patterns.md (from adaptation-patterns.md)

Steps 1-5: Analyze each project

Step 6: Create project-specific instructions
  project1/.github/copilot-instructions.md
    → Links to ~/.shared-docs/
    → Project-specific overrides (from analysis)

  project2/.github/copilot-instructions.md
    → Links to ~/.shared-docs/
    → Project-specific overrides (from analysis)
```

---

## Formula

### Single Project:
```
BASE universal-rules.md
  + Chain analysis (steps 1-5)
  + Project-specific patterns
  ↓
.github/copilot-instructions.md
```

### Multi-Project:
```
BASE templates → ~/.shared-docs/
  ├─ global-rules.md
  ├─ using-existing-code.md
  ├─ sync-workflow.md
  └─ adaptation-patterns.md

Chain analysis (per project) → project/.github/copilot-instructions.md
  ├─ Links to shared docs
  └─ Project-specific overrides
```

---

## Key Features

1. ✅ **Multi-Project Support** - First public example for interconnected projects
2. ✅ **COPY vs ADAPT Patterns** - Catalog of cross-project adaptations
3. ✅ **USER DECIDES Principle** - Explicit approval workflows
4. ✅ **Critical Rules** - Anti-pattern prevention and NEVER invent APIs policy
5. ✅ **Cross-Project Commands** - 40+ ready terminal commands
6. ✅ **File Mapping Tables** - Structured sync decision-making
7. ✅ **Language Flexibility** - English responses by default (customizable)
8. ✅ **6-Step Analysis Chain** - Tech stack, architecture, domain, style guide
9. ✅ **Spec-Driven Development** - Feature specs with TODO tracking
10. ✅ **Automated Sync Workflow** - File mapping with confidence levels

---

## Progress Status

### ✅ Completed

- [x] Analyzed existing documentation (8 files)
- [x] Separated UNIVERSAL vs PROJECT-SPECIFIC rules
- [x] Created BASE templates (2 files)
- [x] Created MULTI-PROJECT templates (3 files)
- [x] Created SPEC templates (2 files)
- [x] Created LANGUAGE templates (5 files: Node.js, Python, C#, Bash, Generic)
- [x] Created prompt chains (7 files: steps 0-6)
- [x] Documented template structure

### 🚧 In Progress

- [ ] Test on real project (scadafence)
- [ ] Validate multi-project workflow
- [ ] Refine based on test results

### ⏳ Planned

- [ ] Create GitHub repository
- [ ] Write usage examples
- [ ] Document real-world sync case (SLDAP)
- [ ] Blog post / documentation
- [ ] Open source release

---

## Next Steps

1. **Test chain execution** ← CURRENT
   - Run on scadafence project (CI)
   - Verify generated instructions
   - Compare with existing .copilot-instructions.md
   - Test language template selection

2. **Validate multi-project workflow**
   - Test on scadafence (CI) + sf-multisite (CW)
   - Verify cross-project sync detection
   - Test File Mapping Table generation

3. **Document usage**
   - Create USAGE.md
   - Add examples
   - Write CONTRIBUTING.md

4. **Prepare for GitHub**
   - Choose repository name
   - Write comprehensive README
   - Add LICENSE (MIT)

---

## Documentation

- **chains/** - 7-step prompt chain for automated analysis
- **templates/base/** - Universal rules + language-specific patterns
- **templates/multi-project/** - Cross-project sync workflows
- **templates/specs/** - Feature specification templates

---

## Prerequisites

- AI coding assistant (GitHub Copilot, Claude, ChatGPT, etc.)
- Access to project files you want to analyze
- Basic understanding of your project's structure

---

## Installation

1. Clone or download this repository to `~/.shared-docs/` (recommended) or any location
2. Optional: Add to `.bashrc` or `.zshrc`:
   ```bash
   export AI_PROMPTS_PATH=~/.shared-docs/new-project-ai-prompts
   ```

---

## Examples

### Single Project Example
```
{project_folders} = ["~/projects/my-web-app"]
```

### Multi-Project Example
```
{project_folders} = ["~/projects/backend-api", "~/projects/frontend-app"]
{shared_docs_path} = ~/.shared-docs
```

---

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Test your changes
4. Submit a pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines (coming soon).

---

## License

This project is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2026 ale4ko69

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Author

**ale4ko69** - Initial work and maintenance

- GitHub: [@ale4ko69](https://github.com/ale4ko69)
- Repository: [new-project-ai-prompts](https://github.com/ale4ko69/new-project-ai-prompts)

---

## Acknowledgments

- Inspired by real-world multi-project development challenges
- Built for improving AI-assisted development workflows
- Community feedback and contributions welcome

---

## Support

- 📖 [Documentation](README.md)
- 🐛 [Issues](https://github.com/ale4ko69/new-project-ai-prompts/issues)
- 💬 [Discussions](https://github.com/ale4ko69/new-project-ai-prompts/discussions)

---

**Last Updated:** 2026-01-24
**Version:** 0.2.0 (Ready for Testing)
