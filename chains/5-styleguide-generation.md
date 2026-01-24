# Step 5: Styleguide Generation

**Purpose:** Extract and document the actual coding style, conventions, and patterns used in the project.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

**Dependencies:**
- Read `{output_folder}/1-determine-techstack.md` for language/framework

---

## ⚠️ CRITICAL PRINCIPLE

**DO NOT prescribe or assume coding style.**
**ANALYZE existing code and EXTRACT actual patterns.**

This is a **descriptive, not prescriptive** step.

---

## Instructions

### 1. Language-Specific Style Analysis

Based on the primary language from Step 1, analyze appropriate style aspects:

#### **JavaScript / TypeScript**

**Quote Style:**
```bash
# Count single vs double quotes
grep -r "import.*['\"]" . | grep -c "'"
grep -r "import.*['\"]" . | grep -c '"'
```

**Analyze:**
- 'Single quotes' vs "Double quotes"
- Percentage distribution
- Document actual usage

**Semicolons:**
```bash
# Check if semicolons are used
grep -r "^import.*;" . | wc -l
grep -r "^const.*;" . | wc -l
```

**Analyze:**
- Semicolons used vs omitted
- Consistency across files

**Naming Conventions:**
```bash
# Check function naming
grep -r "function [a-z]" . | head -20
grep -r "const [a-z].*= (" . | head -20
```

**Analyze:**
- camelCase vs snake_case
- Function naming patterns
- Variable naming patterns
- Component naming (PascalCase vs camelCase)

**Code Formatting:**
- Indentation: tabs vs spaces (2 vs 4)
- Line length limits
- Trailing commas in arrays/objects
- Arrow function parentheses

**Check for:**
- `.prettierrc`, `.eslintrc`, `tsconfig.json`
- Pre-commit hooks
- Actual code samples

#### **Python**

**PEP8 Compliance:**
```bash
# Check if using flake8, black, pylint
find . -name ".flake8" -o -name "pyproject.toml" -o -name "setup.cfg"
```

**Analyze:**
- Line length (79, 88, 100, 120?)
- Indentation (spaces only, 4 spaces standard)
- Import organization (stdlib, third-party, local)

**Naming Conventions:**
```bash
# Check function/class naming
grep -r "^def [a-z_]" . | head -20
grep -r "^class [A-Z]" . | head-20
```

**Analyze:**
- snake_case for functions/variables
- PascalCase for classes
- UPPER_CASE for constants

**Type Hints:**
```bash
# Check usage of type hints
grep -r "def.*->.*:" . | wc -l
grep -r ": str\|: int\|: bool" . | wc -l
```

**Analyze:**
- Percentage of functions with type hints
- Return type annotations

**Documentation:**
```bash
# Check docstring style
grep -A5 '"""' . | head -50
```

**Analyze:**
- Google style vs NumPy style vs Sphinx style
- Docstring presence on functions/classes

#### **C# / .NET**

**Naming Conventions:**
```bash
# Check casing patterns
grep -r "public class" . | head -20
grep -r "public.*void" . | head -20
```

**Analyze:**
- PascalCase for classes, methods, properties
- camelCase for local variables
- _camelCase for private fields (leading underscore)

**Async Patterns:**
```bash
# Check async/await usage
grep -r "async Task" . | wc -l
grep -r "await " . | wc -l
```

**Analyze:**
- Async methods named with "Async" suffix
- Task vs Task<T> usage

**Documentation:**
```bash
# Check XML comments
grep -r "///" . | wc -l
```

**Analyze:**
- XML documentation on public methods
- Summary tags usage

#### **Bash / Shell Scripts**

**ShellCheck Compliance:**
```bash
# Check for shellcheck directives
grep -r "# shellcheck" . | head -20
```

**Analyze:**
- ShellCheck usage
- Common ignored warnings

**Function Style:**
```bash
# Check function definitions
grep -r "function [a-z]" . | head -20
grep -r "^[a-z_]*() {" . | head -20
```

**Analyze:**
- `function name()` vs `name()` syntax
- snake_case vs kebab-case

**Error Handling:**
```bash
# Check for error handling
grep -r "set -e" . | wc -l
grep -r "trap" . | wc -l
```

**Analyze:**
- `set -e` usage (exit on error)
- Trap handlers for cleanup

### 2. Code Organization Patterns

**File Naming:**
```bash
# Analyze file naming patterns
find lib/ -type f | head -30
find src/ -type f | head -30
```

**Analyze:**
- camelCase.js vs PascalCase.jsx vs kebab-case.js vs snake_case.py
- Index files vs named files
- Test file naming (.test.js, .spec.ts, _test.py)

**Import/Export Patterns:**
- ES Modules (`import/export`) vs CommonJS (`require/module.exports`)
- Named exports vs default exports
- Barrel exports (index.js re-exporting)
- Absolute vs relative imports

**Directory Structure:**
- Flat vs nested
- Feature-based vs type-based
- Separation of concerns

### 3. Documentation Style

**Code Comments:**
```bash
# Count comments
grep -r "^[[:space:]]*\/\/" . | wc -l    # JavaScript
grep -r "^[[:space:]]*#" . | wc -l      # Python/Bash
grep -r "^[[:space:]]*\/\/" . | wc -l    # C#
```

**Analyze:**
- Comment density (high, medium, low)
- Comment language (English, Russian, mixed)
- Comment style (inline, block, documentation)

**JSDoc / Docstrings / XML Comments:**
```bash
# JavaScript/TypeScript
grep -r "\/\*\*" . | wc -l

# Python
grep -r '"""' . | wc -l

# C#
grep -r "\/\/\/" . | wc -l
```

**Analyze:**
- Percentage of functions with documentation
- Documentation completeness (params, returns, examples)

**README Files:**
- Quality and detail level
- Sections included (installation, usage, architecture)

### 4. Error Handling Patterns

**JavaScript/TypeScript:**
```bash
# Check error handling patterns
grep -r "try {" . | wc -l
grep -r "\.catch(" . | wc -l
grep -r "catchError(" . | wc -l
```

**Analyze:**
- Try/catch blocks
- Promise `.catch()` usage
- Custom error handlers (e.g., `catchError()` pattern)
- Error classes (custom Error extensions)

**Python:**
```bash
grep -r "try:" . | wc -l
grep -r "except" . | wc -l
```

**C#:**
```bash
grep -r "try {" . | wc -l
grep -r "catch (" . | wc -l
```

### 5. Testing Patterns

**Test File Organization:**
```bash
# Find test files
find . -name "*.test.js" -o -name "*.spec.ts" -o -name "*_test.py"
```

**Analyze:**
- Test file locations (alongside source vs separate test/ directory)
- Naming conventions
- Test framework (Jest, Mocha, pytest, xUnit)

**Test Structure:**
- describe/it (Jest, Mocha)
- test functions (pytest)
- Test class organization (xUnit)

**Mocking:**
- Mock usage patterns
- Dependency injection for testability

### 6. Git Conventions

**Commit Messages:**
```bash
# Analyze recent commit messages
git log --oneline -30
```

**Analyze:**
- Conventional commits (feat:, fix:, docs:)
- Message style (imperative, past tense)
- Length and detail

**Branch Naming:**
```bash
git branch -a | head -20
```

**Analyze:**
- feature/, bugfix/, hotfix/ prefixes
- Naming patterns

### 7. Multi-Project Style Consistency (if applicable)

Compare coding styles between projects:

**Similarities:**
- Shared style guide adherence
- Consistent naming conventions

**Differences:**
- Different quote styles (Project 1: double, Project 2: single)
- Different error handling (Project 1: catchError, Project 2: try/catch)
- Different validation libraries (Project 1: Joi, Project 2: Yup)

**Document both** - future syncs need to adapt.

---

## Output Format

Save to `{output_folder}/5-styleguide-generation.md`:

```markdown
# Code Style Guide (Extracted from Codebase)

## Overview
This style guide is **descriptive** - it documents the **actual patterns** found in the codebase, not prescribed standards.

## Language: {Primary Language from Step 1}

---

## Code Formatting

### Quote Style
**Pattern:** {Single quotes | Double quotes | Mixed}
**Analysis:**
- Single quotes: {count} instances ({percentage}%)
- Double quotes: {count} instances ({percentage}%)
**Recommendation:** Use {predominant style}

### Semicolons
**Pattern:** {Always used | Never used | Inconsistent}
**Analysis:** {percentage}% of statements use semicolons

### Indentation
**Pattern:** {2 spaces | 4 spaces | Tabs}
**Line Length:** {No limit | 80 | 100 | 120} characters

### Trailing Commas
**Pattern:** {Always | Never | ES5-compatible (objects/arrays only)}

### Configuration Files
- `.prettierrc`: {exists | not found}
- `.eslintrc`: {exists | not found}
- `tsconfig.json`: {exists | not found}
- `.editorconfig`: {exists | not found}

---

## Naming Conventions

### Files
**Pattern:** {camelCase.js | PascalCase.jsx | kebab-case.ts | snake_case.py}
**Examples:**
- `{example1.js}`
- `{example2.ts}`

**Components:** {PascalCase.jsx | index.js in PascalCase folder}

**Tests:** {*.test.js | *.spec.ts | *_test.py}

### Variables
**Pattern:** {camelCase | snake_case | PascalCase}
**Constants:** {UPPER_SNAKE_CASE | camelCase}
**Private variables:** {_leadingUnderscore | no prefix}

### Functions
**Pattern:** {camelCase | snake_case}
**Async functions:** {Async suffix | no suffix}
**Examples:**
- `getUserById()` (JavaScript)
- `get_user_by_id()` (Python)
- `GetUserByIdAsync()` (C#)

### Classes
**Pattern:** {PascalCase}
**Examples:**
- `UserService`
- `AssetController`

### React Components
**Pattern:** {PascalCase}
**File organization:** {Component.jsx | Component/index.jsx}

---

## Code Organization

### Import/Export Style
**JavaScript/TypeScript:**
- Module system: {ES Modules | CommonJS | Mixed}
- Import style: {Named imports | Default imports | Mixed}
- Import path: {Relative | Absolute (with aliases)}

**Examples:**
```javascript
{actual import examples from codebase}
```

### File Structure
**Pattern:** {Feature-based | Type-based | Hybrid}
**Example:**
```
{actual directory structure}
```

---

## Documentation

### Code Comments
**Language:** {English | Russian | Mixed}
**Density:** {High | Medium | Low} - {percentage}% of files have comments
**Style:** {Inline // | Block /* */ | Documentation /** */}

### Function Documentation
**JavaScript/TypeScript:**
- JSDoc usage: {High | Medium | Low} - {percentage}% of functions documented
- **Style:**
  ```javascript
  {actual JSDoc example from codebase}
  ```

**Python:**
- Docstring usage: {High | Medium | Low}
- **Style:** {Google | NumPy | Sphinx}
  ```python
  {actual docstring example}
  ```

**C#:**
- XML comments usage: {High | Medium | Low}
  ```csharp
  {actual XML comment example}
  ```

### README Files
**Quality:** {Comprehensive | Basic | Minimal}
**Sections:** {Installation, Usage, Architecture, Contributing, etc.}

---

## Error Handling

### Pattern
**JavaScript/TypeScript:**
```javascript
{actual error handling example from codebase}
// Example: Using catchError() pattern
return catchError(error, "functionName");
```

**Python:**
```python
{actual try/except pattern}
```

**C#:**
```csharp
{actual try/catch pattern}
```

### Custom Error Classes
- {List custom error classes if found}
- {Error inheritance patterns}

### Logging
**Library:** {Winston | Pino | console | logging | Serilog}
**Levels used:** {debug, info, warn, error}
**Pattern:**
```javascript
{actual logging example}
```

---

## Testing

### Test Framework
**Backend:** {Jest | Mocha | pytest | xUnit | NUnit}
**Frontend:** {Jest | Testing Library | Cypress}

### Test File Organization
**Location:** {Alongside source | Separate test/ directory}
**Naming:** {*.test.js | *.spec.ts | *_test.py}

### Test Structure
```javascript
{actual test example from codebase}
```

### Coverage
**Current coverage:** {percentage or qualitative}
**Areas well-tested:** {list}
**Areas needing tests:** {list}

---

## React/Frontend Patterns (if applicable)

### Component Structure
```jsx
{actual component example}
```

### State Management
**Pattern:** {Redux Thunk | Context API | Zustand | etc.}
**File organization:** {slices, stores, actions}

### Hooks Usage
**Common hooks:** {useState, useEffect, custom hooks}
**Custom hooks naming:** {useCamelCase}

### Props
**PropTypes:** {Used | Not used | TypeScript interfaces}
**Destructuring:** {In function params | In function body}

---

## Backend Patterns (if applicable)

### Route → Controller → Service → DAL
**Adherence:** {Strict | Mostly | Inconsistent}

**Example flow:**
```javascript
{actual example from codebase}
```

### Validation
**Library:** {Joi | Yup | Zod | FluentValidation | Pydantic}
**Location:** {lib/schemes/ | validators/ | schemas/}
**Pattern:**
```javascript
{actual validation example}
```

### Database Queries
**ORM/ODM:** {Mongoose | Sequelize | Entity Framework | SQLAlchemy}
**Pattern:**
```javascript
{actual query example}
```

---

## Configuration Management

### Environment Variables
**Files:** {.env | appsettings.json | config.py}
**Access pattern:**
```javascript
{actual config access example}
```

### Secrets
**Storage:** {Environment variables | Config service | Encrypted files}
**Pattern:** {Never committed, .gitignore includes .env}

---

## Multi-Project Style Comparison (if applicable)

### Project 1: {name}
**Quote style:** {pattern}
**Error handling:** {pattern}
**Validation:** {library}

### Project 2: {name}
**Quote style:** {pattern}
**Error handling:** {pattern}
**Validation:** {library}

### Differences to Note for Syncs
- {Difference 1}: Requires adaptation when syncing
- {Difference 2}: Can be copied as-is
- {Difference 3}: Needs manual review

---

## Git Conventions

### Commit Messages
**Style:** {Conventional commits | Free-form | Mixed}
**Examples:**
- `{actual commit message 1}`
- `{actual commit message 2}`

### Branch Naming
**Pattern:** {feature/name | feat-name | name}
**Examples:**
- `{actual branch 1}`
- `{actual branch 2}`

---

## Key Observations

### Consistent Patterns
- {Pattern 1 description}
- {Pattern 2 description}

### Inconsistent Areas
- {Area 1 needing standardization}
- {Area 2 needing standardization}

### Unique Conventions
- {Convention 1 specific to this project}
- {Convention 2 specific to this project}

---

## Recommendations for AI Code Generation

**DO:**
- Use {quote style} for strings
- Follow {naming convention} for variables/functions
- Include {documentation style} on new functions
- Use {error handling pattern} consistently
- Keep line length under {max length}
- Use {validation library} for new schemas

**DON'T:**
- Mix {style A} and {style B}
- Skip {documentation/tests/validation}
- Use deprecated {library/pattern}
- Create files in {restricted locations}

---

## Language-Specific Template
**Include:** `templates/base/languages/{language}-patterns.md`

## Next Steps
Proceed to Step 6: Build Instructions
```

---

## Analysis Tips

**Tools to Use:**
```bash
# Count patterns
grep -r "pattern" . | wc -l

# Sample files
find . -type f -name "*.js" | head -20

# Check configuration
cat .prettierrc
cat .eslintrc
cat tsconfig.json
```

**Avoid:**
- Assuming patterns based on framework defaults
- Prescribing "best practices" that aren't actually used
- Ignoring inconsistencies (document them instead)

**Key Principle:**
This is **code archaeology** - dig through the codebase to find what developers **actually do**, not what they **should do**.

---

**After completion, proceed to chains/6-build-instructions.md**
