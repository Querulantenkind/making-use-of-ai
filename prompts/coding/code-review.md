# Code Review Prompts

## Systematic Code Quality Assessment

These prompts help you use AI for thorough code review, whether reviewing your own code before submission or analyzing others' code.

---

## Comprehensive Review

### Full Code Review

```
Review this code thoroughly.

```
[paste code]
```

Evaluate:
1. **Correctness**: Logic errors, edge cases, off-by-one errors
2. **Security**: Injection, validation, authentication, data exposure
3. **Performance**: Complexity, unnecessary operations, resource leaks
4. **Maintainability**: Readability, naming, structure, documentation
5. **Style**: Consistency, idioms, best practices

For each issue found:
- Severity (critical/major/minor)
- Location (line or section)
- Problem description
- Suggested fix

End with a summary assessment.
```

### Pre-Commit Review

```
I'm about to commit this code. Review it as a careful colleague would.

Context:
- Purpose: [what this code does]
- PR/Change description: [brief description]

```
[code to review]
```

Focus on:
- Obvious bugs I might have missed
- Security issues
- Anything that will cause problems in production
- Maintainability concerns for future developers

Skip: minor style issues, subjective preferences
```

---

## Focused Reviews

### Security Review

```
Perform a security-focused code review.

```
[code]
```

Check for:
1. Input validation and sanitization
2. SQL injection / command injection
3. XSS vulnerabilities
4. Authentication/authorization flaws
5. Sensitive data exposure
6. Insecure cryptography
7. CSRF vulnerabilities
8. Path traversal
9. Deserialization issues
10. Logging sensitive data

For each vulnerability:
- OWASP category (if applicable)
- Attack vector (how could this be exploited?)
- Risk level
- Remediation

Assume the attacker knows our codebase.
```

### Performance Review

```
Review this code for performance.

```
[code]
```

Context:
- Expected data volume: [size/frequency]
- Performance requirements: [latency, throughput]
- Current issues (if any): [observed problems]

Analyze:
1. Time complexity of algorithms
2. Space complexity and memory usage
3. Database/IO operations
4. Unnecessary computations
5. Caching opportunities
6. Resource lifecycle (connections, handles)

For each issue:
- Current complexity/behavior
- Impact at scale
- Suggested optimization
- Trade-offs of the optimization
```

### Maintainability Review

```
Review this code for maintainability.

```
[code]
```

Evaluate:
1. Readability
   - Naming clarity
   - Function/method length
   - Cognitive complexity

2. Structure
   - Single responsibility
   - Appropriate abstraction level
   - Dependency management

3. Documentation
   - Are complex parts explained?
   - Are public APIs documented?
   - Are edge cases noted?

4. Testability
   - Can this be unit tested?
   - Are there hidden dependencies?

5. Future-proofing
   - How hard is this to modify?
   - What would break if requirements change?

Suggest specific improvements with examples.
```

---

## Comparative Reviews

### Before/After Review

```
I've refactored some code. Compare before and after.

Before:
```
[original code]
```

After:
```
[refactored code]
```

Evaluate:
1. Is functionality preserved? (Any behavioral changes?)
2. Is it actually better? (Which improvements succeeded?)
3. Any regressions? (Anything worse than before?)
4. Completeness (Anything missed in the refactoring?)
5. Further improvements possible?
```

### Implementation Comparison

```
I have two implementations of the same functionality. 
Help me choose.

Implementation A:
```
[code A]
```

Implementation B:
```
[code B]
```

Compare on:
- Correctness
- Performance
- Readability
- Maintainability
- Testability
- Error handling

Recommend which to use, with reasoning.
```

---

## Specialized Reviews

### API Review

```
Review this API design.

```
[API code/specification]
```

Evaluate:
1. RESTful conventions (or stated alternative)
2. URL structure and naming
3. HTTP methods and status codes
4. Request/response formats
5. Error handling and messages
6. Versioning approach
7. Authentication/authorization
8. Rate limiting considerations
9. Documentation completeness

Suggest improvements for each issue.
```

### Database Code Review

```
Review this database-related code.

```
[code with database operations]
```

Schema (for context):
```
[relevant schema if available]
```

Check for:
1. SQL injection vulnerabilities
2. N+1 query problems
3. Missing indexes (based on queries)
4. Transaction handling
5. Connection management
6. Error handling for DB operations
7. Data integrity concerns
8. Migration safety
```

### Test Code Review

```
Review these tests.

```
[test code]
```

Code under test:
```
[the code being tested]
```

Evaluate:
1. Coverage: What's tested? What's missing?
2. Quality: Do tests actually verify correct behavior?
3. Edge cases: Are boundaries and errors tested?
4. Isolation: Are tests independent?
5. Readability: Can you understand what's being tested?
6. Maintainability: Will tests break for wrong reasons?
7. Performance: Are tests fast enough?

Suggest additional tests that should be written.
```

---

## Review for Learning

### Explain This Code

```
I'm reviewing code I didn't write. Help me understand it.

```
[code]
```

Please explain:
1. What does this code do? (high-level purpose)
2. How does it work? (step through the logic)
3. Why is it written this way? (design decisions)
4. What are the tricky parts? (subtle logic to be careful of)
5. What would I need to understand to modify it safely?
```

### Why Is This Wrong?

```
I was told this code has issues, but I don't see them.

```
[code]
```

Help me understand:
1. What problems exist?
2. Why are they problems?
3. What could go wrong in practice?
4. How would you fix each issue?

I want to learn, so explain the reasoning.
```

---

## Review Checklists

### Generate Custom Checklist

```
Generate a code review checklist for [language/framework].

Our project specifics:
- Type: [web app, API, library, etc.]
- Key concerns: [security, performance, etc.]
- Common issues we've had: [past problems]

Create a checklist covering:
1. Language-specific issues
2. Framework-specific concerns
3. Our custom requirements
4. General best practices

Format as a checkbox list I can use.
```

### Self-Review Before PR

```
I'm about to submit this PR. Give me a self-review checklist.

```
[diff or code changes]
```

Generate a checklist specific to these changes, covering:
- Things I should double-check
- Tests I should verify pass
- Edge cases specific to this code
- Documentation I might need to update
- Things reviewers will likely ask about
```

---

## Review Response Formats

### Structured Review Output

Request reviews in a consistent format:

```
Use this format for your review:

## Summary
[Overall assessment in 2-3 sentences]

## Critical Issues
[Must fix before merge]

## Important Issues  
[Should fix, but not blocking]

## Suggestions
[Nice to have improvements]

## Questions
[Things to clarify with the author]

## Positive Notes
[What's done well]
```

### Inline Comments Format

```
Provide review comments as if inline in the code.

Format:
[Line/Section]: [Comment type: issue|suggestion|question|praise]
[Your comment]

Example:
Line 42-45: issue
This loop has O(n^2) complexity because [reason].
Consider [alternative approach].
```

---

*Code review is about quality, not criticism. These prompts help you find issues systematically while maintaining constructive focus.*

