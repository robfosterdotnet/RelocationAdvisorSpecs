# Learning Resources - Sprint 2

**Focus Areas:** APIs, ETL Patterns, Data Cleaning

---

## APIs and HTTP

### Must Read
- [HTTP Basics](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) - 15 min
- [REST API Tutorial](https://restfulapi.net/) - Core concepts

### Key Concepts
- HTTP methods (GET, POST, PUT, DELETE)
- Status codes (200 OK, 400 Bad Request, 500 Server Error)
- Query parameters vs. request body
- Headers and authentication

### Python Requests Library
```python
import requests

# Simple GET request
response = requests.get("https://api.example.com/data")
print(response.status_code)  # 200
print(response.json())       # Parse JSON response

# With parameters
response = requests.get(
    "https://api.example.com/data",
    params={"key": "value", "limit": 10}
)

# With headers
response = requests.get(
    "https://api.example.com/data",
    headers={"Authorization": "Bearer token123"}
)
```

---

## Census API Specific

### Resources
- [Census API Documentation](https://www.census.gov/data/developers.html)
- [ACS Variables List](https://api.census.gov/data/2022/acs/acs5/variables.html)
- [FIPS Codes](https://www.census.gov/library/reference/code-lists/ansi.html)

### Understanding Variable Codes
```
B19013_001E
│ │    │  │
│ │    │  └── E = Estimate (vs M = Margin of Error)
│ │    └───── 001 = First (often total/median) variable
│ └────────── 19013 = Table number
└──────────── B = Detailed table (vs S = Subject, DP = Profile)
```

---

## ETL Patterns

### The ETL Pipeline
```
EXTRACT          TRANSFORM           LOAD
┌─────────┐      ┌─────────────┐     ┌──────────┐
│ API     │ ──── │ Clean       │ ─── │ Database │
│ Files   │      │ Validate    │     │          │
│ Streams │      │ Convert     │     │          │
└─────────┘      └─────────────┘     └──────────┘
```

### Best Practices
1. **Separation of Concerns** - Separate E, T, L into different functions
2. **Idempotency** - Running twice should be safe
3. **Logging** - Log everything for debugging
4. **Error Handling** - Don't fail on one bad record
5. **Batching** - Process in chunks for memory efficiency

### UPSERT Pattern (Insert or Update)
```python
# Check if exists, then update or insert
existing = db.query(Model).filter_by(key=value).first()
if existing:
    existing.field = new_value
else:
    db.add(Model(key=value, field=new_value))
db.commit()
```

---

## Data Cleaning

### Common Issues
| Issue | Example | Solution |
|-------|---------|----------|
| Wrong type | "12345" (string) | `int("12345")` |
| Missing data | null, "", None | Check and set default |
| Special codes | -666666666 | Map to None |
| Out of range | income = -500 | Validate and reject |
| Extra whitespace | " Los Angeles " | `.strip()` |

### Python Data Cleaning
```python
def clean_value(value):
    """Clean a string value."""
    if value is None or value == "":
        return None

    value = str(value).strip()

    if value in ["-666666666", "-222222222"]:
        return None

    try:
        return int(value)
    except ValueError:
        return None
```

---

## Python Logging

### Basic Setup
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

logger.debug("Detailed info for debugging")
logger.info("General information")
logger.warning("Something unexpected")
logger.error("Something failed")
```

### Logging to File
```python
logging.basicConfig(
    handlers=[
        logging.StreamHandler(),           # Console
        logging.FileHandler('etl.log')     # File
    ]
)
```

---

## Error Handling

### Try/Except Pattern
```python
def process_record(record):
    try:
        # Processing logic
        return transformed_record
    except ValueError as e:
        logger.warning(f"Invalid value in record: {e}")
        return None
    except Exception as e:
        logger.error(f"Unexpected error: {e}")
        raise  # Re-raise unexpected errors
```

### Continue on Error
```python
for record in records:
    try:
        process(record)
        success_count += 1
    except Exception as e:
        logger.error(f"Failed: {e}")
        error_count += 1
        continue  # Don't stop the whole job
```

---

## Study Time Recommendations

| Topic | Time | Priority |
|-------|------|----------|
| HTTP and REST basics | 1 hour | High |
| Python requests library | 30 min | High |
| Census API exploration | 1 hour | High |
| ETL concepts | 30 min | Medium |
| Python logging | 30 min | Medium |
| Data cleaning patterns | 30 min | Medium |

---

## Knowledge Check

Before moving on:
1. What does ETL stand for?
2. How do you make an HTTP GET request in Python?
3. What is a Census FIPS code?
4. Why is idempotency important in ETL?
5. How do you handle records that fail processing?

---

*Next: Read `07-definition-of-done.md`*
