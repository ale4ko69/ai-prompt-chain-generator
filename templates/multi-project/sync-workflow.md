# Cross-Project Sync Workflow Template

**Purpose:** Workflow for syncing features between related projects
**Used when:** Multi-project setup with shared features

---

## When to Use

**ONLY for:**
- Projects working together (e.g., CI ↔ CW, Backend ↔ Frontend)
- Features that exist in multiple projects
- Shared code that needs synchronization

**NOT for:**
- Single standalone projects
- Completely independent projects

---

## Key Principle

👤 **USER DECIDES, AI EXECUTES**

---

## Sync Workflow

### Step 1: User Specifies Intent

**Examples:**
```
"Transfer feature X from Project A to Project B"
"Sync Y from A to B"
"Add Z to A, will sync to B later"
```

---

### Step 2: AI Reads Source Files

**Using terminal commands (NEVER copy files):**
```bash
cat ~/project-a/src/feature.js
grep -rn "pattern" ~/project-a/
less ~/project-a/config.js
```

**Why terminal commands:**
- Read-only operation
- No file copying
- Prevents accidental modifications

---

### Step 3: AI Creates File Mapping Table

**Template:**
```markdown
## 📊 File Mapping Table (REQUIRES USER APPROVAL)

**AI Suggested Mapping:**

| # | Source (Project A) | Target (Project B) | Action | Adaptation | Confidence |
|---|-------------------|-------------------|--------|------------|------------|
| 1 | src/file1.js | src/file1.js | COPY | None | 🟢 High |
| 2 | src/file2.js | src/file2.js | ADAPT | Change X to Y | 🟢 High |
| 3 | src/file3.js | src/file3.js | ADAPT | Different structure | 🟡 Medium |
| 4 | config.json | config.json | SKIP | Uses shared package | 🟢 High |

**Legend:**
- 🟢 High - AI knows exact pattern
- 🟡 Medium - May need adjustments
- 🔴 Low - Requires user guidance

**Confidence Explanation:**
- File 1: 🟢 High - [Reason]
- File 2: 🟢 High - [Reason]
- File 3: 🟡 Medium - [Reason]
- File 4: 🟢 High - [Reason]
```

---

### Step 4: User Approves/Adjusts Mapping

**AI Message:**
```
📊 Found X files to sync from Project A to Project B.

Proposed mapping above ⬆️

Actions needed:
✅ COPY: X files (minimal/no changes)
✅ ADAPT: Y files (known patterns)
⚠️ SKIP: Z files (not needed)

Review mapping:
1. ✅ Approve all - proceed with implementation
2. 🔄 Adjust - specify which files need changes
3. ❌ Reject - provide correct mapping

Your decision?
```

**User Response Options:**

**A. Approve:**
```
User: "Approve all"
AI: "Starting implementation..."
```

**B. Adjust:**
```
User: "File 3 - use COPY not ADAPT"
AI: "Updated! File 3 → COPY. Proceeding..."
```

**C. Low Confidence - AI Asks:**
```
AI: "File 5: complex-component.js

    🔴 Low confidence - no clear analog found

    Options:
    A. Create new component (specify location)
    B. Integrate into existing (specify which)
    C. Skip this file

    What should I do?"

User: "Option B - integrate into settings/index.js"
AI: "Got it! Updating mapping..."
```

---

### Step 5: Implementation

**AI executes according to approved mapping:**
- COPY files (minimal changes)
- ADAPT files (apply known patterns)
- SKIP files (document why skipped)

---

### Step 6: Update Tracker

**Update sync status file:**
```markdown
# FEATURE-SYNC-STATUS.md

| Feature | Project A | Project B | Files | Complexity | Date |
|---------|-----------|-----------|-------|------------|------|
| Feature X | ✅ v1.0 | ✅ v1.0 | 4 | Medium | 2026-01-21 |
```

---

## File Actions

### COPY - Minimal/No Changes

**When:**
- Configuration field lists
- Default configuration objects
- Translation keys (if paths match)
- Pure utility functions

**Example:**
```javascript
// Project A
{
  field1: "",
  field2: null,
  field3: false
}

// Project B - COPY (identical structure)
{
  field1: "",
  field2: null,
  field3: false
}
```

---

### ADAPT - Known Pattern Changes

**When:**
- Component structure differs
- Import paths differ
- API call patterns differ
- Translation key prefixes differ

**Example:**
```javascript
// Project A
<Input
  field="name"
  onChange={handleChange}  // Function reference
/>

// Project B - ADAPT
<Input
  field="name"
  onChange={handleChange("name")}  // Function call
/>
```

---

### SKIP - Not Needed

**When:**
- Source uses local implementation, target uses shared package
- Feature already exists in target
- Different architecture approach

**Example:**
```
Project A: lib/services/auth.js (local implementation)
Project B: Uses @shared/auth-utils (shared package)
→ Action: SKIP (verify package version)
```

---

## Confidence Levels

### 🟢 High Confidence
**When:**
- Pattern documented in adaptation patterns
- Code structure identical
- Only values/names change
- No structural differences

### 🟡 Medium Confidence
**When:**
- Pattern documented but needs verification
- Minor structural differences
- Paths/naming may differ
- Need to check existing patterns

### 🔴 Low Confidence
**When:**
- No documented pattern
- Significant structural differences
- No clear equivalent found
- Complex logic adaptation needed

**Action:** Always ASK USER before proceeding

---

## Sync Process Flow

```
┌─────────────────────────────────────────┐
│ 1. USER: "Transfer X from A to B"      │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 2. AI: Read source files (cat/grep)    │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 3. AI: Create File Mapping Table       │
│    - COPY/ADAPT/SKIP suggestions        │
│    - Confidence levels (🟢🟡🔴)        │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 4. AI: Present for approval             │
└────────────┬────────────────────────────┘
             │
             ▼
      ┌──────┴──────┐
      │             │
  Approve       Adjust
      │             │
      │    ┌────────▼────────┐
      │    │ USER: Changes   │
      │    └────────┬────────┘
      │             │
      └─────┬───────┘
            │
            ▼
┌─────────────────────────────────────────┐
│ 5. AI: Implement per approved mapping  │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ 6. AI: Update sync tracker             │
└─────────────────────────────────────────┘
```

---

## Important Rules

### ❌ NEVER:
- Copy files without reading source first
- Assume COPY when structure might differ
- Skip File Mapping Table approval
- Implement without checking dependency versions
- Forget to update sync tracker

### ✅ ALWAYS:
- Read source via terminal commands
- Create File Mapping Table with confidence levels
- Get user approval before implementation
- Reference adaptation patterns for suggestions
- Update tracker after completion

---

## Related Documents

**For multi-project setup, also create:**
- `adaptation-patterns.md` - Catalog of known differences
- `sync-status.md` - Tracker template
- `commands-reference.md` - Terminal commands

---

**Template Version:** 1.0
**Last Updated:** 2026-01-21
**For:** Multi-project setups only
