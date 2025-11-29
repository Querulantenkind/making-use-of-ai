# References

## Quick Lookups and Technical Specifications

This section provides fast access to facts, numbers, and specifications you need during AI work. Keep this handy for quick reference when context windows, pricing, or terminology questions arise.

---

## Reference Index

| Reference | Purpose |
|-----------|---------|
| [Terminology](terminology.md) | Glossary of AI terms and concepts |
| [Token Limits](token-limits.md) | Context windows and limits by model |
| [Pricing Comparison](pricing-comparison.md) | Cost breakdown across providers |
| [API Quick Reference](api-quick-reference.md) | Common API patterns |

---

## Quick Facts

### Context Windows (2024)

| Model | Context Window |
|-------|----------------|
| Claude 3.5 Sonnet | 200K tokens |
| Claude 3 Opus | 200K tokens |
| GPT-4o | 128K tokens |
| GPT-4 Turbo | 128K tokens |
| Gemini 1.5 Pro | 1M+ tokens |
| Llama 3.1 | 128K tokens |

### Token Estimation

- **English text**: ~1 token = 0.75 words (~4 characters)
- **Code**: ~1 token = 2-3 characters (more tokens than prose)
- **1,000 words**: ~1,300-1,500 tokens
- **1 page of text**: ~400-500 tokens
- **1 source code file**: ~500-2000 tokens (varies widely)

### Pricing Tiers (2024 Estimates)

| Tier | Cost per 1M tokens (input) | Example Models |
|------|---------------------------|----------------|
| Premium | $10-15+ | Claude 3 Opus, o1-preview |
| Standard | $2.50-5 | Claude 3.5 Sonnet, GPT-4o |
| Budget | $0.10-0.50 | GPT-4o-mini, Claude Haiku |
| Free/Local | Infrastructure only | Llama, Mistral |

---

## Common Questions

### How many tokens is my document?

**Quick estimation:**
- Count words, multiply by 1.3
- Or: character count / 4

**Accurate count:**
- Use provider's tokenizer
- OpenAI: [tiktoken library](https://github.com/openai/tiktoken)
- Anthropic: API returns token counts

### Which model should I use?

| Situation | Recommendation |
|-----------|----------------|
| Default choice | Claude 3.5 Sonnet or GPT-4o |
| Cost-sensitive | GPT-4o-mini or Claude Haiku |
| Maximum quality | Claude 3 Opus or o1-preview |
| Very long documents | Gemini 1.5 Pro |
| Privacy required | Local Llama |

### How do I reduce costs?

1. Use appropriate model tier (don't use Opus for simple tasks)
2. Minimize context (only include relevant files)
3. Use efficient prompts (clear, concise)
4. Cache repeated operations
5. Batch similar requests

---

## Navigation

- **Need a term explained?** [Terminology](terminology.md)
- **Checking context limits?** [Token Limits](token-limits.md)
- **Comparing costs?** [Pricing Comparison](pricing-comparison.md)
- **Setting up an API?** [API Quick Reference](api-quick-reference.md)

---

*References are for quick answers. If you need deeper understanding, see the [Guides](../guides/) section.*

