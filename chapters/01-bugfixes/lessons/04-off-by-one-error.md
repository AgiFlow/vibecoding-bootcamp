# Lesson 4: Off-by-One Error

**Time:** 5-10 minutes
**Difficulty:** Beginner

## Learning Objective
Learn to identify and fix off-by-one errors using an AI coding assistant.

## The Bug
Function that gets the last item from an array returns undefined.

---

## Scenario
```pseudo
function getLastItem(arr):
  return arr[arr.length]  // Bug: array is zero-indexed!
end

fruits = ["apple", "banana", "orange"]
print(getLastItem(fruits))
```

**Expected:** "orange"
**Actual:** undefined (or null, depending on language)

### Why It Happens
Arrays are zero-indexed:
- Index 0: "apple"
- Index 1: "banana"
- Index 2: "orange"
- Index 3: undefined (doesn't exist!)

`arr.length` is 3, but the last index is 2!

---

## Your Task
1. Describe the bug to your AI assistant
2. Ask it to explain why it's happening
3. Implement the fix
4. Test with different arrays:
   - `["apple", "banana", "orange"]` → Should return "orange" ✅
   - `[1]` → Should return 1 ✅
   - `[]` → Should return undefined ✅

## Sample Prompt
```
I have a function that should return the last item from an array:
[paste code]

It returns undefined instead of the last item.
What's the issue and how do I fix it?
```

---

## Expected Fix

**Option 1: Subtract 1 from length**
```pseudo
function getLastItem(arr):
  return arr[arr.length - 1]
end
```

**Option 2: Negative indexing**
```pseudo
function getLastItem(arr):
  return arr[-1]
  // Some languages support negative indices (counting from end)
  // Python, Ruby: arr[-1] gets last item
  // If not supported, use Option 1
end
```

**Option 3: Get last element with slice**
```pseudo
function getLastItem(arr):
  lastItems = arr.slice(-1)  // Get last item as array
  return lastItems[0]        // Return the item
end
```

---

## Success Criteria
- [ ] Function returns the last item correctly
- [ ] Works with arrays of different sizes
- [ ] Handles empty arrays gracefully
- [ ] All test cases pass

---

## Key Learning Points
1. **Zero-Indexed Arrays** - Array indices start at 0, not 1. Last index is always `length - 1`
2. **Off-by-One Errors** - Common bug when converting between counts (1-based) and indices (0-based)
3. **Array Length** - Returns the count of items, not the last index position
4. **Negative Indexing** - Some languages support negative indices to count from the end (e.g., -1 for last item)

## Common Pitfalls
- Forgetting arrays are zero-indexed (thinking `length` is the last index)
- Off-by-one errors in loops (using `i <= arr.length` instead of `i < arr.length`)
- Not handling empty arrays (accessing invalid index may return null/undefined or cause error)
- Confusing array length with last valid index

---

## Next Step
Move to Lesson 5: UI Event Handler Bug
