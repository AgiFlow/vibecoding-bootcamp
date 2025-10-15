# Lesson 1: Add Authentication to Existing App

**Time:** 30 minutes
**Difficulty:** Intermediate

## Learning Objective
Learn to add basic authentication (login/logout) to an existing application that currently has no auth.

## The Enhancement
Your app currently has unprotected pages. Add authentication so only logged-in users can access certain routes.

---

## Current State
```pseudo
// Dashboard page - Currently accessible to anyone
function DashboardPage():
  return renderView(
    <div>
      <h1>Dashboard</h1>
      <p>Secret data here...</p>
    </div>
  )
end
```

**Problem:** Anyone can access `/dashboard` without logging in!

---

## What You'll Build
- Login page with email/password
- Session/token management
- Protected routes (redirect to login if not authenticated)
- Logout functionality

---

## Part 1: Plan the Auth System (5 mins)

### Sample Prompt
```
I need to add authentication to my web application. Currently all pages are public.

Requirements:
1. Login page with email/password
2. Store user session (use cookies or JWT tokens)
3. Protect /dashboard route - redirect to /login if not authenticated
4. Logout button
5. Use best practices for my framework

Please suggest:
1. What auth approach should I use? (library vs custom implementation)
2. Where should I store sessions? (cookies, local storage, server-side)
3. How do I protect routes/pages?
4. What components/files do I need to create?
```

## Part 2: Create Login API Route (10 mins)

### Sample Prompt
```
Create a simple login API endpoint at /api/auth/login that:
1. Accepts POST request with { email, password } in the body
2. Validates credentials (use mock data for now: admin@example.com / password123)
3. Creates a session token
4. Sets an httpOnly cookie (secure, 1 week expiration)
5. Returns { success: true, user: { email } } on success
6. Returns { success: false, error: "message" } on failure with 401 status

Use framework-appropriate API route patterns.
```

### Expected Output
```pseudo
// API Route: /api/auth/login
function POST(request):
  // Parse request body
  { email, password } = parseJSON(request.body)

  // Mock validation - replace with real database check
  if email == 'admin@example.com' AND password == 'password123':
    // Create session token (in production: use JWT or session library)
    token = encodeBase64(email + ":" + currentTimestamp())

    // Set httpOnly cookie for security
    setCookie('session', token, {
      httpOnly: true,
      secure: isProduction(),
      maxAge: 60 * 60 * 24 * 7,  // 1 week in seconds
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
    error: 'Invalid credentials'
  }, status: 401)
end
```

## Part 3: Create Login Page (5 mins)

### Sample Prompt
```
Create a login page at /login with:
1. Email and password form fields (both required)
2. Submit button
3. Calls POST /api/auth/login on submit
4. Shows error message if login fails (red text)
5. Redirects to /dashboard on success
6. Uses client-side state management for form handling
```

### Expected Output
```pseudo
// Login Page Component - Client-side
function LoginPage():
  // State variables
  email = useState('')
  password = useState('')
  error = useState('')

  function handleSubmit(event):
    event.preventDefault()
    error = ''

    // Call login API
    response = fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: toJSON({ email, password })
    })

    data = parseJSON(response.body)

    if data.success:
      navigateTo('/dashboard')
    else:
      error = data.error OR 'Login failed'
    end
  end

  return renderView(
    <div style="max-width: 400px; margin: 100px auto; padding: 20px">
      <h1>Login</h1>
      <form onSubmit={handleSubmit}>
        <div style="margin-bottom: 1rem">
          <label>Email:</label>
          <input
            type="email"
            value={email}
            onChange={setEmail}
            required
            style="width: 100%; padding: 8px"
          />
        </div>
        <div style="margin-bottom: 1rem">
          <label>Password:</label>
          <input
            type="password"
            value={password}
            onChange={setPassword}
            required
            style="width: 100%; padding: 8px"
          />
        </div>
        {error AND <p style="color: red">{error}</p>}
        <button type="submit" style="padding: 10px 20px">
          Login
        </button>
      </form>
    </div>
  )
end
```

## Part 4: Protect Dashboard Route (10 mins)

### Sample Prompt
```
Protect the /dashboard route so:
1. Check if user has valid session cookie
2. If no session, redirect to /login
3. If session exists, show the dashboard
4. Add a logout button that clears the session and redirects to login

Use framework-appropriate middleware/guard patterns.
```

### Expected Output

**Option 1: Middleware/Route Guard (Recommended)**
```pseudo
// Middleware - Runs before request reaches the page
function authMiddleware(request):
  session = getCookie(request, 'session')

  // Protect /dashboard route
  if request.path.startsWith('/dashboard'):
    if NOT session:
      return redirectTo('/login')
    end
  end

  return allowRequest()
end

// Configure which routes to protect
config = {
  protectedPaths: '/dashboard/*'
}
```

**Logout API Route**
```pseudo
// API Route: /api/auth/logout
function POST(request):
  deleteCookie('session')

  return jsonResponse({
    success: true
  }, status: 200)
end
```

**Updated Dashboard with Logout**
```pseudo
// Dashboard Page Component - Server-side
function DashboardPage():
  return renderView(
    <div>
      <h1>Dashboard</h1>
      <p>Secret data here - you're logged in!</p>
      <LogoutButton />
    </div>
  )
end
```

**Logout Button Component**
```pseudo
// Logout Button Component - Client-side
function LogoutButton():

  function handleLogout():
    // Call logout API
    fetch('/api/auth/logout', { method: 'POST' })

    // Redirect to login page
    navigateTo('/login')
  end

  return renderView(
    <button onClick={handleLogout}>Logout</button>
  )
end
```

---

## Success Criteria
- [ ] Can't access /dashboard without logging in
- [ ] Login page accepts email/password
- [ ] Correct credentials redirect to dashboard
- [ ] Wrong credentials show error message
- [ ] Session cookie is set after login
- [ ] Logout clears session and redirects to login
- [ ] Can log in again after logout

## Testing Checklist
1. Visit `/dashboard` while logged out → Redirects to `/login` ✅
2. Try wrong password → Shows error ✅
3. Login with `admin@example.com` / `password123` → Redirects to dashboard ✅
4. Refresh dashboard page → Still logged in ✅
5. Click logout → Redirects to login ✅
6. Try to visit dashboard again → Redirects to login ✅

---

## Key Learning Points
1. **Session Management** - Store sessions in httpOnly cookies for security (not accessible via JavaScript)
2. **Route Protection** - Use middleware/guards to protect multiple routes before they load
3. **API Endpoints** - Separate login/logout logic into dedicated API routes
4. **Client vs Server** - Forms need client-side interactivity, protection happens server-side
5. **Security** - Never store passwords in plain text, always use httpOnly cookies for sessions
6. **Token Generation** - In production, use proper JWT libraries or session management solutions

## Common Pitfalls
- Storing session tokens in localStorage (vulnerable to XSS attacks)
- Not setting httpOnly flag on cookies (allows JavaScript access)
- Forgetting to check authentication on every protected route
- Not clearing session data completely on logout
- Using weak or predictable session tokens
- Not handling token expiration
- Storing sensitive data in cookies without encryption

---

## Production Considerations (Not for this lesson)
- Hash passwords with bcrypt or argon2
- Use proper session library (JWT libraries, session managers)
- Add CSRF protection for forms
- Implement "remember me" functionality
- Add email verification flow
- Support OAuth providers (Google, GitHub, etc.)
- Add rate limiting to prevent brute force attacks
- Implement password reset flow
- Add multi-factor authentication (MFA)
- Log authentication events for security auditing

---

## Next Step
Move to Lesson 2: Add Database Migration for New Field
