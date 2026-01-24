# Step 4: Domain Deep Dive

**Purpose:** Understand the business domain, core functionality, and key features of the application.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

**Dependencies:**
- Read all previous steps for context

---

## Instructions

This step is **language-agnostic** - focus on understanding **what the application does**, not how it's implemented.

### 1. Identify Core Domain Concepts

Analyze entity definitions, models, and database schemas to identify key business concepts:

**Entity Analysis:**

**Node.js/TypeScript:**
- Mongoose schemas
- Sequelize models
- TypeScript interfaces
- Entity configuration files (entity.js)

**Python:**
- SQLAlchemy models
- Django models
- Pydantic schemas
- Dataclasses

**C#:**
- Entity classes
- DTOs (Data Transfer Objects)
- Domain models

**Look for:**
- User/Account management
- Core business entities (Products, Orders, Assets, Networks, etc.)
- Relationships between entities (one-to-many, many-to-many)
- Key attributes and their types

**Document:**
```
Entity: User
  - Fields: id, email, password, role, permissions
  - Relationships: Has many Sessions, Belongs to Organization
  - Purpose: Authentication and authorization

Entity: Asset
  - Fields: id, name, ipAddress, type, vendor, model
  - Relationships: Belongs to Network, Has many Vulnerabilities
  - Purpose: IT/OT device management
```

### 2. Analyze Business Rules

Identify validation rules, constraints, and business logic:

**Validation Schemas:**
- Check `lib/schemes/`, `validators/`, `schemas/` directories
- Look for Joi, Yup, Zod, Pydantic, FluentValidation, or custom validators

**Business Rules Examples:**
```
- Email must be unique
- Port must be between 1 and 65535
- Password requires 8+ characters, uppercase, lowercase, number
- Asset can only be deleted if no active connections
- User with role "admin" can access all resources
```

**Check:**
- Validation at multiple layers? (frontend, backend, database)
- Custom validation functions
- Error messages for rule violations

### 3. Identify Key Features

Analyze routes, controllers, and UI components to map major features:

**Backend Routes:**
```bash
# Scan route definitions
grep -r "app.get\|app.post\|router.get\|@app.route" lib/routes/
grep -r "@RestController\|@ApiController" src/Controllers/
```

**Frontend Pages:**
```bash
# Scan page components
find src/pages/ -type f
find src/components/ -name "*Page.js"
```

**Feature Categories:**

**User Management:**
- Registration, login, logout
- Password reset
- User roles and permissions
- Profile management

**Configuration Management:**
- Application settings
- Third-party integrations (LDAP, AD, SMTP, SLDAP)
- Feature toggles
- System preferences

**Data Management:**
- CRUD operations for core entities
- Bulk operations (import, export)
- Search and filtering
- Reporting and analytics

**Integration Features:**
- External API connections
- Authentication providers (LDAP, Active Directory)
- Email services (SMTP)
- File uploads/downloads

**Administration:**
- System health monitoring
- Logs viewing
- Backup/restore
- License management

### 4. Understand Workflows

Trace critical user journeys through the codebase:

**Example Workflow Analysis:**

**User Login Flow:**
```
1. User submits credentials (frontend form)
2. POST /api/auth/login (route)
3. authController.login() validates input
4. authService.authenticate() checks credentials
5. Session/JWT token created
6. User redirected to dashboard
7. Token stored (localStorage/cookie)
8. Subsequent requests include token
```

**LDAP Configuration Flow:**
```
1. Admin navigates to LDAP settings page
2. Form displays with existing config (GET /api/ldap/config)
3. Admin edits fields (host, port, baseDN, etc.)
4. Frontend validates input (FormInputCard with INPUT_TYPES)
5. POST /api/ldap/config
6. Backend validates with ldapSchema (Joi/Yup)
7. Service tests connection (ldapService.testConnection())
8. If successful, save to database
9. Update entity.js propsToUpdate
10. Return success, update UI
```

**Map 3-5 critical workflows** in the application.

### 5. Identify Domain-Specific Terminology

Build a glossary of terms unique to this domain:

**Examples:**

**IT/OT Security Domain:**
- Asset: A managed device (server, PLC, sensor)
- Network: A logical grouping of assets
- Vulnerability: A security weakness in an asset
- CVE: Common Vulnerabilities and Exposures identifier
- SLDAP: Secure LDAP (LDAP over TLS)

**E-commerce Domain:**
- SKU: Stock Keeping Unit
- Cart: Shopping cart
- Checkout: Purchase finalization process
- Fulfillment: Order processing and shipping

**Healthcare Domain:**
- Patient: Individual receiving care
- Provider: Healthcare professional
- Encounter: Medical visit or interaction
- Diagnosis: Medical condition identification

**Check:**
- README files
- Comments in code
- Translation files (en/glossary.json, en/ui.json)
- Variable/function naming patterns

### 6. Analyze Data Relationships

Map how entities relate to each other:

**Relationship Types:**
- One-to-One (User ↔ Profile)
- One-to-Many (User → Sessions)
- Many-to-Many (Users ↔ Roles)

**Visual Mapping:**
```
User
  ├── has many → Sessions
  ├── has one → UserProfile
  ├── belongs to → Organization
  └── has many through → Permissions (via Roles)

Asset
  ├── belongs to → Network
  ├── has many → Vulnerabilities
  ├── has many → Connections
  └── has many → Events
```

### 7. Identify External Integrations

Document third-party systems and services:

**Authentication:**
- LDAP servers
- Active Directory
- OAuth providers (Google, GitHub)

**Communication:**
- SMTP servers (email)
- SMS gateways
- Push notification services

**Storage:**
- AWS S3
- Azure Blob Storage
- Local file system

**Monitoring:**
- Sentry (error tracking)
- DataDog (metrics)
- CloudWatch (logs)

### 8. Multi-Project Domain Analysis (if applicable)

For multi-project scenarios, identify:

**Shared Domain Concepts:**
- Which entities exist in both projects?
- Are they identical or adapted?

**Project-Specific Features:**
- What features are unique to Project 1?
- What features are unique to Project 2?

**Feature Sync Opportunities:**
```
Feature: SLDAP Configuration
  - Exists in: CI (scadafence)
  - Missing in: CW (sf-multisite)
  - Action: Sync from CI to CW
  - Complexity: Medium (FormInputCard adaptation needed)
```

---

## Output Format

Save to `{output_folder}/4-domain-deep-dive.md`:

```markdown
# Domain Analysis

## Domain Type
**Industry:** [IT/OT Security | E-commerce | Healthcare | Finance | Other]
**Application Type:** [Management Platform | E-commerce Store | Analytics Dashboard | Other]

## Core Entities

### Entity 1: {Name}
**Purpose:** {Brief description}

**Fields:**
- {field_name}: {type} - {description}
- {field_name}: {type} - {description}

**Relationships:**
- {relationship type} → {target entity}
- {relationship type} → {target entity}

**Validation Rules:**
- {rule description}
- {rule description}

**Key Operations:**
- {CRUD operations}
- {Special operations}

### Entity 2: {Name}
[Repeat structure]

[Continue for all major entities...]

## Business Rules

### Validation Rules
- {Rule 1}
- {Rule 2}
- {Rule 3}

### Constraints
- {Constraint 1}
- {Constraint 2}

### Permissions
- {Who can do what}
- {Role-based restrictions}

## Key Features

### Feature Category 1: {e.g., User Management}
**Features:**
- Feature 1: {Description, routes involved}
- Feature 2: {Description, routes involved}

**Implementation:**
- Backend: {files}
- Frontend: {files}

### Feature Category 2: {e.g., Configuration Management}
**Features:**
- LDAP Configuration: {Description}
- SMTP Configuration: {Description}

**Implementation:**
- Backend: {validation schemas, services, controllers}
- Frontend: {configuration pages, forms}

[Continue for all feature categories...]

## Critical Workflows

### Workflow 1: {e.g., User Login}
**Steps:**
1. {Step description}
2. {Step description}
3. {Step description}

**Files Involved:**
- {file path}
- {file path}

**Key Decision Points:**
- {Decision 1}
- {Decision 2}

### Workflow 2: {e.g., Asset Creation}
[Repeat structure]

[Document 3-5 critical workflows]

## Domain Terminology

**Glossary:**
- **{Term 1}:** {Definition}
- **{Term 2}:** {Definition}
- **{Term 3}:** {Definition}

**Acronyms:**
- {Acronym}: {Full form and meaning}

**Industry-Specific Terms:**
- {Term}: {Context and usage}

## Entity Relationships

**Diagram:**
```
{Entity 1}
  ├── {relationship} → {Entity 2}
  ├── {relationship} → {Entity 3}
  └── {relationship} → {Entity 4}

{Entity 2}
  └── {relationship} → {Entity 5}
```

**Key Relationships:**
- {Description of important relationships}
- {Cascading behaviors (delete, update)}

## External Integrations

### Authentication Services
- **LDAP:**
  - Purpose: {User authentication}
  - Configuration: {host, port, baseDN, credentials}
  - Files: {ldapSchema.js, ldapService.js, LdapConfig.js}

- **Active Directory:**
  - Purpose: {User authentication and sync}
  - Configuration: {similar to LDAP}
  - Files: {adSchema.js, adService.js}

### Communication Services
- **SMTP:**
  - Purpose: {Email notifications}
  - Configuration: {host, port, auth}
  - Files: {smtpSchema.js, smtpService.js}

### Storage Services
- {S3, Azure Blob, etc.}

### Monitoring/Analytics
- {Sentry, DataDog, etc.}

## Multi-Project Domain (if applicable)

### Project 1: {name}
**Domain Focus:** {Description}
**Unique Entities:** {List}
**Unique Features:** {List}

### Project 2: {name}
**Domain Focus:** {Description}
**Unique Entities:** {List}
**Unique Features:** {List}

### Shared Domain Concepts
**Entities in Both:**
- {Entity name}: {Differences between projects}

**Features in Both:**
- {Feature name}: {Differences in implementation}

### Sync Opportunities
**From Project 1 to Project 2:**
- {Feature/Entity name}: {Reason, Complexity}

**From Project 2 to Project 1:**
- {Feature/Entity name}: {Reason, Complexity}

## Key Observations
**Domain Complexity:**
- {Simple, Medium, Complex}
- {Reasoning}

**Well-Defined Areas:**
- {List areas with clear domain modeling}

**Areas Needing Clarity:**
- {List areas with unclear domain logic}

**Unique Aspects:**
- {What makes this domain special}
- {Custom business logic}

## Next Steps
Proceed to Step 5: Styleguide Generation
```

---

## Analysis Tips

**Sources to Examine:**
- `README.md` files
- Entity/Model files
- Validation schemas
- Translation files (often contain domain terms)
- Route/Controller files (reveal features)
- Test files (reveal expected behaviors)
- Comments in code

**Questions to Answer:**
- What problem does this application solve?
- Who are the users?
- What are the most important entities?
- What are the critical workflows?
- What external systems does it integrate with?

---

**After completion, proceed to chains/5-styleguide-generation.md**
