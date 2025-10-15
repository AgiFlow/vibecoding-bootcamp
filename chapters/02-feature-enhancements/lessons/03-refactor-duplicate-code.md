# Lesson 3: Refactor Duplicate Code into Reusable Utility

**Time:** 30 minutes
**Difficulty:** Intermediate

## Learning Objective
Learn to identify code duplication, extract it into reusable utilities, and refactor existing code to use them - improving maintainability without changing functionality.

## The Enhancement
You have the same data fetching logic duplicated across multiple pages. Any bug fix or change needs to be applied in multiple places.

---

## Current State

**Duplicated Code Example:**

```pseudo
// Page 1: Users Page
function UsersPage():
  response = fetch('https://api.example.com/users', {
    headers: {
      'Authorization': 'Bearer ' + API_TOKEN,
      'Content-Type': 'application/json'
    },
    cache: { revalidate: 60 }
  })

  if NOT response.ok:
    throw Error('Failed to fetch users')
  end

  users = parseJSON(response.body)
  return renderView(<div>{/* render users */}</div>)
end
```

```pseudo
// Page 2: Posts Page
function PostsPage():
  response = fetch('https://api.example.com/posts', {
    headers: {
      'Authorization': 'Bearer ' + API_TOKEN,
      'Content-Type': 'application/json'
    },
    cache: { revalidate: 60 }
  })

  if NOT response.ok:
    throw Error('Failed to fetch posts')
  end

  posts = parseJSON(response.body)
  return renderView(<div>{/* render posts */}</div>)
end
```

**Problem:** Same fetch logic duplicated. If API token changes or error handling needs updating, you must change multiple files!

---

## Part 1: Identify Duplication (5 mins)

### Sample Prompt
```
I have duplicate fetch logic across multiple files:
[paste both code examples]

Help me identify:
1. What code is duplicated?
2. What varies between the uses?
3. What should be extracted into a utility function?
4. Where should I put the utility file?
```

### What's Duplicated
- Authorization header
- Content-Type header
- Error handling pattern
- Response parsing
- Caching configuration

### What Varies
- Endpoint path (`/users` vs `/posts`)
- Data type returned

## Part 2: Create Utility Function (10 mins)

### Sample Prompt
```
Create a reusable API client utility that:
1. Handles authentication headers automatically
2. Throws errors on failed requests
3. Parses JSON responses
4. Supports caching/revalidation options
5. Can be typed for different response data

Put it in a utilities/helpers directory (e.g., src/lib/api or utils/api)
```

### Expected Utility

```pseudo
// src/lib/api - API Client Utility

// Custom error class for API failures
class APIError extends Error:
  properties:
    status: integer
    statusText: string

  constructor(message, status, statusText):
    super(message)
    this.status = status
    this.statusText = statusText
    this.name = 'APIError'
  end
end

// Main API client function
function apiClient(endpoint, options = {}):
  // Extract caching options
  revalidate = options.revalidate OR 60
  tags = options.tags OR []

  // Build full URL
  baseURL = getEnvironmentVariable('API_URL') OR 'https://api.example.com'
  url = baseURL + endpoint

  // Make HTTP request with auth headers
  response = fetch(url, {
    ...options,
    headers: {
      'Authorization': 'Bearer ' + getEnvironmentVariable('API_TOKEN'),
      'Content-Type': 'application/json',
      ...options.headers  // Allow override
    },
    cache: {
      revalidate: revalidate,
      tags: tags
    }
  })

  // Handle failed requests
  if NOT response.ok:
    throw new APIError(
      'API request failed: ' + endpoint,
      response.status,
      response.statusText
    )
  end

  // Parse and return JSON
  return parseJSON(response.body)
end

// Convenience wrapper functions for common HTTP methods
api = {
  get: function(endpoint, options):
    return apiClient(endpoint, { ...options, method: 'GET' })
  end,

  post: function(endpoint, data, options):
    return apiClient(endpoint, {
      ...options,
      method: 'POST',
      body: toJSON(data)
    })
  end,

  put: function(endpoint, data, options):
    return apiClient(endpoint, {
      ...options,
      method: 'PUT',
      body: toJSON(data)
    })
  end,

  delete: function(endpoint, options):
    return apiClient(endpoint, { ...options, method: 'DELETE' })
  end
}

export { apiClient, api, APIError }
```

## Part 3: Refactor Existing Code (10 mins)

### Sample Prompt
```
Refactor my users and posts pages to use the new api utility:
[paste current page code]
[paste api utility]

Keep the same functionality but use the utility instead of duplicate fetch calls.
```

### Refactored Users Page

```pseudo
// Users Page - Refactored
import { api } from 'src/lib/api'

function UsersPage():
  // Simple one-line API call - no duplication!
  users = await api.get('/users')

  return renderView(
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  )
end
```

**Before:** 15 lines of duplicated fetch logic
**After:** 1 line using the utility

### Refactored Posts Page

```pseudo
// Posts Page - Refactored
import { api } from 'src/lib/api'

function PostsPage():
  // Same simple one-line API call
  posts = await api.get('/posts')

  return renderView(
    <div>
      <h1>Posts</h1>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
        </article>
      ))}
    </div>
  )
end
```

**Before:** 15 lines of duplicated fetch logic
**After:** 1 line using the utility

### Add Error Handling Component (Optional)

```pseudo
// Error Boundary Component - Client-side
import { APIError } from 'src/lib/api'

function ErrorBoundary({ error, reset }):
  // Log error (in development or to monitoring service)
  logError('Page error:', error)

  // Check if it's an API error
  if error instanceof APIError:
    return renderView(
      <div>
        <h2>Failed to load data</h2>
        <p>Status: {error.status} - {error.statusText}</p>
        <button onClick={reset}>Try again</button>
      </div>
    )
  end

  // Generic error fallback
  return renderView(
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={reset}>Try again</button>
    </div>
  )
end
```

## Part 4: Add Advanced Features (5 mins)

### Sample Prompt
```
Enhance the API client to support:
1. Request timeout (abort after 10 seconds)
2. Retry on failure (up to 3 times with exponential backoff)
3. Request/response logging in development mode

Keep the same API interface so existing code doesn't break.
```

### Enhanced API Client

```pseudo
// src/lib/api - Enhanced with timeout and retry

MAX_RETRIES = 3
RETRY_DELAY = 1000  // milliseconds
TIMEOUT_DURATION = 10000  // 10 seconds

// Helper function for delays
function sleep(milliseconds):
  return new Promise(resolve => setTimeout(resolve, milliseconds))
end

// Enhanced API client with retry and timeout
function apiClient(endpoint, options = {}):
  revalidate = options.revalidate OR 60
  tags = options.tags OR []
  baseURL = getEnvironmentVariable('API_URL') OR 'https://api.example.com'
  url = baseURL + endpoint

  lastError = null

  // Retry loop - attempt up to MAX_RETRIES times
  for attempt = 0 to MAX_RETRIES:
    try:
      // Development logging
      if isDevelopment():
        log('[API]', options.method OR 'GET', endpoint)
      end

      // Create timeout controller
      timeoutController = new AbortController()
      timeoutId = setTimeout(() => timeoutController.abort(), TIMEOUT_DURATION)

      // Make request with timeout
      response = fetch(url, {
        ...options,
        headers: {
          'Authorization': 'Bearer ' + getEnvironmentVariable('API_TOKEN'),
          'Content-Type': 'application/json',
          ...options.headers
        },
        cache: { revalidate, tags },
        signal: timeoutController.signal  // Allows timeout cancellation
      })

      clearTimeout(timeoutId)  // Clear timeout if request completes

      // Handle error responses
      if NOT response.ok:
        throw new APIError(
          'API request failed: ' + endpoint,
          response.status,
          response.statusText
        )
      end

      // Parse response
      data = parseJSON(response.body)

      // Development logging
      if isDevelopment():
        log('[API] ✓', endpoint, data)
      end

      return data

    catch error:
      lastError = error

      // Don't retry on 4xx errors (client errors - won't succeed on retry)
      if error instanceof APIError AND error.status >= 400 AND error.status < 500:
        throw error
      end

      // Retry with exponential backoff
      if attempt < MAX_RETRIES - 1:
        waitTime = RETRY_DELAY * (attempt + 1)  // 1s, 2s, 3s
        log('[API] Retrying', endpoint, '(', attempt + 1, '/', MAX_RETRIES, ')')
        await sleep(waitTime)
      end
    end
  end

  // All retries failed
  throw lastError OR new Error('API request failed after retries')
end
```

**Enhancements added:**
- **Timeout**: Aborts request after 10 seconds
- **Retry**: Up to 3 attempts with exponential backoff (1s, 2s, 3s)
- **Logging**: Logs requests/responses in development
- **Smart retry**: Doesn't retry 4xx errors (client mistakes)

---

## Success Criteria
- [ ] API utility created in appropriate directory (e.g., src/lib/api)
- [ ] Duplicated fetch logic removed from all pages
- [ ] All pages use the new utility
- [ ] Error handling is consistent across all API calls
- [ ] Existing functionality unchanged (same behavior)
- [ ] Code is more maintainable and DRY
- [ ] Can easily add new API calls without duplication

---

## Testing Checklist
1. Users page loads correctly ✅
2. Posts page loads correctly ✅
3. Error handling works (test with invalid endpoint) ✅
4. Caching/revalidation still works ✅
5. Timeout works (test with slow endpoint) ✅
6. Retry logic works on network failures ✅
7. Can easily add new API calls using utility ✅

---

## Before & After Comparison

### Before Refactoring
- 15 lines of duplicated code per page
- 3 pages = **45 lines total**
- Bug fix = update 3 files
- Adding auth header = update 3 files
- Adding timeout = update 3 files
- Inconsistent error handling

### After Refactoring
- 40 lines in reusable utility
- 5 lines per page
- 3 pages = 15 lines total
- **Total: 55 lines** (vs 45 before)

**But the benefits:**
- Bug fix = update 1 file!
- Adding new feature = update 1 file!
- Consistent error handling everywhere
- Timeout and retry logic for free
- Easy to add new pages with 1 line

**Trade-off:** More code initially, but MUCH better long-term maintainability!

---

## Key Learning Points
1. **DRY Principle** - Don't Repeat Yourself - duplication makes maintenance harder
2. **Single Responsibility** - Utility handles API concerns, pages handle UI concerns
3. **Maintainability** - Changes in one place automatically affect all uses
4. **Abstraction** - Extract common patterns, parameterize differences
5. **Gradual Refactoring** - Can refactor one file at a time, not all at once
6. **Code Reuse** - Write once, use many times
7. **Consistency** - Shared utility ensures consistent behavior

## Common Pitfalls
- **Over-abstracting** - Making utility too complex with too many options
- **Breaking changes** - Accidentally changing functionality during refactor
- **Incomplete refactoring** - Missing some occurrences of the duplicated code
- **Premature abstraction** - Creating utility before pattern is clear
- **No testing** - Not verifying behavior is preserved after refactor
- **Tight coupling** - Making pages dependent on utility implementation details
- **Gold plating** - Adding features the utility doesn't actually need

## When to Refactor

✅ **Good time to refactor:**
- Pattern duplicated **3+ times** (rule of three)
- Code is **stable** (not changing frequently)
- **Clear abstraction** boundary exists
- **Tests in place** to verify behavior
- You **understand the pattern** well
- Team agrees on the approach

❌ **Bad time to refactor:**
- Code only used **twice** (wait for third use)
- **Requirements still changing** rapidly
- **Unclear** what the pattern should be
- **No tests** to verify behavior doesn't break
- Under **tight deadline** pressure
- **Different use cases** that look similar but aren't

---

## Next Step
Move to Lesson 4: Add Unit Tests for Existing Feature
