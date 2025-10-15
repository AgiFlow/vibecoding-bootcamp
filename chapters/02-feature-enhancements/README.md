# Chapter 2: Feature Enhancements

Learn how to add new capabilities to existing features and improve functionality using AI coding assistants.

## Overview

This chapter teaches you how to enhance existing codebases with common, high-impact improvements that professional developers make regularly: adding authentication, database migrations, refactoring code, writing tests, and securing user input.

Each lesson is designed to take **30 minutes** and focuses on a practical skill you'll use in real projects.

## Quick Start Lessons

### [Lesson 1: Add Authentication](./lessons/01-add-authentication.md) ✅
Add login/logout and route protection to an unprotected app.
- **Time:** 30 minutes
- **Difficulty:** Intermediate
- **What You'll Build:** Login page, session management, protected routes, logout

### [Lesson 2: Database Migration for New Field](./lessons/02-database-migration-new-field.md) ✅
Add a new `role` field to users table using migrations.
- **Time:** 30 minutes
- **Difficulty:** Intermediate
- **What You'll Learn:** Schema changes, migrations, backward compatibility, applying changes safely

### [Lesson 3: Refactor Duplicate Code](./lessons/03-refactor-duplicate-code.md) ✅
Extract duplicated API fetch logic into reusable utility.
- **Time:** 30 minutes
- **Difficulty:** Intermediate
- **What You'll Learn:** DRY principle, code organization, maintainability, type safety

### [Lesson 4: Add Tests (Unit + Integration)](./lessons/04-add-tests.md) ✅
Write unit tests for validation logic and integration tests for API routes.
- **Time:** 30 minutes
- **Difficulty:** Intermediate
- **What You'll Learn:** Unit vs integration tests, mocking, test-driven development, coverage

### [Lesson 5: Security Enhancement](./lessons/05-security-enhancement.md) ✅
Prevent XSS attacks by sanitizing user input.
- **Time:** 30 minutes
- **Difficulty:** Intermediate
- **What You'll Learn:** XSS prevention, input sanitization, output encoding, Content Security Policy

---

## Learning Path

**Recommended Order:**
1. Start with Lesson 1 (Authentication) - Foundational security concept
2. Lesson 2 (Database Migration) - Learn safe schema evolution
3. Lesson 3 (Refactoring) - Improve code quality
4. Lesson 4 (Testing) - Ensure code reliability
5. Lesson 5 (Security) - Protect against attacks

**Total Time:** ~2.5 hours to complete all 5 lessons

---

## What This Chapter Covers

### Authentication & Authorization
- Session management with cookies/JWT
- Login/logout flows
- Route protection
- User roles and permissions

### Database Changes
- Adding new fields to tables
- Creating migrations
- Applying schema changes safely
- Handling existing data
- Rollback strategies

### Code Quality
- Identifying code duplication
- Extracting reusable utilities
- Refactoring without breaking changes
- Type safety with TypeScript
- Code organization

### Testing
- Unit tests for pure functions
- Integration tests for API routes
- Mocking Next.js APIs
- Test coverage
- Testing pyramid

### Security
- XSS (Cross-Site Scripting) prevention
- Input sanitization
- Output encoding
- Content Security Policy
- Security headers

---

## Reference Material

The [lesson-01.md](./lesson-01.md) file contains comprehensive concepts about feature enhancements. Use it as a reference guide after completing the practical lessons.

---

## Key Principles

### 1. Understand Before Changing
Always analyze existing patterns before adding code.

### 2. Maintain Consistency
Follow existing code style, naming, and architecture.

### 3. Don't Break Things
Test existing functionality after enhancements.

### 4. Add Tests
Write tests for new functionality.

### 5. Security First
Always validate and sanitize user input.

---

## Common Enhancement Patterns

### Adding a New Feature
1. Understand existing implementation
2. Plan the enhancement
3. Implement incrementally
4. Add tests
5. Update documentation

### Database Changes
1. Update schema
2. Generate migration
3. Test on dev/staging
4. Backup production
5. Apply migration
6. Update app code

### Security Improvements
1. Identify vulnerabilities
2. Add input validation
3. Sanitize user input
4. Encode output
5. Add security headers
6. Test with malicious input

---

## Backlog Lessons (TBD)

### Lesson 6: Email Notifications
**Status:** TBD - Sending transactional emails, templates, background jobs

### Lesson 7: File Upload Feature
**Status:** TBD - File validation, storage (S3/local), image processing

### Lesson 8: Real-time Updates
**Status:** TBD - WebSocket setup, real-time data sync, presence indicators

### Lesson 9: API Rate Limiting
**Status:** TBD - Rate limiting, throttling, abuse prevention

### Lesson 10: Feature Flags
**Status:** TBD - Feature toggles, A/B testing, gradual rollouts

---

## Tips for Success

1. **Work with AI Assistant** - Use prompts provided in each lesson
2. **Test Incrementally** - Verify each step before moving forward
3. **Understand, Don't Just Copy** - Ask AI to explain the why
4. **Use Real Examples** - Apply lessons to your own projects
5. **Keep Security in Mind** - Always validate and sanitize user input

---

## Next Chapter

After completing feature enhancements, move to:
- **Chapter 3: Greenfield Projects** - Build complete applications from scratch
