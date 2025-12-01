# Cursor Tips and Tricks

## Advanced Techniques for Power Users

These tips go beyond the basics to help you get maximum productivity from Cursor. Organized from quick wins to advanced techniques.

---

## Quick Wins

### Keyboard Shortcuts to Memorize

| Action | Shortcut | Why It Matters |
|--------|----------|----------------|
| Open AI chat | `Cmd/Ctrl + L` | Your primary interaction point |
| Inline edit | `Cmd/Ctrl + K` | Edit code without context switching |
| Accept suggestion | `Tab` | Faster than clicking |
| Reject suggestion | `Esc` | Clean exit from suggestions |
| Toggle AI panel | `Cmd/Ctrl + Shift + L` | More screen space when needed |
| Add file to context | `@filename` in chat | Quick context building |
| Reference symbol | `@symbol` in chat | Precise code references |

### The @ Symbol is Your Friend

Master the `@` references in chat:
- `@filename` - Add entire file to context
- `@foldername` - Add folder contents
- `@codebase` - Search entire codebase
- `@docs` - Reference documentation
- `@git` - Reference git history
- `@web` - Search the web

Combine them: "Using @auth.ts patterns, update @user-service.ts"

---

## Effective Prompting in Cursor

### Be Specific About Location

```
# Less effective
"Add error handling"

# More effective  
"Add try-catch error handling to the fetchUser function in @services/user.ts, 
logging errors to console and returning null on failure"
```

### Include Acceptance Criteria

```
"Refactor the payment processing to:
1. Extract card validation into a separate function
2. Add TypeScript types for all parameters
3. Handle the case where card is expired
4. Keep the existing function signature for backwards compatibility"
```

### Reference Existing Patterns

```
"Add a new API endpoint for products following the same pattern as 
@routes/users.ts, including validation middleware and error handling"
```

---

## Context Management

### Strategic File Selection

- **Add files proactively**: Don't wait for Cursor to find them
- **Include interfaces/types**: Helps Cursor understand your data structures
- **Add test files**: Shows expected behavior patterns
- **Include config files**: Reveals project conventions

### When Context Gets Polluted

If Cursor seems confused or responses are off-target:
1. Start a new chat (`Cmd/Ctrl + L`, then new conversation)
2. Be more selective about which files to include
3. Explicitly state what you're NOT trying to do
4. Clear any system prompts that might conflict

### Large Codebase Strategy

For big projects, be surgical:
```
"Focus only on the authentication module (@auth/).
Don't modify or reference anything in @billing/ or @notifications/"
```

---

## Inline Editing Mastery

### Cmd/Ctrl + K Tips

1. **Select before invoking**: Highlight code first for targeted edits
2. **Write full instructions**: "Convert to async/await with error handling"
3. **Reference nearby code**: "Make this match the style of the function above"
4. **Specify what NOT to change**: "Keep the function signature the same"

### Multi-Cursor + AI

1. Select multiple similar items (Cmd/Ctrl + D)
2. Use Cmd/Ctrl + K with all cursors active
3. AI applies consistent changes to all selections

### Iterative Refinement

```
First pass: "Extract this into a function"
Second pass: "Add TypeScript types"  
Third pass: "Add JSDoc comments"
```

Small, focused edits are more reliable than large rewrites.

---

## Working with Rules

### Project-Specific Rules

Create `.cursorrules` in project root:
```
This is a Node.js TypeScript project using:
- Express for HTTP
- Prisma for database
- Jest for testing

Conventions:
- Use async/await, never callbacks
- Error messages should be user-friendly
- All functions need JSDoc comments
- Tests go in __tests__ folders alongside source
```

### Per-Directory Rules

Different rules for different parts of your codebase:
```
/frontend/.cursorrules  - React conventions
/backend/.cursorrules   - API conventions
/scripts/.cursorrules   - Utility script conventions
```

### Temporary Rules in Chat

For one-off requirements:
```
"For this task only: 
- Use verbose variable names
- Add comments explaining each step
- Don't use any external dependencies"
```

---

## Debugging with Cursor

### Error Message Strategy

```
"I'm getting this error:

[paste full error with stack trace]

The relevant code is in @problematic-file.ts

Help me understand:
1. What's causing this
2. How to fix it
3. How to prevent it in the future"
```

### Incremental Debugging

1. Ask Cursor to add strategic console.logs
2. Run the code
3. Share output with Cursor
4. Iterate until fixed

### Test-Driven Debugging

```
"This test is failing:

[paste failing test]

The implementation is in @src/utils.ts

Help me either:
1. Fix the implementation to pass the test, OR
2. Explain why the test expectation is wrong"
```

---

## Code Generation Patterns

### Scaffold, Then Fill

```
Step 1: "Create the basic structure for a REST API controller 
        with CRUD operations, just function signatures"

Step 2: "Implement the create function with validation"

Step 3: "Implement the read function with pagination"
```

### Reference Implementation First

```
"Here's a working example of what I want:

[paste example]

Create a similar implementation for [new use case], 
adapting [specific parts] while keeping [other parts] the same"
```

### Test-First Generation

```
"I want a function that does X. 

First, write the tests that define the expected behavior.
Then, implement the function to pass those tests."
```

---

## Review and Refactoring

### Code Review Prompt

```
"Review this code for:
1. Bugs or logic errors
2. Performance issues
3. Security concerns
4. Readability improvements
5. Missing edge cases

Prioritize issues by severity. Don't suggest stylistic changes."
```

### Safe Refactoring

```
"Refactor this function to [goal].

Requirements:
- Keep the same function signature (public API unchanged)
- Ensure all existing tests still pass
- Don't change behavior, only structure
- Add comments where logic is non-obvious"
```

### Incremental Modernization

```
"Update this file one pattern at a time:
1. First pass: Replace var with const/let
2. Second pass: Convert callbacks to async/await
3. Third pass: Add TypeScript types

Show me each pass separately for review."
```

---

## Terminal Integration

### Command Generation

```
"What's the terminal command to:
[describe what you want to do]

For context: I'm on [OS], using [shell], in a [project type] project"
```

### Script Writing

```
"Write a bash script that:
1. [step 1]
2. [step 2]
3. [step 3]

Include error handling and helpful output messages."
```

---

## Common Pitfalls

### Pitfall: Too Much Context

**Problem**: Adding entire codebase to every chat
**Solution**: Be selective, add only relevant files

### Pitfall: Vague Instructions

**Problem**: "Make this better"
**Solution**: "Improve readability by extracting the validation logic into a separate function with a descriptive name"

### Pitfall: Not Reviewing Changes

**Problem**: Accepting all suggestions blindly
**Solution**: Review diffs, especially for security-sensitive code

### Pitfall: Fighting the AI

**Problem**: Repeatedly asking for the same thing differently
**Solution**: Start fresh, provide more context, or try a different approach

### Pitfall: Overcomplicating Prompts

**Problem**: Writing essay-length prompts
**Solution**: Clear, concise instructions often work better

---

## Productivity Workflows

### Morning Startup

1. Open project
2. Check git status (Cursor can help: "summarize recent changes in @git")
3. Review TODOs ("find all TODO comments in @src/")
4. Start first task with clear context

### Focused Development Session

1. Define what you're building
2. Add relevant files to context
3. Generate scaffolding
4. Fill in implementation iteratively
5. Generate tests
6. Review and refine

### End of Day

1. "Summarize the changes I made today" (with git context)
2. "Identify any incomplete work or TODOs I added"
3. "Suggest what to tackle tomorrow based on current state"

---

## Advanced: Custom Workflows

### Create Your Own Commands

Frequently used prompts can become templates:

```
# Save as a snippet or note:

"SECURITY REVIEW: Check @[FILE] for:
- SQL injection vulnerabilities
- XSS vulnerabilities  
- Authentication/authorization issues
- Sensitive data exposure
- Input validation gaps

Rate severity: Critical/High/Medium/Low"
```

### Chained Operations

```
"Let's do this in steps:

1. First, show me all the places where [pattern] is used
2. Wait for my confirmation
3. Then update each location to [new pattern]
4. Finally, update any tests that need to change"
```

---

*The best Cursor users aren't those who know every feature. They're the ones who've developed muscle memory for the patterns that work for them. Experiment, find what clicks, and build your own workflow.*

