# Models

## Understanding the Engines That Power AI

This section provides comprehensive documentation for the major AI models you'll encounter. Each model family has its own personality: distinct strengths, characteristic weaknesses, and optimal use patterns. Understanding these differences transforms model selection from guesswork into strategy.

---

## The Landscape at a Glance

### Frontier Models

| Provider | Model | Context | Best For |
|----------|-------|---------|----------|
| Anthropic | Claude 3.5 Sonnet | 200K | Coding, analysis, nuanced writing |
| Anthropic | Claude 3 Opus | 200K | Complex reasoning, long-form content |
| OpenAI | GPT-4o | 128K | General purpose, multimodal, broad knowledge |
| OpenAI | GPT-4 Turbo | 128K | Complex tasks, JSON mode, function calling |
| Google | Gemini 1.5 Pro | 1M+ | Very long context, multimodal |
| OpenAI | o1-series | 128K | Advanced reasoning, mathematics, logic |

### Efficient Models

| Provider | Model | Context | Best For |
|----------|-------|---------|----------|
| Anthropic | Claude 3.5 Haiku | 200K | Fast tasks, cost efficiency |
| OpenAI | GPT-4o-mini | 128K | Balanced quality/cost |
| Google | Gemini 1.5 Flash | 1M | Fast multimodal processing |
| Meta | Llama 3.1 (8B-405B) | 128K | Self-hosted, privacy, customization |
| Mistral | Various | 32K-128K | European hosting, open weights |

### Specialized Models

| Model | Specialty | Notes |
|-------|-----------|-------|
| DeepSeek Coder | Code generation | Strong coding, competitive cost |
| CodeLlama | Code generation | Meta's coding-specific model |
| Mixtral 8x22B | Efficient inference | MoE architecture |
| Phi-3 | Small footprint | Microsoft's efficient model |

---

## Model Deep Dives

### [Claude](claude/)
Anthropic's model family, known for:
- Exceptional code generation and review
- Nuanced, thoughtful analysis
- Strong instruction following
- Careful, considered responses
- Constitutional AI safety approach

**Start with:** [Claude Overview](claude/overview.md)

### [GPT](gpt/)
OpenAI's flagship models, known for:
- Broad general knowledge
- Strong function calling and structured outputs
- Extensive tool integrations
- Established ecosystem
- Frequent capability updates

**Start with:** [GPT Overview](gpt/overview.md)

### [Gemini](gemini/)
Google's multimodal models, known for:
- Massive context windows
- Native multimodal capabilities
- Integration with Google ecosystem
- Strong factual grounding

**Start with:** [Gemini Overview](gemini/overview.md)

### [Llama & Open Source](llama/)
Meta's open-weight models and the broader ecosystem:
- Self-hosting capability
- Privacy and data control
- Fine-tuning flexibility
- Active community
- Competitive capability at scale

**Start with:** [Llama Overview](llama/overview.md)

### [Local Models](local-models/)
Running AI on your own hardware:
- Complete privacy
- Offline capability
- Customization freedom
- Hardware considerations

**Start with:** [Local Models Overview](local-models/overview.md)

---

## Quick Comparison: Strengths & Weaknesses

### Claude

**Strengths:**
- Code quality and debugging
- Long-form analysis and writing
- Nuanced understanding of complex requests
- Honest about uncertainty
- Excellent at following detailed instructions

**Weaknesses:**
- Can be overly cautious on edge cases
- Less extensive tool ecosystem than GPT
- Occasional verbosity

**Best for:** Development workflows, code review, detailed analysis, technical writing

---

### GPT-4

**Strengths:**
- Broad knowledge base
- Strong function calling
- Mature ecosystem and tools
- Reliable JSON output
- Multimodal capability (vision)

**Weaknesses:**
- Can be more verbose than necessary
- Sometimes prioritizes helpfulness over accuracy
- Higher cost for comparable quality

**Best for:** General purpose tasks, tool integration, multimodal work, established workflows

---

### Gemini

**Strengths:**
- Exceptional context length (up to 1M+ tokens)
- Native multimodal design
- Google integration (Search, Docs, etc.)
- Strong factual grounding

**Weaknesses:**
- Less mature than competitors in some areas
- Ecosystem still developing
- Availability varies by region

**Best for:** Very long documents, multimodal tasks, Google ecosystem users

---

### Llama / Open Source

**Strengths:**
- Privacy (data never leaves your control)
- No API costs at scale
- Fine-tuning capability
- Community and customization

**Weaknesses:**
- Requires infrastructure
- Lower ceiling on complex tasks
- Setup and maintenance overhead

**Best for:** Privacy-sensitive work, high-volume processing, specialized fine-tuning

---

## Model Selection by Task

### Quick Reference Matrix

| Task | First Choice | Alternative | Budget Option |
|------|--------------|-------------|---------------|
| Code generation | Claude 3.5 Sonnet | GPT-4o | DeepSeek Coder |
| Code review | Claude 3.5 Sonnet | GPT-4o | Llama 3.1 70B |
| Debugging | Claude 3 Opus | GPT-4o | Claude 3.5 Sonnet |
| Technical docs | Claude 3.5 Sonnet | GPT-4o | GPT-4o-mini |
| Creative writing | Claude 3.5 Sonnet | GPT-4o | Llama 3.1 70B |
| Data analysis | GPT-4o | Claude 3.5 Sonnet | Gemini 1.5 Flash |
| Long documents | Gemini 1.5 Pro | Claude 3.5 Sonnet | Gemini 1.5 Flash |
| Summarization | Claude 3.5 Haiku | GPT-4o-mini | Llama 3.1 8B |
| Translation | GPT-4o | Claude 3.5 Sonnet | Llama 3.1 70B |
| Image analysis | GPT-4o | Claude 3.5 Sonnet | Gemini 1.5 Flash |
| Math/reasoning | o1-preview | Claude 3 Opus | GPT-4o |
| Privacy-critical | Local Llama | Self-hosted Mistral | Local Phi-3 |

---

## Model Economics

### Cost vs. Quality Trade-off

```
                    Higher Quality
                         ^
                         |
          [o1-preview]   |   [Claude 3 Opus]
                         |
    [GPT-4o] -----+------+------+----- [Claude 3.5 Sonnet]
                  |      |      |
                  |  [Gemini    |
                  |   1.5 Pro]  |
                  |      |      |
    [GPT-4o-mini]-+------+------+----- [Claude 3.5 Haiku]
                         |
          [Llama 3.1]    |   [Gemini 1.5 Flash]
                         |
                    Lower Cost
    <------------------- Lower Cost | Higher Cost ------------------->
```

### When to Pay Premium

**Worth the premium cost:**
- Complex debugging that's blocking development
- Critical analysis that needs maximum accuracy
- One-time tasks where quality matters most
- Learning/exploration (understanding model capabilities)

**Use efficient models:**
- High-volume processing
- Simpler, well-defined tasks
- Draft generation before refinement
- Real-time/interactive use where speed matters

---

## Staying Current

The AI landscape evolves rapidly:

- **Model updates**: Providers frequently update models (sometimes without announcement)
- **New releases**: Major new models appear every few months
- **Pricing changes**: Competition drives frequent price adjustments
- **Capability shifts**: Relative strengths change as models improve

**How to stay informed:**
- Follow provider changelogs and blogs
- Monitor benchmark comparisons (with skepticism)
- Test periodically with your specific use cases
- Participate in AI communities for real-world reports

---

## Documentation Structure

Each model's documentation follows a consistent structure:

```
models/[model-name]/
├── overview.md           # Capabilities, strengths, weaknesses, when to use
├── prompting-tips.md     # Model-specific prompting strategies
├── system-prompts.md     # Effective system prompts for this model
├── api-reference.md      # API patterns and code examples
└── quirks.md            # Documented behaviors and edge cases
```

This consistency lets you quickly find relevant information regardless of which model you're working with.

---

*Models are tools with personalities. Learn each one's character and you'll know when to reach for it. Start with one, master it, then expand your toolkit.*

