# Decision Analysis Prompts

## Prompts for Structured Decision Making

Good decisions emerge from clear thinking about options, trade-offs, and consequences. These prompts help you analyze decisions systematically and avoid common decision-making pitfalls.

---

## Decision Framing

### Frame the Decision

```
Help me frame this decision properly:

Situation: [describe what's happening]
Why I need to decide: [trigger for the decision]
Initial options I see: [what I'm considering]

Help me:
1. Define the actual decision to be made
2. Identify what I'm really optimizing for
3. Clarify constraints vs. preferences
4. Find options I might be missing
5. Determine if this is reversible or irreversible
6. Decide how much time/effort this decision deserves

Make sure I'm solving the right problem.
```

### Identify Decision Type

```
What type of decision is this?

Decision: [describe the decision]

Classify it:
1. Reversible vs. irreversible
2. High stakes vs. low stakes
3. Time-sensitive vs. can wait
4. Individual vs. group decision
5. One-time vs. recurring
6. Certain vs. uncertain outcomes

Based on the type, recommend:
- How much analysis is warranted
- What decision framework to use
- Who should be involved
- How fast to decide
```

---

## Option Analysis

### Evaluate Options

```
Help me evaluate these options:

Decision: [what I'm deciding]
Options:
1. [Option A]
2. [Option B]
3. [Option C]

Criteria that matter:
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]

For each option, analyze:
1. How it performs on each criterion
2. Pros and cons
3. Risks and how to mitigate them
4. Hidden costs or benefits
5. Second-order effects

Create a comparison matrix and recommendation.
```

### Weighted Decision Matrix

```
Create a weighted decision matrix for:

Decision: [what I'm deciding]
Options: [list options]
Criteria: [list factors that matter]

For this analysis:
1. Help me assign weights to criteria (should sum to 100%)
2. Score each option on each criterion (1-10)
3. Calculate weighted scores
4. Identify which criteria swing the decision
5. Sensitivity analysis: how robust is the winner?

Guide me through the weighting process.
```

### Identify Hidden Options

```
I'm stuck between these options:

Option A: [describe]
Option B: [describe]

Help me find alternatives I might be missing:
1. Combinations of A and B
2. Completely different approaches
3. "Do nothing" option analysis
4. Delayed decision option
5. Reversible experiments before committing
6. Options I've ruled out that deserve reconsideration

Expand my solution space.
```

---

## Risk and Consequence Analysis

### Pre-Mortem Analysis

```
Perform a pre-mortem for this decision:

Decision: [what I'm planning to do]

Imagine it's [time period] from now and this failed badly.

Analyze:
1. What went wrong? (brainstorm failure modes)
2. What were the warning signs we missed?
3. What assumptions proved false?
4. What risks did we underestimate?
5. What did we fail to consider?

Then:
6. How can we prevent each failure mode?
7. What early warning systems should we set up?
8. What's our contingency plan?
```

### Consequence Mapping

```
Map the consequences of this decision:

Decision: [what I'm considering]

Analyze consequences:
1. Immediate effects (days)
2. Short-term effects (weeks/months)
3. Long-term effects (years)
4. Effects on different stakeholders
5. Reversibility of each effect
6. Second and third-order effects

Consider:
- Best case scenario
- Worst case scenario
- Most likely scenario

What consequences am I underweighting?
```

### Risk-Adjusted Analysis

```
Analyze risks for these options:

Options:
1. [Option A]
2. [Option B]

For each option:
1. List potential risks
2. Estimate probability (low/medium/high)
3. Estimate impact (low/medium/high)
4. Risk mitigation strategies
5. Risk vs. reward trade-off

Create a risk matrix and identify:
- Deal-breaker risks
- Manageable risks
- Acceptable risks
```

---

## Trade-off Analysis

### Explicit Trade-offs

```
Help me understand the trade-offs in this decision:

Decision: [what I'm deciding]
Options: [list options]

For each trade-off:
1. What am I giving up?
2. What am I gaining?
3. Is this trade-off reversible?
4. Am I trading short-term for long-term (or vice versa)?
5. Am I trading certain for uncertain?

Help me see trade-offs I might be ignoring.
```

### Time vs. Quality vs. Cost

```
Analyze this through the lens of time, quality, and cost:

Project/Decision: [describe]
Current constraints: [what's fixed]

If I optimize for TIME:
- What quality and cost trade-offs?
- What are the risks?

If I optimize for QUALITY:
- What time and cost trade-offs?
- What are the risks?

If I optimize for COST:
- What time and quality trade-offs?
- What are the risks?

What's the minimum viable point for each dimension?
```

---

## Bias and Blind Spot Check

### Challenge My Thinking

```
Challenge my decision:

I'm deciding to: [your planned decision]
My reasoning: [why you think this is right]

Play devil's advocate:
1. What am I wrong about?
2. What am I not seeing?
3. What biases might be affecting me?
4. What would someone who disagrees say?
5. What evidence would change my mind?
6. Am I solving the right problem?

Be direct and don't spare my feelings.
```

### Bias Check

```
Check this decision for common biases:

Decision: [describe]
My reasoning: [how I arrived at this]

Check for:
1. Confirmation bias - am I seeking confirming evidence?
2. Sunk cost fallacy - am I factoring in past investments?
3. Status quo bias - am I defaulting to "no change"?
4. Recency bias - am I overweighting recent events?
5. Availability bias - am I influenced by vivid examples?
6. Anchoring - am I stuck on an initial number/idea?
7. Overconfidence - am I too certain?

For each bias found, suggest how to correct for it.
```

### Seek Disconfirming Evidence

```
I believe [your conclusion/decision]:

Evidence supporting this:
[list your supporting evidence]

Help me find disconfirming evidence:
1. What would prove me wrong?
2. Where should I look for contrary evidence?
3. Who would disagree and why?
4. What are the strongest counterarguments?
5. What have I conveniently ignored?
6. What questions haven't I asked?

Update my confidence level based on this analysis.
```

---

## Decision Timing

### Should I Decide Now?

```
Help me determine if I should decide now:

Decision: [what I'm considering]
Pressure to decide: [why I feel urgency]
Cost of waiting: [what I lose by delaying]
Benefit of waiting: [what I gain by delaying]

Analyze:
1. Is the urgency real or artificial?
2. What information would I gain by waiting?
3. Do options disappear over time?
4. Is there a natural deadline?
5. Can I make a reversible decision now and a final one later?

Recommend: Decide now vs. wait for [specific trigger].
```

### Decision Journal Entry

```
Help me document this decision for future review:

Decision: [what I decided]
Date: [when]

Document:
1. What were my options?
2. What did I decide and why?
3. What trade-offs am I accepting?
4. What am I uncertain about?
5. What would make me reconsider?
6. When should I review this decision?
7. How will I know if this was the right choice?

Format as a decision journal entry.
```

---

## Technical Decisions

### Architecture Decision Record

```
Help me create an ADR for:

Decision: [what technical decision]
Context: [background and problem]

Include:
1. Title
2. Status (proposed/accepted/deprecated)
3. Context - why we're making this decision
4. Decision - what we decided
5. Options considered - with pros/cons
6. Consequences - positive and negative
7. When to revisit

Keep it concise but complete.
```

### Technology Selection

```
Help me choose between technologies:

Use case: [what I need to solve]
Options:
1. [Technology A]
2. [Technology B]
3. [Technology C]

Evaluation criteria:
- Technical fit
- Team expertise
- Community/support
- Long-term viability
- Cost (time, money, maintenance)
- Risk level

Create a structured comparison and recommendation.
Account for my specific context, not just general advice.
```

---

## Quick Reference

### Decision Quality Checklist

```
Before finalizing a decision, verify:

[ ] Problem is clearly defined
[ ] Multiple options were considered
[ ] Key criteria are explicit
[ ] Trade-offs are understood
[ ] Risks are identified and acceptable
[ ] Biases have been checked
[ ] Reversibility is understood
[ ] Decision is proportionate to stakes
[ ] Timeline is appropriate
[ ] Success criteria are defined
```

### Quick Decision Frameworks

**10/10/10**: How will I feel about this in 10 minutes? 10 months? 10 years?

**Regret minimization**: Which choice minimizes future regret?

**Two-way door**: Is this reversible? If yes, decide fast. If no, deliberate carefully.

**Even over**: "We value X even over Y" - forces priority clarity.

---

*Good decisions aren't about being right every time. They're about having a reliable process that leads to good outcomes over time. These prompts help you build that process.*

