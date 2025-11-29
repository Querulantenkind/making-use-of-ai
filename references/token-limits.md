# Token Limits

## Context Windows and Constraints by Model

Token limits determine how much information you can include in a single interaction. This reference provides current limits for major models and strategies for working within them.

---

## Context Windows by Model

### Anthropic (Claude)

| Model | Context Window | Notes |
|-------|----------------|-------|
| Claude 3.5 Sonnet | 200K tokens | Standard for most use |
| Claude 3 Opus | 200K tokens | Full context, premium quality |
| Claude 3.5 Haiku | 200K tokens | Fast, maintains full context |

**Output limits:** Configurable up to model maximum

### OpenAI (GPT)

| Model | Context Window | Notes |
|-------|----------------|-------|
| GPT-4o | 128K tokens | Standard flagship |
| GPT-4o-mini | 128K tokens | Same window, lower cost |
| GPT-4 Turbo | 128K tokens | Reliable workhorse |
| o1-preview | 128K tokens | Reasoning model |
| o1-mini | 128K tokens | Efficient reasoning |

**Output limits:** 
- Standard: 4K-16K tokens (configurable)
- With response format: Can be higher

### Google (Gemini)

| Model | Context Window | Notes |
|-------|----------------|-------|
| Gemini 1.5 Pro | 1M tokens (2M in preview) | Largest context available |
| Gemini 1.5 Flash | 1M tokens | Fast + long context |
| Gemini 1.0 Pro | 32K tokens | Previous generation |

**Output limits:** 8K-64K tokens depending on model

### Open Source / Local

| Model | Context Window | Notes |
|-------|----------------|-------|
| Llama 3.1 (8B/70B/405B) | 128K tokens | With RoPE scaling |
| Mistral Large | 128K tokens | |
| Mixtral 8x22B | 64K tokens | MoE architecture |
| Qwen 2 | 32K-128K tokens | Varies by size |
| Phi-3 | 4K-128K tokens | Varies by variant |

---

## Token Estimation

### Rules of Thumb

**English text:**
- 1 token ~ 4 characters
- 1 token ~ 0.75 words
- 1,000 words ~ 1,300 tokens

**Code:**
- More tokens per character than prose
- 1 token ~ 2-3 characters
- Heavily dependent on language and style

**Common file sizes:**
| Content | Approximate Tokens |
|---------|-------------------|
| One paragraph | 50-100 tokens |
| One page of text | 400-500 tokens |
| Short function (20 lines) | 100-200 tokens |
| Medium source file (200 lines) | 500-1,500 tokens |
| Large source file (1,000 lines) | 2,500-5,000 tokens |
| README.md | 500-2,000 tokens |
| API documentation | 2,000-10,000 tokens |

### Calculating Tokens

**Python (OpenAI):**
```python
import tiktoken

encoding = tiktoken.encoding_for_model("gpt-4o")
tokens = encoding.encode("Your text here")
print(f"Token count: {len(tokens)}")
```

**Python (Anthropic):**
```python
# Anthropic's API returns token counts in response
# Use response.usage.input_tokens and response.usage.output_tokens
```

**Online tools:**
- OpenAI Tokenizer: https://platform.openai.com/tokenizer
- Various third-party tokenizer tools

---

## Working Within Limits

### When Context is Tight

**Prioritize information:**
1. Most relevant code/content first
2. System instructions
3. Supporting context
4. Examples (if space permits)

**Reduce context:**
- Include only relevant files, not entire codebase
- Summarize long documents
- Use function signatures instead of full implementations
- Reference external docs instead of including them

**Optimize prompts:**
- Be concise (every word costs tokens)
- Avoid redundant instructions
- Use abbreviations consistently
- Remove verbose formatting

### When Context is Large

**Leverage effectively:**
- Include full files when relationships matter
- Provide more examples for few-shot learning
- Include related code for better context
- Add documentation alongside code

**But watch for:**
- Diminishing returns on very long context
- Quality can degrade at the edges of context
- Cost increases linearly with tokens
- Processing time increases

---

## Context Budget Planning

### For a 128K Token Budget

| Allocation | Tokens | Use |
|------------|--------|-----|
| System prompt | 500-1,000 | Persona, instructions |
| Core files | 10,000-30,000 | Main code under discussion |
| Related files | 20,000-50,000 | Supporting context |
| Examples | 2,000-5,000 | Few-shot patterns |
| User message | 1,000-5,000 | Current task |
| Response buffer | 4,000-16,000 | Model's response |
| **Reserve** | 20,000+ | Safety margin |

### For a 200K Token Budget

More room for:
- Full codebase context
- Extensive documentation
- Multiple iterations without clearing

### For a 1M Token Budget (Gemini)

Enables:
- Entire small-to-medium codebases
- Multiple related documents
- Long transcripts or logs
- Complex multi-document analysis

---

## Token Costs

### Input vs Output Tokens

Most providers charge differently:
- **Input tokens**: Your prompt, context, history
- **Output tokens**: Model's response

Output tokens typically cost 3-5x more than input tokens.

### Cost Optimization

**Reduce input tokens:**
- Minimize context
- Compress instructions
- Use references instead of full content

**Reduce output tokens:**
- Set appropriate max_tokens
- Request concise responses
- Use structured formats

---

## Handling Context Overflow

### Signs of Overflow

- Truncation errors from API
- Model forgetting earlier context
- Inconsistent references to provided information

### Solutions

**Chunking:**
Break large content into processable pieces:
```
Process this document in sections.
I'll provide one section at a time.
Summarize each, then I'll ask for synthesis.

Section 1:
[content]
```

**Summarization:**
Compress earlier context:
```
Here's a summary of our discussion so far:
[summary]

Now continuing with:
[new content]
```

**Selective context:**
Include only what's needed:
```
For this specific question, here's the relevant code:
[specific function]

(Full file available if needed: user_service.py)
```

**Rolling context:**
For long conversations, periodically summarize and reset:
```
Let me summarize where we are before continuing:
[summary of key points and decisions]

Now let's address:
[next topic]
```

---

## Model-Specific Considerations

### Claude
- Uses full context effectively
- Quality remains high across long context
- Cost scales linearly

### GPT-4o
- Strong across full context
- May lose some details at extremes
- Consider retrieval augmentation for very large contexts

### Gemini
- Designed for massive context
- Quality can vary with very long contexts
- Test with your specific use case

### Local Models
- Nominal context often exceeds effective context
- Quality degrades faster than cloud models
- Consider shorter context with better prompts

---

*Token limits are constraints to work within, not problems to solve once. Build context-conscious habits and you'll rarely hit limits unexpectedly.*

