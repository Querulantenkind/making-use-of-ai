# Understanding Legacy Code

## Onboarding to an Unfamiliar Codebase with AI Assistance

This walkthrough demonstrates how to rapidly understand a codebase you've never seen before. The techniques apply whether you're joining a new team, evaluating a project, or inheriting maintenance responsibility.

---

## The Scenario

**Situation:** You've just joined a team maintaining a 3-year-old web application. The original developers have left, documentation is sparse, and you need to make changes within your first week.

**The codebase:**
- Backend: Python/FastAPI
- Frontend: React/TypeScript
- ~50,000 lines of code
- Partial documentation, some outdated

**Your goal:** Understand enough to confidently make changes without breaking things.

---

## Phase 1: Bird's Eye View

### Initial Orientation

Start with structure, not details:

```
I'm onboarding to an unfamiliar codebase. Here's the top-level structure:

```
project/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── services/
│   │   └── utils/
│   ├── tests/
│   └── alembic/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── services/
│   │   └── utils/
│   └── tests/
└── docker/
```

Based on this structure:
1. What architecture pattern does this follow?
2. Where would I find the main entry points?
3. What should I explore first to understand the overall flow?
```

### Understanding the Domain

```
I found these key model files. What domain is this application for?

```python
# models/order.py
class Order(Base):
    id: int
    user_id: int
    status: OrderStatus
    items: List[OrderItem]
    shipping_address_id: int
    payment_id: Optional[int]
    
# models/product.py  
class Product(Base):
    id: int
    sku: str
    name: str
    price: Decimal
    inventory_count: int
    category_id: int
```

What's the likely purpose of this application?
What other models would you expect to find?
```

### Mapping Key Flows

```
This is an e-commerce platform. Help me understand the main user flows:

1. What endpoints handle the checkout process? (I found these in api/)
2. Trace the order creation flow from API to database
3. How does payment processing fit in?

Here are the relevant files:
[list key files or paste route definitions]
```

---

## Phase 2: Dependency Mapping

### External Dependencies

```
Here's the requirements.txt (or package.json):
[paste dependencies]

1. What are the critical dependencies I need to understand?
2. Any deprecated or concerning packages?
3. What does each major dependency do in this context?
```

### Internal Dependencies

```
Help me understand how these modules depend on each other.

I see imports like:
- api/ imports from services/
- services/ imports from models/ and utils/
- core/ is imported by almost everything

Draw me a dependency diagram and explain the layering.
```

### Identifying Coupling Points

```
Looking at the codebase, what areas seem tightly coupled?
What would be risky to change?

I noticed:
- OrderService is imported in 15 places
- The User model has 30+ fields
- There's a utils/helpers.py with 2000 lines

Are these red flags? What should I understand about each?
```

---

## Phase 3: Critical Path Analysis

### Finding Entry Points

```
I need to understand how requests flow through the system.

Here's the main router setup:
[paste router configuration]

And a representative endpoint:
[paste one complex endpoint]

Walk me through what happens when a user hits this endpoint.
```

### Tracing a Feature

Pick a specific feature and trace it completely:

```
I need to understand the "add to cart" feature completely.

Starting from the frontend button click, trace through:
1. Frontend component and state management
2. API call and validation
3. Backend service logic
4. Database operations
5. Response handling

Show me the relevant files for each step.
```

### Understanding Error Handling

```
How does this codebase handle errors?

I found:
- Custom exception classes in core/exceptions.py
- Error handlers in api/middleware.py
- Try/except blocks scattered in services/

Help me understand the error handling strategy:
1. How are errors raised?
2. How are they caught and transformed?
3. What does the user see when something fails?
```

---

## Phase 4: Testing Landscape

### Understanding Test Coverage

```
Analyze the test structure:

```
tests/
├── unit/
│   ├── test_services/
│   └── test_utils/
├── integration/
│   └── test_api/
└── fixtures/
```

1. What testing strategy is being used?
2. Are there gaps in coverage?
3. How do I run tests locally?
```

### Learning from Tests

Tests often document intended behavior:

```
These tests seem to document important business rules:

```python
def test_order_cannot_exceed_inventory():
    ...

def test_discount_applies_before_tax():
    ...

def test_shipping_free_above_threshold():
    ...
```

Explain what business rules these tests encode.
What other rules might exist that aren't tested?
```

---

## Phase 5: Configuration and Environment

### Environment Setup

```
Help me understand the configuration:

.env.example shows:
```
DATABASE_URL=
REDIS_URL=
STRIPE_API_KEY=
SENDGRID_API_KEY=
FEATURE_FLAG_SERVICE=
```

1. What external services does this application depend on?
2. What do I need to set up locally?
3. Are there feature flags I should know about?
```

### Deployment Understanding

```
I found these deployment-related files:
- docker-compose.yml
- kubernetes/
- .github/workflows/

Help me understand:
1. How is this deployed?
2. What environments exist?
3. What's the deployment pipeline?
```

---

## Phase 6: Making Your First Change

### Scoping a Safe First Task

```
I need to make a small change to prove I understand the codebase.
The task: Add a "gift message" field to orders.

Before I start, help me:
1. List all files that might need changes
2. Identify risks or gotchas
3. Plan the implementation order
4. Determine what tests I need
```

### Implementation with Verification

```
I'll add the gift message field. Walk me through:

Step 1: Database change
- Add column to Order model
- Create migration

Let me do this first and verify before continuing.
```

After each step, test before moving on.

### Post-Change Verification

```
Changes are complete. Help me verify:

1. What manual testing should I do?
2. What could I have broken inadvertently?
3. What should the PR description include?
```

---

## Strategies for Different Codebase Sizes

### Small Codebase (<10k lines)

- Read through most files directly
- Build complete mental model
- Focus on understanding all flows

### Medium Codebase (10k-100k lines)

- Focus on key paths first
- Build understanding incrementally
- Use AI to summarize sections

### Large Codebase (100k+ lines)

- Map the architecture first
- Deep dive only where needed
- Rely heavily on AI for summarization
- Build understanding over weeks, not days

---

## Useful Prompts for Exploration

### "What does this file do?"

```
Explain what this file does at a high level:
[paste file]

1. What's its purpose?
2. What does it depend on?
3. What depends on it?
4. Any notable patterns or concerns?
```

### "How does this feature work?"

```
Trace the [feature name] feature through the codebase.

I know it starts at [starting point].
Walk me through each layer until [end point].
Note any complex or unusual parts.
```

### "Is this code safe to change?"

```
I need to modify this function:
[paste function]

1. What calls this function?
2. What could break if I change its behavior?
3. What tests cover this?
4. How should I approach the change safely?
```

### "Why was this done this way?"

```
This code seems unusual:
[paste code]

Why might it have been written this way?
Is there historical context I should know?
Should it be refactored, or is there a good reason for it?
```

---

## Building Your Understanding Map

Create a running document as you explore:

```markdown
# [Project Name] Understanding Map

## Architecture
- Pattern: [MVC/Clean Architecture/etc.]
- Key layers: [list]

## Critical Paths
- User registration: [files involved]
- Checkout: [files involved]

## External Dependencies
- Payment: Stripe via services/payment.py
- Email: SendGrid via services/email.py

## Known Issues / Tech Debt
- [issue from code comments or observation]

## Questions to Clarify
- Why is [X] done this way?
- What's the history behind [Y]?

## Safe Zones (okay to modify)
- [area]

## Danger Zones (be careful)
- [area]
```

---

## Common Pitfalls

### Pitfall: Trying to Understand Everything

**Problem:** Spending days reading code before doing anything.
**Solution:** Understand enough to complete a specific task, then expand.

### Pitfall: Not Validating Understanding

**Problem:** You think you understand, but you're wrong.
**Solution:** Make small changes and observe results. Ask questions.

### Pitfall: Ignoring Tests

**Problem:** Missing the documentation that tests provide.
**Solution:** Tests often show intended behavior better than comments.

### Pitfall: Not Taking Notes

**Problem:** Re-learning the same things.
**Solution:** Document as you go. Your notes help you and future team members.

---

*Understanding legacy code is a skill. AI accelerates the process but doesn't replace careful exploration. Build your mental model piece by piece, verify as you go, and document for the future.*

