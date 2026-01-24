# Templates Documentation

**AI Prompt Chain Generator** uses a modular template system to generate project-specific AI instructions.

---

## 📑 Table of Contents

- [BASE Templates (Universal)](#base-templates-universal)
- [Multi-Project Templates](#multi-project-templates)
- [Spec Templates](#spec-templates)
- [Language-Specific Templates](#language-specific-templates)

---

## BASE Templates (Universal)

Templates applicable to **ALL** projects (single or multi).

### `universal-rules.md` (~600 lines)

Core rules for AI coding assistants:

**Content Sections:**
- **Language & Communication** - English responses, English code comments
- **UI/UX Policy** - ALWAYS discuss design decisions before implementation
- **Using Existing Code** - NEVER invent APIs or endpoints
- **Decision Making** - USER DECIDES, AI EXECUTES principle
- **File Organization** - `.results/` for documentation, structured outputs
- **Terminal Best Practices** - Cross-platform command support
- **Deletion Policy** - Ask before deleting, backup critical files
- **Error Handling Pattern** - Consistent error handling approach
- **Clean Code Principles** - SOLID, DRY, KISS, readability
- **Code Style** - Double quotes, English comments, consistent formatting
- **Task Complexity Categories** - Simple, Medium, Complex task workflows
- **Version Management & Updates** - Automatic update detection, safe migration

**Key Principles:**
- Ask before acting when requirements are unclear
- Study project structure before making changes
- Discuss UI/UX concepts before implementation
- Preserve existing patterns and conventions

---

### `spec-driven-development.md` (~400 lines)

Feature specification workflow for complex tasks:

**When to Use:**
- Features requiring 4+ files
- Architecture changes
- Cross-cutting concerns
- Multi-step implementations

**Workflow:**
1. **Create Spec** - Define requirements, technical design, TODO list
2. **Approve Spec** - User reviews and approves
3. **Implement** - Execute according to spec
4. **Update Progress** - Daily change logs, TODO tracking
5. **Archive** - Move to `.results/specs/archive/` when complete

**Spec Template Includes:**
- Overview and objectives
- Requirements (functional, non-functional, constraints)
- Technical design (architecture, components, data flow)
- TODO list with status tracking
- Acceptance criteria
- Change log with daily summaries
- Progress reporting format

---

## Multi-Project Templates

Templates for managing interconnected projects with shared code.

### `sync-workflow.md` (~300 lines)

Process for syncing features across projects:

**6-Step Process:**
1. **Read Source** - Analyze source project files
2. **Map Files** - Create File Mapping Table
3. **Approve** - User reviews and approves mapping
4. **Implement** - Execute COPY/ADAPT/SKIP actions
5. **Update Tracking** - Update sync status
6. **Verify** - Test and validate sync

**File Mapping Table:**
| Source File | Target File | Action | Confidence | Adaptations Required |
|-------------|-------------|---------|-----------|---------------------|
| src/auth.ts | lib/auth.js | ADAPT | 🟡 Medium | TypeScript → JS, update imports |

**Actions:**
- **COPY** - Direct copy, minimal changes
- **ADAPT** - Modify for target project (imports, configs, framework differences)
- **SKIP** - Not applicable, explain why

**Confidence Levels:**
- 🟢 **High** - Direct copy or simple adaptations
- 🟡 **Medium** - Moderate adaptations required
- 🔴 **Low** - Significant changes, manual review needed

---

### `adaptation-patterns.md` (~250 lines)

Catalog of common cross-project adaptation patterns:

**Pattern Documentation:**
- Pattern name and description
- When to use (detection criteria)
- Confidence level guidelines
- Code examples (before → after)
- Common pitfalls and solutions

**Examples:**
- Framework API differences (Express → Fastify)
- Import path transformations
- Configuration file formats
- Database client differences
- Authentication/authorization patterns

**Quick Reference Table:**
| Pattern | Detection | Confidence | Notes |
|---------|-----------|-----------|-------|
| Framework API | Import statements | 🟡 Medium | Check API compatibility |

---

### `sync-status-tracker.md` (~150 lines)

Progress tracking for cross-project syncs:

**Sections:**
- **Completed Syncs** - Successfully merged features
- **Active Syncs** - Currently in progress
- **Planned Syncs** - Scheduled for future

**Tracking Format:**
| Feature | Source → Target | Status | Date | Files | Spec |
|---------|----------------|--------|------|-------|------|
| Auth | Project-A → B | ✅ Done | 2026-01-20 | 5 | [Link] |

**Complexity Levels:**
- **Low** - 1-3 files, simple COPY
- **Medium** - 4-10 files, moderate ADAPT
- **High** - 10+ files, significant changes, multiple ADAPT patterns

---

## Spec Templates

Templates for feature specifications.

### `feature-spec-single.md` (~150 lines)

Single-project feature specification:

**Sections:**
1. **Overview** - Feature description, objectives
2. **Requirements** - Functional, non-functional, constraints
3. **Technical Design** - Architecture, components, data flow
4. **TODO List** - Task breakdown with status (🔴 Not Started, 🔄 In Progress, ✅ Done)
5. **Acceptance Criteria** - Definition of done
6. **Change Log** - Daily updates and progress summaries

**Use Case:**
- New feature development
- Refactoring existing code
- Performance optimizations
- Bug fixes requiring multiple changes

---

### `cross-project-spec.md` (~250 lines)

Multi-project sync specification:

**Additional Sections:**
- **Project Mapping** - Source and target projects
- **File Mapping Table** - Detailed COPY/ADAPT/SKIP decisions
- **Adaptation Details** - Specific changes per file
- **Dependency Verification** - Check shared packages, versions
- **Testing Strategy** - How to validate sync

**Use Case:**
- Syncing features across projects
- Porting functionality to different frameworks
- Sharing common utilities
- Standardizing patterns across codebase

---

## Language-Specific Templates

Located in `templates/base/languages/`, these templates provide language/framework-specific patterns:

### `nodejs-patterns.md` (~400 lines)
- **Frameworks:** Express, Fastify, NestJS, Next.js, React
- **Patterns:** Async/await, error handling, middleware, REST APIs
- **Testing:** Jest, Mocha, Supertest
- **Package Management:** npm, yarn, pnpm

### `python-patterns.md` (~350 lines)
- **Frameworks:** Flask, Django, FastAPI, Celery
- **Patterns:** Context managers, decorators, type hints
- **Testing:** pytest, unittest, mock
- **Package Management:** pip, poetry, conda

### `csharp-patterns.md` (~350 lines)
- **Frameworks:** ASP.NET Core, Entity Framework, Blazor
- **Patterns:** LINQ, async/await, dependency injection
- **Testing:** xUnit, NUnit, Moq
- **Package Management:** NuGet

### `bash-patterns.md` (~200 lines)
- **Use Cases:** Build scripts, deployment, automation
- **Patterns:** Error handling, parameter parsing, piping
- **Best Practices:** Quoting, shellcheck, portability

### `generic-patterns.md` (~200 lines)
- **Fallback** for languages without specific templates
- **Universal Patterns:** Code organization, naming conventions
- **Adaptable Rules:** Language-agnostic best practices

---

## Version Tracking

### `.ai-prompt-chain-generator.json`

Auto-generated version tracking file:

```json
{
  "version": "0.2.0",
  "generator": "AI Prompt Chain Generator",
  "repository": "https://github.com/ale4ko69/new-project-ai-prompts",
  "generatedAt": "2026-01-24T10:30:00Z",
  "projectMode": "single",
  "configuration": {
    "outputFolder": ".results",
    "finalOutputFile": ".github/copilot-instructions.md",
    "projectFolders": ["~/my-project"]
  },
  "components": {
    "steps": ["0-detect-setup", "1-determine-techstack", ...],
    "templates": {
      "base": ["universal-rules", "spec-driven-development"],
      "language": "nodejs-patterns",
      "multiProject": []
    }
  },
  "techStack": {
    "primaryLanguage": "Node.js/TypeScript",
    "framework": "Express.js",
    "packageManager": "npm"
  },
  "customModifications": false,
  "notes": "Track local changes in this field"
}
```

**Purpose:**
- Tracks PCG version used to generate instructions
- Enables automatic update detection
- Preserves customization flags
- Records project configuration

**Examples:**
- `.ai-prompt-chain-generator.json.example` - Single project
- `.ai-prompt-chain-generator-multi.json.example` - Multi-project

---

## Usage in Prompt Chain

**Step 7 (build-instructions.md)** combines templates:

1. **Load BASE templates:**
   - `universal-rules.md` (always)
   - `spec-driven-development.md` (always)
   - Language-specific from Step 1 analysis

2. **Load Multi-Project templates** (if applicable):
   - `sync-workflow.md`
   - `adaptation-patterns.md`
   - `sync-status-tracker.md`

3. **Load Spec templates** (referenced in instructions):
   - `feature-spec-single.md`
   - `cross-project-spec.md`

4. **Combine with analysis:**
   - Steps 0-6 results (tech stack, architecture, domain, style, dependencies)
   - Project-specific overrides
   - Dependency maintenance schedule

5. **Generate outputs:**
   - `.github/copilot-instructions.md`
   - `.ai-prompt-chain-generator.json`
   - Multi-project: `~/.shared-docs/*.md`

---

**Last Updated:** 2026-01-24
**Version:** 0.2.0
