# Lesson 1: Building a New Dashboard - Leave Request System

**Time:** 2-3 hours
**Difficulty:** Advanced

## Learning Objective
Learn how to build a complete leave request dashboard from scratch, covering authentication, API design, data fetching, and routing - all the fundamentals of a full-stack application.

---

## Overview
Build a simple leave request system where employees can manage their time-off requests through an authenticated dashboard.

---

## Problem Statement
Build a simple leave request system where employees can:
- Log in to see their leave requests
- View a list of their leave requests
- Submit a new leave request
- See the status (pending, approved, rejected)

---

## Key Components to Build

### 1. Authentication
- Login page
- Session/token management
- Protected routes
- Logout functionality

### 2. API Endpoints
- `POST /api/auth/login` - User login
- `GET /api/leave-requests` - Get user's leave requests
- `POST /api/leave-requests` - Create new leave request
- `GET /api/leave-requests/[id]` - Get single request details

### 3. Data Fetching
- Fetch leave requests on page load
- Handle loading states
- Handle errors gracefully
- Refresh data after submission

### 4. Routing
- `/login` - Login page
- `/dashboard` - Main dashboard (protected)
- `/dashboard/new` - Submit new request (protected)
- Redirect logic (logged out → login, logged in → dashboard)

---

## Recommended Approach

### Step 1: Set Up Authentication
```
Create a simple login system:
1. Login page with email/password form
2. API endpoint that validates credentials
3. Store user session/token
4. Redirect to dashboard after login
5. Add logout button
```

### Step 2: Create API Endpoints
```
Create API endpoints for leave requests:
1. GET /api/leave-requests - returns list of requests for logged-in user
2. POST /api/leave-requests - creates new leave request
3. Use mock data or simple database
4. Protect endpoints (require authentication)
```

### Step 3: Build Dashboard Page
```
Create the main dashboard that:
1. Checks if user is logged in (redirect to login if not)
2. Fetches leave requests from API
3. Displays them in a table or list
4. Shows: date, type (sick/vacation), days, status
5. Has button to create new request
```

### Step 4: Add New Request Form
```
Create a page to submit new leave request:
1. Form with: start date, end date, type, reason
2. Submit button that calls POST API
3. Redirect to dashboard after success
4. Show error if submission fails
```

---

## Example Data Structure

### Leave Request Object
```pseudo
LeaveRequest:
  id: unique identifier
  userId: unique identifier
  startDate: date string (ISO format)
  endDate: date string (ISO format)
  type: enum ['sick', 'vacation', 'personal']
  reason: text
  status: enum ['pending', 'approved', 'rejected']
  createdAt: timestamp
```

---

## Simple Implementation Prompt

```
Build a simple leave request dashboard:

1. Authentication:
   - Login page with email/password
   - Use simple session/JWT
   - Protect dashboard routes

2. API:
   - GET /api/leave-requests - get user's requests
   - POST /api/leave-requests - create new request
   - Use [database/mock data]

3. Dashboard Page (/dashboard):
   - Show table of leave requests
   - Columns: dates, type, status
   - Button to create new request
   - Logout button

4. New Request Page (/dashboard/new):
   - Form: start date, end date, type, reason
   - Submit creates request via API
   - Redirect to dashboard on success

Keep it simple - focus on the core flow working.
```

---

## Success Criteria
- [ ] Users can log in and log out
- [ ] Dashboard displays user's leave requests
- [ ] Users can submit a new leave request
- [ ] API endpoints handle CRUD operations
- [ ] Routes protect authenticated pages
- [ ] Data fetching shows loading/error states

---

## What This Teaches

- **Auth**: How to protect pages and APIs
- **API**: Creating endpoints with proper methods (GET/POST)
- **Data Fetching**: Loading data from backend to frontend
- **Routing**: Multiple pages with navigation and redirects

---

## Common Pitfalls

- Overcomplicating auth (keep it simple for learning)
- Not handling loading states
- Forgetting to protect routes
- Not validating form inputs
- Making API too complex

---

## Next Steps

After completing the basic dashboard:
- Add edit/delete functionality
- Add admin view to approve/reject requests
- Add email notifications
- Add date validation (can't request past dates)
- Add team calendar view

---

## Next Step
Congratulations on completing the greenfield project! Review the chapter README for additional project ideas or revisit earlier chapters to solidify your skills.
