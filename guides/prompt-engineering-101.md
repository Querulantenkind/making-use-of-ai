# Prompt Engineering 101

## The Fundamental Techniques That Transform AI Interactions

Prompt engineering is the discipline of crafting inputs that reliably produce desired outputs from language models. It sits at the intersection of communication, psychology, and computer science.

This guide covers the core techniques. Master these, and you'll handle 90% of use cases effectively. The remaining 10% requires the advanced strategies covered in [Advanced Prompting](advanced-prompting.md).

---

## The Core Techniques

### 1. Role Assignment

One of the most powerful techniques is explicitly assigning a role or persona to the model. This shapes its responses by activating relevant patterns from its training data.

#### Basic Role Assignment

```
You are an experienced Python developer who specializes in writing clean, 
maintainable code. You favor explicit over implicit, readability over 
cleverness, and always include comprehensive error handling.

Review the following code and suggest improvements:
[code]
```

The model now approaches the task from that specific perspective, producing reviews that emphasize the stated values.

#### Expert Roles for Specialized Knowledge

```
You are a database performance specialist with 15 years of experience 
optimizing PostgreSQL queries for high-traffic applications.

Analyze this query and explain why it might be slow:
[query]
```

By invoking expertise, you prompt the model to draw on its training related to that domain, often producing more sophisticated analysis.

#### Multiple Roles for Different Perspectives

```
I want you to analyze this business proposal from three perspectives:

1. As a venture capitalist focused on ROI and scalability
2. As a technical architect concerned with implementation feasibility
3. As a potential customer evaluating value proposition

[proposal text]
```

This technique surfaces different considerations you might otherwise miss.

#### Anti-Patterns to Avoid

Don't assign impossible roles:
```
# Bad: The model can't actually be this
You are a sentient AI with feelings and consciousness.

# Good: The model can emulate this perspective
You are a thoughtful analyst who considers emotional impact in your assessments.
```

---

### 2. Few-Shot Learning

Few-shot learning provides examples of the desired input-output pattern before presenting the actual task. The model learns the pattern from examples and applies it to new inputs.

#### Basic Few-Shot Pattern

```
Convert these sentences to formal business language:

Casual: "Hey, just wanted to check if you got my email?"
Formal: "I am writing to confirm receipt of my previous correspondence."

Casual: "This idea is kind of cool, we should try it."
Formal: "This concept merits further exploration and consideration for implementation."

Casual: "Can you help me out with this thing?"
Formal:
```

The model learns the transformation pattern and applies it to the final input.

#### Few-Shot for Code Transformation

```
Convert these JavaScript functions to TypeScript with proper type annotations:

Example 1:
JavaScript:
function add(a, b) {
    return a + b;
}

TypeScript:
function add(a: number, b: number): number {
    return a + b;
}

Example 2:
JavaScript:
function greet(name) {
    return "Hello, " + name;
}

TypeScript:
function greet(name: string): string {
    return "Hello, " + name;
}

Now convert:
JavaScript:
function processUser(user) {
    return {
        id: user.id,
        fullName: user.firstName + " " + user.lastName,
        isActive: user.status === "active"
    };
}

TypeScript:
```

#### Choosing Good Examples

The quality of few-shot learning depends on example quality:

- **Cover edge cases**: Include examples that demonstrate handling of special situations
- **Show the pattern clearly**: Examples should make the transformation obvious
- **Use realistic complexity**: Match example complexity to actual task complexity
- **Three is often enough**: More examples help but have diminishing returns

---

### 3. Chain-of-Thought Prompting

Chain-of-thought (CoT) prompting instructs the model to reason through problems step by step before providing a final answer. This dramatically improves performance on complex reasoning tasks.

#### Basic Chain-of-Thought

```
Solve this problem step by step, showing your reasoning at each stage:

A store sells apples for $0.50 each and oranges for $0.75 each. 
Sarah bought some apples and oranges, spending exactly $4.00. 
She bought twice as many apples as oranges. 
How many of each fruit did she buy?
```

The explicit instruction to work step by step produces:
- More accurate answers
- Visible reasoning you can verify
- Better handling of multi-step problems

#### Chain-of-Thought for Code Problems

```
I need to implement a function that finds the longest palindromic substring 
in a given string.

Before writing code, please:
1. Explain the approach you'll use and why
2. Identify edge cases that need handling
3. Analyze the time and space complexity
4. Then provide the implementation

This ensures you've thought through the problem before coding.
```

#### Zero-Shot Chain-of-Thought

Sometimes a simple addition triggers step-by-step reasoning:

```
What is the result of this calculation? Think step by step.

((15 + 7) * 3 - 12) / 2
```

Adding "Think step by step" or "Let's work through this systematically" often improves complex task performance without needing to specify the exact steps.

---

### 4. Structured Output Control

Controlling output format ensures responses integrate smoothly into your workflows and are easy to parse.

#### Explicit Format Specification

```
Analyze the security vulnerabilities in this code and return your response 
as a JSON object with this exact structure:

{
    "vulnerabilities": [
        {
            "severity": "high|medium|low",
            "type": "string describing the vulnerability type",
            "location": "line number or code section",
            "description": "detailed explanation",
            "remediation": "how to fix it"
        }
    ],
    "overall_risk_score": "1-10",
    "summary": "one paragraph overview"
}

Code to analyze:
[code]
```

#### Markdown Formatting

```
Document this API endpoint using the following markdown structure:

## Endpoint Name
Brief description

### URL
`METHOD /path`

### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|

### Request Body
```json
// example
```

### Response
```json
// example
```

### Error Codes
- `400`: description
- `404`: description
```

#### Using Delimiters for Clear Sections

```
Provide your analysis in clearly marked sections:

===SUMMARY===
[Your summary here]

===STRENGTHS===
[Bullet points of strengths]

===WEAKNESSES===
[Bullet points of weaknesses]

===RECOMMENDATIONS===
[Numbered list of recommendations]

===CONCLUSION===
[Final thoughts]
```

---

### 5. Constraint Setting

Constraints narrow the output space, making responses more predictable and useful.

#### Positive Constraints (What To Do)

```
Write a product description that:
- Uses exactly 100-150 words
- Includes three benefit-focused bullet points
- Ends with a call to action
- Uses active voice throughout
- Targets technical professionals
```

#### Negative Constraints (What To Avoid)

```
Explain quantum entanglement without:
- Using mathematical notation
- Assuming prior physics knowledge
- Using analogies involving "spooky action"
- Exceeding 200 words
```

#### Combining Positive and Negative

```
Refactor this function following these rules:

DO:
- Use descriptive variable names
- Add type hints
- Include a docstring
- Handle potential exceptions

DO NOT:
- Change the function signature
- Add external dependencies
- Modify the core algorithm
- Exceed 30 lines of code
```

---

### 6. Decomposition

Complex tasks become manageable when broken into sequential subtasks.

#### Sequential Decomposition

```
I need to build a feature for user authentication. Let's approach this 
systematically:

Step 1: First, outline the components needed for a secure authentication 
system (just list them, don't implement yet).

[Wait for response, then:]

Step 2: Now, let's design the database schema for user credentials. 
Include considerations for password hashing and session management.

[Wait for response, then:]

Step 3: Implement the registration endpoint based on our schema.

[Continue...]
```

#### Parallel Decomposition

```
Analyze this codebase from three independent angles. For each angle, 
provide a separate assessment:

ANGLE 1 - Performance:
[Your performance analysis]

ANGLE 2 - Security:
[Your security analysis]

ANGLE 3 - Maintainability:
[Your maintainability analysis]

Then synthesize findings:
SYNTHESIS:
[Combined recommendations prioritized by impact]
```

---

## Combining Techniques

The real power emerges when techniques combine.

### Role + Chain-of-Thought + Structured Output

```
You are a senior security auditor conducting a code review.

Analyze the following authentication code by:
1. First, identifying the authentication flow
2. Then, examining each step for potential vulnerabilities
3. Finally, assessing overall security posture

Present findings in this format:

## Flow Analysis
[Step-by-step breakdown of the auth flow]

## Vulnerabilities Found
| Issue | Severity | Location | Impact |
|-------|----------|----------|--------|

## Recommendations
[Prioritized list of fixes]

## Risk Assessment
[Overall security grade with justification]

Code:
[code]
```

### Few-Shot + Constraints + Decomposition

```
Generate unit tests for this function following my project's conventions.

Example test style:
```python
def test_function_does_expected_thing():
    # Arrange
    input_data = create_test_data()
    expected = ExpectedResult()
    
    # Act
    result = function_under_test(input_data)
    
    # Assert
    assert result == expected
    assert result.property == expected.property
```

Constraints:
- Follow AAA (Arrange-Act-Assert) pattern
- Test one behavior per test function
- Name tests descriptively: test_<behavior>_<condition>
- Include edge cases

Generate tests in this order:
1. Happy path (normal successful operation)
2. Edge cases (empty inputs, boundaries)
3. Error cases (invalid inputs, exceptions)

Function to test:
[function]
```

---

## Technique Selection Guide

| Situation | Primary Technique | Why |
|-----------|-------------------|-----|
| Need domain expertise | Role Assignment | Activates relevant knowledge patterns |
| Need consistent formatting | Few-Shot + Structured Output | Examples demonstrate exact format |
| Complex reasoning required | Chain-of-Thought | Forces step-by-step processing |
| Task has specific requirements | Constraint Setting | Narrows output space |
| Large or complex task | Decomposition | Manages complexity through structure |
| Need reliable, repeatable outputs | All of the above combined | Maximum control and consistency |

---

## Common Pitfalls

### Over-Engineering Prompts

Don't apply every technique to every prompt. Simple tasks need simple prompts:

```
# Over-engineered for the task
You are a world-class expert in string manipulation with 20 years of 
experience. Using your deep expertise, and thinking step by step through 
the problem, convert the following string to uppercase. Provide your 
response in JSON format with fields for "original", "reasoning", and 
"result"...

# Appropriate
Convert "hello world" to uppercase.
```

### Contradictory Instructions

Watch for conflicts in your constraints:

```
# Problematic
Be concise (use as few words as possible) and be comprehensive 
(cover all aspects thoroughly).

# Better
Provide a thorough analysis, but express each point concisely. 
Aim for completeness without redundancy.
```

### Forgetting to Iterate

These techniques improve prompts but don't guarantee perfection. Expect to refine:

1. Apply techniques thoughtfully
2. Evaluate the response
3. Identify gaps
4. Refine the prompt
5. Repeat until satisfied

---

## Practice Exercises

Put these techniques into practice:

1. **Role Assignment**: Ask for code review from three different perspectives (security expert, performance engineer, junior developer's mentor)

2. **Few-Shot**: Create a prompt that transforms error messages into user-friendly explanations, providing 3 examples first

3. **Chain-of-Thought**: Have the model design a database schema, requiring it to explain reasoning before presenting the schema

4. **Structured Output**: Request an API documentation page with a specific markdown structure

5. **Combined**: Create a prompt that uses role assignment, chain-of-thought, and structured output together for a complex analysis task

---

## Next Steps

These fundamental techniques handle most prompting needs. For sophisticated use cases, continue to:

**[Advanced Prompting Strategies](advanced-prompting.md)**

Topics covered there include:
- Meta-prompting and self-reflection
- Adversarial prompting for robustness
- Prompt chaining and orchestration
- Custom instruction optimization
- Handling model limitations creatively

---

*Techniques are tools. Like all tools, they become extensions of your capability through practice. Use them deliberately until they become instinct.*

