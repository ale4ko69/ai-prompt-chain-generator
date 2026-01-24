# Step 1: Determine Tech Stack

**Purpose:** Identify the primary programming language(s), frameworks, libraries, and tools used in the project(s).

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

**Dependencies:** Must read `{output_folder}/0-detect-setup.md` to know project mode

---

## Instructions

### 1. Identify Primary Language(s)

Analyze configuration files and source code distribution:

**Configuration Files to Check:**
- `package.json` → Node.js/JavaScript/TypeScript
- `requirements.txt` / `pyproject.toml` / `setup.py` → Python
- `*.csproj` / `*.sln` → C# / .NET
- `pom.xml` / `build.gradle` → Java
- `go.mod` → Go
- `Cargo.toml` → Rust
- `Gemfile` → Ruby
- `composer.json` → PHP

**Source Code Analysis:**
```bash
# Count files by extension
find . -type f -name "*.py" | wc -l
find . -type f -name "*.js" | wc -l
find . -type f -name "*.ts" | wc -l
find . -type f -name "*.cs" | wc -l
find . -type f -name "*.sh" | wc -l
```

Determine **primary language** (most files) and **secondary languages** (significant presence).

### 2. Identify Frameworks

**Backend Frameworks:**
- **Python:** Flask, Django, FastAPI, Tornado
- **Node.js:** Express, NestJS, Fastify, Koa
- **C#:** ASP.NET Core, ASP.NET MVC, Minimal APIs
- **Java:** Spring Boot, Quarkus, Micronaut
- **Go:** Gin, Echo, Chi
- **Ruby:** Rails, Sinatra

**Frontend Frameworks:**
- React, Angular, Vue, Svelte, Next.js, Nuxt, SvelteKit
- UI Libraries: Material-UI, Ant Design, Kendo React, Bootstrap, Tailwind CSS

**Check:**
- `package.json` → dependencies section
- Import statements in source files
- Framework-specific directory structures

### 3. Identify Databases

**SQL:**
- PostgreSQL, MySQL, SQL Server, SQLite, Oracle

**NoSQL:**
- MongoDB, Redis, Cassandra, CouchDB, Elasticsearch

**ORM/ODM:**
- Python: SQLAlchemy, Django ORM, Peewee
- Node.js: Sequelize, TypeORM, Mongoose, Prisma
- C#: Entity Framework, Dapper

**Check:**
- Connection strings in config files
- Import statements for database clients
- Docker compose database services

### 4. Identify Infrastructure & DevOps

**Containerization:**
- Docker (Dockerfile presence)
- Docker Compose (docker-compose.yml)
- Kubernetes (k8s manifests)

**CI/CD:**
- GitHub Actions (.github/workflows/)
- GitLab CI (.gitlab-ci.yml)
- Jenkins (Jenkinsfile)
- Azure DevOps (azure-pipelines.yml)

**Scripting:**
- Bash scripts (*.sh)
- PowerShell (*.ps1)
- Python automation scripts

### 5. Identify Testing Tools

- **JavaScript/TypeScript:** Jest, Mocha, Chai, Cypress, Playwright
- **Python:** pytest, unittest, nose
- **C#:** xUnit, NUnit, MSTest
- **Java:** JUnit, TestNG

### 6. Identify Build Tools

- **Node.js:** npm, yarn, pnpm, webpack, vite, rollup
- **Python:** pip, poetry, pipenv
- **C#:** MSBuild, dotnet CLI
- **Java:** Maven, Gradle

### 7. Detect Shared Packages (Multi-Project)

If multi-project mode, identify shared libraries:

```
Check for:
- Monorepo packages (packages/*, libs/*)
- Scoped packages (@company/package-name)
- Local path dependencies in package.json
- Shared Python modules
- Shared NuGet packages
```

---

## Output Format

Save to `{output_folder}/1-determine-techstack.md`:

```markdown
# Tech Stack Analysis

## Primary Language
**Language:** [Python | JavaScript | TypeScript | C# | Java | Go | Bash | Other]
**Version:** [e.g., Python 3.11, Node.js 21.2.0, .NET 8.0]

## Secondary Languages (if any)
- Language: Version (usage context)

## Backend Stack
**Framework:** [Express 5.0 | Django 4.2 | ASP.NET Core 8.0 | etc.]
**Runtime/Platform:** [Node.js | Python | .NET | JVM]
**Key Libraries:**
- Library 1 (version) - purpose
- Library 2 (version) - purpose

## Frontend Stack (if applicable)
**Framework:** [React 19.2 | Angular 17 | Vue 3 | etc.]
**UI Library:** [Kendo React | Material-UI | Ant Design | etc.]
**State Management:** [Redux | Zustand | Pinia | etc.]
**Build Tool:** [Vite | Webpack | etc.]

## Database
**Primary:** [MongoDB 7.0 | PostgreSQL 15 | SQL Server 2022 | etc.]
**ORM/ODM:** [Mongoose | Sequelize | Entity Framework | etc.]
**Caching:** [Redis | Memcached | etc.] (if applicable)

## Infrastructure
**Containerization:** [Docker | None]
**Orchestration:** [Docker Compose | Kubernetes | None]
**CI/CD:** [GitHub Actions | GitLab CI | Jenkins | None]

## Testing
**Unit Testing:** [Jest | pytest | xUnit | etc.]
**Integration Testing:** [Supertest | pytest | etc.]
**E2E Testing:** [Cypress | Playwright | Selenium | etc.]

## Shared Packages (Multi-Project Only)
- @scadafence/node-utils (^10.0.11) - Backend utilities
- @scadafence/shared-utils (^5.0.3) - Common utilities
[list all shared packages]

## Build & Package Management
**Package Manager:** [npm | yarn | pnpm | pip | NuGet | Maven | etc.]
**Build Tool:** [npm scripts | webpack | MSBuild | Maven | etc.]

## Scripting & Automation
- Bash scripts: [count] files
- PowerShell scripts: [count] files
- Python automation: [count] files

## Language Template Selection
**Selected Template:** `templates/base/languages/{primary_language}-patterns.md`
**Fallback:** `templates/base/languages/generic-patterns.md` (if specific not available)

## Next Steps
Proceed to Step 2: Categorize Files
```

---

## Analysis Depth

**Minimum (Quick):**
- Read package.json / requirements.txt / *.csproj
- Identify primary language and framework

**Recommended (Thorough):**
- Scan all configuration files
- Count source files by type
- Identify all major dependencies
- Check Docker/CI configuration

**Maximum (Deep):**
- Parse import statements
- Analyze dependency tree
- Identify version conflicts
- Check for deprecated packages

---

**After completion, proceed to chains/2-categorize-files.md**
