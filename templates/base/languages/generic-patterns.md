# Generic Coding Patterns

**Purpose:** Fallback template for languages not covered by specific templates.

**Note:** This template provides **universal coding principles** that apply across most programming languages. For language-specific patterns, create a dedicated template.

---

## General Principles

### 1. Code Readability
- **Clarity over cleverness:** Write code that others (and future you) can understand
- **Self-documenting code:** Use descriptive names for variables, functions, and classes
- **Consistent formatting:** Follow project conventions (indentation, spacing, line length)
- **Avoid deep nesting:** Extract complex logic into separate functions

### 2. Error Handling
- **Always handle errors:** Don't ignore exceptions or error codes
- **Fail fast:** Detect errors early and provide clear error messages
- **Log errors:** Include context (function name, parameters, stack trace)
- **Graceful degradation:** When possible, continue operation with reduced functionality

### 3. Code Organization
- **Single Responsibility Principle:** Each function/class should do one thing well
- **Separation of Concerns:** Keep business logic, data access, and presentation separate
- **DRY (Don't Repeat Yourself):** Extract reusable logic into functions/modules
- **Modular design:** Organize code into logical units (modules, packages, namespaces)

### 4. Testing
- **Write tests:** Unit tests for business logic, integration tests for workflows
- **Test edge cases:** Empty inputs, null values, boundary conditions
- **Mock external dependencies:** Isolate code under test
- **Test-driven development (TDD):** Write tests before implementation (when appropriate)

### 5. Documentation
- **Document public APIs:** Explain parameters, return values, side effects
- **Comment "why," not "what":** Code should show what it does; comments explain why
- **Keep documentation updated:** Outdated docs are worse than no docs
- **README files:** Explain setup, usage, architecture

---

## Naming Conventions

### Variables
- **Descriptive names:** `user_count` over `uc`, `total_amount` over `ta`
- **Avoid abbreviations:** Unless widely understood (`id`, `url`, `api`)
- **Boolean variables:** Use prefixes like `is_`, `has_`, `can_`, `should_`
  - `is_active`, `has_permission`, `can_edit`, `should_retry`

### Functions/Methods
- **Verb-based names:** `get_user()`, `calculate_total()`, `validate_email()`
- **Clear purpose:** Function name should describe what it does
- **Consistent naming:** Use same verbs across codebase (`get`, `set`, `create`, `delete`, `update`, `find`)

### Classes
- **Noun-based names:** `UserService`, `DataProcessor`, `EmailValidator`
- **Specific, not generic:** `OrderCalculator` over `Calculator`

### Constants
- **Uppercase with underscores:** `MAX_RETRIES`, `API_BASE_URL`, `DEFAULT_TIMEOUT`
- **Grouped logically:** Keep related constants together

---

## Code Structure

### Layer Separation

**Presentation Layer (UI/API):**
- Handles user input/output
- Minimal business logic
- Calls business layer

**Business Logic Layer:**
- Core application logic
- Domain rules and workflows
- Independent of presentation and data layers

**Data Access Layer:**
- Database operations (CRUD)
- External API calls
- File I/O

**Example Flow:**
```
User Request → Presentation → Business Logic → Data Access → Database
```

### Function Design

**Keep functions small:**
- Aim for 10-50 lines of code
- If function is too long, extract sub-functions

**Single responsibility:**
```
# ✅ Good: Does one thing
def calculate_tax(amount, rate):
    return amount * rate

# ❌ Bad: Does multiple things
def process_order(order):
    validate_order(order)
    calculate_tax(order)
    charge_customer(order)
    send_email(order)
    update_inventory(order)
    # Too many responsibilities
```

**Pure functions (when possible):**
- Same input always produces same output
- No side effects (doesn't modify external state)
- Easier to test and reason about

---

## Error Handling Patterns

### Try/Catch (or equivalent)
```
try:
    result = risky_operation()
except SpecificError as e:
    log_error("Operation failed", e)
    handle_error(e)
except Exception as e:
    log_error("Unexpected error", e)
    raise  # Re-raise if can't handle
```

### Return Error Codes/Objects
```
result = operation()
if result.is_error():
    handle_error(result.error)
else:
    process(result.value)
```

### Validation Before Processing
```
if not is_valid_input(data):
    raise ValidationError("Invalid input")

process(data)
```

---

## Common Patterns

### Repository Pattern (Data Access)
```
Repository abstracts data storage:
- get_by_id(id)
- get_all()
- create(entity)
- update(entity)
- delete(id)

Allows swapping data sources without changing business logic.
```

### Service Pattern (Business Logic)
```
Service encapsulates business operations:
- UserService.register_user(user_data)
- OrderService.calculate_total(order)
- EmailService.send_notification(recipient, message)

Keeps controllers/views thin.
```

### Factory Pattern (Object Creation)
```
Factory creates objects based on input:
- NotificationFactory.create("email") → EmailNotification
- NotificationFactory.create("sms") → SMSNotification

Centralizes object creation logic.
```

### Dependency Injection
```
Pass dependencies to functions/classes instead of creating them internally:

# ✅ Good: Dependency injected
class UserService:
    def __init__(self, user_repository):
        self.repository = user_repository

# ❌ Bad: Creates dependency internally
class UserService:
    def __init__(self):
        self.repository = UserRepository()  # Hard to test
```

---

## Configuration Management

### Environment Variables
- Store configuration in environment variables
- Use `.env` files for local development
- Never commit secrets to version control

### Configuration Files
- Separate configs for dev/staging/production
- Use structured formats (JSON, YAML, TOML)
- Validate configuration on startup

### Secrets Management
- Use secret management tools (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault)
- Encrypt secrets at rest
- Rotate secrets regularly

---

## Version Control (Git)

### Commit Messages
**Format:**
```
<type>: <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `refactor`: Code restructuring (no functional changes)
- `test`: Adding or updating tests
- `chore`: Build process, dependencies, tooling

**Example:**
```
feat: Add user authentication

Implemented JWT-based authentication with login and logout endpoints.
Added middleware to protect authenticated routes.

Closes #123
```

### Branch Naming
- `feature/feature-name` - New features
- `bugfix/issue-description` - Bug fixes
- `hotfix/critical-issue` - Urgent production fixes
- `refactor/area-name` - Code refactoring

---

## Testing Best Practices

### Unit Tests
- Test individual functions/methods in isolation
- Mock external dependencies
- Cover edge cases and error conditions

### Integration Tests
- Test interaction between components
- Use test database/environment
- Verify end-to-end workflows

### Test Naming
```
test_<function>_<scenario>_<expected_result>

Examples:
- test_get_user_by_id_existing_user_returns_user
- test_get_user_by_id_non_existing_user_returns_null
- test_create_user_invalid_email_raises_error
```

### Test Coverage
- Aim for 70-80% code coverage
- Focus on critical paths and complex logic
- Don't test trivial code (getters/setters)

---

## Security Best Practices

### Input Validation
- Validate all user input
- Sanitize data before processing
- Use allowlists, not denylists

### Authentication & Authorization
- Use strong password hashing (bcrypt, Argon2)
- Implement multi-factor authentication (MFA) when possible
- Check authorization on every request

### Secrets
- Never hardcode secrets in code
- Use environment variables or secret managers
- Rotate secrets regularly

### Dependencies
- Keep dependencies updated
- Scan for known vulnerabilities
- Use only trusted packages

---

## Performance Considerations

### Avoid Premature Optimization
- Make it work first, then make it fast
- Profile before optimizing
- Optimize bottlenecks, not everything

### Common Optimizations
- **Caching:** Store expensive computation results
- **Database indexing:** Add indexes on frequently queried fields
- **Batch operations:** Process multiple items at once
- **Lazy loading:** Load data only when needed
- **Asynchronous processing:** Use async for I/O-bound operations

---

## Code Review Checklist

**Functionality:**
- Does the code do what it's supposed to do?
- Are edge cases handled?
- Are errors handled appropriately?

**Code Quality:**
- Is the code readable and maintainable?
- Are names descriptive?
- Is there duplicated code?
- Are functions/classes too large?

**Testing:**
- Are there tests for new functionality?
- Do tests cover edge cases?
- Do all tests pass?

**Security:**
- Is user input validated?
- Are secrets properly managed?
- Are there potential security vulnerabilities?

**Documentation:**
- Are public APIs documented?
- Are complex algorithms explained?
- Is the README updated?

---

## Specific Language Guidance

**If your project uses:**

- **JavaScript/TypeScript/Node.js:** Use `templates/base/languages/nodejs-patterns.md`
- **Python:** Use `templates/base/languages/python-patterns.md`
- **C# / .NET:** Use `templates/base/languages/csharp-patterns.md`
- **Bash/Shell:** Use `templates/base/languages/bash-patterns.md`
- **Other languages:** Follow this generic template and create a language-specific template for your project

---

## Key Takeaways

1. **Readability matters:** Code is read more than it's written
2. **Handle errors:** Don't ignore failures
3. **Test your code:** Automated tests prevent regressions
4. **Document why, not what:** Code should be self-documenting
5. **Keep it simple:** Simple solutions are easier to maintain
6. **Separate concerns:** UI, business logic, and data access should be independent
7. **Secure by default:** Validate input, protect secrets, check authorization
8. **Review before merging:** Peer review catches bugs and improves quality

---

**Note:** This is a **fallback template**. For better guidance, create a language-specific pattern file in `templates/base/languages/`.
