# Claude

## Anthropic's Thoughtful Powerhouse

Claude represents Anthropic's approach to building AI that is helpful, harmless, and honest. The model family has earned a reputation for nuanced understanding, excellent code generation, and thoughtful analysis that often surfaces considerations you hadn't thought to ask about.

---

## The Claude 3 Family

### Claude 3.5 Sonnet
**The everyday workhorse**

- **Context:** 200K tokens
- **Best at:** Code generation, analysis, technical writing, general tasks
- **Character:** Fast, capable, excellent balance of quality and speed
- **Cost tier:** Mid-range

Claude 3.5 Sonnet hits the sweet spot for most tasks. It's significantly faster than Opus while maintaining high quality. For development work especially, it's often the optimal choice.

### Claude 3 Opus
**Maximum capability**

- **Context:** 200K tokens
- **Best at:** Complex reasoning, nuanced analysis, difficult problems
- **Character:** Thorough, considered, willing to engage with complexity
- **Cost tier:** Premium

Opus is the model you reach for when the problem is genuinely hard. It shows its value in multi-step reasoning, complex analysis, and situations requiring maximum capability.

### Claude 3.5 Haiku
**Speed and efficiency**

- **Context:** 200K tokens
- **Best at:** Quick tasks, high-volume processing, real-time applications
- **Character:** Fast, concise, capable for straightforward tasks
- **Cost tier:** Budget

Haiku excels when speed matters more than maximum capability. It's excellent for summarization, classification, and other well-defined tasks.

---

## Claude's Distinctive Characteristics

### What Claude Does Well

**Code Generation & Review**
Claude produces clean, well-structured code and excels at understanding existing codebases. It tends to:
- Include appropriate error handling
- Add helpful comments without over-explaining
- Follow language idioms and best practices
- Catch subtle bugs during review

**Nuanced Analysis**
Claude engages thoughtfully with complex topics, often identifying considerations and trade-offs without being asked. It handles ambiguity well and admits uncertainty rather than confabulating.

**Instruction Following**
Claude closely follows detailed instructions. Complex, multi-part prompts work well. It respects formatting requirements and output structure specifications reliably.

**Long-Form Content**
With a 200K context window, Claude handles substantial documents effectively. Its quality remains high across long outputs, making it excellent for documentation, reports, and detailed analysis.

**Honest Uncertainty**
Claude tends to acknowledge when it's uncertain rather than presenting guesses as facts. It will say "I'm not sure" or "this could be wrong" when appropriate.

### Claude's Limitations

**Occasional Over-Caution**
Claude can be conservative on edge cases, sometimes declining requests that are actually fine. This reflects Anthropic's safety-focused training.

**Can Be Verbose**
Claude sometimes provides more context and explanation than requested. Explicit instructions for brevity help.

**Knowledge Cutoff**
Like all models, Claude's training data has a cutoff. It will explicitly acknowledge when asked about events or developments beyond its knowledge.

**Less Tool Ecosystem**
Compared to OpenAI's GPT, Claude has fewer native tool integrations. This gap is narrowing but remains.

---

## When to Choose Claude

### Strongly Prefer Claude For:

- **Development workflows** - Code quality is excellent
- **Code review** - Catches issues others miss
- **Technical documentation** - Clear, accurate, well-structured
- **Complex analysis** - Surfaces relevant considerations
- **Long documents** - Maintains quality across length
- **Nuanced writing** - Avoids cliches and generic outputs

### Consider Alternatives For:

- **Multimodal tasks** - GPT-4 Vision has more features currently
- **Tool-heavy workflows** - OpenAI's function calling is more mature
- **Maximum speed** - Some competitors are faster for simple tasks
- **Specific integrations** - Depends on what tools you're using

---

## Working With Claude Effectively

### System Prompt Philosophy

Claude responds well to system prompts that:
- Establish clear expectations and constraints
- Specify the role or perspective to adopt
- Indicate formatting preferences
- Set quality bars and standards

```
You are a senior software engineer reviewing code for a production 
deployment. Your reviews are thorough but practical, focusing on:
- Correctness and edge cases
- Security implications
- Performance considerations
- Maintainability

Flag issues by severity (critical/major/minor). Be direct and specific.
Avoid generic advice; point to specific lines and explain concrete impacts.
```

### Prompting Tips Specific to Claude

**Be direct about what you want.** Claude follows instructions well, so specificity pays off:
```
# Good
Review this function for SQL injection vulnerabilities specifically. 
Assume all other aspects are correct.

# Less effective
Review this function for security issues.
```

**Request reasoning when it matters.** Claude's explanations are often valuable:
```
Suggest a data structure for this use case and explain why it's 
appropriate. Consider time complexity for the operations we'll 
need most frequently.
```

**Use the full context window when helpful.** Claude handles long contexts well:
```
Here is the full source file for context. Focus your review on the 
function starting at line 145, but reference other parts of the file 
if they're relevant to your analysis.
```

**Explicitly request brevity if needed.** Claude tends toward thoroughness:
```
Provide a brief answer (2-3 sentences max). I can ask follow-ups 
if I need more detail.
```

---

## API Basics

### Message Structure

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    system="You are a helpful assistant.",
    messages=[
        {"role": "user", "content": "Hello, Claude"}
    ]
)

print(message.content[0].text)
```

### Available Models

```
claude-3-5-sonnet-20241022  # Latest Sonnet
claude-3-opus-20240229       # Opus
claude-3-haiku-20240307      # Haiku
```

### Key Parameters

| Parameter | Description | Typical Values |
|-----------|-------------|----------------|
| `model` | Model identifier | See above |
| `max_tokens` | Maximum response length | 1024-4096 typical |
| `temperature` | Randomness (0-1) | 0 for deterministic, 0.7 for creative |
| `system` | System prompt | Role/context setting |
| `messages` | Conversation history | User/assistant turns |

---

## Integration Patterns

### With Cursor

Claude works excellently with Cursor IDE. Key configuration:
- Use Claude 3.5 Sonnet as default for speed and quality
- Switch to Opus for complex debugging sessions
- Include project context in .cursorrules for consistency

### With LangChain

```python
from langchain_anthropic import ChatAnthropic

llm = ChatAnthropic(model="claude-3-5-sonnet-20241022")
response = llm.invoke("Your prompt here")
```

### Direct API

See [API Reference](api-reference.md) for comprehensive examples.

---

## Practical Examples

### Example 1: Code Review Request

```
Review this Python function for:
1. Correctness - does it do what the docstring says?
2. Edge cases - what inputs might cause problems?
3. Style - does it follow Python best practices?

Be specific. Reference line numbers. Provide fixed code for any issues found.

```python
def merge_sorted_lists(list1: list, list2: list) -> list:
    """Merge two sorted lists into a single sorted list."""
    result = []
    i = j = 0
    while i < len(list1) and j < len(list2):
        if list1[i] <= list2[j]:
            result.append(list1[i])
            i += 1
        else:
            result.append(list2[j])
            j += 1
    result.extend(list1[i:])
    result.extend(list2[j:])
    return result
```
```

### Example 2: Architecture Discussion

```
I'm designing a notification system for a SaaS application. Requirements:
- Multiple channels (email, SMS, push, in-app)
- User preferences per channel
- Rate limiting to prevent spam
- Delivery tracking and retry logic
- Must scale to 10M users

Before proposing an architecture, ask me clarifying questions about 
anything ambiguous or any requirements I might have forgotten to mention.
```

### Example 3: Documentation Generation

```
Generate API documentation for this endpoint in OpenAPI 3.0 format.
Include realistic examples for request/response bodies.

[endpoint code]
```

---

## Further Reading

- [Claude Prompting Tips](prompting-tips.md) - Detailed strategies
- [System Prompts for Claude](system-prompts.md) - Ready-to-use templates
- [API Reference](api-reference.md) - Complete API guide
- [Known Quirks](quirks.md) - Behaviors to be aware of

---

*Claude rewards clarity and specificity. Tell it exactly what you want, provide the context it needs, and it will deliver thoughtful, high-quality outputs.*

