# Refactoring Prompts

## Improving Code Without Changing Behavior

Refactoring is the art of restructuring code to improve quality while preserving functionality. These prompts help you leverage AI for systematic code improvement.

---

## Assessment Prompts

### Refactoring Opportunities

```
Analyze this code for refactoring opportunities.

```
[code]
```

Identify:
1. Code smells (long methods, large classes, duplications, etc.)
2. Design issues (tight coupling, missing abstractions, etc.)
3. Readability problems
4. Maintainability concerns

For each issue:
- Describe the problem
- Explain why it matters
- Suggest a refactoring approach
- Estimate effort (simple/moderate/significant)

Prioritize by impact/effort ratio.
```

### Technical Debt Assessment

```
Assess the technical debt in this code.

```
[code]
```

Identify:
1. **Code debt**: Quick fixes, workarounds, poor structure
2. **Design debt**: Missing abstractions, architectural issues
3. **Test debt**: Missing or inadequate tests
4. **Documentation debt**: Missing or outdated docs

For each item:
- Severity (how much does it hurt us?)
- Risk (what could go wrong?)
- Effort to fix
- Recommendation (fix now, plan for later, accept)
```

---

## Extraction Refactorings

### Extract Method/Function

```
This function is too long. Help me extract parts into smaller functions.

```
[long function]
```

Guidelines:
- Each extracted function should have a single, clear purpose
- Names should describe what, not how
- Consider what parameters each extraction needs
- Maintain readability of the original function

Show me:
1. What to extract and why
2. The extracted functions
3. The refactored original function
```

### Extract Class

```
This class has too many responsibilities. Help me break it apart.

```
[large class]
```

Identify cohesive groups of functionality that could become separate classes.

For each extraction:
- What to extract
- The responsibility of the new class
- How it interacts with the original
- The interface between them

Show the resulting class structure.
```

### Extract Interface

```
I want to create an abstraction for this concrete class.

```
[concrete class]
```

Help me:
1. Identify the public contract that clients depend on
2. Design an interface that captures that contract
3. Consider if multiple implementations might exist
4. Show the interface and how the class implements it
```

---

## Simplification Refactorings

### Simplify Conditionals

```
These conditionals are complex. Help me simplify.

```
[code with complex conditionals]
```

Apply appropriate techniques:
- Decompose conditional (extract to well-named methods)
- Consolidate conditional (combine similar conditions)
- Replace nested conditionals with guard clauses
- Replace conditional with polymorphism (if appropriate)
- Use lookup tables or maps (if appropriate)

Show the before/after with explanation of each change.
```

### Reduce Complexity

```
This function has high cyclomatic complexity. Help me reduce it.

```
[complex function]
```

Strategies to apply:
- Extract helper functions
- Use early returns
- Replace conditionals with strategy pattern
- Break into smaller, focused functions

Show the refactored code with complexity improvements explained.
```

### Remove Duplication

```
I have duplicated code that needs consolidation.

Location 1:
```
[code block 1]
```

Location 2:
```
[code block 2]
```

[Additional locations if any]

Help me:
1. Identify what's truly duplicated vs. coincidentally similar
2. Create a shared abstraction that serves all cases
3. Ensure the abstraction is properly parameterized
4. Show how each location uses the shared code
```

---

## Structural Refactorings

### Improve Class Design

```
Improve the design of this class.

```
[class code]
```

Consider:
- Single Responsibility: Does it do one thing well?
- Cohesion: Do all methods relate to the same purpose?
- Coupling: Is it too dependent on other classes?
- Encapsulation: Is internal state properly protected?
- Interface: Is the public API clean and minimal?

Show the improved class with explanations.
```

### Organize Imports/Dependencies

```
Clean up the imports and dependencies in this file.

```
[code with messy imports]
```

Apply:
1. Remove unused imports
2. Group and order imports properly
3. Identify circular dependencies
4. Suggest if any imports indicate design issues
5. Consider if imports should be injected instead

Show the cleaned-up version.
```

### Improve File/Module Structure

```
This file has grown too large. Help me split it.

```
[large file]
```

Current file does: [brief description]

Help me:
1. Identify natural boundaries for splitting
2. Determine what goes in each new file
3. Handle cross-references between the new files
4. Maintain a clean public API
5. Consider import patterns for consumers
```

---

## Pattern-Based Refactorings

### Apply Design Pattern

```
I think a design pattern could improve this code.

```
[code]
```

The problem: [describe the design issue]

Suggest:
1. Which pattern(s) might apply and why
2. How to implement the pattern in this context
3. The benefits we'd gain
4. Any trade-offs to consider

Show the refactored code using the pattern.
```

### Modernize Legacy Code

```
This code uses outdated patterns. Help me modernize it.

```
[legacy code]
```

Language/Framework: [language and version]

Update to use modern idioms:
- [specific modern features to use]
- Current best practices
- Improved error handling patterns
- Better async patterns (if applicable)

Show the modernized version with explanations.
```

---

## Safe Refactoring

### Verify Behavior Preservation

```
I've refactored this code. Verify I haven't changed behavior.

Original:
```
[original code]
```

Refactored:
```
[refactored code]
```

Please:
1. Identify any behavioral differences
2. Check edge cases
3. Verify error handling is preserved
4. Note any subtle semantic changes
5. Suggest tests that would catch regressions
```

### Plan Incremental Refactoring

```
I need to refactor this code, but I must do it incrementally.

```
[code to refactor]
```

Target state: [what I want it to look like]

Create a plan:
1. Break the refactoring into safe, small steps
2. Each step should leave code working
3. Each step should be independently committable
4. Order steps to minimize risk
5. Note what tests to run at each step
```

### Refactor Without Breaking Tests

```
Help me refactor this code while keeping tests passing.

Code:
```
[code]
```

Tests:
```
[existing tests]
```

Planned refactoring: [what I want to change]

Show me how to:
1. Do the refactoring in steps that keep tests green
2. Update tests if interface changes
3. Maintain test coverage through the refactoring
```

---

## Specific Refactorings

### Rename for Clarity

```
Suggest better names in this code.

```
[code]
```

For each name that should change:
- Current name
- Suggested name
- Why it's better
- Where it's used (so I can update all references)
```

### Improve Error Handling

```
The error handling in this code needs improvement.

```
[code]
```

Current issues I see: [any known problems]

Help me:
1. Identify all error handling gaps
2. Design a consistent error handling strategy
3. Add appropriate error handling
4. Ensure errors surface useful information
5. Handle errors at the right level

Show the improved code.
```

### Add Type Safety

```
Add proper types to this code.

```
[code without types or with weak types]
```

Language: [language]

Please:
1. Add type annotations throughout
2. Replace any `any` types with proper types
3. Create type definitions where needed
4. Handle nullable types properly
5. Make the code compile in strict mode

Show the fully typed version.
```

---

## Quick Refactoring Templates

### Minimal Refactor Request

```
Refactor this for better [readability/performance/maintainability]:
```
[code]
```
```

### With Constraints

```
Refactor this code:
```
[code]
```

Constraints:
- Don't change the public interface
- Must remain backward compatible
- Keep it in one file
- [other constraints]
```

---

*Refactoring improves code health. Small, continuous improvements compound into significant quality gains. These prompts help you refactor systematically and safely.*

