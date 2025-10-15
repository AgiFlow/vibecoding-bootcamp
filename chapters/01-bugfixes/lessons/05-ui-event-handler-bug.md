# Lesson 5: UI Event Handler Bug

**Time:** 5-10 minutes
**Difficulty:** Beginner

## Learning Objective
Learn to debug event handler issues in UI components using an AI coding assistant.

## The Bug
Form submits and refreshes the page instead of calling the handler function.

---

## Scenario
```pseudo
function handleSubmit(event):
  print("Form submitted!")
  // API call code here...
end

// UI Form
<form onSubmit="handleSubmit()">
  <input type="text" name="username" />
  <button type="submit">Submit</button>
</form>
```

**Expected:** Logs message, stays on page, calls API
**Actual:** Page refreshes immediately, API call never happens

---

## Your Task
1. Describe the problem to your AI assistant
2. Ask why the page is refreshing
3. Implement the fix
4. Test the form submission:
   - Submit form → Should log message without page refresh ✅
   - Handler executes → Should see output ✅
   - Page stays loaded → No reload ✅

## Sample Prompt
```
I have a form with a submit handler:
[paste code]

When I click submit, the page refreshes and my handler doesn't run.
How do I prevent the page refresh and let my handler execute?
```

---

## Expected Fix

**Option 1: Prevent default behavior in handler**
```pseudo
function handleSubmit(event):
  event.preventDefault()  // Add this to stop default form submission!
  print("Form submitted!")
  // API call code here...
end

// UI Form - pass event parameter
<form onSubmit="handleSubmit(event)">
  <input type="text" name="username" />
  <button type="submit">Submit</button>
</form>
```

**Option 2: Return false from handler**
```pseudo
function handleSubmit(event):
  print("Form submitted!")
  return false  // Returning false prevents default behavior
end

// UI Form - use return with handler call
<form onSubmit="return handleSubmit(event)">
  <input type="text" name="username" />
  <button type="submit">Submit</button>
</form>
```

**Option 3: Attach event listener programmatically**
```pseudo
// Find the form element
formElement = findElement('form')

// Attach submit event handler
formElement.on('submit', function(event):
  event.preventDefault()
  print("Form submitted!")
  // API call code here...
end)
```

---

## Success Criteria
- [ ] Form doesn't refresh the page
- [ ] Handler function executes
- [ ] Log message appears
- [ ] API call would execute (if added)

## Key Learning Points
1. **Default Form Behavior** - Forms have built-in submit behavior that typically refreshes/navigates the page
2. **Event Prevention** - Use `event.preventDefault()` to stop default behavior and handle submission yourself
3. **Event Parameter** - Always pass the event object to handlers to access prevention methods
4. **Programmatic Listeners** - Modern approach is to attach event listeners in code rather than inline HTML

## Common Pitfalls
- Forgetting to pass the `event` parameter to the handler function
- Not calling `preventDefault()` early enough in the handler
- Using inline event handlers without understanding event flow
- Assuming the handler will run before page refresh (it won't without prevention)
- Confusing form validation (which can also prevent submission) with default behavior

---

## Next Step
Congratulations! You've completed the basic bug fixing lessons. Review the chapter README for next steps.
