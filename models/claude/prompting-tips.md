# Prompting Tips for Claude

## Getting the Most From Anthropic's Models

Claude's training emphasizes helpfulness, accuracy, and thoughtful engagement. Understanding these priorities helps you craft prompts that align with how the model works best.

---

## General Principles

### Be Explicit About Your Needs

Claude follows instructions closely. The more specific your requirements, the more precisely it delivers.

**Vague (unpredictable results):**
```
Help me with this code.
```

**Specific (consistent results):**
```
Review this Python function for performance issues. Specifically:
1. Identify any O(n^2) or worse operations
2. Check for unnecessary object creation in loops
3. Look for opportunities to use built-in functions instead of manual iteration

For each issue found, explain the performance impact and show the improved code.
```

### Leverage the Context Window

Claude handles 200K tokens well. Don't artificially truncate context if the full information is helpful:

```
I'm providing my entire configuration file for context. While I only need 
help with the database section (lines 45-80), having the full file helps 
you understand our conventions and any cross-references.

[full configuration file]

Please focus on optimizing the database connection pool settings for 
high-traffic scenarios (10K concurrent connections).
```

### Request the Format You Need

Claude respects formatting instructions reliably:

```
Provide your response in exactly this format:

## Summary
[2-3 sentences]

## Issues Found
- **Issue 1**: [description]
  - Location: [specific location]
  - Severity: [high/medium/low]
  - Fix: [code block with fix]

## Additional Recommendations
[bullet list of improvements that aren't bugs]
```

---

## Role-Based Prompting

Claude adapts effectively to assigned roles. Choose roles that provide useful framing for your task.

### Technical Expert Roles

```
You are a database performance specialist with deep expertise in PostgreSQL 
internals. You understand query planning, index selection, and the trade-offs 
between different optimization approaches.

Analyze this query and explain why it's performing poorly on a table with 
50M rows. Consider index usage, join strategies, and any hidden full table scans.
```

### Review/Critic Roles

```
You are a security auditor reviewing code before deployment. You're known for 
being thorough but practical, focusing on issues that could actually be 
exploited rather than theoretical concerns.

Review this authentication flow. Assume the attacker has:
- Network access to intercept traffic
- Knowledge of common vulnerabilities
- Time and motivation to find weaknesses

Prioritize findings by exploitability, not just severity category.
```

### Teaching Roles

```
You are a patient mentor helping a junior developer understand this codebase. 
Explain concepts clearly without being condescending. Use analogies when 
helpful. Assume they're intelligent but lack specific domain experience.

Walk through how this event sourcing implementation works. Start with the 
high-level architecture, then drill into each component.
```

---

## Chain-of-Thought Strategies

Claude responds well to explicit reasoning requests, especially for complex problems.

### Structured Reasoning

```
Solve this problem using explicit steps:

1. UNDERSTAND: Restate the problem in your own words
2. IDENTIFY: List the key constraints and requirements
3. EXPLORE: Consider multiple approaches
4. EVALUATE: Analyze trade-offs between approaches
5. DECIDE: Select the best approach and explain why
6. IMPLEMENT: Provide the solution
7. VERIFY: Check your solution against requirements

Problem: [complex problem description]
```

### Think-Then-Answer

```
Before answering, think through the problem step by step in a <thinking> block. 
Consider edge cases, potential issues, and alternative interpretations.

Then provide your final answer clearly labeled.

Question: [question that benefits from careful reasoning]
```

### Explicit Reasoning Triggers

Simple additions that improve reasoning quality:

- "Think through this step by step before answering"
- "Consider potential issues with each approach before recommending one"
- "What are the tradeoffs I should consider?"
- "Walk me through your reasoning"

---

## Handling Complex Tasks

### Decomposition Requests

```
This is a complex task. Before implementing, break it down:

1. List all subtasks required
2. Identify dependencies between subtasks
3. Note any ambiguities that need clarification
4. Propose an order of operations

Then we'll tackle each subtask one by one.

Task: [complex task description]
```

### Iterative Refinement

```
Let's build this incrementally:

Phase 1: Provide a basic working implementation (minimal features)
Phase 2: I'll review and we'll add error handling
Phase 3: We'll add edge case handling
Phase 4: We'll optimize if needed

Start with Phase 1. Keep it simple and correct.
```

### Scope Management

```
For this first response, focus ONLY on [specific aspect]. 

I know there are other considerations (like X, Y, Z), but let's handle 
this part thoroughly first. We'll address those in follow-up messages.
```

---

## Code-Specific Strategies

### Code Review Requests

```
Review this code with the following priorities (in order):

1. CORRECTNESS: Does it produce correct results for all inputs?
2. SECURITY: Any vulnerabilities or unsafe patterns?
3. PERFORMANCE: Any obvious inefficiencies?
4. STYLE: Major style issues (skip minor nitpicks)

Format each issue as:
- What: [description]
- Where: [line number or code snippet]
- Why: [explanation of impact]
- Fix: [corrected code]

Only flag issues that matter. Skip cosmetic suggestions.

[code to review]
```

### Code Generation Requests

```
Implement [functionality] in [language].

Requirements:
- [specific requirement 1]
- [specific requirement 2]

Constraints:
- No external dependencies beyond [list]
- Must work with [version/environment]
- Follow [style guide/conventions]

Include:
- Type hints/annotations
- Docstrings/comments for public interfaces
- Error handling for [specific conditions]

Do NOT include:
- Tests (I'll write those separately)
- Example usage (unless complex)
```

### Debugging Assistance

```
I'm debugging an issue where [description of symptom].

Expected behavior: [what should happen]
Actual behavior: [what actually happens]
When it occurs: [conditions/inputs that trigger it]

Here's the relevant code:
[code]

Here's the error/output I'm seeing:
[error message or unexpected output]

I've already tried:
- [thing you tried 1]
- [thing you tried 2]

Help me identify the root cause, not just a workaround.
```

---

## Managing Output Length

### When You Want Brevity

```
Provide a brief answer (3-4 sentences maximum). If more detail would be 
helpful, mention what I could ask about for elaboration.
```

```
One paragraph only. Be direct.
```

```
Answer in bullet points. Maximum 5 bullets.
```

### When You Want Thoroughness

```
This is an important decision. Please provide a comprehensive analysis 
covering all relevant factors. Don't worry about length; thoroughness 
is more valuable here than brevity.
```

```
I need a complete implementation including:
- All edge case handling
- Comprehensive error messages
- Documentation for each public function
- Inline comments explaining complex logic
```

---

## Handling Uncertainty

### Requesting Confidence Indicators

```
For any factual claims in your response, indicate your confidence:
- [HIGH] - Well-established, very likely accurate
- [MEDIUM] - Reasonable inference, should verify
- [LOW] - Uncertain, definitely verify before using
- [UNKNOWN] - Outside my knowledge, cannot assess
```

### Requesting Caveats

```
After your answer, include a "Caveats" section listing:
- Assumptions you made
- Cases where this advice might not apply
- Things I should verify independently
```

### Requesting Alternatives

```
Provide your recommendation, but also describe one alternative approach 
with its pros/cons. This helps me understand the decision space better.
```

---

## Common Patterns That Work Well

### The "Before You Begin" Pattern

```
Before providing your solution:
1. Confirm you understand what I'm asking
2. Ask any clarifying questions
3. Identify any assumptions you'll need to make

Then proceed with the solution.
```

### The "Show Your Work" Pattern

```
I want to understand the reasoning, not just the answer.

For each decision or recommendation:
- State what you decided
- Explain why (what factors led to this)
- Note what alternatives you considered
```

### The "Prioritize" Pattern

```
You may find multiple issues/suggestions. Prioritize them:

[CRITICAL] - Must fix before deployment
[IMPORTANT] - Should fix soon
[NICE-TO-HAVE] - Worth considering but not urgent

Focus your detailed explanation on the critical and important items.
```

### The "In Context Of" Pattern

```
Answer this question in the context of:
- A startup with limited engineering resources
- A system that prioritizes availability over consistency
- Users who are technical but not expert in this domain

Given these constraints, what would you recommend?
```

---

## Anti-Patterns to Avoid

### Over-Constraining

**Too rigid:**
```
You must answer in exactly 47 words. Use no adjectives. Start every 
sentence with a verb. Include exactly 3 examples.
```

**Better:**
```
Be concise. Use clear, direct language. Include examples where helpful.
```

### Conflicting Instructions

**Problematic:**
```
Be thorough and comprehensive. Also be brief.
```

**Better:**
```
Be thorough on technical details, but keep explanations concise. 
Depth of analysis matters more than length of explanation.
```

### Assuming Shared Context

**Missing context:**
```
Fix the bug we discussed.
```

**Self-contained:**
```
I'm working on the user authentication bug where sessions aren't 
persisting after server restart. The issue is in session-handler.js.

[relevant code]

Can you identify why sessions are being lost?
```

---

*Claude is a precise tool. The clearer your instructions, the more precisely it delivers. Invest time in prompt clarity and it pays back in output quality.*

