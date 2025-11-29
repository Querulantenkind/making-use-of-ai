# Guides

## The Foundation of Effective AI Collaboration

This section contains the conceptual groundwork and methodological frameworks that underpin everything else in this repository. Before diving into specific models, agents, or prompts, invest time here. The principles you learn will compound across every interaction you have with AI systems.

---

## Guide Index

| Guide | Description | Reading Time |
|-------|-------------|--------------|
| [Getting Started](getting-started.md) | Mental models, core concepts, and your first effective prompts | 15 min |
| [Prompt Engineering 101](prompt-engineering-101.md) | The fundamental techniques that form the basis of all advanced work | 25 min |
| [Advanced Prompting](advanced-prompting.md) | Sophisticated strategies for complex tasks and edge cases | 30 min |
| [Choosing the Right Model](choosing-the-right-model.md) | A framework for matching AI capabilities to your specific needs | 20 min |

---

## Reading Order

**If you're new to working with AI models:**

```
Getting Started --> Prompt Engineering 101 --> Choosing the Right Model
```

These three guides form the core curriculum. They'll transform you from someone who uses AI tools to someone who leverages them effectively.

**If you're already experienced:**

Jump directly to [Advanced Prompting](advanced-prompting.md), then use the other guides as reference material when you encounter specific challenges.

---

## Key Concepts Preview

### The Context Window

Every AI model operates within a finite context window, measured in tokens (roughly 3/4 of a word). This window contains your conversation history, any documents you've provided, and the model's responses. When this window fills, older content gets truncated.

Understanding context windows is fundamental to effective prompting:
- Plan your conversations to preserve important information
- Front-load critical context rather than burying it
- Know when to start fresh versus continue a conversation

### The System Prompt

The system prompt (or system message) sets the behavioral parameters for a model before your conversation begins. It's where you define personas, establish constraints, and shape how the model will respond.

Mastering system prompts is the difference between generic outputs and precisely tailored assistance.

### Temperature and Creativity

Models have adjustable parameters that control their output characteristics. Temperature is the most common:
- **Low temperature (0.0-0.3)**: More deterministic, focused, consistent
- **High temperature (0.7-1.0)**: More creative, varied, surprising

Match temperature to task. Code generation wants low temperature. Brainstorming wants high.

### The Feedback Loop

AI collaboration is iterative. Your first prompt rarely produces the perfect result, and that's expected. The skill lies in:
1. Analyzing why a response missed the mark
2. Formulating targeted follow-ups
3. Learning patterns that work for future prompts

---

## Principles That Appear Throughout

These themes recur across all guides:

**Specificity defeats ambiguity.** Vague prompts produce vague results. Every detail you specify is a constraint that narrows the output space toward your intent.

**Context is king.** Models know nothing about you, your project, or your preferences unless you tell them. Assume nothing is understood; state everything that matters.

**Structure enables complexity.** Complex tasks become manageable when broken into structured components. Use formatting, numbered steps, and clear sections.

**Iteration beats perfection.** Don't spend twenty minutes crafting the perfect first prompt. Spend five minutes on a reasonable prompt, then refine based on what you learn.

---

## Supplementary Materials

As you work through these guides, you may want to reference:

- [Terminology Glossary](../references/terminology.md) for unfamiliar terms
- [Token Limits Reference](../references/token-limits.md) for model-specific context windows
- [Prompt Library](../prompts/) for practical examples of techniques discussed

---

*The goal is not to memorize techniques but to develop intuition. Read actively. Experiment constantly. The guides point the direction; you must walk the path.*

