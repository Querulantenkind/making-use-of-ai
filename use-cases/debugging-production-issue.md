# Debugging Production Issues

## Systematic Bug Investigation with AI Assistance

This walkthrough demonstrates using AI to investigate and resolve a production issue. The approach emphasizes systematic diagnosis over trial-and-error fixes.

---

## The Scenario

**Situation:** Users report intermittent 500 errors on the checkout endpoint. The issue started after a recent deployment and affects roughly 10% of requests.

**Available information:**
- Error logs showing generic "Internal Server Error"
- Application metrics showing increased error rate
- Recent deployment with multiple changes
- Access to codebase and staging environment

---

## Phase 1: Information Gathering

### Framing the Problem

Start by organizing what you know:

```
I'm debugging a production issue. Let me share what I know:

SYMPTOMS:
- 500 errors on POST /api/checkout
- Started: Tuesday after 14:00 deployment
- Frequency: ~10% of requests
- User reports: "payment failed" errors

DEPLOYMENT CHANGES:
- Updated payment processing SDK (v2.3 -> v2.5)
- Added inventory validation step
- Refactored order creation logic

ERROR LOG SAMPLE:
[paste representative error with stack trace]

WHAT I'VE CHECKED:
- Database connections: healthy
- Payment provider status: no outages
- Server resources: normal

Help me create a systematic investigation plan.
```

### Getting an Investigation Framework

The AI should help you structure your approach:

```
Good framework. Before diving into code, help me:

1. List the most likely root causes in order of probability
2. For each cause, what evidence would confirm or rule it out?
3. What additional information should I gather?

I want to narrow down before I start changing things.
```

---

## Phase 2: Log Analysis

### Understanding the Stack Trace

```
Here's the full stack trace from one of the errors:

```
Traceback (most recent call last):
  File "/app/handlers/checkout.py", line 145, in process_checkout
    order = await create_order(cart, user)
  File "/app/services/order.py", line 89, in create_order
    inventory_check = await validate_inventory(items)
  File "/app/services/inventory.py", line 34, in validate_inventory
    result = await inventory_client.check_batch(item_ids)
  File "/app/clients/inventory.py", line 67, in check_batch
    response = await self.session.post(url, json=payload, timeout=5)
asyncio.TimeoutError
```

Analyze this trace:
1. What's the immediate cause?
2. What questions does this raise?
3. What should I investigate next?
```

### Correlating with Metrics

```
The stack trace points to inventory client timeouts. 

Here's additional data:
- Inventory service response times (p99): 2s -> 8s after deployment
- Inventory service error rate: unchanged
- The timeout in our client is set to 5s

Why would inventory service latency increase after our deployment
if the inventory service itself wasn't changed?
```

---

## Phase 3: Code Investigation

### Reviewing the Changes

```
The new inventory validation was added in this commit.
Here's the relevant code:

```python
# checkout.py - new code
async def process_checkout(cart: Cart, user: User) -> Order:
    # NEW: validate inventory before processing
    for item in cart.items:
        await validate_inventory(item)  # Called per item!
    
    # ... rest of checkout
```

```python
# inventory.py - existing code
async def validate_inventory(item: CartItem) -> bool:
    # Makes HTTP call to inventory service
    result = await inventory_client.check(item.sku, item.quantity)
    return result.available
```

Do you see the problem?
```

### Confirming the Hypothesis

```
You identified the N+1 problem - we're making one HTTP call per item
instead of batching.

Let me verify:
1. Average cart has 5-7 items
2. Inventory service response time: ~1.5s per call
3. Total time: 7.5-10.5s for inventory checks alone
4. Our timeout: 5s

This explains why ~10% fail (larger carts).

Before I fix this, help me understand:
1. Why didn't this fail in staging?
2. How should I verify this is the actual cause?
```

---

## Phase 4: Implementing the Fix

### Designing the Solution

```
Confirmed: staging data had smaller carts (1-2 items), so timeouts didn't occur.

Now let's fix it. I need to:
1. Batch inventory checks into a single call
2. Maintain the same validation logic
3. Not break anything else

Show me the fixed implementation with:
- The batched validation function
- Updated checkout code
- Appropriate error handling
```

### Review and Refinement

```
The implementation looks good, but I have concerns:

1. What if one item in the batch fails? Do we fail the whole checkout?
2. The inventory service has a batch limit of 50 items - what if cart exceeds that?
3. How do we handle partial inventory availability?

Update the implementation to handle these cases.
```

### Adding Safety Measures

```
Add these safety measures to the fix:

1. Fallback to sequential calls if batch endpoint fails
2. Logging to track batch vs sequential usage
3. Metric for inventory check duration

I want visibility into whether this fix is working.
```

---

## Phase 5: Testing the Fix

### Local Verification

```
Help me create a test plan for this fix:

1. Unit tests for the new batching logic
2. Integration test simulating various cart sizes
3. Test cases for error scenarios:
   - Partial inventory availability
   - Inventory service timeout
   - Batch size exceeding limit
```

### Staging Validation

```
Before deploying to production, I need to validate in staging.

Create a test script that:
1. Generates carts of various sizes (1, 5, 10, 20, 50, 100 items)
2. Runs checkout flow for each
3. Measures response time
4. Reports success/failure rate

I'll run this against staging to verify the fix.
```

### Production Deployment Plan

```
The fix is validated in staging. Help me create a safe deployment plan:

1. Rollout strategy (canary? percentage-based?)
2. Metrics to monitor during rollout
3. Rollback criteria - what would trigger reverting?
4. Communication plan for stakeholders
```

---

## Phase 6: Post-Incident

### Root Cause Documentation

```
The fix is deployed and working. Help me write a post-incident report:

Incident summary:
- Duration: 4 hours
- Impact: ~10% of checkouts failed
- Root cause: N+1 inventory API calls causing timeouts

Include:
1. Timeline of events
2. Root cause analysis
3. What went well
4. What could be improved
5. Action items to prevent recurrence
```

### Preventing Similar Issues

```
This bug made it to production because:
1. Code review didn't catch the N+1 pattern
2. Staging data didn't represent production
3. No performance tests on checkout flow

Help me create:
1. A code review checklist item for this pattern
2. Staging data requirements for realistic testing
3. A simple performance test for the checkout endpoint
```

---

## Key Debugging Patterns

### The Funnel Approach

```
WIDE: What are all possible causes?
     ↓
NARROW: What evidence supports/refutes each?
     ↓  
FOCUSED: What specific thing is broken?
     ↓
FIX: Change only what's necessary
```

### The Timeline Method

For issues that "started after X":
1. List all changes at time X
2. For each change, how could it cause this symptom?
3. Find the change that best explains the evidence

### The Replication Method

Before fixing, replicate:
1. Can I reproduce locally?
2. Can I reproduce in staging?
3. What conditions trigger it?

Understanding reproduction helps verify the fix.

---

## Prompts for Common Debugging Situations

### When You Have a Stack Trace

```
Here's a stack trace from production:
[trace]

1. What's the immediate cause?
2. What's the underlying cause?
3. What should I investigate?
```

### When You Don't Have a Stack Trace

```
Users report [symptom] but I have no error logs.

The feature works by:
[explain the flow]

What could cause [symptom] without throwing an error?
What logging should I add to diagnose this?
```

### When the Bug is Intermittent

```
This bug affects ~X% of requests.

What patterns could cause intermittent failures?
- Race conditions?
- Resource exhaustion?
- External dependency issues?
- Data-dependent logic?

What data would help distinguish between these?
```

### When You're Stuck

```
I've been debugging this for [time] and I'm stuck.

What I know: [summary]
What I've tried: [list]
Current hypothesis: [your best guess]

What am I missing? What should I try next?
```

---

## Common Pitfalls

### Pitfall: Fixing Symptoms, Not Causes

**Problem:** Adding a retry fixes the timeout, but the N+1 is still there.
**Solution:** Always ask "why is this happening?" not just "how do I make it stop?"

### Pitfall: Not Verifying the Fix

**Problem:** Fix looks right but doesn't actually solve the issue.
**Solution:** Create a test that fails before the fix and passes after.

### Pitfall: Incomplete Investigation

**Problem:** You find *a* bug and assume it's *the* bug.
**Solution:** Verify your fix resolves the original symptom in production.

### Pitfall: Deploying Under Pressure

**Problem:** Rush to fix causes new bugs.
**Solution:** Even urgent fixes need review and staged rollout.

---

*Debugging with AI isn't about asking "fix this bug." It's about using AI as a thinking partner to systematically narrow down and resolve issues.*

