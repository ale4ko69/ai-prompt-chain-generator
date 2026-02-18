### Agent Creation & Update Consent
Before creating or updating project-specific agent files (including during backup or automation), ALWAYS request explicit user consent. Clearly explain the advantages of individualized agents (project adaptation, automation, quality control, transparency, update support). Only proceed after user approval. This rule must be enforced in all automation logic and backup/update workflows.

# Universal Coding Rules (BASE Template)

**Purpose:** Core rules applicable to ALL projects (single or multi). For the up-to-date list of user commands for all agents (including subagents and chains), see [user-command-reference.md](../base/user-command-reference.md).
**Used in:** Every `.github/copilot-instructions.md` generated file

---

## Language & Communication

### Response Language
- **Primary:** English for responses (unless specified otherwise)
- **Code Comments:** ALWAYS English
- **Documentation:** English in code files

- **Deep Adaptation for Agents:** When creating or updating agent prompt files, ALWAYS adapt them to the specific goals, structure, and workflows of the current project. Avoid generic or abstract descriptions—clearly state project purpose, key scenarios, and required tools/skills for the agent. Review project files and documentation before finalizing any agent prompt.
- **Verify Before Stating:** ALWAYS check the actual content of a file or code before making any claims, conclusions, or recommendations.
- **Ask First:** If unclear, ALWAYS ask questions before writing code
- **Focused Answers:** Provide ONLY what was requested (e.g., if asked for jsDoc, don't implement the function)
- **Study Structure:** ALWAYS study project structure before starting work

### Documentation Review for Document-Centric Projects
In projects focused on documentation, code reviewers MUST check cross-references between files, consistency of section names, detect duplicate sections, and suggest improvements to text clarity and structure. This is required for all documentation-driven and template-based repositories.

### Questions to Ask
- "Where are components for [feature] located?"
- "Is there an existing method for [task]?"
- "Where should I place new [component/hook/utility]?"
- "What folder structure is used for [section]?"

---

## 🚨 CRITICAL: UI/UX Changes - Design Discussion FIRST

**UI changes require DESIGN DECISIONS, not just technical implementation.**

### ❌ FORBIDDEN Without Discussion:
- Adding UI components
- Choosing UI implementation method (checkboxes, toggles, wizards, tabs, accordion, modals...)
- Adding translations for non-existent UI elements
- Changing form layouts or validation logic
- Implementing complex UI patterns (wizards, steppers, conditional sections)

### ✅ CORRECT UI Change Process:

#### Step 1: DISCUSS UX Concept

**Before ANY code, present:**

```
🎨 UI/UX Discussion Required:

WHAT needs to be added/changed:
- Feature: [description]

WHERE in the UI:
- Page: [which page/section]
- Context: [when user sees this]

DESIGN OPTIONS:
1. Simple checkboxes/inputs
2. Toggle switches
3. Accordion/collapsible section
4. Wizard/stepper (multi-step)
5. Tabs
6. Modal dialog
7. Inline editing
8. [Other options...]

QUESTIONS:
- Preferred UI pattern?
- Required vs optional fields?
- Show/hide logic?
- Validation approach?
- Mobile responsiveness?
```

#### Step 2: WAIT for User Decision

**User decides:**
- ✅ UI pattern (checkboxes vs toggles vs wizard...)
- ✅ Layout (inline vs modal vs separate page)
- ✅ Field requirements
- ✅ Interaction flow
- ✅ Validation rules

#### Step 3: ONLY THEN Implement

**After approval, in this order:**
1. Create/modify components (following approved design)
2. Add validation logic
3. Test functionality
4. **LAST:** Add translations (can be postponed)

### 📝 Translation Priority:

**Translations are LOWEST priority for UI changes:**
- ✅ Can be added at the very end
- ✅ Can be postponed until feature testing
- ✅ Not critical for UI concept discussion/prototyping
- ⚠️ Add only when UI is finalized and approved

---

## 🚨 CRITICAL: Using Existing Code - NEVER Invent APIs

**GOLDEN RULE:** NEVER invent non-existent methods/classes/utilities!

### ❌ FORBIDDEN:
1. **Inventing methods "by analogy"**
   ```javascript
   // WRONG - invented non-existent method
   this.logger.log()      // Doesn't exist!
   this.logger.error()    // Doesn't exist!
   ```

2. **Assuming methods exist**
   ```javascript
   // WRONG - didn't verify method exists
   INPUT_TYPES.number     // Doesn't exist!
   INPUT_TYPES.textarea   // Doesn't exist!
   ```

3. **Using without verification**
   ```javascript
   // WRONG - didn't check real implementation
   someService.doSomething() // May not exist!
   ```

### ✅ CORRECT 4-Step Verification Process:

#### Step 1: Search for Examples in Code
```bash
grep -r "methodName" lib/**/*.js
grep -r "ClassName" src/**/*.js
```

#### Step 2: ASK User for Example
**Instead of guessing - ASK:**
```
"Can you show an example of using this method/class in the project?"
"Where in the code is this utility used?"
"Show me how to correctly call this service?"
```

#### Step 3: Read Class/Function Definition
- Open the definition file
- Read JSDoc
- Understand parameters and return values

#### Step 4: Verify Method Exists
- Check in definition file that method actually exists
- DON'T assume method exists "by analogy"

**Algorithm:**
```
Need to use method/class?
  ↓
Do I know HOW it's used?
  ↓ No
ASK USER for example
  ↓
Search for examples (grep_search)
  ↓
Read definition file
  ↓
Verify method exists
  ↓
Use correctly
```

---

## 🚨 CRITICAL: Decision Making Policy

**Principle:** 👤 **USER DECIDES, AI EXECUTES**

### ❌ NEVER Do Without Approval:
1. Making changes affecting multiple files
2. Creating files (documentation, analysis, etc.) without asking
3. Deleting files or folders
4. Writing documentation when user just wants chat output
5. Refactoring large code sections
6. Installing/updating dependencies

### ✅ ALWAYS: Discuss → Approve → Execute

**Workflow:**
```
User asks → Analyze → Discuss options → Wait for approval → Execute
```

**Example: User asks for analysis**

❌ WRONG:
```
- Immediately create analysis.md file
- Write full documentation
- Save to .results/
```

✅ CORRECT:
```
- Analyze in memory
- Present findings in CHAT
- Ask: "Should I save this as a document?"
- Wait for "yes" → THEN create file
```

### When Action is OK Without Asking

**ONLY these cases:**
1. ✅ User explicitly says "do it", "create it", "fix it", "implement it"
2. ✅ Simple read operations (`read_file`, `grep_search`, `list_dir`)
3. ✅ Running tests/scripts user explicitly asked to run
4. ✅ Showing code examples in chat (without saving)

### Approval Language

**Clear approval (OK to proceed):**
- "Yes, do it"
- "Go ahead"
- "Proceed"
- "Create it"
- "Fix it"
- "Implement it"

**Need clarification (STOP and ask):**
- "What do you think?" → Answer, don't act
- "Can you help?" → Discuss options
- "How would you fix this?" → Explain, don't fix yet
- "Show me..." → Show in chat, don't create files

---

## File Organization

### Documentation & Analysis Rules

**❌ NEVER create files in project root!**

**✅ ALWAYS use `.results/` folder:**

```javascript
// ❌ WRONG
create_file({filePath: "/project/my-analysis.md"})

// ✅ CORRECT
create_file({filePath: "/project/.results/my-analysis.md"})
```

### File Placement

| File Type | Location | Examples |
|-----------|----------|----------|
| Analysis results | `project/.results/` | `dependencies-analysis.md` |
| Specifications | `project/.results/spec/` | `feature-name.md` |
| Archived specs | `project/.results/spec/archive/` | `feature_2026-01-15.md` |
| Source code | `project/lib/`, `project/src/` | `myModule.js` |
| Config files | `project/config/` | `settings.json` |

### Why This Matters
1. **`.results/` is in `.gitignore`** - analysis won't pollute git
2. **Project root stays clean** - only source code and configs
3. **Consistency** - all projects use same structure
4. **Easy cleanup** - can delete `.results/` safely

### Shared Documentation Exception
**For cross-project docs:**
```javascript
// ✅ OK - shared docs across all projects
create_file({filePath: "~/.shared-docs/.results/cross-project-analysis.md"})
```

---

## Terminal Best Practices

### Prefer File Tools for Reading
- ✅ `read_file()` - read file contents
- ✅ `grep_search()` - search in files
- ✅ `file_search()` - find files by pattern
- ✅ `list_dir()` - list directory contents

### Use Terminal for Execution
- ✅ `npm install`, `npm update`
- ✅ `git commit`, `git push`
- ✅ `npm run build`
- ✅ `rm`, `mv`, `mkdir`

### Historical Note (WebStorm WSL)
💡 **WebStorm <2024** had timeout issues with `cat`/`grep` in WSL (FIXED in 2024+)

**Best practice remains:** File tools are FASTER for reading even after bug fix.

---

## 🚨 CRITICAL: Deletion Policy

### NEVER Execute Without Confirmation

**ALL deletion commands MUST:**
1. ✅ Be shown SEPARATELY
2. ✅ Clearly explain WHAT will be deleted
3. ✅ Wait for EXPLICIT user confirmation
4. ✅ NEVER be batched with other commands

**Pattern:**
```
⚠️ DELETION COMMAND - REQUIRES CONFIRMATION:

rm -rf ~/.folder-name

WHAT WILL BE DELETED:
- ~/.folder-name/file1.txt
- All contents will be PERMANENTLY LOST

⚠️ THIS IS IRREVERSIBLE!

Do you confirm deletion? (yes/no)
```

**ONLY after "yes" - execute!**

---

## Error Handling Pattern (Backend)

### Use `return catchError()` Pattern

**❌ WRONG:**
```javascript
if (!isValid) {
  throw new Error("Validation failed");
}
```

**✅ CORRECT:**
```javascript
if (!isValid) {
  return catchError(new Error("Validation failed"));
}
```

### Why?
- `catchError` ensures consistent error format: `{status, error: {...}}`
- Proper serialization for API responses
- Client-side consistency

### When to Apply:
- ✅ Input validation
- ✅ Business rule checks
- ✅ Exception handling in catch blocks
- ✅ Any API error return

**Exception:** `throw` only for system-level exceptions in libraries

---

## Clean Code Principles

### 1. Separate Concerns
```javascript
// ❌ Bad - everything in one component
const UserProfile = () => {
  const [user, setUser] = useState(null);
  useEffect(() => {
    fetch("/api/user").then(res => res.json()).then(setUser);
  }, []);
  return <div>{user?.name}</div>;
};

// ✅ Good - separated
const useUserData = () => { /* data fetching */ };
const UserProfile = () => {
  const user = useUserData();
  return <div>{user?.name}</div>;
};
```

### 2. Single Responsibility
```javascript
// ❌ Bad - function does too much
const processOrder = (order) => {
  validateOrder(order);
  db.query("INSERT...");
  sendEmail(order.email);
};

// ✅ Good - each function has one job
const processOrder = (order) => {
  validateOrder(order);
  saveOrder(order);
  notifyCustomer(order);
};
```

### 3. Pure Functions
```javascript
// ❌ Bad - side effects
let total = 0;
const addToTotal = (amount) => { total += amount; };

// ✅ Good - pure function
const addToTotal = (currentTotal, amount) => currentTotal + amount;
```

### 4. Descriptive Names
```javascript
// ❌ Bad
const u = getUserData();
const calc = (x, y) => x * y * 0.2;

// ✅ Good
const currentUser = getUserData();
const calculateDiscountedPrice = (price, quantity) => price * quantity * 0.2;
```

### 5. Minimal Public API
```javascript
// ❌ Bad - exposes internals
const module = {
  _internalCache: {},
  _validate: () => {},
  getUser: () => {},
  updateUser: () => {}
};

// ✅ Good - only public API
const module = {
  getUser: () => {},
  updateUser: () => {}
};
```

---

## Code Style

### Quotes
- ✅ **ALWAYS double quotes** (`"..."`)
- ❌ **NEVER single quotes** (`'...'`)

```javascript
// ✅ CORRECT
const name = "John";
import {Component} from "react";

// ❌ WRONG
const name = 'John';
import {Component} from 'react';
```

### Comments
- ✅ **ALWAYS English**
- ❌ **NEVER Russian in code**

```javascript
// ✅ CORRECT
// Initialize component state
const [data, setData] = useState([]);

// ❌ WRONG
// Инициализация состояния компонента
const [data, setData] = useState([]);
```

### Modern Syntax
- ✅ `async/await` (not `.then()`)
- ✅ Arrow functions
- ✅ Destructuring
- ✅ Template literals
- ✅ Spread operators

---

## Task Complexity Categories

### Simple Tasks (Execute Immediately)
**Examples:**
- Write one function
- Add jsDoc
- Fix bug in one place
- Small code tweak

**Action:** Provide solution immediately, no questions.

---

### Medium Tasks (Plan → Execute)
**Examples:**
- Component with hook (2-3 files)
- Refactoring one component
- Adding feature to existing section

**Action:**
1. Propose plan
2. Wait for confirmation
3. Execute ALL steps (no intermediate approvals)

---

### Complex Tasks (Step-by-Step)
**Examples:**
- New project section (4+ files)
- Changes across multiple sections
- Large refactoring (5+ files)
- Architecture changes

**Action:**
1. Discuss plan
2. Break into steps
3. Execute first step
4. Ask: "Continue with next step?"
5. Repeat for each step

---

## ❌ NEVER Do This

- ❌ Write code without clarifying requirements (if unclear)
- ❌ Ask 10 questions for simple tasks
- ❌ Give irrelevant explanations when asked for specific code
- ❌ Use single quotes in code
- ❌ Write Russian comments in code
- ❌ Change old code without necessity
- ❌ Start work without studying project structure
- ❌ Choose UI pattern without user discussion
- ❌ Add translations before UI finalization

---

## ✅ ALWAYS Do This

- ✅ Study project structure before starting
- ✅ Ask questions if truly unclear
- ✅ Discuss approach before complex tasks
- ✅ **Discuss UI/UX concept before ANY UI changes**
- ✅ For simple tasks - do immediately
- ✅ For medium tasks - plan + execute
- ✅ Use double quotes everywhere
- ✅ Write all comments in English
- ✅ Apply clean code principles

---

## 🔄 Version Management & Updates

### Check for Updates (Periodic)

**When to Check:**
- At the start of a new work session (once per day)
- When user mentions update/upgrade
- If you notice instructions seem outdated

**Process:**

1. **Read Local Version File:**
   ```
   Check if file exists: .ai-prompt-chain-generator.json
   OR for multi-project: ~/.shared-docs/.ai-prompt-chain-generator.json
   ```

2. **Extract Current Version:**
   ```json
   {
     "version": "0.2.0",
     "generatedAt": "2026-01-24T10:30:00Z",
     "customModifications": false
   }
   ```

3. **Check GitHub for Latest:**
   ```
   Repository: https://github.com/ale4ko69/ai-prompt-chain-generator
   Check: Latest release version
   Compare with local version
   ```

4. **If New Version Available:**
   ```
   🆕 Update Available!

   Current version: {local_version}
   Latest version: {github_version}
   Released: {release_date}

   What's new:
   {List new features from GitHub release notes}

   ⚠️ IMPORTANT: You have customizations in your instructions!

   To safely update while preserving your changes:
   📖 Follow: https://github.com/ale4ko69/ai-prompt-chain-generator/blob/main/UPDATE-INSTRUCTIONS.md

   Would you like me to explain the update process?
   ```

5. **If Customizations Detected:**
   ```
   Check: "customModifications": true in version file

   Warning: Do NOT regenerate instructions automatically
   Reason: User has custom rules that would be lost
   Action: Direct to UPDATE-INSTRUCTIONS.md
   ```

6. **If No Customizations:**
   ```
   Offer: "Would you like me to regenerate with the latest templates?"

   If YES:
   - Create backup of current instructions
   - Run prompt chain with latest version
   - Update version file
   ```

### Version Check Frequency
- **Automatic:** Once per day (start of session)
- **Manual:** When user asks "any updates?" or "check for updates"
- **Never:** Don't spam user with daily messages if already notified

### Version File Maintenance
- **After Manual Edits:** Set `customModifications: true`
- **After Regeneration:** Set `customModifications: false`, update `generatedAt`
- **Track Changes:** Use `notes` field for custom modification descriptions

---

**Template Version:** 1.0
**Last Updated:** 2026-01-24
**For:** ALL projects (single or multi)
