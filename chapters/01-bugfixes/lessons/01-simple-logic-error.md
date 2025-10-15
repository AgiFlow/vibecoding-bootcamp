# Lesson 1: Simple Logic Error

**Time:** 5-10 minutes
**Difficulty:** Beginner

## Learning Objective
Learn to identify and fix a simple logic error using an AI coding assistant.

## The Bug
A button that should enable when a form is valid is not working correctly.

---

## Scenario
```pseudo
// Button should be enabled when name is filled AND age is >= 18
function isFormValid(name, age):
  return name.length > 0 OR age >= 18  // Bug is here!
end
```

**Expected:** Button enables only when BOTH conditions are true
**Actual:** Button enables when EITHER condition is true

---

## Your Task
1. Describe the bug to your AI assistant
2. Ask it to identify the issue
3. Implement the fix
4. Test with these cases:
   - Name: "John", Age: 20 → Should be valid ✅
   - Name: "", Age: 25 → Should be invalid ❌
   - Name: "Jane", Age: 16 → Should be invalid ❌
   - Name: "", Age: 10 → Should be invalid ❌

## Sample Prompt
```
I have a validation function that should return true only when:
1. Name has at least one character AND
2. Age is 18 or above

Currently it's returning true when EITHER condition is met.
Here's the code: [paste code]
What's wrong and how do I fix it?
```

---

## Expected Fix
```pseudo
function isFormValid(name, age):
  return name.length > 0 AND age >= 18  // Changed OR to AND
end
```

---

## Success Criteria
- [ ] Bug correctly identified (OR should be AND)
- [ ] Fix implemented
- [ ] All test cases pass

---

## Key Learning Points
1. **Logical Operators** - AND requires both conditions to be true, OR requires only one
2. **Boolean Logic** - Understanding the difference between AND/OR is critical for validation
3. **Test Cases** - Always test all combinations of conditions to verify logic

## Common Pitfalls
- Confusing AND and OR operators in boolean expressions
- Not testing edge cases (both conditions false, only one true, both true)
- Assuming the code works without testing all scenarios

---

## Next Step
Move to Lesson 2: Null/Undefined Handling
