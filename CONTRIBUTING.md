# Contributing to Prompt Chain Generator

First off, thank you for considering contributing to Prompt Chain Generator! 🎉

It's people like you that make this tool better for everyone working with AI coding assistants.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Improving Documentation](#improving-documentation)
  - [Adding Language Templates](#adding-language-templates)
  - [Contributing to Chains](#contributing-to-chains)
- [Development Process](#development-process)
  - [Setting Up](#setting-up)
  - [Making Changes](#making-changes)
  - [Testing Your Changes](#testing-your-changes)
  - [Submitting Changes](#submitting-changes)
- [Style Guidelines](#style-guidelines)
  - [Markdown Style](#markdown-style)
  - [Prompt Writing Guidelines](#prompt-writing-guidelines)
  - [Commit Messages](#commit-messages)
- [Project Structure](#project-structure)

---

## Code of Conduct

This project and everyone participating in it is governed by our commitment to creating a welcoming and respectful environment. By participating, you are expected to uphold this standard.

**Expected Behavior:**
- Be respectful and inclusive
- Welcome newcomers and help them get started
- Accept constructive criticism gracefully
- Focus on what is best for the community

**Unacceptable Behavior:**
- Harassment or discriminatory language
- Trolling or insulting comments
- Personal or political attacks
- Publishing others' private information

---

## How Can I Contribute?

### Reporting Bugs

Found a bug? Help us fix it!

**Before submitting a bug report:**
- Check the [Issues](https://github.com/ale4ko69/ai-prompt-chain-generator/issues) to see if it's already reported
- Test with the latest version from `main` branch
- Gather information about the bug

**How to submit a bug report:**

1. Open a new issue with the label `bug`
2. Use a clear and descriptive title
3. Include:
   - Steps to reproduce
   - Expected behavior
   - Actual behavior
   - Screenshots (if applicable)
   - Environment details (AI assistant, OS, project type)
   - Example project structure (if relevant)

**Example:**
```markdown
**Bug:** Step 6 dependency audit fails for Go projects

**Steps to reproduce:**
1. Run chain on Go project with `go.mod`
2. Execute Step 6
3. Error: "govulncheck not found"

**Expected:** Should offer to install or skip
**Actual:** Script fails without prompt

**Environment:**
- AI Assistant: GitHub Copilot
- OS: Windows 11
- Go version: 1.21
```

---

### Suggesting Enhancements

Have an idea to improve the project? We'd love to hear it!

**Before suggesting:**
- Check existing [Issues](https://github.com/ale4ko69/ai-prompt-chain-generator/issues) and [Discussions](https://github.com/ale4ko69/ai-prompt-chain-generator/discussions)
- Consider if it fits the project's scope
- Think about how it benefits other users

**How to suggest an enhancement:**

1. Open a new issue with the label `enhancement`
2. Provide:
   - Clear description of the enhancement
   - Use case / problem it solves
   - Proposed solution or implementation
   - Alternative solutions considered
   - Examples (if applicable)

**Example:**
```markdown
**Enhancement:** Add support for Rust projects

**Use Case:** Many projects now use Rust for performance-critical components

**Proposed Solution:**
- Add `templates/base/languages/rust-patterns.md`
- Update Step 1 to detect `Cargo.toml`
- Add Rust-specific patterns (ownership, traits, error handling)

**Alternatives:**
- Use generic-patterns.md (less specific)
```

---

### Improving Documentation

Documentation improvements are always welcome!

**Areas to contribute:**
- Fix typos or clarify confusing sections
- Add examples for different project types
- Translate documentation (future)
- Improve README sections
- Add tutorials or guides

**How to contribute documentation:**
1. Fork the repository
2. Make changes to markdown files
3. Submit a Pull Request with label `documentation`

---

### Adding Language Templates

Want to add support for a new programming language?

**Required components:**

1. **Language patterns file:** `templates/base/languages/{language}-patterns.md`

Must include:
- Language-specific best practices
- Framework conventions (if applicable)
- Common patterns and anti-patterns
- Error handling style
- Testing approaches
- Package/dependency management
- Code organization standards

2. **Update Step 1:** Modify `chains/1-determine-techstack.md` to detect the language

3. **Examples:** Provide at least 2-3 real-world examples

**See existing templates:**
- `nodejs-patterns.md` - Good example of framework coverage
- `python-patterns.md` - Shows multiple framework patterns
- `csharp-patterns.md` - Enterprise patterns example

---

### Contributing to Chains

Improving analysis chains helps everyone!

**Current chains (Steps 0-7):**
- `0-detect-setup.md` - Project mode detection
- `1-determine-techstack.md` - Technology analysis
- `2-categorize-files.md` - File categorization
- `3-identify-architecture.md` - Architecture patterns
- `4-domain-deep-dive.md` - Domain analysis
- `5-styleguide-generation.md` - Style extraction
- `6-dependency-audit.md` - Dependency health
- `7-build-instructions.md` - Final assembly

**How to improve chains:**
- Add detection for new patterns
- Improve analysis accuracy
- Add error handling
- Expand examples
- Add edge case handling

**Testing chains:**
Test on multiple project types:
- Small projects (<10 files)
- Medium projects (10-100 files)
- Large projects (100+ files)
- Monorepos
- Multi-language projects

---

## Development Process

### Setting Up

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/ai-prompt-chain-generator.git
   cd ai-prompt-chain-generator
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

### Making Changes

1. **Make your changes** in the appropriate files
2. **Follow style guidelines** (see below)
3. **Test thoroughly** (see testing section)
4. **Update documentation** if needed
5. **Update version if needed** (for significant changes)

### Testing Your Changes

**For template changes:**
- Test with at least 2-3 different projects
- Verify generated instructions are clear and accurate
- Check markdown formatting renders correctly

**For chain changes:**
- Run the full chain (Steps 0-7)
- Test on multiple project types
- Verify output files are generated correctly
- Check error handling works

**For language template additions:**
- Test with real projects using that language
- Verify patterns are accurate and up-to-date
- Include framework-specific patterns
- Test with different project structures

**Test checklist:**
```markdown
- [ ] Tested on single-project setup
- [ ] Tested on multi-project setup
- [ ] Verified markdown syntax is correct
- [ ] Checked for typos
- [ ] Validated with real project(s)
- [ ] Generated instructions are accurate
- [ ] No broken links or references
```

### Submitting Changes

1. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add Rust language template"
   ```
   (See [Commit Messages](#commit-messages) for conventions)

2. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

3. **Create Pull Request**
   - Go to the original repository
   - Click "New Pull Request"
   - Select your branch
   - Fill in the PR template:
     - Description of changes
     - Related issues (if any)
     - Testing performed
     - Screenshots (if UI changes)

4. **Respond to feedback**
   - Be open to suggestions
   - Make requested changes
   - Push updates to the same branch

---

## Style Guidelines

### Markdown Style

**General:**
- Use ATX-style headers (`#` instead of underlines)
- One blank line between sections
- Use code fences with language identifiers
- Keep lines under 120 characters when possible

**Headers:**
```markdown
# Top Level (only one per file)
## Second Level
### Third Level
```

**Lists:**
- Use `-` for unordered lists
- Use `1.` for ordered lists
- Indent nested lists with 2 spaces

**Code blocks:**
````markdown
```bash
# Use language identifiers
npm install
```
````

**Links:**
```markdown
[descriptive text](url)
[internal section](#section-name)
```

### Prompt Writing Guidelines

**For chain prompts:**

1. **Clear purpose statement**
   ```markdown
   **Purpose:** [What this step does]

   **Parameters:**
   - {parameter_name} - Description

   **Dependencies:**
   - Previous steps required
   ```

2. **Step-by-step instructions**
   - Number steps clearly
   - One action per step
   - Include examples
   - Explain why, not just what

3. **User interaction points**
   - Clearly mark when to ask user
   - Provide options/choices
   - Explain implications of choices

4. **Error handling**
   - What to do if command fails
   - Fallback options
   - Clear error messages

**For template prompts:**

1. **Context first**
   - When to use this pattern
   - Why it matters
   - Common scenarios

2. **Concrete examples**
   - Real-world code samples
   - Before/after comparisons
   - Anti-patterns to avoid

3. **Framework-specific sections**
   - Clear separation
   - Version notes if relevant

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

**Format:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation only
- `style:` - Formatting, missing semicolons, etc.
- `refactor:` - Code change that neither fixes a bug nor adds a feature
- `test:` - Adding tests
- `chore:` - Updating build tasks, package manager configs, etc.

**Examples:**
```bash
feat(chains): add Step 6 dependency audit

Add comprehensive dependency checking with security audit,
outdated package detection, and monorepo support.

Closes #42

---

fix(templates): correct Node.js async/await pattern

Fixed example showing incorrect error handling with async/await.

---

docs(readme): update installation instructions

Added Prerequisites section and clarified usage examples.

---

feat(languages): add Rust language template

Add Rust-specific patterns including ownership, traits,
and error handling with Result/Option types.
```

---

## Project Structure

Understanding the structure helps you contribute effectively:

```
ai-prompt-chain-generator/
├── README.md                    # Main documentation
├── CONTRIBUTING.md              # This file
├── LICENSE                      # MIT License
├── assets/                      # Images, logos
│   └── logo-pcg-350.png
├── chains/                      # Analysis chain steps
│   ├── 0-detect-setup.md        # Project mode detection
│   ├── 1-determine-techstack.md # Tech stack analysis
│   ├── 2-categorize-files.md    # File categorization
│   ├── 3-identify-architecture.md # Architecture patterns
│   ├── 4-domain-deep-dive.md    # Domain analysis
│   ├── 5-styleguide-generation.md # Code style extraction
│   ├── 6-dependency-audit.md    # Dependency health check
│   └── 7-build-instructions.md  # Final assembly
└── templates/
    ├── base/
    │   ├── universal-rules.md   # Core rules for all projects
    │   ├── spec-driven-development.md # Feature spec workflow
    │   └── languages/           # Language-specific patterns
    │       ├── nodejs-patterns.md
    │       ├── python-patterns.md
    │       ├── csharp-patterns.md
    │       ├── bash-patterns.md
    │       └── generic-patterns.md
    ├── multi-project/
    │   ├── sync-workflow.md     # Cross-project sync
    │   ├── adaptation-patterns.md # COPY/ADAPT/SKIP
    │   └── sync-status-tracker.md # Progress tracking
    └── specs/
        ├── feature-spec-single.md # Single project features
        └── cross-project-spec.md  # Multi-project sync
```

**File purposes:**

- **chains/** - Sequential analysis steps executed by AI
- **templates/base/** - Universal templates included in all outputs
- **templates/multi-project/** - Templates for interconnected projects
- **templates/specs/** - Feature specification templates
- **templates/base/languages/** - Language-specific coding patterns

---

## Questions?

- **General questions:** [Discussions](https://github.com/ale4ko69/ai-prompt-chain-generator/discussions)
- **Bug reports:** [Issues](https://github.com/ale4ko69/ai-prompt-chain-generator/issues)
- **Feature requests:** [Issues](https://github.com/ale4ko69/ai-prompt-chain-generator/issues) with `enhancement` label

---

## Recognition

All contributors will be recognized in our:
- GitHub contributors list
- Project acknowledgments (future releases)

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for making Prompt Chain Generator better!** 🚀

---

**Last Updated:** 2026-01-24
