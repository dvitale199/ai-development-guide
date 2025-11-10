# Python/FastAPI Stack Guide

**AI-Assisted Development Guide for Python with FastAPI**

This guide provides Python and FastAPI-specific implementations of the language-agnostic principles in the [AI Development Guide](../../../AI_DEVELOPMENT_GUIDE.md).

---

## Quick Reference

For a concise checklist of requirements when working with AI coding assistants on Python/FastAPI projects, see:

**[CLAUDE.md](CLAUDE.md)** - Quick reference for AI tools

---

## Overview

This stack guide covers:
- Python 3.10+ best practices with AI assistance
- FastAPI API development patterns
- Streamlit app development
- Testing with pytest
- Docker deployment
- CI/CD patterns for Python projects

---

## Project Structure

### Recommended Directory Layout

```
project/
├── app/                    # FastAPI application
│   ├── __init__.py
│   ├── main.py            # FastAPI app initialization
│   ├── routes/            # API endpoints
│   ├── dependencies.py    # Dependency injection
│   └── middleware.py      # Custom middleware
├── services/              # Business logic
│   ├── __init__.py
│   └── processors.py      # Data processing functions
├── schemas/               # Pydantic models
│   ├── __init__.py
│   └── models.py          # Request/response schemas
├── streamlit_app/         # Streamlit UI (if applicable)
│   ├── __init__.py
│   └── app.py
├── tests/                 # Test suite
│   ├── __init__.py
│   ├── test_routes.py
│   ├── test_services.py
│   └── conftest.py        # pytest fixtures
├── docs/
│   └── API_REFERENCE.md   # API documentation
├── .env.example           # Environment variable template
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml         # Project dependencies (Poetry)
├── requirements.txt       # or requirements.txt (pip)
└── README.md
```

### Module Responsibilities

**`/app`** - FastAPI routes and API layer
- Request/response handling
- Input validation (via Pydantic)
- HTTP concerns (status codes, headers)
- Route definitions

**`/services`** - Business logic
- Data processing algorithms
- External API calls
- Complex calculations
- Domain-specific logic

**`/schemas`** - Data validation
- Pydantic models for request/response
- Type definitions
- Validation rules

**`/streamlit_app`** - UI layer (if applicable)
- User interface components
- Presentation logic
- Visualization

**`/tests`** - Test suite
- Unit tests for services
- Integration tests for API endpoints
- Fixtures and test data

---

## Python Conventions

### Code Quality Tools

**Required tools:**
```bash
# Linting
flake8

# Formatting
black

# Type checking
mypy

# Import sorting
isort
```

**Configuration:**

`.flake8`:
```ini
[flake8]
max-line-length = 88
extend-ignore = E203, W503
exclude = .git,__pycache__,venv,build,dist
```

`pyproject.toml` (for Black and isort):
```toml
[tool.black]
line-length = 88
target-version = ['py310']

[tool.isort]
profile = "black"
line_length = 88
```

### Type Hints

**Always use type hints for public functions:**

```python
from typing import List, Optional, Dict, Any

def process_items(
    items: List[str],
    config: Optional[Dict[str, Any]] = None
) -> List[Dict[str, str]]:
    """
    Process items according to configuration.

    Args:
        items: List of item identifiers to process
        config: Optional configuration dictionary

    Returns:
        List of processed items with metadata

    Raises:
        ValueError: If items list is empty
    """
    if not items:
        raise ValueError("Items list cannot be empty")

    # [ai-assisted] Implementation
    return [{"item": item, "status": "processed"} for item in items]
```

### Docstring Standard

Use Google-style docstrings:

```python
def calculate_score(data: List[float], weights: List[float]) -> float:
    """
    Calculate weighted score from input data.

    This function computes a weighted sum of the input data using the
    provided weights. All inputs must be non-negative.

    Args:
        data: List of numeric values to score
        weights: List of weights (must match length of data)

    Returns:
        Weighted score as a float between 0.0 and 1.0

    Raises:
        ValueError: If data and weights have different lengths
        ValueError: If any value is negative

    Example:
        >>> calculate_score([1.0, 2.0, 3.0], [0.5, 0.3, 0.2])
        1.9

    Notes:
        - [ai-assisted] Algorithm based on standard weighted average
        - Normalizes output to [0, 1] range
    """
    pass
```

---

## FastAPI Patterns

### API Endpoint Structure

```python
from fastapi import APIRouter, HTTPException, Depends, status
from typing import List
from schemas.models import ItemResponse, ItemCreate
from services.processor import process_item
from app.dependencies import get_current_user

router = APIRouter(prefix="/api/v1/items", tags=["items"])

@router.post(
    "/",
    response_model=ItemResponse,
    status_code=status.HTTP_201_CREATED,
    summary="Create new item",
    description="Create a new item with validation"
)
async def create_item(
    item: ItemCreate,
    current_user: str = Depends(get_current_user)
) -> ItemResponse:
    """
    Create a new item.

    Args:
        item: Item data from request body
        current_user: Current authenticated user (injected)

    Returns:
        Created item with generated ID

    Raises:
        HTTPException: 400 if validation fails
        HTTPException: 401 if not authenticated
    """
    try:
        result = await process_item(item)
        return ItemResponse(**result)
    except ValueError as e:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail=str(e)
        )
```

### Pydantic Schema Patterns

```python
from pydantic import BaseModel, Field, validator
from typing import Optional
from datetime import datetime

class ItemBase(BaseModel):
    """Base schema for item data."""
    name: str = Field(..., min_length=1, max_length=100)
    description: Optional[str] = Field(None, max_length=500)
    value: float = Field(..., ge=0.0)

class ItemCreate(ItemBase):
    """Schema for creating new items."""
    pass

class ItemResponse(ItemBase):
    """Schema for item API responses."""
    id: str
    created_at: datetime
    updated_at: datetime

    class Config:
        orm_mode = True

class ItemUpdate(BaseModel):
    """Schema for updating items (all fields optional)."""
    name: Optional[str] = Field(None, min_length=1, max_length=100)
    description: Optional[str] = Field(None, max_length=500)
    value: Optional[float] = Field(None, ge=0.0)

    @validator('value')
    def value_must_be_positive(cls, v):
        if v is not None and v < 0:
            raise ValueError('value must be non-negative')
        return v
```

### Dependency Injection

```python
# app/dependencies.py
from fastapi import Header, HTTPException, status
from typing import Optional

async def get_current_user(
    authorization: Optional[str] = Header(None)
) -> str:
    """
    Extract and validate current user from auth header.

    Args:
        authorization: Authorization header value

    Returns:
        User identifier

    Raises:
        HTTPException: 401 if authentication fails
    """
    if not authorization:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Missing authorization header"
        )

    # [ai-assisted] Validate token and extract user
    # Simplified example - use proper auth in production
    try:
        token = authorization.split(" ")[1]
        user_id = validate_token(token)
        return user_id
    except Exception:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials"
        )
```

---

## Testing with pytest

### Test Structure

```python
# tests/test_services.py
import pytest
from services.processor import process_item
from schemas.models import ItemCreate

class TestItemProcessing:
    """Test suite for item processing."""

    def test_process_valid_item(self):
        """Test processing with valid input."""
        # Arrange
        item = ItemCreate(name="Test", description="Test item", value=10.0)

        # Act
        result = process_item(item)

        # Assert
        assert result["name"] == "Test"
        assert result["value"] == 10.0
        assert "id" in result

    def test_process_item_empty_name(self):
        """Test that empty name raises ValueError."""
        # Arrange
        item = ItemCreate(name="", description="Test", value=10.0)

        # Act & Assert
        with pytest.raises(ValueError, match="name cannot be empty"):
            process_item(item)

    @pytest.mark.parametrize("value,expected", [
        (0.0, 0.0),
        (10.0, 10.0),
        (100.0, 100.0),
    ])
    def test_process_item_various_values(self, value, expected):
        """Test processing with various value inputs."""
        item = ItemCreate(name="Test", value=value)
        result = process_item(item)
        assert result["value"] == expected
```

### API Testing

```python
# tests/test_routes.py
import pytest
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

class TestItemRoutes:
    """Test suite for item API endpoints."""

    def test_create_item_success(self):
        """Test successful item creation."""
        # Arrange
        payload = {
            "name": "Test Item",
            "description": "A test item",
            "value": 42.0
        }

        # Act
        response = client.post("/api/v1/items/", json=payload)

        # Assert
        assert response.status_code == 201
        data = response.json()
        assert data["name"] == "Test Item"
        assert data["value"] == 42.0
        assert "id" in data

    def test_create_item_invalid_data(self):
        """Test that invalid data returns 400."""
        # Arrange
        payload = {
            "name": "",  # Invalid: empty name
            "value": -1.0  # Invalid: negative value
        }

        # Act
        response = client.post("/api/v1/items/", json=payload)

        # Assert
        assert response.status_code == 400

    def test_get_item_not_found(self):
        """Test that missing item returns 404."""
        response = client.get("/api/v1/items/nonexistent-id")
        assert response.status_code == 404
```

### Fixtures

```python
# tests/conftest.py
import pytest
from typing import Generator
from fastapi.testclient import TestClient
from app.main import app

@pytest.fixture
def client() -> Generator[TestClient, None, None]:
    """Provide test client for API testing."""
    with TestClient(app) as test_client:
        yield test_client

@pytest.fixture
def sample_item() -> dict:
    """Provide sample item data for tests."""
    return {
        "name": "Sample Item",
        "description": "A sample item for testing",
        "value": 10.0
    }

@pytest.fixture
def mock_database(monkeypatch):
    """Mock database for isolated testing."""
    # [ai-assisted] Database mocking implementation
    pass
```

---

## Docker Setup

### Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Run as non-root user
RUN useradd -m appuser && chown -R appuser:appuser /app
USER appuser

# Expose port
EXPOSE 8000

# Run application
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### docker-compose.yml

```yaml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - SECRET_KEY=${SECRET_KEY}
    env_file:
      - .env
    depends_on:
      - db
    volumes:
      - ./app:/app/app
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=${DB_NAME}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  streamlit:
    build:
      context: .
      dockerfile: Dockerfile.streamlit
    ports:
      - "8501:8501"
    environment:
      - API_URL=http://api:8000
    depends_on:
      - api

volumes:
  postgres_data:
```

---

## Environment Configuration

### .env.example

```bash
# Application
APP_NAME=My FastAPI App
DEBUG=false
SECRET_KEY=your-secret-key-here

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
DB_USER=user
DB_PASSWORD=password
DB_NAME=dbname

# API
API_V1_PREFIX=/api/v1
CORS_ORIGINS=http://localhost:3000,http://localhost:8501

# External Services
EXTERNAL_API_KEY=your-api-key
EXTERNAL_API_URL=https://api.example.com

# Note: Never commit actual secrets to version control
# Copy this to .env and fill in actual values
```

---

## AI-Specific Guidance for Python/FastAPI

### When AI Generates Code

1. **Always run type checking:**
   ```bash
   mypy app/ services/ schemas/
   ```

2. **Run linting and formatting:**
   ```bash
   black .
   isort .
   flake8
   ```

3. **Run tests:**
   ```bash
   pytest tests/ -v --cov=app --cov=services
   ```

4. **Verify Pydantic models:**
   - Check that validation rules are appropriate
   - Test with invalid data to ensure errors are raised
   - Verify response_model matches actual return type

### Common Python/AI Pitfalls

**1. Incorrect async/await usage:**
```python
# ❌ BAD - AI might forget await
async def get_data():
    result = fetch_from_db()  # Missing await!
    return result

# ✅ GOOD
async def get_data():
    result = await fetch_from_db()
    return result
```

**2. Pydantic validation bypassing:**
```python
# ❌ BAD - AI might use dict directly
def process(data: dict):
    return data["field"]

# ✅ GOOD - Use Pydantic for validation
def process(data: ItemSchema):
    return data.field
```

**3. Missing exception handling:**
```python
# ❌ BAD - AI might not handle exceptions
@router.get("/items/{item_id}")
async def get_item(item_id: str):
    return get_item_from_db(item_id)  # What if not found?

# ✅ GOOD - Proper error handling
@router.get("/items/{item_id}")
async def get_item(item_id: str):
    try:
        item = get_item_from_db(item_id)
        if not item:
            raise HTTPException(status_code=404, detail="Item not found")
        return item
    except Exception as e:
        logger.error(f"Error fetching item {item_id}: {e}")
        raise HTTPException(status_code=500, detail="Internal server error")
```

---

## CI/CD Example

### GitHub Actions Workflow

```yaml
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install -r requirements-dev.txt

    - name: Run linting
      run: |
        flake8 app/ services/ schemas/
        black --check .
        isort --check-only .

    - name: Run type checking
      run: mypy app/ services/ schemas/

    - name: Run tests
      run: pytest tests/ -v --cov=app --cov=services --cov-report=xml

    - name: Upload coverage
      uses: codecov/codecov-action@v3
```

---

## Resources

### Python/FastAPI Learning
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [pytest Documentation](https://docs.pytest.org/)

### Python Best Practices
- [PEP 8 - Style Guide](https://peps.python.org/pep-0008/)
- [Type Hints (PEP 484)](https://peps.python.org/pep-0484/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)

### Back to Main Guide
- [AI Development Guide](../../../AI_DEVELOPMENT_GUIDE.md) - Language-agnostic principles
- [README](../../../README.md) - Main repository overview

---

**Last Updated:** 2025-11-10
