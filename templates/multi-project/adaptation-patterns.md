# Adaptation Patterns Template

**Purpose:** Catalog known differences between projects for sync operations
**Used when:** Multi-project setup, documenting COPY vs ADAPT patterns

---

## Purpose

Document **known differences** between projects to guide File Mapping Table creation.

---

## Quick Reference Template

| Pattern | Project A | Project B | Action | Confidence |
|---------|-----------|-----------|--------|------------|
| Pattern 1 | Implementation A | Implementation B | COPY/ADAPT/SKIP | 🟢/🟡/🔴 |
| Pattern 2 | Implementation A | Implementation B | COPY/ADAPT/SKIP | 🟢/🟡/🔴 |

---

## Pattern Documentation Template

### Pattern N: [Pattern Name]

**Scenario:** [When this occurs]

**Project A:** [Code example or description]
```language
// Example from Project A
```

**Project B:** [Code example or description]
```language
// Example from Project B
```

**Action:** COPY / ADAPT / SKIP

**Confidence:** 🟢 High / 🟡 Medium / 🔴 Low

**Notes:** [Important considerations, gotchas, edge cases]

**Adaptation Steps** (if ADAPT):
1. Step 1
2. Step 2
3. Step 3

---

## Example Patterns

### Pattern 1: Configuration Objects

**Scenario:** Default configuration objects

**Project A:**
```javascript
export const DEFAULT_CONFIG = {
  field1: "",
  field2: null,
  field3: false
};
```

**Project B:**
```javascript
export const DEFAULT_CONFIG = {
  field1: "",
  field2: null,
  field3: false
};
```

**Action:** COPY
**Confidence:** 🟢 High
**Notes:** Structures are identical, just copy values

---

### Pattern 2: Component Event Handlers

**Scenario:** Different event handler patterns

**Project A:**
```javascript
const onChange = (field, value) => {
  // Handle change
};

<Input field="name" onChange={onChange} />
```

**Project B:**
```javascript
const onChange = field => value => {
  // Handle change
};

<Input field="name" onChange={onChange("name")} />
```

**Action:** ADAPT
**Confidence:** 🟢 High
**Notes:** Pattern B uses currying

**Adaptation Steps:**
1. Change `onChange={handler}` to `onChange={handler("fieldName")}`
2. Ensure handler function supports currying

---

### Pattern 3: Shared Package Usage

**Scenario:** One project uses local implementation, other uses shared package

**Project A:**
```javascript
// lib/services/auth.js
class AuthService {
  // Local implementation
}
```

**Project B:**
```javascript
const {AuthService} = require("@shared/auth");
```

**Action:** SKIP
**Confidence:** 🟢 High
**Notes:** Verify shared package version, update if needed

**Verification Steps:**
1. Check Project B `package.json`
2. Verify `@shared/auth` version
3. Check CHANGELOG for feature availability
4. Update if outdated: `npm update @shared/auth`

---

## Detection Decision Tree

```
1. Is it a configuration object?
   YES → Likely COPY (🟢 High)

2. Is it using shared package in one project?
   YES → Likely SKIP, verify version (🟢 High)

3. Is it a component with event handlers?
   YES → Check handler pattern
         - If different → ADAPT (🟢 High)
         - If same → COPY (🟡 Medium - verify)

4. Is it a new/unknown pattern?
   YES → Mark 🔴 Low confidence
         ASK USER for guidance
```

---

## Confidence Level Guidelines

### 🟢 High Confidence
**When:**
- Pattern is documented in this file
- Code structure is identical or has documented adaptation
- Only field names/values change
- No structural differences

**Examples:**
- Configuration objects (COPY)
- Known event handler patterns (ADAPT)
- Shared package usage (SKIP)

---

### 🟡 Medium Confidence
**When:**
- Pattern is documented but requires verification
- Minor structural differences exist
- Path/naming may differ
- Need to check existing project patterns first

**Examples:**
- Translation key paths (may have different prefix)
- Component structure (may nest differently)
- Import paths (may differ)

---

### 🔴 Low Confidence
**When:**
- No documented pattern exists
- Significant structural differences
- No clear equivalent found
- Complex logic adaptation needed

**Examples:**
- New component with no analog
- Complex state management differences
- Unknown integration points

**Action:** Always ASK USER before proceeding

---

## Common Pitfalls

### Pitfall 1: [Description]
**Issue:** [What goes wrong]
**Solution:** [How to avoid]

**Example:**
```
Pitfall: Assuming component structure is same
Issue: Project A uses single file, Project B uses nested folders
Solution: Check existing structure, adapt to Project B patterns
```

---

### Pitfall 2: [Description]
**Issue:** [What goes wrong]
**Solution:** [How to avoid]

---

## Adding New Patterns

**When you discover a new difference:**

1. **Document it** using the pattern template
2. **Add to Quick Reference** table
3. **Assign confidence level**
4. **Update related specs** that use this pattern

**Template:**
```markdown
### Pattern N: [Pattern Name]

**Scenario:** [When this occurs]

**Project A:** [Code example]

**Project B:** [Code example]

**Action:** COPY/ADAPT/SKIP
**Confidence:** 🟢/🟡/🔴
**Notes:** [Important considerations]
```

---

## Usage During Sync

**Step 1:** AI reads this file before creating File Mapping Table

**Step 2:** AI matches source files to documented patterns

**Step 3:** AI assigns action (COPY/ADAPT/SKIP) and confidence level

**Step 4:** AI presents mapping to user for approval

**Step 5:** If new pattern found → mark 🔴, ask user

---

**Template Version:** 1.0
**Last Updated:** 2026-01-21
**For:** Multi-project setups - pattern catalog
