# Cursor Workflows

## Patterns for Effective AI-Assisted Development

Workflows are repeatable patterns that make your AI collaboration more effective. These patterns have been refined through real-world use to maximize the value of AI assistance.

---

## The Core Workflows

### 1. The Exploration Workflow

**When:** Starting new work in unfamiliar code

```
PHASE 1: Understand
Open Chat (Cmd+L) and ask:
"Explain how [feature/system] works. Start with the entry point 
and trace through the key code paths."

PHASE 2: Map
"What are the key files involved in [feature]? List them with 
brief descriptions of each file's role."

PHASE 3: Identify
"What would I need to modify to [your intended change]? 
Don't make changes yet, just explain."

PHASE 4: Plan
"Given this understanding, outline a plan for implementing [change]. 
What order should I tackle the changes?"
```

This prevents blind modifications. You understand before you act.

### 2. The Implementation Workflow

**When:** Adding new features or substantial code

```
PHASE 1: Define
Write a clear specification in Chat:
"I need to implement [feature]. 

Requirements:
- [Requirement 1]
- [Requirement 2]

Constraints:
- [Constraint 1]
- [Constraint 2]

Files that will be involved:
@file1 @file2

Questions before we start?"

PHASE 2: Scaffold
"Create the basic structure for this feature. Include:
- File/folder structure
- Interface definitions
- Empty function shells with signatures
Don't implement logic yet."

PHASE 3: Implement
Work through components one at a time:
"Implement [Component 1]"
Review, test, then:
"Implement [Component 2]"

PHASE 4: Integrate
"Now connect these components. Update any integration points."

PHASE 5: Verify
"Review the full implementation for:
- Missing error handling
- Edge cases
- Consistency with codebase patterns"
```

### 3. The Bug Fix Workflow

**When:** Debugging issues

```
PHASE 1: Reproduce
First, have clear reproduction steps. If not:
"Help me understand how to reliably reproduce this bug based on 
the error message and code."

PHASE 2: Locate
"Given this error:
[error message]

And this behavior:
[what's happening vs what should happen]

Analyze these files to identify the root cause:
@relevant-file-1 @relevant-file-2"

PHASE 3: Understand
"Explain why this bug occurs. What's the actual cause, 
not just what line throws the error?"

PHASE 4: Fix
"Propose a fix. Consider:
- Does it address the root cause?
- Could it introduce other issues?
- Are there related places that might have the same bug?"

PHASE 5: Test
"What test cases should verify this fix? Include:
- The reproduction case
- Edge cases that might be related
- Regression cases"
```

### 4. The Refactoring Workflow

**When:** Improving existing code

```
PHASE 1: Assess
"Analyze this code for improvement opportunities:
@file-to-refactor

Consider:
- Readability
- Maintainability  
- Performance
- Adherence to our patterns (see .cursorrules)

Don't change anything yet."

PHASE 2: Plan
"Prioritize improvements by impact/effort ratio. 
Separate into:
- High impact, low risk
- High impact, higher risk
- Nice-to-have"

PHASE 3: Execute Safely
Start with low-risk changes:
"Apply the low-risk improvements first."

Then proceed to higher-risk changes with review at each step.

PHASE 4: Verify
"Compare before and after. 
- Is functionality preserved?
- Are there any subtle behavior changes?
- Did we miss anything?"
```

---

## Task-Specific Workflows

### Writing Tests

```
Context:
@file-to-test

"Analyze this code and identify:
1. Public interface to test
2. Key behaviors to verify
3. Edge cases to cover
4. Error conditions to test

Then generate tests following our testing conventions."
```

For thorough coverage:
```
"What cases are missing from these tests?
@existing-tests

Consider:
- Boundary conditions
- Error paths
- Integration scenarios
- Race conditions (if concurrent)"
```

### Code Review Preparation

Before submitting a PR:
```
"Review these changes as a careful code reviewer:

Changes:
[git diff or list of modified files]

Check for:
- Bugs or logic errors
- Security issues
- Performance concerns
- Missing error handling
- Violations of our conventions
- Missing tests

Be critical. I want to find issues before reviewers do."
```

### Documentation Generation

```
"Generate documentation for this code:
@file-to-document

Include:
- Module/file overview
- Public API documentation
- Usage examples
- Any important notes or caveats

Follow our documentation style:
[reference to existing docs or style guide]"
```

### Performance Investigation

```
"This code has performance issues. Analyze for:
@slow-code

1. Algorithmic complexity (are there O(n^2) operations?)
2. Unnecessary allocations
3. Repeated computations
4. Database/IO inefficiencies
5. Missing caching opportunities

For each issue, explain the impact and suggest improvements."
```

---

## Multi-File Workflows

### Feature Across Multiple Files

```
Using Composer (Cmd+I):

"Implement [feature] across these files:
@file1 - Add the data model
@file2 - Add the API endpoint  
@file3 - Add the UI component
@file4 - Add tests

Requirements:
[requirements]

Implement all files consistently."
```

### Consistent Renaming/Refactoring

```
"Rename 'OldName' to 'NewName' across the codebase:
@codebase

This should update:
- Class/function definitions
- All references
- Import statements
- Test files
- Documentation

Show me the full list of changes before applying."
```

### Pattern Migration

```
"Migrate from [old pattern] to [new pattern] in:
@directory/

Example of old pattern:
[code example]

Example of new pattern:
[code example]

Apply this migration to all files in the directory."
```

---

## Conversation Patterns

### The Socratic Debug

When you're stuck, have the AI ask you questions:

```
"I'm stuck on this bug. Instead of giving me a solution, 
ask me questions that will help me figure it out myself.
Start with diagnostic questions."
```

### The Rubber Duck

For thinking through problems:

```
"I'm going to explain my approach to solving [problem]. 
Listen and point out any issues with my reasoning.

My thinking:
[your explanation]"
```

### The Second Opinion

When unsure about a decision:

```
"I'm considering [approach]. Before I commit to this:
1. What are the arguments against this approach?
2. What alternatives should I consider?
3. What could go wrong?

Play devil's advocate."
```

### The Checklist

For thorough verification:

```
"Before I deploy this change, generate a checklist of things 
to verify. Be thorough - I don't want to miss anything."
```

---

## Workflow Combinations

### New Feature (Complete)

```
1. Exploration Workflow - Understand affected areas
2. Implementation Workflow - Build the feature
3. Writing Tests - Ensure coverage
4. Code Review Preparation - Self-review
5. Documentation Generation - Update docs
```

### Production Bug Fix

```
1. Bug Fix Workflow - Find and fix issue
2. Writing Tests - Prevent regression
3. Performance Investigation - Ensure fix doesn't degrade performance
4. Code Review Preparation - Final check
```

### Technical Debt Reduction

```
1. Exploration Workflow - Understand current state
2. Refactoring Workflow - Improve incrementally
3. Writing Tests - Add missing coverage
4. Documentation Generation - Update stale docs
```

---

## Workflow Anti-Patterns

### The Blind Faith Pattern

**Don't:**
```
"Implement [complex feature]"
[Accept without review]
[Deploy]
```

**Do:**
```
"Implement [complex feature]"
[Review carefully]
[Test manually]
[Verify against requirements]
[Then accept]
```

### The Kitchen Sink Pattern

**Don't:**
```
"Implement authentication, add dark mode, optimize database queries, 
update all tests, and refactor the API layer."
```

**Do:**
Break into separate, focused sessions.

### The Context Amnesia Pattern

**Don't:**
Assume the AI remembers previous sessions.

**Do:**
Re-establish context at the start of each session:
```
"We're continuing work on [feature]. Context:
- We completed [X]
- We're working on [Y]
- The current issue is [Z]"
```

---

## Workflow Customization

### Capture Your Patterns

When you find a workflow that works:

```markdown
## My Feature Development Workflow

### Before Starting
1. ...

### Implementation
1. ...

### Verification
1. ...

### Definition of Done
- [ ] ...
```

### Team Workflows

Share effective workflows with your team:

```markdown
## Team Standard: Bug Fix Process

1. Create branch from main
2. Reproduce bug locally
3. Use Bug Fix Workflow to identify and fix
4. Add regression test
5. Self-review with Code Review Preparation
6. PR with clear description
7. Wait for CI before merging
```

---

*Workflows are habits. Practice them until they're automatic. The consistency will compound into significant productivity gains over time.*

