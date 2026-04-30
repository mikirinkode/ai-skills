# Test Data Column Format — API Test Cases

This document defines the structured format for the **Test Data** column (column H) in QA spreadsheets, enabling automated API test execution by the `qa-api-tester` skill.

## Format Rules

- One key-value pair per line
- Key and value separated by `: ` (colon + space)
- Keys are UPPERCASE
- JSON values must be valid JSON (use double quotes)
- Lines starting with `#` are comments (ignored by parser)

## Supported Keys

### Required
```
METHOD: GET|POST|PUT|PATCH|DELETE
ENDPOINT: /path/to/resource
EXPECT_STATUS: 200
```

### Optional — Request
```
HEADERS: {"Content-Type": "application/json", "X-Custom": "val"}
BODY: {"field": "value"}
QUERY: {"page": 1, "limit": 10}
```

### Optional — Response Assertions
```
EXPECT_BODY_CONTAINS: ["key1", "key2"]
EXPECT_BODY_MATCH: {"success": true, "count": 5}
EXPECT_BODY_NOT_CONTAINS: ["password", "token"]
EXPECT_ERROR_MESSAGE: "Invalid credentials"
EXPECT_ARRAY_MIN_LENGTH: 1
EXPECT_ARRAY_MAX_LENGTH: 50
```

### Optional — Chaining
```
SAVE_RESPONSE: tc_001_response
USE_FROM: tc_001_response.data.id → ENDPOINT /api/v1/items/{value}
```

## Full Examples

### GET with query params
```
METHOD: GET
ENDPOINT: /api/v1/members/sick
QUERY: {"start_date": "2025-01-01", "end_date": "2025-01-31"}
EXPECT_STATUS: 200
EXPECT_BODY_CONTAINS: ["data", "total"]
EXPECT_ARRAY_MIN_LENGTH: 1
```

### POST with body
```
METHOD: POST
ENDPOINT: /api/v1/members
BODY: {"name": "Test User", "email": "test@example.com", "role": "staff"}
EXPECT_STATUS: 201
EXPECT_BODY_MATCH: {"success": true}
EXPECT_BODY_CONTAINS: ["id", "name"]
SAVE_RESPONSE: tc_create_member
```

### Auth failure test (negative)
```
METHOD: POST
ENDPOINT: /api/v1/auth/login
BODY: {"email": "wrong@test.com", "password": "invalid"}
EXPECT_STATUS: 401
EXPECT_ERROR_MESSAGE: "Invalid"
```

### DELETE
```
METHOD: DELETE
ENDPOINT: /api/v1/members/{{tc_create_member.id}}
EXPECT_STATUS: 200
EXPECT_BODY_MATCH: {"success": true}
```

### Unauthorized access test
```
# This test runs WITHOUT auth header
METHOD: GET
ENDPOINT: /api/v1/admin/settings
HEADERS: {"Authorization": ""}
EXPECT_STATUS: 401
```

## Parsing Logic

```python
def parse_test_data(text):
    """Parse structured Test Data into a dict."""
    result = {}
    if not text:
        return result
    for line in text.strip().split('\n'):
        line = line.strip()
        if not line or line.startswith('#'):
            continue
        if ': ' not in line:
            continue
        key, value = line.split(': ', 1)
        key = key.strip().upper()
        # Try JSON parse for structured values
        try:
            import json
            result[key] = json.loads(value)
        except (json.JSONDecodeError, ValueError):
            result[key] = value
    return result
```

## Detection Rule

A test case is considered **API-auto-testable** if its Test Data column contains BOTH:
1. `METHOD:` line with a valid HTTP method
2. `ENDPOINT:` line with a path
3. `EXPECT_STATUS:` line with a numeric code

If any of these three are missing, the test is marked as **manual** and skipped during auto-execution.
