# Testing Prompts

## Prompts for Writing Tests, Test Planning, and Test Analysis

Testing is where AI assistance can dramatically accelerate development. These prompts help you generate comprehensive tests, identify edge cases, and improve test quality.

---

## Unit Test Generation

### Basic Unit Test Generation

```
Write unit tests for the following code:

[paste your code]

Requirements:
- Use [testing framework: pytest/jest/junit/etc.]
- Cover all public methods
- Include happy path and error cases
- Use descriptive test names that explain what is being tested
- Add brief comments explaining the purpose of non-obvious tests
```

### Comprehensive Unit Test Generation

```
Generate comprehensive unit tests for this code:

[paste your code]

I need:
1. Happy path tests for normal operation
2. Edge case tests (empty inputs, boundary values, null/undefined)
3. Error handling tests (invalid inputs, exceptions)
4. Tests for any side effects
5. Mock setup for external dependencies

Testing framework: [framework]
Mocking library: [library, if applicable]

Follow these conventions:
- [your team's testing conventions]
```

### Test Generation with Existing Style

```
Here is my existing test file:

[paste existing tests]

Add tests for this new functionality:

[paste new code]

Match the existing:
- Naming conventions
- Test structure
- Assertion style
- Mock patterns
```

---

## Edge Case Discovery

### Find Edge Cases

```
Analyze this function and identify all edge cases that should be tested:

[paste your code]

For each edge case:
1. Describe the scenario
2. Explain why it matters
3. Provide example input
4. State expected behavior

Focus on:
- Boundary conditions
- Empty/null/undefined inputs
- Type coercion issues
- Concurrency concerns (if applicable)
- Resource limits
```

### Edge Case Test Generation

```
Generate tests specifically for edge cases in this code:

[paste your code]

For each edge case test:
- Name the test descriptively (test_[scenario]_[expected_behavior])
- Include a comment explaining why this edge case matters
- Use realistic edge case values

Do not include happy path tests - focus only on edge cases and boundaries.
```

---

## Test-Driven Development

### TDD: Generate Tests First

```
I want to implement the following functionality:

[describe the feature/function in detail]

Write the tests first (TDD style). Include:
1. Tests that define the expected interface
2. Tests for core functionality
3. Tests for edge cases
4. Tests for error handling

The tests should fail initially (no implementation exists yet).
After the tests, provide a minimal implementation that makes all tests pass.

Framework: [testing framework]
```

### TDD: Add Feature to Existing Code

```
I need to add this feature to existing code:

Existing code:
[paste existing code]

Existing tests:
[paste existing tests]

New feature:
[describe the feature]

Provide:
1. New tests for the feature (following existing test style)
2. The minimal code changes to make tests pass
```

---

## Test Quality Improvement

### Test Review

```
Review these tests for quality and completeness:

Code under test:
[paste implementation]

Current tests:
[paste tests]

Analyze:
1. Coverage gaps - what's not tested?
2. Test quality - are assertions meaningful?
3. Test isolation - are tests independent?
4. Test clarity - are test names descriptive?
5. Edge cases - what's missing?
6. Brittleness - will tests break on valid refactoring?

Provide specific recommendations for improvement.
```

### Improve Test Assertions

```
These tests pass but might not catch all bugs. Improve the assertions:

[paste tests]

For each test:
1. Identify what could slip through current assertions
2. Add more specific assertions
3. Add negative assertions where appropriate (verify what should NOT happen)
4. Consider testing side effects, not just return values
```

### Reduce Test Duplication

```
These tests have duplication. Refactor to reduce repetition while maintaining clarity:

[paste tests]

Apply appropriate patterns:
- Parameterized tests for similar cases
- Setup/teardown for common initialization
- Helper functions for repeated assertions
- Fixtures for shared test data

Maintain test independence and readability.
```

---

## Integration and E2E Testing

### Integration Test Generation

```
Write integration tests for this component:

Component code:
[paste component]

Dependencies it interacts with:
[list dependencies: database, API, file system, etc.]

Generate tests that verify:
1. Correct interaction with real dependencies (or realistic mocks)
2. Data flows correctly through the system
3. Error handling when dependencies fail
4. Transaction/rollback behavior (if applicable)

Test framework: [framework]
Note any setup requirements (test database, mock servers, etc.)
```

### API Endpoint Testing

```
Write tests for this API endpoint:

Endpoint: [method] [path]
Handler code:
[paste handler]

Generate tests covering:
1. Successful request with valid data
2. Request validation (missing fields, invalid types)
3. Authentication/authorization (if applicable)
4. Error responses
5. Edge cases in request data

Include:
- Request setup
- Expected response status codes
- Response body assertions
- Header assertions (if relevant)

Framework: [jest/supertest/pytest/etc.]
```

---

## Mock and Stub Generation

### Generate Mocks

```
Create mocks for these dependencies used in my tests:

Dependencies:
[paste interfaces/classes to mock]

Code that uses them:
[paste code under test]

Provide:
1. Mock implementations with configurable behavior
2. Spy setup to verify calls
3. Common mock configurations for different test scenarios
4. Reset/cleanup helpers

Mocking library: [library]
```

### Mock External API

```
Create a mock for this external API used in my code:

API documentation/examples:
[paste API examples or describe the API]

My code that calls it:
[paste code]

Provide:
1. Mock that simulates successful responses
2. Mock configurations for error responses
3. Mock for edge cases (empty data, pagination, rate limits)
4. Verification helpers to check call parameters
```

---

## Test Data Generation

### Generate Test Fixtures

```
Generate test fixtures/factories for this data model:

Model:
[paste model/schema/type definition]

Provide:
1. Factory function that generates valid instances
2. Variations for common test scenarios
3. Edge case data (minimum values, maximum length, special characters)
4. Invalid data examples for negative testing

Use: [factory library if applicable, or plain functions]
```

### Generate Realistic Test Data

```
Generate realistic test data for:

[describe the data type: users, orders, transactions, etc.]

Schema/type:
[paste schema]

I need:
1. 5-10 realistic examples for happy path testing
2. Edge case examples (boundary values, special characters, unicode)
3. Invalid examples for validation testing

Data should be realistic but obviously fake (no real personal information).
```

---

## Property-Based Testing

### Generate Property Tests

```
Create property-based tests for this function:

[paste function]

Identify properties that should always hold:
1. Invariants (what should always be true)
2. Idempotency (if applicable)
3. Symmetry (encode/decode, serialize/deserialize)
4. Bounds (output should always be within range)

Generate tests using: [hypothesis/fast-check/etc.]
Include example-based tests for specific important cases.
```

---

## Test Debugging

### Debug Failing Test

```
This test is failing and I don't understand why:

Test:
[paste failing test]

Code under test:
[paste implementation]

Error message:
[paste error]

Help me understand:
1. Why the test is failing
2. Whether the bug is in the test or the implementation
3. How to fix it
```

### Fix Flaky Test

```
This test passes sometimes and fails other times:

[paste flaky test]

Code under test:
[paste implementation]

Analyze potential causes:
1. Timing/race conditions
2. Shared state between tests
3. External dependencies
4. Order dependence

Provide a fix that makes the test deterministic.
```

---

## Coverage Analysis

### Analyze Coverage Gaps

```
Here's my code and test coverage report:

Code:
[paste code]

Current tests:
[paste tests]

Coverage report:
[paste coverage report or describe uncovered lines]

For each uncovered section:
1. Explain what scenario would hit that code
2. Determine if it's worth testing
3. If yes, provide the test
4. If no, explain why (dead code, defensive programming, etc.)
```

---

## Quick Reference

### Test Naming Convention

```
Tests should be named: test_[unit]_[scenario]_[expected_behavior]

Examples:
- test_calculate_total_empty_cart_returns_zero
- test_user_login_invalid_password_raises_auth_error
- test_parse_date_leap_year_handles_feb_29
```

### Test Structure (AAA Pattern)

```
def test_example():
    # Arrange - set up test data and conditions
    input_data = create_test_data()
    
    # Act - execute the code under test
    result = function_under_test(input_data)
    
    # Assert - verify the results
    assert result == expected_value
```

---

*Good tests are documentation that runs. They explain what the code should do and verify that it does it. Write tests that future you will thank past you for.*
