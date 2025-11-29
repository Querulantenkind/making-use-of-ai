# Advanced Prompting Strategies

## Beyond the Fundamentals

You've mastered the basics: role assignment, few-shot learning, chain-of-thought, structured outputs, and constraints. Now we enter territory where prompting becomes both art and engineering.

These techniques address complex scenarios: tasks that resist straightforward prompting, situations requiring maximum reliability, and problems that push against model limitations.

---

## Meta-Prompting and Self-Reflection

### Prompting About Prompting

Meta-prompting uses the model to improve its own outputs through reflection.

#### Self-Critique Pattern

```
[First, get an initial response to your task]

Now, critically evaluate your previous response:
1. What assumptions did you make that might be wrong?
2. What edge cases did you not consider?
3. What would a skeptical expert find problematic?
4. Rate your confidence (1-10) and explain why.

Based on this critique, provide an improved response.
```

This works because models can often identify flaws in their own outputs when explicitly asked to look for them.

#### Verification Prompt

```
You provided the following solution:
[previous response]

Before I use this, verify it by:
1. Trace through the logic with a concrete example
2. Identify any potential failure modes
3. Check for unstated assumptions
4. Confirm the solution actually addresses the original problem

If you find issues, provide corrections.
```

#### Pre-Mortem Analysis

```
Before implementing, let's do a pre-mortem. Assume this solution has 
failed in production. What are the most likely reasons for failure?

Original solution:
[solution]

Identify:
1. Technical failure modes
2. Edge cases that could cause problems
3. Assumptions that might not hold
4. Integration issues with existing systems

Then recommend preventive measures for each identified risk.
```

---

## Adversarial Prompting for Robustness

### Stress-Testing Outputs

When reliability matters, deliberately challenge the model's outputs.

#### Devil's Advocate Pattern

```
You've proposed [approach]. Now argue against it:

1. What are the strongest objections to this approach?
2. What alternatives might be better and why?
3. What would a critic point to as the fatal flaw?

After presenting objections, either defend your original approach 
or revise it based on valid criticisms.
```

#### Edge Case Hunting

```
For the function you've written, systematically identify inputs that 
could cause unexpected behavior:

1. Boundary values (0, -1, MAX_INT, empty strings, null)
2. Type edge cases (NaN, Infinity, undefined)
3. Concurrency scenarios (if applicable)
4. Resource constraints (very large inputs, deep recursion)
5. Encoding issues (unicode, special characters)

For each category, provide specific test cases and expected behavior.
```

#### Failure Mode Analysis

```
This system needs to be highly reliable. Analyze failure modes:

COMPONENT FAILURES:
- What happens if [dependency] is unavailable?
- How does the system behave under [resource constraint]?
- What's the impact of [data corruption scenario]?

CASCADING FAILURES:
- How could a failure in [component A] affect [component B]?
- What are the blast radius boundaries?

RECOVERY:
- How does the system recover from each failure mode?
- What data could be lost? What would be corrupted?

Provide concrete scenarios, not abstract possibilities.
```

---

## Prompt Chaining and Orchestration

### Building Complex Workflows

For tasks too complex for single prompts, chain multiple prompts together.

#### Sequential Refinement Chain

```
PROMPT 1: Generate initial draft
"Write a first draft of [content]. Focus on getting the main ideas 
down without worrying about polish."

PROMPT 2: Structural review
"Review this draft for structure and flow. Identify:
- Missing sections
- Illogical ordering
- Redundancies
Provide a restructured outline."

PROMPT 3: Apply restructuring
"Rewrite the draft following this improved structure: [outline]"

PROMPT 4: Language polish
"Edit this text for clarity and concision. Remove unnecessary words.
Improve sentence variety. Maintain technical accuracy."

PROMPT 5: Final review
"Perform a final review checking for:
- Factual accuracy
- Consistency
- Completeness relative to [original requirements]"
```

#### Decomposition Chain

```
PROMPT 1: Break down the problem
"I need to [complex task]. Break this into independent subtasks 
that can be addressed separately."

PROMPT 2-N: Solve each subtask
"Focusing only on [subtask N]: [specific prompt for that subtask]"

PROMPT FINAL: Integration
"Here are the solutions to each subtask:
[subtask solutions]
Integrate these into a cohesive final solution, resolving any 
conflicts or inconsistencies."
```

#### Parallel Processing Pattern

For tasks with independent components, process in parallel then merge:

```
# Run simultaneously:
PROMPT A: "Analyze [input] for security vulnerabilities"
PROMPT B: "Analyze [input] for performance issues"  
PROMPT C: "Analyze [input] for maintainability"

# Then merge:
PROMPT D: "Given these three analyses:
[A results]
[B results]
[C results]

Create a prioritized action plan that addresses the most critical 
issues first, noting where improvements in one area might affect 
another."
```

---

## Working With Model Limitations

### Handling Context Window Limits

When content exceeds context windows, strategic approaches help.

#### Chunking Strategy

```
I have a large document that exceeds your context window. I'll provide 
it in sections. For each section:

1. Summarize key points
2. Note any references to other sections
3. Flag items that need full-document context to resolve

After all sections, I'll ask you to synthesize.

SECTION 1 of N:
[content]
```

#### Progressive Summarization

```
Process this content in layers:

LAYER 1: For each paragraph, extract the core claim or fact (one line)
LAYER 2: Group related claims into themes
LAYER 3: For each theme, provide a synthesized summary
LAYER 4: Create an executive summary from the theme summaries

This compression preserves essential information while fitting in context.
```

### Handling Knowledge Cutoffs

Models have training cutoffs. Work around them strategically.

```
Your knowledge has a training cutoff. For this task:

1. Provide your best response based on knowledge up to your cutoff
2. Clearly mark any information that might be outdated
3. Identify what I should verify against current sources
4. Suggest specific things to search for recent updates

[task]
```

### Handling Uncertainty

```
For this analysis, explicitly flag your confidence levels:

Use these markers:
[HIGH CONFIDENCE] - Well-established facts, core patterns
[MEDIUM CONFIDENCE] - Reasonable inferences, common patterns
[LOW CONFIDENCE] - Speculation, edge cases, uncertain areas
[VERIFY] - Claims that should be independently confirmed

[analysis task]
```

---

## Custom Instruction Optimization

### Systematic Prompt Development

When you need a prompt for repeated use, develop it systematically.

#### A/B Testing Approach

```
Iteration 1:
- Create initial prompt version
- Run against 5-10 test cases
- Document failure modes

Iteration 2:
- Modify prompt to address failures
- Retest all cases
- Document improvements and regressions

Iteration 3:
- Refine based on patterns
- Test edge cases specifically
- Finalize prompt

Document: What works, what doesn't, and why.
```

#### Prompt Components to Optimize

```
For critical prompts, tune each component:

ROLE: Test different expertise levels and perspectives
CONTEXT: Vary how much background information to include
INSTRUCTIONS: Try different phrasings of the same task
FORMAT: Experiment with output structure requirements
EXAMPLES: Test 0, 1, 3, and 5 few-shot examples
CONSTRAINTS: Add/remove constraints and observe impact
```

### Instruction Clarity Testing

```
Test your prompt's clarity by asking:

"Before responding, restate my request in your own words to confirm 
you understand what I'm asking for. Include:
- The main task
- Any constraints or requirements
- The expected output format

Then proceed with your response."

If the restatement misses key points, your prompt needs clarification.
```

---

## Specialized Patterns

### The Socratic Pattern

Instead of asking for answers, ask for questions:

```
I'm designing [system]. Instead of suggesting a design, ask me the 
questions I should be asking myself. Probe my assumptions. Help me 
discover gaps in my thinking.

Focus questions on:
- Requirements I might not have considered
- Trade-offs I'll need to navigate
- Potential failure modes
- Scalability considerations
```

### The Rubber Duck Pattern

```
I need to think through [problem]. Act as my rubber duck. 

As I explain my thinking, ask clarifying questions that help me 
articulate my reasoning more clearly. Don't solve the problem for 
me; help me solve it myself by:
- Pointing out logical gaps
- Asking "what about..." questions
- Requesting clarification on vague statements
- Noting unstated assumptions

[Start explaining problem]
```

### The Steelman Pattern

```
I'm considering [controversial approach]. Before I decide, steelman 
this approach:

1. Present the strongest possible case for this approach
2. Identify scenarios where it's clearly the right choice
3. Address common objections with substantive responses
4. Explain what someone choosing this approach must value

Then do the same for the alternative: [alternative approach]

Present both cases fairly, as if arguing for each position.
```

### The Inversion Pattern

```
Instead of asking how to achieve [goal], let's invert:

1. How would I guarantee failure at [goal]?
2. What are all the ways this could go wrong?
3. What mistakes do people commonly make?
4. What anti-patterns should I avoid?

Now, derive the path to success by inverting these failure modes.
```

---

## Prompt Templates for Complex Tasks

### Architectural Decision Record

```
Help me create an ADR (Architectural Decision Record) for [decision].

## Title
[Generate a descriptive title]

## Status
[Draft/Proposed/Accepted/Deprecated/Superseded]

## Context
[What is the issue that we're seeing that is motivating this decision?]
Questions to answer:
- What is the current situation?
- What problems does it create?
- What constraints exist?

## Decision
[What is the change that we're proposing and/or doing?]

## Alternatives Considered
[For each alternative:]
- Description
- Pros
- Cons
- Why it wasn't chosen

## Consequences
[What becomes easier or harder because of this decision?]
- Positive consequences
- Negative consequences
- Neutral observations

## References
[Links to related decisions, documentation, discussions]
```

### Comprehensive Code Review

```
Perform a comprehensive code review with the following structure:

## Summary
[One paragraph overall assessment]

## Correctness
- [ ] Logic is sound
- [ ] Edge cases handled
- [ ] Error handling appropriate
[Specific issues found]

## Security
- [ ] Input validation present
- [ ] No sensitive data exposure
- [ ] Authentication/authorization correct
[Specific concerns]

## Performance
- [ ] Efficient algorithms
- [ ] Resource management
- [ ] No obvious bottlenecks
[Specific observations]

## Maintainability
- [ ] Clear naming
- [ ] Appropriate abstraction
- [ ] Adequate documentation
[Specific suggestions]

## Testing
- [ ] Testable design
- [ ] Tests present/passing
- [ ] Edge cases covered
[Gaps identified]

## Priority Fixes
[Numbered list, most critical first]

## Nice-to-Haves
[Improvements that aren't required but would help]

Code:
[code to review]
```

---

## When Not to Use Advanced Techniques

Advanced techniques add complexity. Use them when:
- Simple prompts have failed
- Reliability requirements are high
- Tasks are genuinely complex
- Outputs need to be auditable

Don't use them when:
- Simple prompts work fine
- Speed matters more than perfection
- The task is straightforward
- You're exploring/brainstorming

The goal is effective output, not impressive prompts.

---

## Continuous Improvement

### Building Your Prompt Library

Maintain a personal collection of effective prompts:

```
## Prompt: [Name]
**Purpose:** What task this solves
**Model:** Which models it works well with
**Version:** Iteration number

**The Prompt:**
[Actual prompt text]

**Variables:** What to replace
**Notes:** When to use, gotchas, variations

**Examples:**
[Input/output examples]

**History:**
- v1: Initial version
- v2: Added constraint X because of issue Y
- v3: Changed structure for better results
```

### Debugging Failed Prompts

When prompts don't work:

1. **Isolate the problem**: Which part of the response is wrong?
2. **Check assumptions**: Is the model interpreting your words as you intended?
3. **Simplify first**: Remove complexity until it works, then add back
4. **Try reformulation**: Same instruction, different words
5. **Add examples**: Show don't tell
6. **Increase constraints**: Narrow the possibility space

---

*Advanced prompting is about understanding models deeply enough to work with their strengths and around their limitations. Techniques matter less than the thinking behind them. Develop intuition through experimentation.*

