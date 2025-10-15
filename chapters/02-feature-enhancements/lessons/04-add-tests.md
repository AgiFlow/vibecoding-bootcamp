# Lesson 4: Add Tests (Unit + Integration) for Existing Feature

**Time:** 30 minutes
**Difficulty:** Intermediate

## Learning Objective
Learn to write both unit tests and integration tests for existing code, understanding when to use each type and how they complement each other.

## The Enhancement
You have a working login feature but no tests. Add unit tests for validation logic and integration tests for the full login flow.

---

## Current State (No Tests)

### Validation Utility
```pseudo
// src/lib/validation
function validateLoginCredentials(email, password):
  errors = {}

  // Email validation
  if NOT email OR trim(email).length == 0:
    errors.email = 'Email is required'
  else if NOT contains(email, '@'):
    errors.email = 'Invalid email format'
  end

  // Password validation
  if NOT password OR password.length == 0:
    errors.password = 'Password is required'
  else if password.length < 8:
    errors.password = 'Password must be at least 8 characters'
  end

  return {
    isValid: size(errors) == 0,
    errors: errors
  }
end
```

### Login API Route
```pseudo
// API Route: POST /api/auth/login
function POST(request):
  { email, password } = parseJSON(request.body)

  // Validate input
  validation = validateLoginCredentials(email, password)
  if NOT validation.isValid:
    return jsonResponse({
      success: false,
      errors: validation.errors
    }, status: 400)
  end

  // Check credentials (mock - replace with real database)
  if email == 'admin@example.com' AND password == 'password123':
    token = encodeBase64(email + ':' + currentTimestamp())

    setCookie('session', token, {
      httpOnly: true,
      secure: isProduction(),
      maxAge: 60 * 60 * 24 * 7,  // 1 week
      path: '/'
    })

    return jsonResponse({
      success: true,
      user: { email: email }
    }, status: 200)
  end

  // Invalid credentials
  return jsonResponse({
    success: false,
    errors: { credentials: 'Invalid email or password' }
  }, status: 401)
end
```

**Problem:** No tests! How do you know validation works? How do you know the login flow works end-to-end?

---

## Part 1: Set Up Testing Infrastructure (5 mins)

### Sample Prompt
```
I want to add tests to my project.

I need:
1. Unit tests for validation functions
2. Integration tests for API routes

Help me:
1. Install required testing packages for my language/framework
2. Create config files if needed
3. Set up test scripts in my build tool
4. Show me folder structure for organizing tests
```

### Install Dependencies (Examples)

```bash
# JavaScript/TypeScript examples:
# - npm install -D jest
# - npm install -D vitest @vitest/ui
# - npm install -D mocha chai

# Python examples:
# - pip install pytest pytest-cov
# - pip install unittest (built-in)

# Go examples:
# - testing package (built-in)
# - go get github.com/stretchr/testify

# Choose based on your language and preferences
```

### Test Configuration

```pseudo
// Test config file (test.config or similar)
{
  // Test environment (node, browser, etc.)
  environment: 'node',

  // Enable global test functions (describe, it, expect)
  globals: true,

  // Setup files to run before tests
  setupFiles: ['./test-setup'],

  // Path aliases for imports
  moduleNameMapper: {
    '@/': './src/'
  },

  // Coverage thresholds
  coverage: {
    branches: 80,
    functions: 80,
    lines: 80
  }
}
```

### Test Scripts

```pseudo
// In package.json, Makefile, or build scripts
{
  "scripts": {
    "test": "[run tests in watch mode]",
    "test:run": "[run all tests once]",
    "test:coverage": "[run with coverage report]",
    "test:ui": "[run with visual UI - if supported]"
  }
}

// Examples:
// - "test": "jest"
// - "test": "pytest"
// - "test": "go test ./..."
```

### Folder Structure

```
project/
├── src/
│   ├── lib/
│   │   ├── validation.js
│   │   └── __tests__/          # Unit tests next to code
│   │       └── validation.test.js
│   └── api/
│       └── auth/
│           ├── login.js
│           └── __tests__/      # Integration tests next to code
│               └── login.test.js
```

---

## Part 2: Write Unit Tests (10 mins)

**Unit tests** test individual functions in isolation without external dependencies.

### Sample Prompt
```
Write unit tests for this validation function:
[paste validateLoginCredentials]

Cover all scenarios:
1. Valid email and password
2. Missing email
3. Invalid email format
4. Missing password
5. Short password
6. Multiple validation errors at once

Use my testing framework's patterns (describe/it/expect or equivalent).
```

### Unit Test File

```pseudo
// src/lib/__tests__/validation.test
import { describe, it, expect } from 'testing-framework'
import { validateLoginCredentials } from '../validation'

describe('validateLoginCredentials', () => {

  describe('valid credentials', () => {
    it('should pass with valid email and password', () => {
      result = validateLoginCredentials('user@example.com', 'password123')

      expect(result.isValid).toBe(true)
      expect(result.errors).toEqual({})
    })
  })

  describe('email validation', () => {
    it('should fail when email is empty', () => {
      result = validateLoginCredentials('', 'password123')

      expect(result.isValid).toBe(false)
      expect(result.errors.email).toBe('Email is required')
    })

    it('should fail when email is only whitespace', () => {
      result = validateLoginCredentials('   ', 'password123')

      expect(result.isValid).toBe(false)
      expect(result.errors.email).toBe('Email is required')
    })

    it('should fail when email has no @ symbol', () => {
      result = validateLoginCredentials('invalid.com', 'password123')

      expect(result.isValid).toBe(false)
      expect(result.errors.email).toBe('Invalid email format')
    })
  })

  describe('password validation', () => {
    it('should fail when password is empty', () => {
      result = validateLoginCredentials('user@example.com', '')

      expect(result.isValid).toBe(false)
      expect(result.errors.password).toBe('Password is required')
    })

    it('should fail when password is too short', () => {
      result = validateLoginCredentials('user@example.com', 'short')

      expect(result.isValid).toBe(false)
      expect(result.errors.password).toBe('Password must be at least 8 characters')
    })
  })

  describe('multiple validation errors', () => {
    it('should return all errors when both fields are invalid', () => {
      result = validateLoginCredentials('', 'abc')

      expect(result.isValid).toBe(false)
      expect(result.errors.email).toBe('Email is required')
      expect(result.errors.password).toBe('Password must be at least 8 characters')
    })

    it('should return all errors for invalid formats', () => {
      result = validateLoginCredentials('invalid', 'short')

      expect(result.isValid).toBe(false)
      expect(result.errors.email).toBe('Invalid email format')
      expect(result.errors.password).toBe('Password must be at least 8 characters')
    })
  })
})
```

**Test structure:**
- `describe()` - Group related tests together
- `it()` - Define individual test case
- `expect()` - Assert expected behavior

---

## Part 3: Write Integration Tests (15 mins)

**Integration tests** test how multiple components work together (API routes, database, external services, etc).

### Sample Prompt
```
Write integration tests for this login API route:
[paste login route code]

Test the full HTTP request/response flow:
1. Successful login with valid credentials
2. Failed login with invalid credentials
3. Validation errors for missing/invalid fields
4. Session cookie is set on success
5. No cookie set on failure

Mock/stub external dependencies (cookies, database, etc).
```

### Integration Test File

```pseudo
// app/api/auth/login/__tests__/route.test
import { describe, it, expect, mock, beforeEach } from 'testing-framework'
import { POST } from '../route'
import { cookies } from 'framework-headers'

// Mock the cookies API
mock('framework-headers', () => ({
  cookies: mockFunction()
}))

describe('POST /api/auth/login', () => {
  mockCookies = null

  beforeEach(() => {
    // Reset mocks before each test
    mockCookies = {
      set: mockFunction(),
      get: mockFunction(),
      delete: mockFunction()
    }
    setMockReturnValue(cookies, mockCookies)
  })

  describe('successful login', () => {
    it('should return success with valid credentials', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: 'admin@example.com',
          password: 'password123'
        })
      })

      response = await POST(request)
      data = parseJSON(response.body)

      expect(response.status).toBe(200)
      expect(data.success).toBe(true)
      expect(data.user.email).toBe('admin@example.com')
    })

    it('should set session cookie on successful login', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: 'admin@example.com',
          password: 'password123'
        })
      })

      await POST(request)

      expect(mockCookies.set).toHaveBeenCalledWith(
        'session',
        anyString,
        objectContaining({
          httpOnly: true,
          path: '/',
          maxAge: 604800  // 7 days in seconds
        })
      )
    })
  })

  describe('failed login', () => {
    it('should return 401 with invalid credentials', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: 'admin@example.com',
          password: 'wrongpassword'
        })
      })

      response = await POST(request)
      data = parseJSON(response.body)

      expect(response.status).toBe(401)
      expect(data.success).toBe(false)
      expect(data.errors.credentials).toBe('Invalid email or password')
    })

    it('should not set cookie on failed login', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: 'admin@example.com',
          password: 'wrongpassword'
        })
      })

      await POST(request)

      expect(mockCookies.set).not.toHaveBeenCalled()
    })
  })

  describe('validation errors', () => {
    it('should return 400 with missing email', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: '',
          password: 'password123'
        })
      })

      response = await POST(request)
      data = parseJSON(response.body)

      expect(response.status).toBe(400)
      expect(data.success).toBe(false)
      expect(data.errors.email).toBe('Email is required')
    })

    it('should return all validation errors at once', async () => {
      request = createRequest('http://localhost/api/auth/login', {
        method: 'POST',
        body: toJSON({
          email: '',
          password: 'abc'
        })
      })

      response = await POST(request)
      data = parseJSON(response.body)

      expect(response.status).toBe(400)
      expect(data.errors.email).toBeDefined()
      expect(data.errors.password).toBeDefined()
    })
  })
})
```

**Key integration testing concepts:**
- **Mock external dependencies** - Replace cookies, database, etc. with mocks
- **Test full flow** - Request → validation → logic → response
- **Test HTTP details** - Status codes, headers, body structure
- **BeforeEach hooks** - Reset mocks/state before each test

---

## Run Tests

```bash
# Run all tests once
npm run test:run
# or: pytest
# or: go test ./...
# or: cargo test

# Run with coverage report
npm run test:coverage
# or: pytest --cov
# or: go test -cover ./...
# or: cargo test --coverage

# Run in watch mode (re-run on file changes)
npm test
# or: pytest-watch
# or: (use nodemon or similar)
```

### Expected Output

```
✓ src/lib/__tests__/validation.test (8 tests)
✓ app/api/auth/login/__tests__/route.test (7 tests)

Test Files  2 passed (2)
     Tests  15 passed (15)
  Duration  142ms

Coverage:  95% statements, 90% branches, 100% functions
```

---

## Success Criteria
- [ ] Testing framework installed and configured
- [ ] Unit tests cover validation logic
- [ ] Integration tests cover API route
- [ ] All happy paths tested (valid inputs)
- [ ] All error cases tested (invalid inputs)
- [ ] External dependencies mocked/stubbed
- [ ] All tests pass (green ✓)
- [ ] Coverage report shows good coverage (80%+)

---

## Unit vs Integration Tests

### Unit Tests
- **What:** Test individual functions/methods in complete isolation
- **When:** Validation, formatting, calculations, pure logic, utility functions
- **Speed:** ✅ Very fast (milliseconds) - no external dependencies
- **Example:** `validateLoginCredentials()` validation function
- **Mocks:** None needed (pure functions with no dependencies)
- **Scope:** Single function or class

### Integration Tests
- **What:** Test how multiple components work together
- **When:** API routes, database queries, service interactions, full workflows
- **Speed:** ⚠️ Slower (requires setup, mocks, and teardown)
- **Example:** Full login API request/response with validation + cookies
- **Mocks:** Mock external dependencies (database, APIs, cookies)
- **Scope:** Multiple functions/modules working together

### Testing Pyramid

```
        /\
       /  \  E2E Tests (Few)
      /____\  Browser automation, full user flows
     /      \
    / Integ. \ Integration Tests (Some)
   /__________\ API + DB + Services working together
  /            \
 /     Unit     \ Unit Tests (Many)
/________________\ Individual functions in isolation
```

**Recommended distribution:**
- 70% Unit tests - Fast feedback, easy to debug, test edge cases
- 20% Integration tests - Ensure components work together
- 10% E2E tests - Verify critical user journeys

---

## Key Learning Points
1. **Unit Tests** - Fast, test functions in isolation, no external dependencies
2. **Integration Tests** - Test how multiple components work together as a system
3. **Mocking** - Replace external dependencies (cookies, DB, APIs) with test doubles
4. **Coverage** - Measure what percentage of code is tested (aim for 80%+)
5. **Both Types Needed** - Unit tests find logic bugs, integration tests find interaction bugs
6. **Test Pyramid** - Many unit tests, some integration tests, few E2E tests
7. **Test Organization** - Keep tests close to the code they test (co-located)
8. **Red-Green-Refactor** - Write failing test → make it pass → improve code

## Common Pitfalls
- **Only unit tests** - Miss bugs in how components interact
- **Only integration tests** - Slow test suite, hard to debug failures
- **No mocks** - Tests depend on external services, become flaky
- **Testing implementation** - Tests break when refactoring internal code
- **Forgetting error cases** - Only testing happy path, not edge cases
- **No test isolation** - Tests affect each other, fail when run in different orders
- **Unclear test names** - Can't tell what failed from the test name
- **Too much setup** - Tests become hard to understand and maintain
- **Not running tests regularly** - Tests rot and become outdated

---

## Next Step
Move to Lesson 5: Add Input Sanitization for Security
