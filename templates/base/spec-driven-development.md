# Spec-Driven Development Template

**Purpose:** Framework for complex feature development
**Used when:** New feature (4+ files), architecture changes, cross-project sync

---

## When to Use Specs

### ✅ Requires Spec:
- New API endpoint
- New feature module (4+ files)
- New page/view
- Database schema changes
- Significant refactoring
- Cross-project feature sync

### ❌ No Spec Needed:
- Simple bug fix
- Single utility function
- Adding validation rule
- Updating error message
- Small UI tweak

**When in doubt:** Ask user if spec is needed.

---

## Spec Workflow

### 1. Before Any Code - Create Specification

**Location:** `.results/spec/feature-name.md`

**Steps:**
1. Generate spec from template
2. Fill in Overview, Requirements, Technical Design
3. Present to user for approval
4. Only after approval - start development

### 2. During Development

**Progress Reporting:**
```
🔨 Step 1/6: Creating backend validation schema...
✅ Step 1/6 complete: Schema created with all required fields

🔨 Step 2/6: Creating DAL layer (4 sub-tasks)...
  ✅ 2.1: find() method
  ✅ 2.2: findById() method
  🔄 2.3: insert() method (writing...)
  ⏳ 2.4: update() method

⚠️ Security concern: User input needs sanitization.
   Pausing for discussion...
```

**Update Spec:**
- Update TODO list after each step
- Add important decisions to Change Log immediately
- If conflict/question arises → STOP, discuss with user

### 3. End of Day

Add daily summary to Change Log:
```markdown
### YYYY-MM-DD - Development Day 1

**Decisions:**
- Decision 1: Explanation
- Decision 2: Explanation

**Progress:**
- ✅ Task 1
- 🔄 Task 2 (in progress)
- ⏳ Task 3 (pending)

**Daily Summary:**
Summary of what was accomplished today and plan for tomorrow.
```

### 4. On Completion

**Checklist:**
- [ ] All TODO items completed
- [ ] All Acceptance Criteria met
- [ ] User reviewed and approved

**Archive:**
```bash
# Move to archive with completion date
.results/spec/feature-name.md
  → .results/spec/archive/feature-name_2026-01-15.md
```

---

## Full Spec Template

```markdown
# Feature: [Feature Name]
**Started:** YYYY-MM-DD

## Status
- [x] Planning
- [ ] In Development
- [ ] Code Review
- [ ] Completed
- [ ] Archived

---

## 📑 Table of Contents

### Quick Navigation
- [Overview](#overview)
- [Requirements](#requirements)
- [Technical Design](#technical-design)
  - [Backend Files](#backend-files)
  - [Frontend Files](#frontend-files)
- [TODO List](#todo-list)
- [Implementation Notes](#implementation-notes)
- [Acceptance Criteria](#acceptance-criteria)
- [Change Log](#change-log)

---

## Overview

Brief description of the feature and its purpose.

**User Story:**
As a [user type], I want to [action] so that [benefit].

**Business Value:**
[Why this feature is important]

---

## Requirements

### Functional Requirements
- [ ] Requirement 1: [Description]
- [ ] Requirement 2: [Description]

### Security Requirements
- [ ] Authentication: [JWT, SAML, etc.]
- [ ] Authorization: [Role-based access]
- [ ] Data validation: [Schema validation]

### UI/UX Requirements
- [ ] UI pattern: [Checkboxes, toggles, wizard, etc.]
- [ ] Layout: [Inline, modal, separate page]
- [ ] Responsiveness: [Mobile support]

### Performance Requirements
- [ ] Response time: [< 200ms]
- [ ] Scalability: [Handle X concurrent users]

---

## Technical Design

### Backend Files

**Location:** `[project]/backend/`

- [ ] `./lib/schemes/local/featureSchema.js` - AJV validation schema
  - Fields: [list]
  - Validation rules: [required, format, etc.]

- [ ] `./lib/internal/dal/featureDal.js` - Data Access Layer
  - Methods: find(), findById(), insert(), update(), delete()
  - Collection: [collection name]

- [ ] `./lib/common/services/featureServices.js` - Business logic
  - Methods: [list]
  - Dependencies: [other services]

- [ ] `./lib/backend-api/controllers/feature.controller.js` - Controller
  - Endpoints: [list]
  - Error handling: [catchError pattern]

- [ ] `./lib/backend-api/routes/feature.routes.js` - Express routes
  - Middleware: [auth, validation, rate limiting]

### Frontend Files

**Location:** `[project]/frontend/`

- [ ] `./src/services/featureApi.js` - API service
  - Methods: [list]
  - Error handling: [try/catch]

- [ ] `./src/store/feature/actions.js` - Redux actions
  - Action types: [list]
  - Thunks: [async actions]

- [ ] `./src/store/feature/reducer.js` - Redux reducer
  - State shape: [describe]
  - Actions handled: [list]

- [ ] `./src/components/pages/Feature.js` - Page component
  - Layout: [description]
  - Child components: [list]

- [ ] `./src/components/elements/FeatureForm.js` - Form component
  - Fields: [list]
  - Validation: [rules]

- [ ] `./public/locales/en/ui.json` - Translations (English only)
  - Keys: [list with paths]

### API Endpoints

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/api/feature` | JWT | Fetch all items |
| GET | `/api/feature/:id` | JWT | Fetch by ID |
| POST | `/api/feature` | JWT | Create new |
| PUT | `/api/feature/:id` | JWT | Update |
| DELETE | `/api/feature/:id` | JWT | Delete |

### Components (React)

**Component Hierarchy:**
```
FeaturePage
├── FeatureGrid (Kendo Grid)
├── FeatureForm
│   ├── FormInputCard (multiple)
│   └── FeatureActions
└── FeatureDetails
```

### Database Schema

**Collection:** `feature_collection`

```json
{
  "_id": "ObjectId",
  "name": "string",
  "description": "string",
  "status": "enum: [active, inactive]",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

**Indexes:**
- name (unique)
- status
- createdAt

---

## TODO List

### Backend
- [ ] Create validation schema (AJV)
- [ ] Create DAL methods (find, insert, update, delete)
- [ ] Create service layer (business logic)
- [ ] Create controller (request handling)
- [ ] Create routes (Express endpoints)
- [ ] Add authentication middleware
- [ ] Add rate limiting

### Frontend
- [ ] Create API service
- [ ] Create Redux actions/reducers
- [ ] Create page component
- [ ] Create form component
- [ ] Create grid component
- [ ] Add translations (English only)
- [ ] Add loading states
- [ ] Add error handling

### Testing
- [ ] Backend unit tests
- [ ] Backend integration tests
- [ ] Frontend component tests
- [ ] E2E tests

### Documentation
- [ ] API documentation
- [ ] Component documentation
- [ ] User guide updates

---

## Implementation Notes

### Key Decisions

**Decision 1: [Topic]**
- Rationale: [Why]
- Alternative considered: [What else]
- Chosen approach: [Final decision]

**Decision 2: [Topic]**
- ...

### Edge Cases

1. **Edge Case 1:** [Description]
   - Handling: [How to handle]

2. **Edge Case 2:** [Description]
   - Handling: [How to handle]

### Dependencies

**Backend:**
- Package 1: version X.Y.Z
- Package 2: version X.Y.Z

**Frontend:**
- Package 1: version X.Y.Z
- Package 2: version X.Y.Z

### Security Considerations

- Input sanitization: [How]
- SQL/NoSQL injection prevention: [How]
- XSS prevention: [How]
- CSRF protection: [How]

---

## Acceptance Criteria

### Functional
- [ ] Feature works as expected for all user roles
- [ ] All API endpoints return correct data
- [ ] UI displays correct information
- [ ] Form validation works correctly
- [ ] Error messages are clear and helpful

### Security
- [ ] All endpoints secured with authentication
- [ ] Request bodies validated with AJV schemas
- [ ] User input sanitized
- [ ] No sensitive data exposed in responses

### Performance
- [ ] API responses < 200ms for typical queries
- [ ] UI renders without lag
- [ ] No memory leaks

### Code Quality
- [ ] Code follows project style guide
- [ ] All functions have JSDoc comments
- [ ] Error handling implemented everywhere
- [ ] Tests cover critical paths

### UI/UX
- [ ] UI uses approved design pattern
- [ ] All text internationalized (English)
- [ ] Responsive design works on mobile
- [ ] Loading states implemented
- [ ] Error states handled gracefully

---

## Change Log

### YYYY-MM-DD - Initial Planning

**Decisions:**
- Spec created
- Requirements discussed
- Technical design approved

**Status:** Planning phase complete

---

### YYYY-MM-DD - Development Day 1

**Decisions:**
- Decision 1: [Explanation]
- Decision 2: [Explanation]

**Progress:**
- ✅ Created validation schema
- ✅ Created DAL layer
- 🔄 Working on service layer (in progress)
- ⏳ Controller pending

**Blockers:**
- None

**Daily Summary:**
Completed backend validation schema and DAL layer. Started service layer implementation. Tomorrow: finish service layer and start controller.

---

### YYYY-MM-DD - Development Day 2

**Decisions:**
- ...

**Progress:**
- ...

**Blockers:**
- ...

**Daily Summary:**
...

---

### YYYY-MM-DD - Completion

**Final Status:**
- ✅ All features implemented
- ✅ All acceptance criteria met
- ✅ User approved

**Archived:** YYYY-MM-DD

---
```

---

## Progress Reporting Examples

### Starting Work
```
🔨 Feature: [Name]
📋 Reading spec: .results/spec/feature-name.md

Plan:
1. Backend (3 files)
2. Frontend (4 files)
3. Testing

Starting with backend validation schema...
```

### During Work
```
🔨 Step 2/6: Creating service layer...
  ✅ 2.1: fetchAll() method
  ✅ 2.2: create() method
  🔄 2.3: update() method (writing...)
  ⏳ 2.4: delete() method
```

### When Blocked
```
⚠️ Question: Should deleted items be soft-deleted or hard-deleted?

Options:
A. Soft delete (mark as deleted, keep in DB)
B. Hard delete (remove from DB)

Current implementation: Hard delete
Recommendation: Soft delete for audit trail

Your decision?
```

### Completion
```
✅ Feature complete!

Created:
- 3 backend files (schema, DAL, service)
- 4 frontend files (API, Redux, components)
- 15 tests (all passing)

Updated:
- Translations (English only)
- API documentation

Spec archived to:
.results/spec/archive/feature-name_2026-01-15.md

Ready for review!
```

---

**Template Version:** 1.0
**Last Updated:** 2026-01-21
**For:** Complex features (4+ files)
