# Lesson 3: Type Error Bug

**Time:** 5-10 minutes
**Difficulty:** Beginner

## Learning Objective
Learn to identify and fix type-related errors using an AI coding assistant.

## The Bug
Function that calculates total price returns wrong results.

---

## Scenario
```pseudo
function calculateTotal(price, quantity):
  return price + quantity  // Bug: string concatenation instead of math!
end

// Usage from form input
price = getFormInput('price')      // "10" (string)
quantity = getFormInput('quantity') // "5" (string)

print(calculateTotal(price, quantity))
```

**Expected:** 10 * 5 = 50
**Actual:** "105" (string concatenation)

---

## Your Task
1. Explain the bug to your AI assistant
2. Ask why it's happening and how to fix it
3. Implement the fix
4. Test with different values:
   - Price: "10", Quantity: "5" → Should return 50 ✅
   - Price: "20", Quantity: "3" → Should return 60 ✅
   - Price: "7.50", Quantity: "2" → Should return 15 ✅

## Sample Prompt
```
I have a function that should multiply price and quantity:
[paste code]

When I get values from HTML form inputs, it returns "105" instead of 50.
The values are "10" and "5". What's wrong?
```

---

## Expected Fix

**Option 1: Convert to numbers**
```pseudo
function calculateTotal(price, quantity):
  return toNumber(price) * toNumber(quantity)
end
```

**Option 2: Explicit type conversion**
```pseudo
function calculateTotal(price, quantity):
  priceNum = convertToNumber(price)
  quantityNum = convertToNumber(quantity)
  return priceNum * quantityNum
end
```

**Option 3: Convert decimal strings**
```pseudo
function calculateTotal(price, quantity):
  return toDecimal(price) * toDecimal(quantity)
  // Use toDecimal/toFloat for values with decimal points
end
```

---

## Success Criteria
- [ ] Function returns number, not string
- [ ] Calculation is correct (50, not "105")
- [ ] Works with form input values
- [ ] Handles decimal numbers correctly

---

## Key Learning Points
1. **Type Coercion** - The `+` operator may concatenate strings if either operand is a string (behavior varies by language)
2. **Form Input Types** - Form input values are typically strings by default, even for numeric inputs
3. **Type Conversion** - Always explicitly convert strings to numbers before mathematical operations
4. **Operator Behavior** - Multiplication operator typically forces numeric conversion, but addition may concatenate strings

## Common Pitfalls
- Using `+` for math without converting strings to numbers first
- Assuming form inputs are numbers (they're usually strings)
- Not handling decimal values properly (use decimal/float conversion instead of integer conversion)
- Forgetting that `"10" + "5"` may equal `"105"` not `15` depending on the language

---

## Next Step
Move to Lesson 4: Off-by-One Error
