# Debugging Prompts

## Finding and Fixing Bugs Systematically

These prompts help you leverage AI for debugging, from understanding errors to identifying root causes to implementing fixes.

---

## Error Analysis

### Basic Error Investigation

```
I'm encountering this error:

```
[paste full error message and stack trace]
```

The error occurs when: [describe what triggers the error]

Expected behavior: [what should happen]
Actual behavior: [what actually happens]

Relevant code:
```
[paste relevant code sections]
```

Please:
1. Explain what this error means
2. Identify the likely cause
3. Suggest a fix with explanation
```

### Complex Error Investigation

```
I'm debugging a complex issue. Help me systematically investigate.

## Error Details
```
[error message, stack trace, logs]
```

## Context
- When it happens: [conditions, inputs, timing]
- When it doesn't happen: [working scenarios]
- What changed recently: [deployments, code changes, data]
- Environment: [dev/staging/prod, versions, dependencies]

## What I've Tried
- [diagnostic step 1 and result]
- [diagnostic step 2 and result]

## Relevant Code
```
[code sections]
```

Please:
1. Analyze what we know and form hypotheses (ranked by likelihood)
2. For the top hypothesis, explain the mechanism
3. Suggest diagnostic steps to confirm/refute
4. Propose a fix if the hypothesis is confirmed
```

---

## Root Cause Analysis

### Finding the Root Cause

```
This code has a bug, but I'm not sure where exactly.

## Symptom
[describe the incorrect behavior]

## Code
```
[relevant code]
```

## What I've Verified
- [any debugging already done]

Please trace through the logic and identify the root cause.
Don't just point to the symptom - find the underlying reason.
Explain the chain of events that leads to the bug.
```

### Why Does This Fix Work?

```
I fixed a bug, but I don't fully understand why my fix works.

## The Bug
[describe original behavior]

## Original Code
```
[buggy code]
```

## Fixed Code
```
[working code]
```

Explain:
1. What was wrong with the original code
2. Why my fix addresses the root cause
3. Whether there are any edge cases my fix might miss
4. If there's a better fix I should consider
```

---

## Specific Bug Types

### Null/Undefined Reference

```
I'm getting null/undefined reference errors.

```
[error message]
```

Code:
```
[code where error occurs]
```

Please:
1. Identify what's null/undefined and why
2. Trace back to where it should have been set
3. Explain the failure path
4. Suggest defensive fixes (not just null checks - address root cause)
```

### Race Condition Investigation

```
I suspect a race condition in this code.

## Symptoms
- [intermittent failure description]
- [timing patterns noticed]

## Code
```
[concurrent/async code]
```

Please:
1. Identify potential race conditions
2. Explain the sequence of events that causes the bug
3. Suggest fixes (proper synchronization, not just delays)
4. Point out any other concurrency issues
```

### Memory Leak Investigation

```
Application memory grows over time. Help me find the leak.

## Observations
- [memory growth pattern]
- [when it's most noticeable]

## Suspect Code
```
[code that might leak]
```

Please:
1. Identify potential memory leak sources
2. Explain why each candidate leaks
3. Suggest fixes
4. Recommend diagnostic tools/techniques
```

### Performance Bug

```
This code is slower than expected.

## Performance Observation
- Expected: [target performance]
- Actual: [measured performance]
- Conditions: [data size, frequency, etc.]

## Code
```
[slow code]
```

Please:
1. Analyze algorithmic complexity
2. Identify bottlenecks
3. Explain why each bottleneck matters
4. Suggest optimizations with expected improvements
```

---

## Debugging Strategies

### Generate Debugging Plan

```
I need to debug this issue but I'm not sure where to start.

## The Problem
[describe the bug]

## What I Know
[any information about conditions, timing, etc.]

## The Codebase
[relevant file/module descriptions]

Create a debugging plan:
1. Prioritized list of hypotheses
2. For each hypothesis: diagnostic steps to confirm/refute
3. Suggested logging or instrumentation to add
4. Tools or techniques that might help
```

### Log Analysis

```
I have logs from when the bug occurred. Help me analyze them.

## Expected Behavior
[what should have happened]

## Logs
```
[relevant log entries]
```

## Code (for reference)
```
[related code]
```

Please:
1. Identify anomalies or unexpected patterns
2. Reconstruct what happened
3. Pinpoint where things went wrong
4. Suggest what additional logging would help
```

---

## Fix Verification

### Verify My Fix

```
I think I've fixed a bug. Help me verify.

## Original Bug
[describe the bug]

## Root Cause
[what I determined was wrong]

## My Fix
```
[the fix I implemented]
```

Please evaluate:
1. Does this actually address the root cause?
2. Could it introduce new bugs?
3. Are there edge cases not covered?
4. Is this the cleanest solution?
5. What tests should I write to verify?
```

### Regression Check

```
I'm fixing a bug and want to avoid regressions.

## The Fix
```
[code change]
```

## Related Code
```
[code that might be affected]
```

Please identify:
1. Other code paths that might be affected
2. Edge cases my fix might break
3. Assumptions that might be invalidated
4. Tests I should run or write
```

---

## Debugging Conversation Patterns

### Progressive Investigation

Use this pattern for complex bugs:

```
## Message 1: Initial Report
"Here's the bug: [symptom]. Relevant code: [code]. 
What are the most likely causes?"

## Message 2: Hypothesis Testing  
"The most likely hypothesis is [X]. 
Here's what I found when I checked: [result].
What does this tell us?"

## Message 3: Narrowing Down
"Based on that, it seems like [refined hypothesis].
Here's more context: [additional info].
Can you confirm and suggest the fix?"

## Message 4: Fix Review
"Here's my fix: [fix]. Does this look right? 
What should I test?"
```

### Rubber Duck Pattern

```
I'm going to walk through my debugging process. 
Help me catch flaws in my reasoning.

The bug is: [description]

My thinking:
1. [First observation/hypothesis]
2. [What I checked and found]
3. [Next hypothesis based on that]
4. [What I checked and found]
...
[Continue your reasoning]

My current conclusion: [what you think the cause is]

Do you see any flaws in my reasoning? 
What am I missing?
```

---

## Quick Reference Templates

### Minimal Bug Report

```
Error: [error message]
When: [trigger condition]
Code: [snippet]
Help me fix this.
```

### Complete Bug Analysis Request

```
## Bug Summary
[one sentence]

## To Reproduce
1. [step]
2. [step]

## Expected vs Actual
Expected: [expected]
Actual: [actual]

## Error Output
```
[errors, logs]
```

## Environment
- [language/framework version]
- [relevant dependencies]

## Code
```
[relevant code]
```

## Investigation So Far
[what you've tried]

## Specific Questions
1. [question]
```

---

*Debugging is systematic investigation. These prompts structure that investigation. Adapt them to your specific context.*

