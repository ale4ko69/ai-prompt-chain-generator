# Feature Sync Status Tracker Template

**Purpose:** Track cross-project feature synchronization
**Used when:** Multi-project setup with ongoing sync operations

---

## Active Sync Status

| Feature | Project A | Project B | Files | Complexity | Last Sync | Notes |
|---------|-----------|-----------|-------|------------|-----------|-------|
| *No active syncs* | - | - | - | - | - | - |

---

## Planned Syncs

| Feature | Source | Target | Planned Date | Priority | Spec Location |
|---------|--------|--------|--------------|----------|---------------|
| *No planned syncs* | - | - | - | - | - |

---

## Completed Syncs

| Feature | Direction | Files Synced | Completion Date | Spec Location | Notes |
|---------|-----------|--------------|-----------------|---------------|-------|
| *No completed syncs* | - | - | - | - | - |

---

## Statistics

**Total Features Tracked:** 0
**In Progress:** 0
**Completed:** 0
**Planned:** 0

---

## Feature Details Template

### Feature Name

**Status:** 🔄 In Progress / ✅ Completed / ⏳ Planned
**Direction:** Project A → Project B
**Started:** YYYY-MM-DD
**Completed:** YYYY-MM-DD (if applicable)

**Source Files (Project A):**
1. `path/to/file1.js` - Description
2. `path/to/file2.js` - Description
3. `path/to/file3.js` - Description

**Target Files (Project B):**
1. `path/to/file1.js` - COPY (description)
2. `path/to/file2.js` - ADAPT (description)
3. `path/to/file3.js` - SKIP (description)

**Complexity:** Low / Medium / High

**Spec:** `.results/spec/feature-name.md`

**Dependencies:**
- @shared/package-name ≥ version (verify)

**Progress:**
- [x] Spec created
- [x] File mapping defined
- [x] User approval
- [ ] Implementation
- [ ] Testing
- [ ] Archive spec

**Notes:**
- Any important notes or blockers

---

## Complexity Levels

### Low (1-3 files)
- Identical or near-identical code
- Minimal adaptations
- No dependency changes

**Example:** Adding single field to config

---

### Medium (4-10 files)
- Mix of COPY and ADAPT actions
- Known adaptation patterns
- May require package updates

**Example:** Feature with backend + frontend changes

---

### High (10+ files)
- Significant adaptations needed
- Complex logic changes
- Multiple dependency updates
- New components or services

**Example:** Complete authentication system

---

## Status Indicators

**🔄 In Progress** - Currently being synced
**✅ Completed** - Successfully synced
**⏳ Planned** - Scheduled for future sync
**❌ Blocked** - Cannot proceed (with blocker description)

---

## Usage

### Starting New Sync

1. Add row to "Active Sync Status" table
2. Create feature detail section
3. Mark status as 🔄 In Progress

### Completing Sync

1. Update status to ✅ Completed
2. Move row to "Completed Syncs" table
3. Update statistics
4. Archive spec

### Planning Future Sync

1. Add row to "Planned Syncs" table
2. Set priority (High/Medium/Low)
3. Note spec location (if created)

---

**Template Version:** 1.0
**Last Updated:** 2026-01-21
**For:** Multi-project feature tracking
