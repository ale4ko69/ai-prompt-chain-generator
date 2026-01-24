# Python Coding Patterns

**Purpose:** Language-specific coding patterns for Python projects.

---

## Code Style

### PEP 8 Compliance
Follow [PEP 8](https://pep8.org/) style guide unless project specifies otherwise.

### Indentation
**Standard:** 4 spaces (not tabs)

### Line Length
**Standard:** 79 characters (code), 72 characters (docstrings/comments)
**Common relaxation:** 88-120 characters (when using Black formatter)

### Quotes
**Standard:**
- Double quotes (`"`) for user-facing strings
- Single quotes (`'`) acceptable for internal strings
- Triple double-quotes (`"""`) for docstrings

```python
message = "Hello, world"  # User-facing
internal_key = 'some_key'  # Internal
```

### Imports
**Order:**
1. Standard library imports
2. Related third party imports
3. Local application/library imports

**Format:**
```python
# Standard library
import os
import sys
from typing import List, Dict, Optional

# Third party
import requests
from flask import Flask, request, jsonify
from sqlalchemy import Column, Integer, String

# Local
from app.models import User
from app.utils import validate_email
```

**Avoid:**
```python
# ❌ Bad: Wildcard imports
from flask import *

# ✅ Good: Explicit imports
from flask import Flask, request, jsonify
```

---

## Naming Conventions

### Files & Modules
```python
# snake_case for files
user_service.py
data_processor.py
__init__.py
```

### Variables & Functions
```python
# snake_case
user_name = "John"
def get_user_by_id(user_id):
    pass
```

### Classes
```python
# PascalCase
class UserService:
    pass

class DataProcessor:
    pass
```

### Constants
```python
# UPPER_SNAKE_CASE
MAX_RETRIES = 3
API_BASE_URL = "https://api.example.com"
DEFAULT_TIMEOUT = 30
```

### Private Methods/Attributes
```python
class MyClass:
    def __init__(self):
        self._private_attr = None  # Single underscore: internal use
        self.__very_private = None  # Double underscore: name mangling

    def _internal_method(self):
        """Internal use only"""
        pass
```

---

## Type Hints

### Function Annotations
```python
from typing import List, Dict, Optional, Union

def get_user(user_id: int) -> Optional[Dict[str, str]]:
    """Retrieve a user by ID."""
    pass

def process_items(items: List[str], limit: int = 10) -> List[str]:
    """Process items with optional limit."""
    pass

def calculate(a: int, b: int) -> Union[int, float]:
    """Calculate result, returns int or float."""
    pass
```

### Class Annotations
```python
from typing import Optional
from dataclasses import dataclass

@dataclass
class User:
    id: int
    email: str
    name: str
    role: str = "user"
    is_active: bool = True
```

---

## Flask Patterns

### Application Structure
```
app/
├── __init__.py         # Flask app factory
├── models.py           # Database models
├── routes/
│   ├── __init__.py
│   ├── users.py        # User routes
│   └── auth.py         # Auth routes
├── services/
│   ├── __init__.py
│   ├── user_service.py
│   └── auth_service.py
├── schemas/
│   ├── __init__.py
│   └── user_schema.py  # Validation schemas
└── utils/
    ├── __init__.py
    └── validators.py
```

### Flask App Factory
```python
# app/__init__.py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app(config_name="development"):
    app = Flask(__name__)
    app.config.from_object(f"config.{config_name}")

    db.init_app(app)

    # Register blueprints
    from app.routes import users, auth
    app.register_blueprint(users.bp)
    app.register_blueprint(auth.bp)

    return app
```

### Blueprint (Route Definition)
```python
# app/routes/users.py
from flask import Blueprint, request, jsonify
from app.services.user_service import UserService
from app.schemas.user_schema import CreateUserSchema
from marshmallow import ValidationError

bp = Blueprint("users", __name__, url_prefix="/api/users")
user_service = UserService()

@bp.route("/", methods=["GET"])
def get_users():
    """Get all users."""
    try:
        users = user_service.get_all()
        return jsonify(users), 200
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@bp.route("/", methods=["POST"])
def create_user():
    """Create a new user."""
    try:
        schema = CreateUserSchema()
        user_data = schema.load(request.json)
        user = user_service.create(user_data)
        return jsonify(user), 201
    except ValidationError as e:
        return jsonify({"error": e.messages}), 400
    except Exception as e:
        return jsonify({"error": str(e)}), 500
```

### Service Layer
```python
# app/services/user_service.py
from typing import List, Dict, Optional
from app.models import User
from app import db

class UserService:
    """Service for user-related operations."""

    def get_all(self) -> List[Dict]:
        """Get all users."""
        users = User.query.all()
        return [user.to_dict() for user in users]

    def get_by_id(self, user_id: int) -> Optional[Dict]:
        """Get user by ID."""
        user = User.query.get(user_id)
        return user.to_dict() if user else None

    def create(self, user_data: Dict) -> Dict:
        """Create a new user."""
        user = User(**user_data)
        db.session.add(user)
        db.session.commit()
        return user.to_dict()

    def update(self, user_id: int, user_data: Dict) -> Optional[Dict]:
        """Update a user."""
        user = User.query.get(user_id)
        if not user:
            return None

        for key, value in user_data.items():
            setattr(user, key, value)

        db.session.commit()
        return user.to_dict()
```

---

## Django Patterns

### Project Structure
```
myproject/
├── manage.py
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── apps/
    ├── users/
    │   ├── __init__.py
    │   ├── models.py
    │   ├── views.py
    │   ├── serializers.py
    │   ├── urls.py
    │   └── tests.py
    └── products/
        └── ...
```

### Models
```python
# apps/users/models.py
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    """Custom user model."""
    email = models.EmailField(unique=True)
    role = models.CharField(
        max_length=20,
        choices=[("admin", "Admin"), ("user", "User")],
        default="user"
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = "users"
        ordering = ["-created_at"]

    def __str__(self):
        return self.email
```

### Views (Class-Based)
```python
# apps/users/views.py
from rest_framework import generics, status
from rest_framework.response import Response
from .models import User
from .serializers import UserSerializer

class UserListCreateView(generics.ListCreateAPIView):
    """List and create users."""
    queryset = User.objects.all()
    serializer_class = UserSerializer

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        self.perform_create(serializer)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

---

## SQLAlchemy Patterns

### Model Definition
```python
# app/models.py
from sqlalchemy import Column, Integer, String, Boolean, DateTime
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime

Base = declarative_base()

class User(Base):
    """User model."""
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    email = Column(String(255), unique=True, nullable=False)
    password = Column(String(255), nullable=False)
    name = Column(String(100), nullable=False)
    role = Column(String(20), default="user")
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)

    def to_dict(self):
        """Convert model to dictionary."""
        return {
            "id": self.id,
            "email": self.email,
            "name": self.name,
            "role": self.role,
            "is_active": self.is_active,
            "created_at": self.created_at.isoformat(),
            "updated_at": self.updated_at.isoformat()
        }
```

### Database Operations
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine("postgresql://user:pass@localhost/dbname")
SessionLocal = sessionmaker(bind=engine)

def get_user_by_id(user_id: int) -> Optional[User]:
    """Get user by ID."""
    session = SessionLocal()
    try:
        user = session.query(User).filter(User.id == user_id).first()
        return user
    finally:
        session.close()

def create_user(user_data: Dict) -> User:
    """Create a new user."""
    session = SessionLocal()
    try:
        user = User(**user_data)
        session.add(user)
        session.commit()
        session.refresh(user)
        return user
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()
```

---

## Error Handling

### Try/Except Blocks
```python
def process_data(data: str) -> Dict:
    """Process data with error handling."""
    try:
        result = json.loads(data)
        return result
    except json.JSONDecodeError as e:
        raise ValueError(f"Invalid JSON: {e}")
    except Exception as e:
        raise RuntimeError(f"Processing failed: {e}")
```

### Custom Exceptions
```python
# app/exceptions.py
class ValidationError(Exception):
    """Raised when validation fails."""
    def __init__(self, message: str, field: str = None):
        self.message = message
        self.field = field
        super().__init__(self.message)

class NotFoundError(Exception):
    """Raised when resource is not found."""
    pass

class AuthenticationError(Exception):
    """Raised when authentication fails."""
    pass
```

### Context Managers
```python
from contextlib import contextmanager

@contextmanager
def database_session():
    """Database session context manager."""
    session = SessionLocal()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()

# Usage
with database_session() as session:
    user = User(email="test@example.com")
    session.add(user)
```

---

## Validation (Marshmallow)

### Schema Definition
```python
# app/schemas/user_schema.py
from marshmallow import Schema, fields, validate, validates, ValidationError

class CreateUserSchema(Schema):
    """Schema for creating a user."""
    email = fields.Email(required=True)
    password = fields.String(
        required=True,
        validate=validate.Length(min=8, max=255)
    )
    name = fields.String(
        required=True,
        validate=validate.Length(min=2, max=100)
    )
    role = fields.String(
        validate=validate.OneOf(["admin", "user"]),
        load_default="user"
    )

    @validates("email")
    def validate_email_unique(self, value):
        """Ensure email is unique."""
        if User.query.filter_by(email=value).first():
            raise ValidationError("Email already exists")
```

### Usage
```python
from app.schemas.user_schema import CreateUserSchema
from marshmallow import ValidationError

schema = CreateUserSchema()
try:
    user_data = schema.load(request.json)
    # Proceed with validated data
except ValidationError as e:
    return jsonify({"errors": e.messages}), 400
```

---

## Testing (pytest)

### Test Structure
```python
# tests/test_user_service.py
import pytest
from app.services.user_service import UserService
from app.models import User

@pytest.fixture
def user_service():
    """Create user service instance."""
    return UserService()

@pytest.fixture
def sample_user_data():
    """Sample user data."""
    return {
        "email": "test@example.com",
        "password": "password123",
        "name": "Test User"
    }

def test_create_user(user_service, sample_user_data):
    """Test creating a user."""
    user = user_service.create(sample_user_data)

    assert user["email"] == sample_user_data["email"]
    assert user["name"] == sample_user_data["name"]
    assert "password" not in user  # Should not return password

def test_get_user_by_id(user_service):
    """Test getting user by ID."""
    user = user_service.get_by_id(1)

    assert user is not None
    assert user["id"] == 1
```

### Mocking
```python
from unittest.mock import Mock, patch

def test_external_api_call():
    """Test function that calls external API."""
    with patch("requests.get") as mock_get:
        mock_get.return_value.json.return_value = {"data": "test"}
        mock_get.return_value.status_code = 200

        result = fetch_external_data()

        assert result == {"data": "test"}
        mock_get.assert_called_once()
```

---

## Async Patterns (asyncio)

### Async Functions
```python
import asyncio
import aiohttp

async def fetch_url(url: str) -> str:
    """Fetch URL asynchronously."""
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def fetch_multiple(urls: List[str]) -> List[str]:
    """Fetch multiple URLs concurrently."""
    tasks = [fetch_url(url) for url in urls]
    results = await asyncio.gather(*tasks)
    return results

# Usage
asyncio.run(fetch_multiple(["http://example.com", "http://example.org"]))
```

### FastAPI Async Routes
```python
from fastapi import FastAPI, HTTPException
from typing import List

app = FastAPI()

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    """Get user by ID (async)."""
    user = await user_service.get_by_id_async(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

---

## Documentation (Docstrings)

### Google Style (Recommended)
```python
def get_user_by_id(user_id: int) -> Optional[Dict]:
    """Retrieve a user by their ID.

    Args:
        user_id: The unique identifier for the user.

    Returns:
        A dictionary containing user data, or None if not found.

    Raises:
        ValueError: If user_id is not a positive integer.

    Example:
        >>> user = get_user_by_id(123)
        >>> print(user["name"])
        John Doe
    """
    if user_id <= 0:
        raise ValueError("user_id must be positive")

    # Implementation
    pass
```

### NumPy Style
```python
def calculate_average(numbers: List[float]) -> float:
    """Calculate the average of a list of numbers.

    Parameters
    ----------
    numbers : List[float]
        A list of numerical values.

    Returns
    -------
    float
        The arithmetic mean of the input numbers.

    Examples
    --------
    >>> calculate_average([1, 2, 3, 4, 5])
    3.0
    """
    return sum(numbers) / len(numbers)
```

---

## Configuration Management

### Environment Variables
```python
# config.py
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
    """Base configuration."""
    SECRET_KEY = os.getenv("SECRET_KEY", "dev-secret-key")
    SQLALCHEMY_DATABASE_URI = os.getenv("DATABASE_URL")
    SQLALCHEMY_TRACK_MODIFICATIONS = False

class DevelopmentConfig(Config):
    """Development configuration."""
    DEBUG = True

class ProductionConfig(Config):
    """Production configuration."""
    DEBUG = False
```

### Using Pydantic for Settings
```python
from pydantic import BaseSettings, Field

class Settings(BaseSettings):
    """Application settings."""
    database_url: str = Field(..., env="DATABASE_URL")
    secret_key: str = Field(..., env="SECRET_KEY")
    debug: bool = Field(False, env="DEBUG")

    class Config:
        env_file = ".env"

settings = Settings()
```

---

## Common Patterns

### Singleton
```python
class DatabaseConnection:
    """Singleton database connection."""
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            # Initialize connection
        return cls._instance
```

### Context Manager
```python
class FileManager:
    """File manager with automatic cleanup."""
    def __init__(self, filename: str, mode: str = "r"):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

# Usage
with FileManager("data.txt", "w") as f:
    f.write("Hello, world!")
```

---

## Key Principles

1. **Follow PEP 8** style guide
2. **Use type hints** for function parameters and returns
3. **snake_case for functions/variables**, **PascalCase for classes**
4. **Docstrings** on all public functions and classes
5. **Error handling** with specific exception types
6. **Validation** using Marshmallow or Pydantic
7. **Testing** with pytest
8. **Virtual environments** (venv, virtualenv, poetry)
9. **Import ordering** (stdlib, third-party, local)
10. **4-space indentation** (no tabs)

---

**This template should be used for all Python projects (Flask, Django, FastAPI, CLI tools, etc.).**
