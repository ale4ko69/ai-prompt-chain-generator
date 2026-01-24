# Step 2: Categorize Files

**Purpose:** Organize project files into logical categories based on their purpose and functionality.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

**Dependencies:**
- Read `{output_folder}/0-detect-setup.md` for project mode
- Read `{output_folder}/1-determine-techstack.md` for language/framework info

---

## Instructions

### 1. Language-Specific Categorization

Based on the primary language detected in Step 1, use appropriate categorization:

#### **Node.js / JavaScript / TypeScript**

```
Backend:
  ├── Routes (routing layer)
  ├── Controllers (request handling)
  ├── Services (business logic)
  ├── DAL (Data Access Layer - database operations)
  ├── Models (data models/schemas)
  ├── Middleware (authentication, validation, etc.)
  └── Utils (helper functions)

Frontend:
  ├── Components (React/Vue/Angular components)
  ├── Pages (route-level components)
  ├── State (Redux/Zustand/Pinia stores)
  ├── Services (API calls)
  ├── Hooks (custom React hooks)
  ├── Utils (helper functions)
  └── Assets (images, styles, fonts)

Shared:
  ├── Types/Interfaces (TypeScript definitions)
  ├── Constants (configuration values)
  └── Validation Schemas (Joi, Yup, Zod)
```

#### **Python**

```
Backend (Flask/Django/FastAPI):
  ├── Views/Routes (endpoint definitions)
  ├── Controllers/ViewSets (request handling)
  ├── Services (business logic)
  ├── Models (SQLAlchemy/Django models)
  ├── Schemas (Pydantic/Marshmallow)
  ├── Middleware (request/response processing)
  └── Utils (helper functions)

CLI/Scripts:
  ├── Commands (Click/argparse commands)
  ├── Tasks (background jobs)
  └── Automation (deployment, data processing)

Tests:
  ├── Unit Tests (pytest)
  ├── Integration Tests
  └── Fixtures (test data)
```

#### **C# / .NET**

```
Backend (ASP.NET Core):
  ├── Controllers (API endpoints)
  ├── Services (business logic)
  ├── Repositories (data access)
  ├── Models (domain entities)
  ├── DTOs (Data Transfer Objects)
  ├── Middleware (request pipeline)
  └── Extensions (helper methods)

Desktop/Console:
  ├── Program.cs (entry point)
  ├── Commands (CLI commands)
  ├── Services (business logic)
  └── Infrastructure (external integrations)

Tests:
  ├── Unit Tests (xUnit/NUnit)
  ├── Integration Tests
  └── Test Helpers
```

#### **Bash / Shell Scripts**

```
Scripts:
  ├── Installation (setup scripts)
  ├── Deployment (CI/CD automation)
  ├── Maintenance (backup, cleanup)
  ├── Monitoring (health checks, log analysis)
  └── Utilities (helper functions)

Library Functions:
  ├── lib/ (reusable functions)
  ├── config/ (configuration loading)
  └── common/ (shared utilities)
```

#### **Docker / Infrastructure**

```
Containerization:
  ├── Dockerfiles (image definitions)
  ├── docker-compose.yml (service orchestration)
  ├── .dockerignore (exclusions)
  └── entrypoint scripts

Kubernetes:
  ├── Deployments
  ├── Services
  ├── ConfigMaps
  ├── Secrets
  └── Ingress

CI/CD:
  ├── GitHub Actions workflows
  ├── Jenkinsfiles
  ├── Build scripts
  └── Deployment scripts
```

### 2. Identify Critical Files

**Configuration Files:**
- Environment configs (.env, appsettings.json, config.py)
- Build configs (package.json, requirements.txt, *.csproj)
- Server configs (nginx.conf, apache.conf)
- Database migrations

**Entry Points:**
- Backend: server.js, app.py, Program.cs, main.go
- Frontend: index.html, App.js, main.ts
- CLI: cli.py, program.cs, main.sh

**Core Business Logic:**
- Entity definitions (User, Product, Order models)
- Key algorithms (pricing, recommendations, analytics)
- Domain-specific services

### 3. Map File Locations

For each category, document actual file paths:

```
Example (Node.js project):

Routes:
  - lib/routes/index.js
  - lib/routes/users.js
  - lib/routes/assets.js

Controllers:
  - lib/controllers/usersController.js
  - lib/controllers/assetsController.js

Services:
  - lib/common/services/userServices.js
  - lib/common/services/adServices.js

Validation Schemas:
  - lib/schemes/local/usersScheme.js
  - lib/schemes/local/adSchema.js
```

### 4. Identify Patterns

Look for consistent patterns in the codebase:

**Naming Conventions:**
- File naming: camelCase, PascalCase, kebab-case, snake_case
- Class naming: User vs user_model vs UserModel
- Function naming: getUser vs get_user vs GetUser

**Directory Structure:**
- Flat vs nested
- Feature-based vs type-based
- Monolithic vs modular

**Import/Export Patterns:**
- ES Modules vs CommonJS
- Relative vs absolute imports
- Barrel exports (index.js/ts)

### 5. Multi-Project Categorization

If multi-project mode, categorize separately for each project:

```
Project 1 (Backend API):
  - Routes: 42 files
  - Controllers: 38 files
  - Services: 52 files

Project 2 (Frontend UI):
  - Components: 124 files
  - Pages: 18 files
  - State: 12 Redux slices
```

Identify **shared categories** (appear in both projects):
- Entity definitions
- Validation schemas
- API type definitions

---

## Output Format

Save to `{output_folder}/2-categorize-files.md`:

```markdown
# File Categorization

## Project Mode
[Single | Multi-Project]

## Primary Language
[from Step 1]

## Category Structure

### Backend (if applicable)
**Pattern:** [3-layer (Route→Controller→Service→DAL) | MVC | Other]

**Routes/Endpoints:**
- Location: `{path}`
- Count: {number} files
- Pattern: {description}

**Controllers/Views:**
- Location: `{path}`
- Count: {number} files
- Naming: {pattern}

**Services/Business Logic:**
- Location: `{path}`
- Count: {number} files
- Examples: {key services}

**Data Access:**
- Location: `{path}`
- Count: {number} files
- ORM: {Mongoose | Sequelize | EF | etc.}

**Models/Schemas:**
- Location: `{path}`
- Count: {number} files
- Examples: {User, Asset, Config models}

**Middleware:**
- Location: `{path}`
- Count: {number} files
- Types: {authentication, validation, error handling}

### Frontend (if applicable)
**Framework:** [from Step 1]

**Components:**
- Location: `{path}`
- Count: {number} files
- Naming: {PascalCase.jsx | index.js | etc.}

**Pages/Routes:**
- Location: `{path}`
- Count: {number} files

**State Management:**
- Location: `{path}`
- Pattern: {Redux Thunk | Context API | Zustand | etc.}
- Slices/Stores: {list}

**Services/API Calls:**
- Location: `{path}`
- Count: {number} files

### Configuration
**Environment:**
- Dev: {files}
- Prod: {files}
- Test: {files}

**Build:**
- {package.json, webpack.config.js, vite.config.ts, etc.}

**Database:**
- Migrations: {location}
- Seeds: {location}

### Infrastructure
**Docker:**
- Dockerfiles: {count}
- Compose files: {count}

**CI/CD:**
- {GitHub Actions, Jenkins, GitLab CI}
- Workflows: {count}

**Scripts:**
- Bash: {count} files in {locations}
- Python: {count} files in {locations}
- PowerShell: {count} files in {locations}

### Tests
**Unit Tests:**
- Location: `{path}`
- Count: {number} files
- Framework: {Jest, pytest, xUnit}

**Integration Tests:**
- Location: `{path}`
- Count: {number} files

**E2E Tests:**
- Location: `{path}`
- Count: {number} files
- Framework: {Cypress, Playwright}

## Multi-Project Breakdown (if applicable)

### Project 1: {name}
[repeat categorization above]

### Project 2: {name}
[repeat categorization above]

### Shared Categories
- Entity configs: {locations}
- Validation schemas: {locations}
- Type definitions: {locations}

## Key Observations
- Most complex area: {description}
- Well-structured areas: {list}
- Areas needing attention: {list}
- Consistent patterns: {list}
- Inconsistent patterns: {list}

## Next Steps
Proceed to Step 3: Identify Architecture
```

---

## Tips

**For Large Codebases:**
- Use `find` and `wc -l` for counts
- Focus on top-level directories first
- Sample files from each category for pattern detection

**For Monorepos:**
- Categorize each package/workspace separately
- Identify shared patterns across packages

**For Microservices:**
- Categorize each service independently
- Document inter-service communication patterns

---

**After completion, proceed to chains/3-identify-architecture.md**
