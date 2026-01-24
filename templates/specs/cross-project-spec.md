# Cross-Project Feature Specification Template

**Feature Name:** [Feature Name]
**Started:** YYYY-MM-DD
**Projects:** [Project A] ↔ [Project B]
**Direction:** [Project A → Project B] or [Bidirectional]

---

## Status
- [x] Planning
- [ ] File Mapping Created
- [ ] User Approval
- [ ] In Development
- [ ] Code Review
- [ ] Completed
- [ ] Archived

---

## 📑 Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Project Mapping](#project-mapping)
- [File Mapping Table](#file-mapping-table)
- [Technical Design](#technical-design)
- [TODO List](#todo-list)
- [Acceptance Criteria](#acceptance-criteria)
- [Change Log](#change-log)

---

## Overview

[Brief description of the feature and sync purpose]

**Why Sync:**
[Reason for syncing this feature between projects]

**Business Value:**
[Why this sync is important]

---

## Requirements

### Functional Requirements
- [ ] Feature X works in Project A
- [ ] Feature X needs to work in Project B
- [ ] Maintain consistency between projects

### Cross-Project Considerations
- [ ] Dependency versions compatible
- [ ] Shared packages up-to-date
- [ ] Architecture differences addressed

---

## Project Mapping

**Project A Location:** `~/path/to/project-a/`
**Project B Location:** `~/path/to/project-b/`

**Component Mapping:**
| Component | Project A | Project B |
|-----------|-----------|-----------|
| Backend | `project-a/backend/` | `project-b/backend/` |
| Frontend | `project-a/frontend/` | `project-b/frontend/` |

---

## 📊 File Mapping Table

**⚠️ REQUIRES USER APPROVAL BEFORE IMPLEMENTATION**

| # | Source (Project A) | Target (Project B) | Action | Adaptation | Confidence | Notes |
|---|-------------------|-------------------|--------|------------|------------|-------|
| 1 | `path/file1.js` | `path/file1.js` | COPY | None | 🟢 High | Identical structure |
| 2 | `path/file2.js` | `path/file2.js` | ADAPT | Change X to Y | 🟢 High | Known pattern |
| 3 | `path/file3.js` | `path/file3.js` | ADAPT | Different handler | 🟡 Medium | Verify first |
| 4 | `path/file4.js` | N/A | SKIP | Uses shared pkg | 🟢 High | Check version |

**Legend:**
- **COPY** - Minimal/no changes needed
- **ADAPT** - Known pattern changes required
- **SKIP** - Not needed (explain why)

**Confidence:**
- 🟢 High - AI knows exact approach
- 🟡 Medium - May need adjustments
- 🔴 Low - Requires user guidance

**Status:** ⏳ Awaiting user approval / ✅ Approved / 🔄 Adjusted

---

## Technical Design

### Source Implementation (Project A)

**Backend Files:**
1. `path/to/source1.js` - [Description]
2. `path/to/source2.js` - [Description]

**Frontend Files:**
1. `path/to/source3.js` - [Description]
2. `path/to/source4.js` - [Description]

### Target Implementation (Project B)

**Backend Files:**
1. `path/to/target1.js` - COPY from source1.js
2. `path/to/target2.js` - ADAPT from source2.js (change X)

**Frontend Files:**
1. `path/to/target3.js` - ADAPT from source3.js (handler pattern)
2. `path/to/target4.js` - SKIP (uses shared package)

### Adaptations Required

#### Adaptation 1: [Name]
**Source (Project A):**
```javascript
// Example from Project A
```

**Target (Project B):**
```javascript
// Adapted for Project B
```

**Pattern:** [Reference to adaptation pattern doc]

#### Adaptation 2: [Name]
**Source (Project A):**
```javascript
// Example from Project A
```

**Target (Project B):**
```javascript
// Adapted for Project B
```

**Pattern:** [Reference to adaptation pattern doc]

### Dependencies

**Project A:**
- @shared/package: v1.2.3

**Project B:**
- @shared/package: v1.2.5 (✅ compatible)

**Actions:**
- [ ] Verify @shared/package version in Project B
- [ ] Update if needed: `npm update @shared/package`

---

## TODO List

### File Mapping
- [x] Create File Mapping Table
- [ ] Get user approval
- [ ] Adjust mapping (if needed)

### Reading Source
- [ ] Read source file 1 (via `cat`)
- [ ] Read source file 2 (via `grep`)
- [ ] Read source file 3
- [ ] Read source file 4

### Implementation
- [ ] COPY files (Files 1)
- [ ] ADAPT files (Files 2, 3)
- [ ] SKIP files (File 4 - verify package)
- [ ] Test in Project B
- [ ] Verify functionality

### Sync Tracker
- [ ] Update FEATURE-SYNC-STATUS.md
- [ ] Mark as completed

---

## Acceptance Criteria

### Functional
- [ ] Feature works in Project B as it does in Project A
- [ ] All adaptations applied correctly
- [ ] Shared packages verified

### Code Quality
- [ ] COPY files are identical (where appropriate)
- [ ] ADAPT files follow Project B patterns
- [ ] SKIP files documented (reason given)

### Documentation
- [ ] File Mapping Table approved
- [ ] Sync status updated
- [ ] Adaptation patterns documented (if new)

---

## Change Log

### YYYY-MM-DD - Planning & Mapping

**Actions:**
- Spec created
- File Mapping Table created
- Presented to user for approval

**Status:** Awaiting approval

---

### YYYY-MM-DD - User Approval

**Decisions:**
- File Mapping approved: [Yes/Adjusted]
- Changes requested: [List if any]

**Status:** Approved, ready for implementation

---

### YYYY-MM-DD - Implementation

**Progress:**
- ✅ Read source files
- ✅ COPY files 1
- 🔄 ADAPT files 2, 3 (in progress)
- ⏳ SKIP file 4 (pending verification)

**Summary:** [Daily progress]

---

### YYYY-MM-DD - Completion

**Final Status:**
- ✅ All files synced
- ✅ Feature tested in Project B
- ✅ Sync tracker updated

**Archived:** YYYY-MM-DD to `archive/feature-name_YYYY-MM-DD.md`

---
