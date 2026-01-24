# Node.js / JavaScript / TypeScript Patterns

**Purpose:** Language-specific coding patterns for Node.js-based projects.

---

## Code Style

### Quote Style
**Standard:** Double quotes (`"`) for all strings
```javascript
const message = "Hello, world";
import { Router } from "express";
```

**Exception:** Template literals when interpolation is needed
```javascript
const greeting = `Hello, ${name}`;
```

### Semicolons
**Standard:** Always use semicolons
```javascript
const result = calculate();
return result;
```

### Indentation
**Standard:** 2 spaces (not tabs)

### Line Length
**Recommendation:** Keep under 120 characters

---

## Naming Conventions

### Files
- **JavaScript:** `camelCase.js`
- **TypeScript:** `camelCase.ts`
- **React Components:** `PascalCase.jsx` or `PascalCase.tsx`
- **Tests:** `filename.test.js` or `filename.spec.ts`

### Variables & Functions
```javascript
// camelCase for variables and functions
const userName = "John";
function getUserById(id) { }

// PascalCase for classes and React components
class UserService { }
const MyComponent = () => { };

// UPPER_SNAKE_CASE for constants
const MAX_RETRIES = 3;
const API_BASE_URL = "https://api.example.com";
```

### Private Methods/Properties
```javascript
class Service {
  _privateMethod() { }  // Leading underscore
  #privateField;        // Private class fields (ES2022+)
}
```

---

## Backend Architecture (Express / NestJS)

### 3-Layer Pattern
```
Route → Controller → Service → DAL
```

**Route (Endpoint Definition):**
```javascript
// lib/routes/users.js
const express = require("express");
const router = express.Router();
const usersController = require("../controllers/usersController");

router.get("/users", usersController.getUsers);
router.post("/users", usersController.createUser);

module.exports = router;
```

**Controller (Request Handling):**
```javascript
// lib/controllers/usersController.js
const usersService = require("../common/services/usersService");
const { catchError } = require("../utils/errorHandler");

async function getUsers(req, res) {
  try {
    const users = await usersService.getUsers();
    res.json(users);
  } catch (error) {
    return catchError(error, "getUsers");
  }
}

module.exports = { getUsers };
```

**Service (Business Logic):**
```javascript
// lib/common/services/usersService.js
const usersDal = require("../../dal/usersDal");

async function getUsers() {
  return await usersDal.find();
}

async function createUser(userData) {
  // Business logic: validation, transformation
  return await usersDal.insert(userData);
}

module.exports = { getUsers, createUser };
```

**DAL (Data Access Layer):**
```javascript
// lib/dal/usersDal.js
const User = require("../models/User");

async function find(query = {}) {
  return await User.find(query);
}

async function insert(userData) {
  const user = new User(userData);
  return await user.save();
}

module.exports = { find, insert };
```

---

## Error Handling

### Pattern: `catchError()`
```javascript
const { catchError } = require("../utils/errorHandler");

async function myFunction() {
  try {
    const result = await someAsyncOperation();
    return result;
  } catch (error) {
    return catchError(error, "myFunction");
  }
}
```

### Custom Error Classes
```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
    this.statusCode = 400;
  }
}

class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = "NotFoundError";
    this.statusCode = 404;
  }
}
```

### Express Error Middleware
```javascript
// lib/middleware/errorHandler.js
function errorHandler(err, req, res, next) {
  const statusCode = err.statusCode || 500;
  const message = err.message || "Internal Server Error";

  res.status(statusCode).json({
    error: {
      message,
      statusCode,
      ...(process.env.NODE_ENV === "development" && { stack: err.stack })
    }
  });
}

module.exports = errorHandler;
```

---

## Validation

### Joi Schemas (Backend)
```javascript
// lib/schemes/local/usersScheme.js
const Joi = require("joi");

const createUserSchema = Joi.object({
  email: Joi.string().email().required(),
  password: Joi.string().min(8).required(),
  name: Joi.string().min(2).max(100).required(),
  role: Joi.string().valid("admin", "user").default("user")
});

module.exports = { createUserSchema };
```

### Validation Middleware
```javascript
// lib/middleware/validate.js
function validate(schema) {
  return (req, res, next) => {
    const { error, value } = schema.validate(req.body);
    if (error) {
      return res.status(400).json({ error: error.details[0].message });
    }
    req.body = value;
    next();
  };
}

module.exports = validate;
```

### Usage in Routes
```javascript
const validate = require("../middleware/validate");
const { createUserSchema } = require("../schemes/local/usersScheme");

router.post("/users", validate(createUserSchema), usersController.createUser);
```

---

## React Patterns

### Functional Components
```javascript
// src/components/UserCard.jsx
import React from "react";

const UserCard = ({ user, onEdit, onDelete }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onEdit(user.id)}>Edit</button>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
};

export default UserCard;
```

### Hooks
```javascript
import { useState, useEffect } from "react";

const UserList = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    try {
      const response = await fetch("/api/users");
      const data = await response.json();
      setUsers(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

### Custom Hooks
```javascript
// src/hooks/useFetch.js
import { useState, useEffect } from "react";

export const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};
```

---

## State Management (Redux)

### Redux Toolkit Slice
```javascript
// src/store/slices/usersSlice.js
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";
import { fetchUsers } from "../../services/api";

export const loadUsers = createAsyncThunk(
  "users/loadUsers",
  async (_, { rejectWithValue }) => {
    try {
      const users = await fetchUsers();
      return users;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const usersSlice = createSlice({
  name: "users",
  initialState: {
    items: [],
    loading: false,
    error: null
  },
  reducers: {
    addUser: (state, action) => {
      state.items.push(action.payload);
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(loadUsers.pending, (state) => {
        state.loading = true;
      })
      .addCase(loadUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(loadUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});

export const { addUser } = usersSlice.actions;
export default usersSlice.reducer;
```

### Usage in Component
```javascript
import { useDispatch, useSelector } from "react-redux";
import { loadUsers } from "../store/slices/usersSlice";

const UserList = () => {
  const dispatch = useDispatch();
  const { items, loading, error } = useSelector(state => state.users);

  useEffect(() => {
    dispatch(loadUsers());
  }, [dispatch]);

  // ...render logic
};
```

---

## TypeScript Patterns

### Interface Definitions
```typescript
// src/types/user.ts
export interface User {
  id: string;
  email: string;
  name: string;
  role: "admin" | "user";
  createdAt: Date;
}

export interface CreateUserDto {
  email: string;
  password: string;
  name: string;
}
```

### Typed Components
```typescript
// src/components/UserCard.tsx
import React from "react";
import { User } from "../types/user";

interface UserCardProps {
  user: User;
  onEdit: (id: string) => void;
  onDelete: (id: string) => void;
}

const UserCard: React.FC<UserCardProps> = ({ user, onEdit, onDelete }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onEdit(user.id)}>Edit</button>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
};

export default UserCard;
```

### Generic Utility Functions
```typescript
// src/utils/api.ts
export async function fetchData<T>(url: string): Promise<T> {
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  return await response.json();
}

// Usage
const users = await fetchData<User[]>("/api/users");
```

---

## Async/Await Patterns

### Promise Handling
```javascript
// ✅ Good: Proper error handling
async function fetchUser(id) {
  try {
    const user = await userService.getById(id);
    return user;
  } catch (error) {
    return catchError(error, "fetchUser");
  }
}

// ✅ Good: Parallel requests
async function fetchUserData(userId) {
  const [user, posts, comments] = await Promise.all([
    fetchUser(userId),
    fetchPosts(userId),
    fetchComments(userId)
  ]);
  return { user, posts, comments };
}

// ❌ Bad: Sequential when could be parallel
async function fetchUserDataBad(userId) {
  const user = await fetchUser(userId);
  const posts = await fetchPosts(userId);
  const comments = await fetchComments(userId);
  return { user, posts, comments };
}
```

---

## Modules & Imports

### ES Modules (Preferred)
```javascript
// Named exports
export const helper1 = () => { };
export const helper2 = () => { };

// Named imports
import { helper1, helper2 } from "./helpers";
```

### CommonJS (Legacy)
```javascript
// Exports
module.exports = { helper1, helper2 };

// Imports
const { helper1, helper2 } = require("./helpers");
```

### Barrel Exports
```javascript
// src/utils/index.js
export { formatDate } from "./dateUtils";
export { validateEmail } from "./validators";
export { catchError } from "./errorHandler";

// Usage
import { formatDate, validateEmail } from "./utils";
```

---

## Testing (Jest)

### Unit Test Example
```javascript
// lib/services/__tests__/usersService.test.js
const usersService = require("../usersService");
const usersDal = require("../../dal/usersDal");

jest.mock("../../dal/usersDal");

describe("usersService", () => {
  describe("getUsers", () => {
    it("should return all users", async () => {
      const mockUsers = [{ id: 1, name: "John" }];
      usersDal.find.mockResolvedValue(mockUsers);

      const result = await usersService.getUsers();

      expect(result).toEqual(mockUsers);
      expect(usersDal.find).toHaveBeenCalledTimes(1);
    });
  });
});
```

### React Testing Library
```javascript
// src/components/__tests__/UserCard.test.jsx
import { render, screen, fireEvent } from "@testing-library/react";
import UserCard from "../UserCard";

describe("UserCard", () => {
  const mockUser = { id: "1", name: "John Doe", email: "john@example.com" };
  const mockOnEdit = jest.fn();
  const mockOnDelete = jest.fn();

  it("renders user information", () => {
    render(<UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />);

    expect(screen.getByText("John Doe")).toBeInTheDocument();
    expect(screen.getByText("john@example.com")).toBeInTheDocument();
  });

  it("calls onEdit when edit button is clicked", () => {
    render(<UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />);

    fireEvent.click(screen.getByText("Edit"));
    expect(mockOnEdit).toHaveBeenCalledWith("1");
  });
});
```

---

## Documentation (JSDoc)

### Function Documentation
```javascript
/**
 * Retrieves a user by their ID.
 *
 * @param {string} id - The user's unique identifier
 * @returns {Promise<User>} The user object
 * @throws {NotFoundError} If user does not exist
 *
 * @example
 * const user = await getUserById("123");
 * console.log(user.name);
 */
async function getUserById(id) {
  const user = await usersDal.findById(id);
  if (!user) {
    throw new NotFoundError(`User ${id} not found`);
  }
  return user;
}
```

### Class Documentation
```javascript
/**
 * Service for managing user operations.
 *
 * @class UserService
 */
class UserService {
  /**
   * Creates a new user.
   *
   * @param {CreateUserDto} userData - User data
   * @returns {Promise<User>} Created user
   */
  async createUser(userData) {
    // Implementation
  }
}
```

---

## Environment Configuration

### .env File
```bash
# Database
DB_HOST=localhost
DB_PORT=27017
DB_NAME=myapp

# Server
PORT=3000
NODE_ENV=development

# External Services
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=user@example.com
SMTP_PASS=secret
```

### Config File
```javascript
// config/index.js
require("dotenv").config();

module.exports = {
  port: process.env.PORT || 3000,
  env: process.env.NODE_ENV || "development",
  db: {
    host: process.env.DB_HOST || "localhost",
    port: process.env.DB_PORT || 27017,
    name: process.env.DB_NAME || "myapp"
  },
  smtp: {
    host: process.env.SMTP_HOST,
    port: process.env.SMTP_PORT,
    auth: {
      user: process.env.SMTP_USER,
      pass: process.env.SMTP_PASS
    }
  }
};
```

---

## Common Patterns

### Singleton Pattern
```javascript
// lib/utils/logger.js
let instance = null;

class Logger {
  constructor() {
    if (instance) {
      return instance;
    }
    instance = this;
  }

  log(message) {
    console.log(`[${new Date().toISOString()}] ${message}`);
  }
}

module.exports = new Logger();
```

### Factory Pattern
```javascript
// lib/factories/notificationFactory.js
class EmailNotification {
  send(message) { /* ... */ }
}

class SMSNotification {
  send(message) { /* ... */ }
}

function createNotification(type) {
  switch (type) {
    case "email":
      return new EmailNotification();
    case "sms":
      return new SMSNotification();
    default:
      throw new Error(`Unknown notification type: ${type}`);
  }
}

module.exports = { createNotification };
```

---

## Entity Configuration Pattern

### entity.js Pattern
```javascript
// lib/config/entity.js
module.exports = {
  users: {
    collectionName: "users",
    propsToUpdate: ["email", "name", "role", "password", "updatedAt"],
    timestamps: true
  },
  assets: {
    collectionName: "assets",
    propsToUpdate: ["name", "ipAddress", "type", "vendor", "model", "networkId", "updatedAt"],
    timestamps: true
  }
};
```

**⚠️ CRITICAL:** When adding new fields to an entity, **ALWAYS** update `propsToUpdate` array!

---

## Key Principles

1. **Double quotes everywhere** (except template literals)
2. **Always use semicolons**
3. **camelCase for variables/functions**, **PascalCase for classes/components**
4. **3-layer backend architecture:** Route → Controller → Service → DAL
5. **Error handling:** Always use `catchError()` or try/catch
6. **Validation:** Use Joi/Yup schemas in `lib/schemes/local/`
7. **Entity updates:** Always update `propsToUpdate` in entity.js
8. **Async/await:** Preferred over Promises
9. **JSDoc comments:** On all exported functions
10. **Testing:** Unit tests for services, integration tests for controllers

---

**This template should be used for all Node.js/JavaScript/TypeScript projects.**
