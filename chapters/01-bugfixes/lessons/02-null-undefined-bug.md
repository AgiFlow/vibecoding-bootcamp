# Lesson 2: Null/Undefined Handling

**Time:** 5-10 minutes
**Difficulty:** Beginner

## Learning Objective
Learn to identify and fix null/undefined errors using an AI coding assistant.

## The Bug
Application crashes when user object doesn't have an address.

---

## Scenario
```pseudo
function getUserCity(user):
  return user.address.city  // Crashes if address is null or undefined!
end

// Test cases
user1 = { name: "John", address: { city: "NYC" } }
user2 = { name: "Jane" }  // No address property

getUserCity(user1)  // Works: "NYC"
getUserCity(user2)  // ERROR: Cannot read property 'city' of null/undefined
```

**Expected:** Return a default value or handle missing address gracefully
**Actual:** Application crashes

---

## Your Task
1. Describe the error to your AI assistant
2. Ask for a fix that handles missing data
3. Implement the solution
4. Test with both users:
   - User with address → Should return "NYC" ✅
   - User without address → Should return "Unknown" ✅

## Sample Prompt
```
I have a function that gets a user's city:
[paste code]

It crashes when the user object doesn't have an address property.
How can I fix this to handle missing data gracefully?
```

---

## Expected Fix

**Option 1: Safe Navigation / Null Coalescing**
```pseudo
function getUserCity(user):
  return user?.address?.city ?? "Unknown"
  // Safe navigation checks each level for null/undefined
  // If any level is null/undefined, return "Unknown"
end
```

**Option 2: Guard Clause**
```pseudo
function getUserCity(user):
  if user is null OR user.address is null:
    return "Unknown"
  end
  return user.address.city
end
```

---

## Success Criteria
- [ ] No crashes when address is missing
- [ ] Returns appropriate default (e.g., "Unknown" or null)
- [ ] Still works when address exists

---

## Key Learning Points
1. **Safe Navigation (Optional Chaining)** - Safely access nested properties without errors if intermediate values are null/undefined
2. **Null Coalescing** - Provide default values when a value is null or undefined
3. **Defensive Programming** - Always check for missing data before accessing nested properties
4. **Guard Clauses** - Early returns that prevent errors by validating data first

## Common Pitfalls
- Assuming all objects have all properties (data can be incomplete)
- Forgetting to handle both null and undefined cases
- Using OR instead of null coalescing (which may treat valid values like 0 or empty string as missing)
- Not testing with incomplete/missing data scenarios

---

## Next Step
Move to Lesson 3: Type Error Bug
