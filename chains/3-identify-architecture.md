# Step 3: Identify Architecture

**Purpose:** Analyze the project's architectural patterns, design decisions, and structural organization.

**Parameters:**
- `{workspace_root}` - Root directory of the workspace
- `{output_folder}` - Where to save analysis results (e.g., `.results/`)

**Dependencies:**
- Read `{output_folder}/1-determine-techstack.md` for language/framework
- Read `{output_folder}/2-categorize-files.md` for file organization

---

## Instructions

This step is **language-agnostic** - analyze architectural patterns based on the tech stack identified in Step 1.

### 1. Identify Layering Pattern

Analyze how the application is structured into layers:

#### **Backend Architectures**

**3-Layer Architecture (Common in Express, Flask, ASP.NET):**
```
Presentation Layer (Routes/Controllers)
    ↓
Business Logic Layer (Services)
    ↓
Data Access Layer (Repositories/DAL)
```

**MVC (Model-View-Controller):**
```
Views (presentation)
    ↔
Controllers (orchestration)
    ↔
Models (data + business logic)
```

**Clean Architecture / Hexagonal:**
```
Domain (entities, business rules)
    ←
Use Cases (application logic)
    ←
Interface Adapters (controllers, presenters)
    ←
Infrastructure (DB, external services)
```

**Microservices:**
```
Service 1 (independently deployable)
Service 2 (independently deployable)
    ↕
API Gateway / Message Queue
```

**Check:**
- Are layers clearly separated into different directories?
- Do files in higher layers import from lower layers only?
- Is there a clear dependency direction?

#### **Frontend Architectures**

**Component-Based (React, Vue, Angular):**
```
Pages/Containers (route-level, state management)
    ↓
Components (reusable UI elements)
    ↓
UI Primitives (buttons, inputs)
```

**State Management Patterns:**
- Flux/Redux (unidirectional data flow)
- Context API (prop drilling avoidance)
- Zustand/Pinia (simplified state)
- Server State (React Query, SWR)

**Routing:**
- File-based (Next.js, Nuxt)
- Configuration-based (React Router, Vue Router)

### 2. Identify Design Patterns

Look for common design patterns in the code:

**Creational Patterns:**
- Singleton (database connections, loggers)
- Factory (object creation based on type)
- Builder (complex object construction)

**Structural Patterns:**
- Adapter (wrapping third-party libraries)
- Decorator (middleware, higher-order components)
- Facade (simplified API over complex system)

**Behavioral Patterns:**
- Strategy (interchangeable algorithms)
- Observer (event systems, pub/sub)
- Repository (data access abstraction)
- Dependency Injection (constructor injection, service containers)

**Check:**
- Scan for pattern keywords in file/class names
- Look for consistent object creation approaches
- Identify abstraction layers

### 3. Analyze Data Flow

Map how data moves through the application:

#### **Backend Data Flow**

```
HTTP Request
    ↓
Middleware (authentication, validation)
    ↓
Route (endpoint definition)
    ↓
Controller (request parsing, response formatting)
    ↓
Service (business logic)
    ↓
DAL/Repository (database operations)
    ↓
Database
```

**Check:**
- Where does request validation happen? (middleware vs controller vs service)
- Where is business logic located? (controller vs service)
- How are database operations abstracted?

#### **Frontend Data Flow**

```
User Action
    ↓
Event Handler (onClick, onSubmit)
    ↓
Action Creator (Redux) / API Call (direct)
    ↓
Reducer (state update) / setState
    ↓
Re-render (component update)
```

**Check:**
- How is async data fetching handled? (thunks, sagas, queries)
- Where is API logic located? (components vs services vs hooks)
- How is loading/error state managed?

### 4. Analyze Communication Patterns

#### **Internal Communication**

**Synchronous:**
- Direct function calls
- HTTP REST APIs
- gRPC

**Asynchronous:**
- Message queues (RabbitMQ, Kafka, Redis)
- Event emitters
- WebSockets

**Check:**
- How do services/modules communicate?
- Are there event buses or message brokers?
- Is there a clear API contract (OpenAPI, GraphQL schema)?

#### **External Communication**

- REST APIs (fetch, axios, requests library)
- GraphQL (Apollo, Relay)
- WebSockets (Socket.io, native WebSocket)
- Third-party integrations (LDAP, AD, SMTP, etc.)

### 5. Identify Configuration Management

How is configuration handled across environments?

**Approaches:**
- Environment variables (.env files)
- Configuration files (JSON, YAML, TOML)
- Config services (AWS Parameter Store, Azure Key Vault)
- Hard-coded defaults with overrides

**Check:**
- `config/` directory
- `.env`, `.env.example` files
- `appsettings.json`, `config.py`, `settings.ts`
- How secrets are managed (vault, env vars, encrypted files)

### 6. Identify Error Handling Strategy

**Patterns:**
- Try/catch blocks
- Error boundaries (React)
- Global error handlers (Express middleware, Django middleware)
- Result/Either monads (functional approach)

**Check:**
- Are errors handled consistently?
- Is there centralized error logging?
- How are errors returned to clients? (status codes, error objects)

### 7. Analyze Authentication & Authorization

**Authentication:**
- JWT tokens
- Session-based (cookies)
- OAuth 2.0 / OpenID Connect
- API keys

**Authorization:**
- Role-based access control (RBAC)
- Permission-based
- Attribute-based (ABAC)

**Check:**
- `middleware/auth.js`, `guards/`, `decorators/`
- Passport.js, JWT libraries, OAuth clients
- Where are auth checks performed? (middleware vs route vs controller)

### 8. Multi-Project Architecture (if applicable)

For multi-project scenarios, identify:

**Integration Patterns:**
- Shared database vs separate databases
- API-based communication
- Shared packages/libraries
- Event-driven communication

**Deployment Architecture:**
- Monolith vs microservices
- Containers (Docker Compose services)
- Load balancing
- Service mesh

**Example:**
```
Project 1 (Backend API)
    ↕ (REST API)
Project 2 (Frontend UI)
    ↕ (API calls)
Project 3 (Data Gateway)
    ↕ (WebSocket)
Shared MongoDB Database
```

---

## Output Format

Save to `{output_folder}/3-identify-architecture.md`:

```markdown
# Architecture Analysis

## Overall Architecture
**Type:** [Monolith | Microservices | Serverless | Other]
**Pattern:** [3-layer | MVC | Clean Architecture | Hexagonal | Custom]

## Backend Architecture (if applicable)
**Layering:**
```
[Diagram or description of layers]
Presentation: {routes, controllers}
Business Logic: {services}
Data Access: {repositories, DAL}
```

**Key Patterns:**
- Pattern 1: {description, files using it}
- Pattern 2: {description, files using it}

**Data Flow:**
```
HTTP Request → {middleware} → {route} → {controller} → {service} → {DAL} → Database
```

**Database Strategy:**
- ORM/ODM: {Mongoose, Sequelize, Entity Framework, SQLAlchemy}
- Transaction management: {yes/no, how}
- Migration strategy: {files/tools}

## Frontend Architecture (if applicable)
**Framework:** [from Step 1]
**Pattern:** {Component-based, Pages, Layouts}

**Component Hierarchy:**
```
Pages (route-level)
  ├── Containers (state management)
  ├── Components (reusable UI)
  └── Primitives (basic elements)
```

**State Management:**
- Approach: {Redux, Context, Zustand, etc.}
- Structure: {slices, stores, reducers}
- Async handling: {thunks, sagas, queries}

**Routing:**
- Library: {React Router, Vue Router, Next.js}
- Pattern: {file-based, configuration-based}

**Data Flow:**
```
User Action → {handler} → {action/API call} → {state update} → Re-render
```

## Communication Patterns

### Internal (between modules/services)
**Synchronous:**
- {Direct calls, REST APIs, gRPC}

**Asynchronous:**
- {Message queues, event emitters}
- Tools: {RabbitMQ, Redis, EventEmitter}

### External (third-party integrations)
- REST APIs: {list services}
- Authentication services: {LDAP, AD, OAuth}
- Email: {SMTP configuration}
- Other: {list integrations}

## Configuration Management
**Approach:** {Environment variables, Config files, Config service}
**Files:**
- {.env, appsettings.json, config.py}
- {config/ directory structure}

**Environment Separation:**
- Development: {config approach}
- Production: {config approach}
- Testing: {config approach}

**Secrets Management:**
- {Environment variables, Azure Key Vault, AWS Secrets Manager, encrypted files}

## Error Handling
**Strategy:** {Global handlers, Try/catch, Error boundaries}
**Implementation:**
- Backend: {middleware/errorHandler.js, custom error classes}
- Frontend: {ErrorBoundary components, try/catch in async}

**Logging:**
- Tool: {Winston, Pino, Serilog, logging module}
- Levels: {debug, info, warn, error}
- Output: {console, files, external service}

## Authentication & Authorization
**Authentication:**
- Method: {JWT, Session, OAuth 2.0}
- Implementation: {Passport.js, custom middleware, libraries}
- Token storage: {localStorage, cookies, memory}

**Authorization:**
- Pattern: {RBAC, Permission-based, ABAC}
- Implementation: {middleware, guards, decorators}
- Location: {where checks happen}

## Testing Architecture
**Unit Tests:**
- Coverage: {percentage or qualitative assessment}
- Mocking: {approach to mocking dependencies}

**Integration Tests:**
- Scope: {API endpoints, database operations}
- Setup: {test database, fixtures}

**E2E Tests:**
- Tool: {Cypress, Playwright, Selenium}
- Coverage: {critical paths tested}

## Multi-Project Architecture (if applicable)

### Project 1: {name}
- Type: {Backend API, Frontend UI, etc.}
- Architecture: {3-layer, MVC, etc.}

### Project 2: {name}
- Type: {Backend API, Frontend UI, etc.}
- Architecture: {3-layer, MVC, etc.}

### Integration
**Communication:**
- Project 1 ↔ Project 2: {REST API, WebSocket, Message Queue}

**Shared Resources:**
- Database: {Shared MongoDB, Separate databases}
- Authentication: {Shared auth service, Separate}
- File storage: {Shared S3, Local}

**Deployment:**
- {Docker Compose with multiple services}
- {Separate deployments}
- {Kubernetes cluster}

### Cross-Project Patterns
- Shared packages: {list from Step 1}
- Consistent error handling: {yes/no, describe}
- Consistent authentication: {yes/no, describe}

## Infrastructure
**Containerization:**
- Docker: {yes/no}
- Orchestration: {Docker Compose, Kubernetes}
- Services: {list services from docker-compose.yml}

**Deployment:**
- CI/CD: {GitHub Actions, Jenkins, GitLab CI}
- Environments: {dev, staging, production}
- Strategy: {blue/green, rolling, canary}

## Key Observations
**Strengths:**
- {Clear layer separation}
- {Consistent error handling}
- {Well-defined API contracts}

**Areas for Improvement:**
- {Tight coupling in some areas}
- {Inconsistent validation}
- {Missing integration tests}

**Unique Patterns:**
- {Custom patterns specific to this project}
- {Innovative solutions}

## Next Steps
Proceed to Step 4: Domain Deep Dive
```

---

## Analysis Tips

**Look for:**
- README.md files describing architecture
- Diagrams in docs/ folder
- Comments explaining design decisions
- Consistent file/folder naming patterns

**Avoid:**
- Assuming architecture based solely on framework
- Missing custom patterns unique to the project
- Ignoring multi-project interactions

---

**After completion, proceed to chains/4-domain-deep-dive.md**
