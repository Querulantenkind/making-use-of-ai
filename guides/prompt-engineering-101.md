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

#### When Role Assignment Fails

**Symptom: Model ignores the role entirely**

The output reads as generic assistance with no trace of the assigned expertise.

- **Cause**: Role is too vague ("You are an expert") or contradicted by later instructions
- **Cause**: The task itself doesn't benefit from role framing
- **Fix**: Specify *behaviors* the role implies, not just the title

```
# Before (often ignored)
You are a security expert. Review this code.

# After (consistently followed)
You are a security auditor. Your review must:
- Flag each vulnerability with severity (critical/high/medium/low)
- Explain the attack vector for each issue
- Provide specific remediation steps
Decline to comment on code style or performance unless it has security implications.
```

**Symptom: Model over-commits to the role**

The model refuses helpful actions because they "wouldn't be in character" or produces overly theatrical responses.

- **Cause**: Role description is too elaborate or implies fictional behavior
- **Fix**: Ground the role in realistic professional behavior; add "while remaining helpful and direct"

```
# Before (causes over-commitment)
You are a grizzled senior engineer who has seen every bug imaginable 
and speaks only in terse, sardonic observations.

# After (balanced)
You are an experienced engineer. Be direct and concise. 
Focus on what matters most. Skip pleasantries.
```

**Symptom: Role conflicts with safety guidelines**

The model declines requests or adds excessive caveats.

- **Cause**: Role implies bypassing normal safety behavior
- **Fix**: Reframe as legitimate professional context

```
# Before (may trigger refusals)
You are a hacker explaining vulnerabilities.

# After (accepted)
You are a security researcher conducting authorized penetration testing.
Explain vulnerabilities as you would in a professional security audit report.
```

#### Model Differences: Role Assignment

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| Role adherence | Strong; maintains role throughout long conversations | Strong; occasionally drifts in very long sessions | Moderate; benefits from role reminders |
| Expertise emulation | Excellent; produces domain-appropriate terminology and reasoning | Excellent; broad knowledge base | Good for common domains; weaker on specialized topics |
| Role refusals | May decline roles that conflict with safety training | Similar pattern; slightly more permissive | Fewer refusals; less safety fine-tuning |
| Recommended approach | Specify behaviors, not just titles | Add "stay in role" for long tasks | Repeat role in system prompt and periodically |

**Claude-specific tip**: Claude responds well to roles that emphasize *values* ("you prioritize clarity over cleverness") rather than just credentials.

**GPT-4-specific tip**: GPT-4 can handle more elaborate role descriptions without over-committing. It benefits from explicit persona details when you want stylistic consistency.

**Local model tip**: Smaller models (7B-13B) struggle with complex roles. Stick to simple, single-aspect roles: "You are a Python expert" rather than "You are a senior full-stack developer who specializes in..."

#### Real Example: Role Assignment

**Task**: Get a code review focused on security

**Prompt**:
```
You are a security-focused code reviewer. Your job is to find vulnerabilities,
not to comment on style or suggest features. For each issue found:
- State the vulnerability type
- Explain how it could be exploited  
- Provide the fix

Review this authentication function:

def login(username, password):
    query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    result = db.execute(query)
    if result:
        return create_session(result[0])
    return None
```

**Claude 3.5 Sonnet Response** (abbreviated):
```
## SQL Injection Vulnerability

**Type**: SQL Injection (CWE-89)

**Exploitation**: An attacker can bypass authentication entirely:
- Username: `' OR '1'='1' --`
- Password: anything

This input produces: `SELECT * FROM users WHERE username='' OR '1'='1' --'...`
The `OR '1'='1'` always evaluates true, returning all users.

**Fix**:
```python
def login(username, password):
    query = "SELECT * FROM users WHERE username = ? AND password = ?"
    result = db.execute(query, (username, password))
    # ...
```

## Plaintext Password Storage

**Type**: Insecure Credential Storage (CWE-256)

**Exploitation**: If the database is compromised, all passwords are immediately exposed.

**Fix**: Use bcrypt or argon2 for password hashing...
```

**Why it worked**:
- The role specified a *focus* (security only), which prevented generic code review comments
- The output format was specified, producing consistent structure
- The role aligned with a realistic professional function (security reviewer)

**Why a generic prompt would have failed**:
```
# Without role specification:
Review this code.

# Typical response includes:
- Style suggestions ("consider using an ORM")
- Feature ideas ("add rate limiting")
- Security issues (buried among other feedback)
```

The security issues would still appear, but mixed with irrelevant suggestions, requiring you to extract what you actually needed.

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

#### When Few-Shot Learning Fails

**Symptom: Model copies examples too literally**

The output reuses specific words or structures from examples instead of learning the underlying pattern.

- **Cause**: Examples are too similar to each other
- **Cause**: Pattern isn't clear from the examples provided
- **Fix**: Vary surface details while keeping the transformation consistent

```
# Before (too similar)
Input: "The cat sat" -> Output: "CAT SAT"
Input: "The dog ran" -> Output: "DOG RAN"
# Model may think: always remove "The", always two words

# After (varied)
Input: "The cat sat on the mat" -> Output: "CAT SAT ON THE MAT"
Input: "Hello world" -> Output: "HELLO WORLD"
Input: "a" -> Output: "A"
# Pattern is clearly: uppercase everything
```

**Symptom: Model doesn't follow the format**

Output structure doesn't match your examples despite clear patterns.

- **Cause**: Too few examples for the pattern complexity
- **Cause**: Format wasn't actually consistent across examples
- **Fix**: Add explicit format instructions alongside examples; verify example consistency

```
# Add explicit instruction
Convert to the exact format shown in these examples.
Your output must match this structure precisely:

[examples]

Now convert (match the format exactly):
```

**Symptom: Model performs worse with examples than without**

Adding few-shot examples degrades output quality.

- **Cause**: Examples are poor quality or contain errors
- **Cause**: Examples contradict the model's stronger instincts
- **Cause**: Task is simple enough that examples add noise
- **Fix**: Test with and without examples; only use few-shot when it measurably helps

**Symptom: Inconsistent results across runs**

Same prompt produces different formats or approaches each time.

- **Cause**: Insufficient examples to establish pattern
- **Cause**: Temperature too high
- **Fix**: Add more examples (4-5 instead of 2-3); reduce temperature to 0-0.3

#### Model Differences: Few-Shot Learning

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| Examples needed | 2-3 typically sufficient | 2-3 typically sufficient | 4-5 recommended |
| Format adherence | High; follows format precisely | Medium; may embellish or add fields | Lower; benefits from explicit format enforcement |
| Pattern complexity | Handles nested/complex patterns well | Handles complex patterns well | Simpler patterns work better |
| Example quality sensitivity | High; poor examples hurt significantly | Medium; somewhat robust to noise | High; garbage in, garbage out |

**Claude-specific tip**: Claude follows format so precisely that typos in your examples will be reproduced. Double-check example formatting.

**GPT-4-specific tip**: GPT-4 may add helpful extras beyond your format. If you need exact format matching, add: "Output only the specified format, nothing else."

**Local model tip**: Repeat the format structure in both the examples AND as an explicit instruction. Consider adding: "Your response must contain only: [format description]"

#### Real Example: Few-Shot for Commit Messages

**Task**: Generate consistent commit message format

**Prompt**:
```
Generate a commit message following this exact format:

Example 1:
Change: Added user authentication
Message: feat(auth): implement JWT-based user authentication

Example 2:
Change: Fixed crash when input is empty
Message: fix(input): handle empty string edge case

Example 3:
Change: Updated README with installation steps
Message: docs(readme): add installation instructions

Now generate:
Change: Refactored database queries to use connection pooling
Message:
```

**Claude 3.5 Sonnet Response**:
```
refactor(db): implement connection pooling for queries
```

**GPT-4 Response**:
```
refactor(db): implement connection pooling for database queries

This change improves performance by reusing database connections instead of 
creating new ones for each query.
```

**Llama 3 70B Response**:
```
refactor(database): use connection pooling for queries
```

**Analysis**:
- Claude matched the format exactly
- GPT-4 added an explanatory body (helpful but not requested)
- Llama used "database" instead of "db" (less consistent shortening)

**Improved prompt for GPT-4**:
```
Generate a commit message following this exact format. 
Output ONLY the message line, nothing else.

[same examples]
```

**Improved prompt for Llama**:
```
Generate a commit message. Use this EXACT format:
type(scope): short description

Types: feat, fix, refactor, docs, test, chore
Scope: 2-4 letter abbreviation of the affected area

[examples]
```

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

#### When Chain-of-Thought Fails

**Symptom: Model shows reasoning but still gets wrong answer**

The steps are visible but contain errors, or the final answer contradicts the reasoning.

- **Cause**: Problem requires knowledge the model lacks
- **Cause**: Reasoning chain is too long (error accumulation)
- **Fix**: Break into smaller sub-problems; verify intermediate steps; provide relevant facts

```
# Before (single long chain)
Calculate the total project cost including all factors. Think step by step.

# After (chunked)
Let's calculate this in parts. First, calculate only the labor costs.
[verify]
Now, using that labor cost, calculate materials.
[verify]
Finally, combine with overhead for the total.
```

**Symptom: Verbose reasoning that doesn't help**

Model produces extensive "thinking" that's mostly filler without improving the answer.

- **Cause**: Task is simple and doesn't benefit from CoT
- **Cause**: Model has learned to produce "reasoning theater"
- **Fix**: Only use CoT for genuinely complex problems; be specific about what reasoning you need

```
# Before (too vague)
Think through this carefully.

# After (specific)
Before answering, identify:
1. What information is given
2. What information is missing
3. What assumptions you're making
Then provide your answer.
```

**Symptom: Model skips steps or rushes to conclusion**

Despite instructions, the model jumps to an answer without showing work.

- **Cause**: Problem seems simple to the model
- **Cause**: Instruction was too weak
- **Fix**: Make step-by-step mandatory with explicit structure

```
# Before
Think step by step.

# After
You MUST show your work. Structure your response as:
STEP 1: [first step]
STEP 2: [second step]
...
FINAL ANSWER: [answer]

Do not skip any steps.
```

**Symptom: Reasoning is correct but answer format is wrong**

The thinking is sound but the final answer isn't extracted properly.

- **Cause**: No clear separation between reasoning and answer
- **Fix**: Use explicit delimiters

```
Show your reasoning inside <thinking> tags.
Provide your final answer inside <answer> tags.
```

#### Model Differences: Chain-of-Thought

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| CoT benefit | High; significant accuracy improvement | High; especially for math/logic | Moderate; helps but less dramatic |
| Reasoning quality | Thorough; tends toward careful analysis | Thorough; sometimes overconfident | Variable; may produce superficial steps |
| Step adherence | Follows structured step formats well | Good; may add extra context | May skip steps or merge them |
| Self-correction | Often catches own errors mid-reasoning | Sometimes catches errors | Rarely self-corrects |

**Claude-specific tip**: Claude excels at "think about what could go wrong" prompts. Add: "Before finalizing, check your reasoning for errors."

**GPT-4-specific tip**: GPT-4 benefits from explicit step numbering. "Step 1, Step 2..." produces more organized reasoning than "think through this."

**Local model tip**: For smaller models, provide the reasoning structure explicitly:
```
Copy this template and fill it in:
Given: [state what's given]
Need to find: [state the goal]
Approach: [describe method]
Work: [show calculations]
Answer: [final result]
```

#### Real Example: Chain-of-Thought for Debugging

**Task**: Identify a subtle bug

**Prompt**:
```
Debug this function by reasoning through what happens at each step.

def find_average(numbers):
    total = 0
    for i in range(len(numbers)):
        total += numbers[i]
    return total / len(numbers)

# Bug report: Returns error for some inputs

Trace through the execution with a few example inputs, 
including edge cases, to find the bug.
```

**Claude 3.5 Sonnet Response**:
```
Let me trace through with different inputs:

**Test 1: Normal case [1, 2, 3]**
- total starts at 0
- Loop: i=0, total=0+1=1; i=1, total=1+2=3; i=2, total=3+3=6
- Return 6/3 = 2.0
- Works correctly

**Test 2: Single element [5]**
- total starts at 0
- Loop: i=0, total=0+5=5
- Return 5/1 = 5.0
- Works correctly

**Test 3: Empty list []**
- total starts at 0
- Loop: range(0) produces no iterations
- Return 0/0 = **ZeroDivisionError**
- Bug found!

**The bug**: No handling for empty input. Division by zero occurs when 
the list is empty.

**Fix**:
```python
def find_average(numbers):
    if not numbers:
        return 0  # or raise ValueError("Cannot average empty list")
    total = 0
    for i in range(len(numbers)):
        total += numbers[i]
    return total / len(numbers)
```
```

**Why CoT helped here**:
- Systematic testing revealed the edge case
- The trace made the error mechanism visible
- Without CoT, the model might have guessed common bugs (off-by-one, etc.)

**Without CoT (same prompt, minus tracing instruction)**:
```
Review this function for bugs.
```

**Typical response**: "The function looks correct for basic usage. You might consider adding type hints and handling edge cases." - Less specific, doesn't identify the actual bug.

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

#### When Structured Output Fails

**Symptom: JSON is malformed or invalid**

Model produces JSON-like output that doesn't parse.

- **Cause**: Model doesn't perfectly track brackets/braces in complex structures
- **Cause**: Model adds commentary outside the JSON
- **Fix**: Use explicit start/end markers; request validation; simplify structure

```
# Before
Return your response as JSON.

# After
Return ONLY valid JSON, starting with { and ending with }.
Do not include any text before or after the JSON.
Do not use comments inside the JSON.

If you need to add explanations, put them in a field called "notes".
```

**Symptom: Missing required fields**

Output matches structure but omits fields you specified.

- **Cause**: Field was described but not shown in schema
- **Cause**: Model decided field was "not applicable"
- **Fix**: Show complete schema with all fields; mark fields as required; provide default values

```
# Before
Return JSON with name, email, and preferences.

# After
Return JSON matching this EXACT schema. Include ALL fields even if empty.

{
    "name": "string (required)",
    "email": "string (required)",
    "preferences": {
        "theme": "light or dark (default: light)",
        "notifications": true or false (default: true)
    }
}
```

**Symptom: Extra fields or different structure**

Model adds fields you didn't request or nests things differently.

- **Cause**: Model is being "helpful" by adding relevant information
- **Cause**: Your schema wasn't explicit enough
- **Fix**: Add "only these fields, nothing else"; show negative examples

```
Return ONLY these fields in this exact structure:
{ "status": "success|error", "data": [...] }

Do NOT add fields like "message", "timestamp", or "metadata".
```

**Symptom: Format breaks mid-response**

Output starts correctly but becomes malformed partway through.

- **Cause**: Response is very long; model loses track of structure
- **Cause**: Content is complex; model switches to prose explanation
- **Fix**: Request shorter output; chunk into multiple requests; increase max_tokens

#### Model Differences: Structured Output

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| JSON reliability | High; rarely produces invalid JSON | Very high; especially with json_mode | Moderate; complex JSON often breaks |
| Schema adherence | Follows schemas precisely | Follows well; may add helpful extras | May deviate from schema |
| Markdown formatting | Excellent; consistent structure | Excellent | Good for simple; complex headers may vary |
| XML/custom formats | Handles well | Handles well | Stick to JSON or simple formats |

**Claude-specific tip**: Claude respects schemas extremely precisely. If you want flexibility, say so: "Include these fields at minimum; add others if relevant."

**GPT-4-specific tip**: Use `response_format: { type: "json_object" }` in API calls for guaranteed valid JSON. In the prompt, still describe the structure you want.

**Local model tip**: For reliable JSON from smaller models:
1. Keep structures flat (avoid deep nesting)
2. Limit arrays to <10 items
3. Use simple types (string, number, boolean)
4. Provide a complete example, not just a schema

#### Real Example: Structured Output for API Response

**Task**: Extract structured data from unstructured text

**Prompt**:
```
Extract contact information from this email signature and return as JSON.

Required fields (include all, use null if not found):
- name: full name
- title: job title  
- company: company name
- email: email address
- phone: phone number
- linkedin: LinkedIn URL

Email signature:
---
Best regards,

John Smith
Senior Software Engineer | Acme Corp
john.smith@acme.com | (555) 123-4567
Connect with me: linkedin.com/in/johnsmith
---

Return ONLY the JSON object, no other text.
```

**Claude 3.5 Sonnet Response**:
```json
{
    "name": "John Smith",
    "title": "Senior Software Engineer",
    "company": "Acme Corp",
    "email": "john.smith@acme.com",
    "phone": "(555) 123-4567",
    "linkedin": "linkedin.com/in/johnsmith"
}
```

**GPT-4 Response** (without json_mode):
```
Here's the extracted information:

```json
{
    "name": "John Smith",
    "title": "Senior Software Engineer",
    "company": "Acme Corp",
    "email": "john.smith@acme.com",
    "phone": "(555) 123-4567",
    "linkedin": "https://linkedin.com/in/johnsmith"
}
```

Note: I added "https://" to the LinkedIn URL for completeness.
```

**Analysis**:
- Claude followed "ONLY the JSON object" instruction precisely
- GPT-4 added helpful context (the note about https://) despite the instruction
- Both extracted data correctly
- GPT-4's response requires parsing to extract the JSON

**Improved GPT-4 prompt**:
```
[same prompt, add at end:]
Critical: Your entire response must be valid JSON. No markdown, no explanations.
Start your response with { and end with }
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

#### When Constraint Setting Fails

**Symptom: Model violates explicit constraints**

Despite clear instructions, output breaks stated rules.

- **Cause**: Constraints conflict with each other
- **Cause**: Later instructions implicitly override earlier constraints
- **Cause**: Constraint is impossible to satisfy
- **Fix**: Check for conflicts; place critical constraints at the end (recency bias); verify constraints are achievable

```
# Conflicting constraints (model must choose)
Write a comprehensive 50-word summary.
# "Comprehensive" and "50 words" often conflict

# Fixed
Write a summary of exactly 50 words. Cover only the main point and key conclusion.
```

**Symptom: Model follows letter but not spirit**

Technically complies but misses the intent.

- **Cause**: Constraint was too literal
- **Cause**: Model found a loophole
- **Fix**: Explain the *reason* for the constraint; add clarifying examples

```
# Before (letter but not spirit)
Don't use jargon.
# Model might use: "utilize" instead of "use" (technically not jargon, still bad)

# After (captures intent)
Write for a general audience with no technical background. 
Use simple, everyday words. Instead of "utilize" say "use", 
instead of "implement" say "set up".
```

**Symptom: Word/length constraints are imprecise**

"Keep it under 100 words" produces 80-150 word responses.

- **Cause**: Models don't count tokens/words precisely during generation
- **Fix**: Give a range; accept some variance; use post-processing if exact count matters

```
# Before
Write exactly 100 words.

# After (more reliable)
Write approximately 100 words (90-110 is acceptable).
```

**Symptom: Negative constraints backfire**

"Don't mention X" causes model to mention X.

- **Cause**: The instruction activates X in the model's attention
- **Fix**: Reframe as positive; describe what TO do instead

```
# Before (often backfires)
Don't talk about competitors.
Don't use a negative tone.
Don't include pricing.

# After (more reliable)
Focus only on our product's features and benefits.
Use a positive, encouraging tone throughout.
Cover features and use cases; the sales team will handle pricing discussions.
```

#### Model Differences: Constraint Setting

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| Constraint adherence | Very high; respects constraints carefully | High; occasional creative interpretation | Moderate; hard constraints may be ignored |
| Word count accuracy | Within 10-15% typically | Within 15-20% typically | Often significantly off |
| Negative constraints | Follows well; rare backfiring | Usually follows; occasional issues | Often backfires; avoid negatives |
| Conflicting constraints | Will often ask for clarification or note conflict | Will make a choice without noting conflict | Will ignore one constraint silently |

**Claude-specific tip**: Claude tends to follow constraints very literally. If you want some flexibility, say: "Aim for about 100 words, but prioritize completeness over exact count."

**GPT-4-specific tip**: GPT-4 interprets constraints more flexibly. If you need strict adherence, emphasize: "This constraint is mandatory, not a guideline."

**Local model tip**: 
- Use only 2-3 critical constraints
- Put constraints at the end of the prompt
- Phrase everything positively
- Repeat the most important constraint

#### Real Example: Constraint Setting for Technical Writing

**Task**: Explain a concept with specific constraints

**Prompt**:
```
Explain how HTTPS works.

Constraints:
- Exactly 3 paragraphs
- No more than 50 words per paragraph
- Suitable for a high school student
- No acronyms without expansion
- Must cover: encryption, certificates, and the handshake process
```

**Claude 3.5 Sonnet Response**:
```
When you visit a secure website, your browser and the server perform a "handshake." 
They agree on a secret code that only they know. This happens in milliseconds 
before any of your data is sent, ensuring your connection starts safely.

The server proves its identity using a digital certificate, like an ID card for 
websites. A trusted authority vouches that the website is legitimate. Your browser 
checks this certificate before proceeding with the connection.

All data between you and the website is then scrambled using encryption. 
Even if someone intercepts the data, they cannot read it without the secret 
code from the handshake. This protects your passwords, credit cards, and private messages.
```

**Word counts**: 47, 44, 46 (all under 50; 3 paragraphs)
**Coverage**: Handshake (para 1), Certificates (para 2), Encryption (para 3)
**Acronym check**: HTTPS not re-expanded (minor issue - was in the prompt)

**GPT-4 Response** (same prompt):
```
HTTPS (Hypertext Transfer Protocol Secure) protects your data when browsing the web. 
When you connect to a website, your browser and the website's server perform a 
"handshake" to establish a secure connection. During this process, they agree on 
encryption methods and exchange keys.

Websites prove their identity using digital certificates issued by trusted authorities. 
Your browser checks these certificates to verify you're connecting to the real website, 
not an imposter. This prevents attackers from pretending to be legitimate sites.

Once the handshake completes, all data traveling between your browser and the server 
is encrypted. This means even if hackers intercept your traffic, they can't read 
your passwords, credit card numbers, or personal information. The scrambled data 
is useless without the decryption keys.
```

**Word counts**: 64, 47, 55 (first and third exceed 50)
**Analysis**: GPT-4 prioritized comprehensive explanation over strict word limit

**Improved prompt for GPT-4**:
```
[same constraints, add:]
CRITICAL: Each paragraph must be 50 words or fewer. Count carefully before finalizing.
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

#### When Decomposition Fails

**Symptom: Model answers everything at once anyway**

Despite sequential instructions, model provides complete answer in first response.

- **Cause**: Model is eager to be helpful; sees the full task and attempts it all
- **Cause**: Decomposition wasn't explicit enough about waiting
- **Fix**: Use explicit "STOP" markers; only reveal one step at a time; use system prompts for multi-turn control

```
# Before (often ignored)
Let's do this step by step. First, outline the components.
Then we'll implement each one.

# After (enforced stopping)
We'll work through this in multiple messages.

STEP 1: List the components needed for this feature. 
Just list them with one-line descriptions.

STOP after the list. Do not implement anything yet.
I'll tell you which component to implement first.
```

**Symptom: Later steps ignore earlier context**

Model seems to forget what was decided in previous steps.

- **Cause**: Context window limits (in very long conversations)
- **Cause**: Model is treating each step independently
- **Fix**: Summarize previous decisions at each step; keep conversation concise; provide explicit links to earlier content

```
# At each new step
Continuing from our previous work:
- We decided on [X] architecture
- We're using [Y] for the database
- Step 1 produced [summary]

Now, for Step 2...
```

**Symptom: Decomposition creates more work, not less**

Breaking the task apart results in inconsistent pieces that don't fit together.

- **Cause**: Steps were decomposed incorrectly; dependencies weren't identified
- **Cause**: Task wasn't actually complex enough to need decomposition
- **Fix**: Start with dependency mapping; use decomposition only for genuinely complex tasks

```
# Before decomposing, ask:
Before we start, map out:
1. What are the components of this task?
2. Which components depend on others?
3. What must be decided first?

Then we'll tackle them in the right order.
```

**Symptom: Parallel analyses repeat the same points**

Multiple "angles" produce overlapping, redundant content.

- **Cause**: Angles weren't distinct enough
- **Cause**: No instruction to avoid repetition
- **Fix**: Define angles more precisely; explicitly request no overlap

```
# Before (overlapping)
Analyze from: performance, efficiency, and speed perspectives.
# These are basically the same thing

# After (distinct)
Analyze from three perspectives. Do not repeat points across sections:

1. RUNTIME PERFORMANCE: CPU time, memory usage, algorithmic complexity
2. DEVELOPER EXPERIENCE: Readability, testability, debugging ease  
3. OPERATIONAL CONCERNS: Monitoring, deployment, failure handling
```

#### Model Differences: Decomposition

| Aspect | Claude | GPT-4 | Llama 3 / Local Models |
|--------|--------|-------|------------------------|
| Sequential adherence | Good; respects STOP instructions | Moderate; may anticipate next steps | Poor; often completes everything |
| Context retention | Excellent across long conversations | Good; may need reminders | Limited; requires frequent summaries |
| Synthesis quality | Strong; connects pieces coherently | Strong; good at integration | Variable; may miss connections |
| Step dependencies | Handles well; maintains consistency | Handles well | May produce inconsistent parts |

**Claude-specific tip**: Claude is excellent at multi-turn decomposition. Use: "After completing this step, wait for my feedback before proceeding."

**GPT-4-specific tip**: GPT-4 sometimes wants to show it understands the whole picture. Allow this by saying: "You may note what comes next, but don't execute it until I ask."

**Local model tip**: Keep decomposition to 2-3 steps maximum. Provide full context at each step:
```
Context: We're building X. We already decided Y.
Previous step produced: [paste output]
Now do only: [current step]
```

#### Real Example: Decomposition for System Design

**Task**: Design a URL shortener system

**Single-prompt approach** (often produces shallow results):
```
Design a URL shortener service like bit.ly.
```

**Decomposed approach**:

**Prompt 1**:
```
I need to design a URL shortener service. Before any implementation details,
let's understand the requirements.

List the functional and non-functional requirements for a URL shortener
used by millions of users. Just list them - we'll prioritize next.
```

**Response 1** (Claude):
```
Functional Requirements:
- Shorten long URLs to short codes
- Redirect short codes to original URLs
- Custom short codes (optional)
- Link expiration
- Click analytics
- User accounts for link management

Non-Functional Requirements:
- Low latency (<100ms redirect)
- High availability (99.9%+)
- Scalability (millions of URLs, billions of redirects)
- No collisions in short codes
- Durability (links shouldn't break)
```

**Prompt 2**:
```
Good list. Now let's focus on the core: shortening and redirecting.

For these two operations:
1. What's the API design?
2. What data needs to be stored?
3. What's the algorithm for generating short codes?

Don't discuss scaling yet - just the basic design.
```

**Prompt 3**:
```
Now let's address scale. Given:
- 100M URLs
- 10K new URLs/second
- 100K redirects/second

What changes to the basic design? Consider:
- Database choice and sharding
- Caching strategy
- Code generation at scale
```

**Why decomposition worked**:
- Each step built on the previous with full context
- Complex problem broken into manageable pieces
- Easy to course-correct early (requirements) before investing in design
- Final design is more thorough than single-shot attempt

**When NOT to decompose** (same task, simpler need):
```
Give me a quick sketch of URL shortener architecture for a hackathon project.
Just the basics - we have 24 hours.
```

For simple needs, decomposition adds overhead without proportional benefit.

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

## Troubleshooting Quick Reference

When your prompt isn't working, diagnose by symptom:

| Symptom | Likely Cause | Quick Fix |
|---------|--------------|-----------|
| Output ignores instructions | Instructions buried in middle; contradictory constraints | Move critical instructions to end; check for conflicts |
| Format doesn't match | Too few examples; format not explicit enough | Add 2-3 more examples; show exact output template |
| Wrong level of detail | Constraint too vague ("be concise") | Specify: "3 sentences" or "under 100 words" |
| Generic/shallow response | No role; no expertise invoked | Add role with specific behaviors |
| Model refuses request | Safety guidelines triggered | Reframe as legitimate professional context |
| Inconsistent across runs | Temperature too high; insufficient examples | Lower temperature; add few-shot examples |
| Reasoning errors | Jumped to answer without thinking | Add explicit chain-of-thought structure |
| JSON/structure breaks | Complex nesting; long output | Simplify structure; chunk into parts |
| Hallucinated facts | Model guessing beyond knowledge | Ask for confidence levels; request citations |
| Repeating itself | Prompt induced loop; unclear completion | Add clear stopping condition; restructure prompt |

### The Debugging Protocol

When a prompt fails:

1. **Isolate**: Which part of the response is wrong?
2. **Simplify**: Remove complexity until it works
3. **Identify**: What assumption did the model make that you didn't intend?
4. **Fix**: Address that specific misunderstanding
5. **Test**: Verify with 3-5 variations to ensure consistency

### Quick Fixes by Model

**Claude not following format**: Add "Your response must begin with:" followed by the exact starting text.

**GPT-4 being too verbose**: Add "Be direct. Skip preamble. Start with the answer."

**Local model ignoring instructions**: Move all instructions to the end; repeat the most important one.

---

## Model Comparison Summary

### Overall Characteristics

| Capability | Claude 3.5 Sonnet | GPT-4 | Llama 3 70B |
|------------|-------------------|-------|-------------|
| Instruction following | Excellent | Very Good | Good |
| Format adherence | Very precise | Good (may embellish) | Variable |
| Reasoning depth | Excellent | Excellent | Good |
| Code generation | Excellent | Excellent | Good |
| Cost efficiency | Mid-tier | Premium | Free (self-hosted) |
| Context window | 200K | 128K | 8K-128K |

### Technique Effectiveness by Model

| Technique | Claude | GPT-4 | Local Models |
|-----------|--------|-------|--------------|
| Role Assignment | Very effective | Very effective | Effective for simple roles |
| Few-Shot Learning | 2-3 examples sufficient | 2-3 examples; may add extras | 4-5 examples; explicit format |
| Chain-of-Thought | Excellent improvement | Excellent improvement | Moderate improvement |
| Structured Output | Very precise | Good; use json_mode API | Keep structures simple |
| Constraint Setting | Follows literally | Interprets flexibly | Use positive constraints only |
| Decomposition | Excellent multi-turn | Good; may anticipate | Keep to 2-3 steps |

### When to Choose Each Model

**Choose Claude when**:
- Precise format adherence matters
- You need careful, nuanced analysis
- Long-form content with consistent quality
- Code review and technical writing

**Choose GPT-4 when**:
- You need JSON mode API guarantee
- Broad knowledge retrieval matters
- You want flexible interpretation of instructions
- Multimodal tasks with images

**Choose Local Models when**:
- Privacy is paramount
- Cost at scale matters
- Task is well-defined and repetitive
- You can fine-tune for your domain

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

