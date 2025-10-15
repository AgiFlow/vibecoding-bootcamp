# Lesson 2: Add Database Migration for New Field

**Time:** 30 minutes
**Difficulty:** Intermediate

## Learning Objective
Learn to add a new field to an existing database table using migrations, update the schema, and use the new field in your app.

## The Enhancement
Your `users` table currently only has `id`, `name`, and `email`. Add a `role` field to support admin vs regular users.

---

## Current State

**Current Database Schema:**
```pseudo
// Users table definition
Table: users
  id: INTEGER, primary key, auto-increment
  name: TEXT, not null
  email: TEXT, not null, unique
  created_at: TIMESTAMP, default: current time
```

**Current:** Users don't have roles
**Enhancement:** Add `role` field (values: 'admin' or 'user', defaults to 'user')

---

## Part 1: Plan the Schema Change (5 mins)

### Sample Prompt
```
I have a users table with id, name, email, created_at.

I want to add a 'role' field that:
1. Can be either 'admin' or 'user' (constrained values)
2. Defaults to 'user'
3. Is required (not null)
4. Doesn't break existing users in the database

Using my ORM/database migration tool:
1. How do I update the schema definition?
2. How do I create a migration file?
3. How do I apply it without losing existing data?
4. What SQL will be generated?
```

## Part 2: Update Schema (5 mins)

### Sample Prompt
```
Update my database schema definition to add a role field:
[paste current schema]

Requirements:
- Type: ENUM or TEXT with constraint
- Allowed values: 'admin' or 'user'
- Default value: 'user'
- Not null
- Add it to the existing users table definition
```

### Expected Schema Update

```pseudo
// Updated users table definition
Table: users
  id: INTEGER, primary key, auto-increment
  name: TEXT, not null
  email: TEXT, not null, unique
  role: TEXT, enum['admin', 'user'], not null, default: 'user'  // NEW FIELD
  created_at: TIMESTAMP, default: current time
```

**Key changes:**
- Added `role` field with constrained values
- Set default to `'user'` so existing rows won't break
- Marked as `not null` for data integrity

## Part 3: Generate Migration (10 mins)

### Sample Prompt
```
Using my ORM's migration tool, show me how to:
1. Generate a migration for adding the role field
2. What command to run
3. What the migration file will look like (SQL)
4. How to apply it to the database safely

Current schema: [paste updated schema]
```

### Generate Migration Command

```bash
# Generate migration (command varies by tool)
# Examples:
# - npm run migration:generate add_user_role
# - rake db:create_migration add_user_role
# - python manage.py makemigrations
# - prisma migrate dev --name add_user_role

# This creates a migration file like:
# migrations/0002_add_user_role.sql
# OR migrations/20250115_add_user_role.up.sql
```

### Expected Migration File (SQL)

```sql
-- Migration: Add role column to users table
-- Created: 2025-01-15

-- Step 1: Add role column with default value
-- DEFAULT is crucial - it populates existing rows!
ALTER TABLE users
ADD COLUMN role TEXT DEFAULT 'user' NOT NULL;

-- Step 2: Add check constraint for valid values
-- Ensures only 'admin' or 'user' can be stored
ALTER TABLE users
ADD CONSTRAINT users_role_check
CHECK (role IN ('admin', 'user'));
```

**Why this works:**
- `DEFAULT 'user'` - Existing rows automatically get 'user' role
- `NOT NULL` - After default is set, enforce data integrity
- `CHECK` constraint - Prevents invalid role values

### Apply Migration

```bash
# Run migration (command varies by tool)
# Examples:
# - npm run migration:run
# - rake db:migrate
# - python manage.py migrate
# - prisma migrate deploy
# - flyway migrate

# Verify it worked
# - Check table structure
# - Query existing users to see default role applied
```

### Migration Script Example (if needed)

```pseudo
// Migration runner script
function runMigrations():
  connection = connectToDatabase(DATABASE_URL)

  print("Running migrations...")

  try:
    applyMigrations(connection, {
      migrationsFolder: './migrations'
    })

    print("Migrations complete!")
    exit(0)
  catch error:
    print("Migration failed!", error)
    exit(1)
  end
end

runMigrations()
```

**Add to build/deployment scripts:**
```pseudo
// In package.json, Makefile, or deployment config
scripts:
  "db:migrate": "[command to run migration script]"
```

## Part 4: Use the New Field (10 mins)

### Sample Prompt
```
Now that I have the role field in the database, update my code to:
1. Show user's role in the dashboard
2. Add an admin-only section (visible only to admins)
3. Update user creation API to accept and set role
4. Create a helper function to check if user is admin

Current dashboard code: [paste code]
```

### Update User Creation API

```pseudo
// API Route: POST /api/users
function POST(request):
  { name, email, role } = parseJSON(request.body)

  // Create new user in database
  newUser = database.insert('users', {
    name: name,
    email: email,
    role: role OR 'user'  // Default to 'user' if not provided
  })

  return jsonResponse(newUser, status: 201)
end
```

### Display Role in Dashboard

```pseudo
// Dashboard Page Component
function DashboardPage():
  // Get current user (simplified - use real auth system)
  currentUser = database.query('users')
    .where('email', '=', currentUserEmail)
    .first()

  return renderView(
    <div>
      <h1>Dashboard</h1>
      <p>Welcome, {currentUser.name}!</p>
      <p>Role: {currentUser.role}</p>

      // Admin-only section - conditional rendering
      {currentUser.role == 'admin' AND (
        <div style="background: #ffe; padding: 1rem; margin-top: 1rem">
          <h2>Admin Controls</h2>
          <p>Only admins can see this!</p>
          <button>Manage Users</button>
          <button>View Reports</button>
          <button>System Settings</button>
        </div>
      )}
    </div>
  )
end
```

### Create Admin Check Helper

```pseudo
// Helper function - src/lib/auth
function isAdmin(email):
  // Query database for user's role
  user = database.query('users')
    .select('role')
    .where('email', '=', email)
    .first()

  // Return true if user exists and is admin
  return user?.role == 'admin'
end

// Usage example
if isAdmin(currentUserEmail):
  // Allow admin action
else:
  return error("Unauthorized", 403)
end
```

---

## Success Criteria
- [ ] Schema updated with role field
- [ ] Migration file generated
- [ ] Migration applied successfully
- [ ] Existing users have 'user' role (default)
- [ ] Can create new users with admin role
- [ ] Dashboard shows user's role
- [ ] Admin-only content hidden from regular users
- [ ] No data loss during migration

---

## Testing Checklist

### Before Migration
```sql
-- Check current users (before adding role column)
SELECT * FROM users;

-- Expected: All users, NO role column
```

### After Migration
```sql
-- Verify role column exists
SELECT id, name, email, role FROM users;

-- Expected: All users now have role column with 'user' value

-- Verify default role was applied to existing users
SELECT COUNT(*) FROM users WHERE role = 'user';

-- Expected: Count matches total user count (all got default)

-- Test constraint (this should FAIL)
INSERT INTO users (name, email, role)
VALUES ('Test User', 'test@example.com', 'invalid');

-- Expected: Error - constraint violation
-- Message: "value 'invalid' violates check constraint"
```

### Application Testing
1. Start app and check no errors ✅
2. View dashboard → See role displayed ("user" or "admin") ✅
3. Create user with 'admin' role → See admin controls ✅
4. Create user with 'user' role → Admin controls hidden ✅
5. Try setting invalid role → Should fail validation ✅
6. Query database → Verify role is stored correctly ✅

---

## Key Learning Points
1. **Schema Evolution** - Databases evolve over time; migrations track and apply changes systematically
2. **Default Values** - CRITICAL when adding NOT NULL columns to tables with existing data
3. **Data Integrity** - Use constraints (CHECK, FOREIGN KEY, UNIQUE) to enforce business rules at database level
4. **Migration Safety** - Always test migrations on dev/staging environments before production
5. **Code Synchronization** - Update application code to use new fields after migration
6. **Idempotency** - Migrations should be safe to run multiple times (use IF NOT EXISTS, IF EXISTS)
7. **Versioning** - Migrations are numbered/timestamped and applied in order

## Common Pitfalls
- **Forgetting default value** - Adding NOT NULL column without DEFAULT breaks existing rows
- **No constraints** - Allowing invalid values defeats purpose of typed fields
- **No backup** - Running migration on production without backup is dangerous
- **Not testing rollback** - Migrations can fail; always test reverting changes
- **Code-database mismatch** - Deploying app code before/after migration runs
- **Missing indexes** - Not adding indexes on new columns that will be queried
- **Large table locks** - Some migrations lock tables, causing downtime on large datasets
- **Timezone issues** - TIMESTAMP columns without timezone consideration

---

## Rollback Strategy

If something goes wrong, you need to reverse the migration:

```sql
-- Rollback migration - Remove role column
ALTER TABLE users DROP COLUMN role;

-- This will:
-- - Remove the column entirely
-- - Delete all role data (irreversible!)
-- - Remove associated constraints automatically
```

**Important:**
- Test rollback on dev/staging first
- Rollbacks may cause data loss
- Document rollback steps in migration file
- Some changes can't be rolled back (e.g., data deletion)

---

## Production Checklist
- [ ] Test migration on development database ✅
- [ ] Test migration on staging database ✅
- [ ] **Backup production database** ✅
- [ ] Verify backup can be restored ✅
- [ ] Test rollback procedure ✅
- [ ] Plan deployment window (low-traffic time) ✅
- [ ] Notify team of database changes ✅
- [ ] Run migration during planned window ✅
- [ ] Monitor application logs for errors ✅
- [ ] Monitor database performance ✅
- [ ] Verify data integrity with queries ✅
- [ ] Keep rollback plan ready ✅

---

## Next Step
Move to Lesson 3: Refactor Duplicate Code into Utility Function
