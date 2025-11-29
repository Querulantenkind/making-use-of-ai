# GPT

## OpenAI's Foundational Models

GPT (Generative Pre-trained Transformer) represents OpenAI's flagship model family and remains the most widely deployed large language model in production. Its ecosystem maturity, broad capabilities, and extensive tooling make it a default choice for many applications.

---

## The GPT-4 Family

### GPT-4o
**The balanced flagship**

- **Context:** 128K tokens
- **Best at:** General tasks, multimodal (vision), function calling
- **Character:** Capable, fast, well-rounded
- **Cost tier:** Mid-high

GPT-4o ("o" for "omni") represents OpenAI's current balanced offering. It handles text and images natively, responds quickly, and maintains high quality across diverse tasks.

### GPT-4 Turbo
**The workhorse**

- **Context:** 128K tokens
- **Best at:** Complex tasks, JSON mode, reliable function calling
- **Character:** Thorough, instruction-following
- **Cost tier:** Higher

GPT-4 Turbo remains excellent for tasks requiring careful instruction following and structured outputs. Its JSON mode is particularly reliable for API integrations.

### GPT-4o-mini
**Efficiency at scale**

- **Context:** 128K tokens
- **Best at:** High-volume tasks, cost-sensitive applications
- **Character:** Fast, capable, economical
- **Cost tier:** Budget

GPT-4o-mini delivers impressive capability at a fraction of the cost. It's often the right choice for production systems processing high volumes.

### o1-series (o1-preview, o1-mini)
**Advanced reasoning**

- **Context:** 128K tokens
- **Best at:** Mathematics, logic, complex multi-step reasoning
- **Character:** Deliberate, thorough, reasoning-focused
- **Cost tier:** Premium

The o1 series represents a different approach: models trained to "think" through problems before responding. They excel at tasks requiring genuine reasoning but are slower and more expensive.

---

## GPT's Distinctive Characteristics

### What GPT Does Well

**Broad Knowledge Base**
GPT models demonstrate extensive knowledge across domains. They handle obscure topics, specialized terminology, and cross-domain questions effectively.

**Function Calling & Tool Use**
OpenAI pioneered reliable function calling. GPT models excel at:
- Deciding when to use tools
- Extracting appropriate parameters
- Handling multi-step tool chains

**Structured Outputs**
JSON mode and structured output features produce reliable, parseable responses. This matters for production integrations where unpredictable formats cause failures.

**Multimodal Understanding**
GPT-4 Vision capabilities are mature and reliable. Image analysis, document understanding, and visual reasoning work well across diverse inputs.

**Ecosystem & Integrations**
The OpenAI ecosystem is extensive:
- ChatGPT for consumer access
- API with comprehensive documentation
- Assistants API for stateful interactions
- Wide third-party tool support

### GPT's Limitations

**Can Prioritize Helpfulness Over Accuracy**
GPT models sometimes provide confident-sounding answers when "I'm not sure" would be more appropriate. Verification remains essential.

**Verbosity**
Responses can be longer than necessary, especially for simple questions. Explicit brevity instructions help.

**Cost at Scale**
For high-volume applications, costs add up. Careful model selection and prompt optimization matter.

**Occasional Instruction Drift**
In long conversations, GPT can drift from initial instructions. Periodic reinforcement of key constraints helps.

---

## When to Choose GPT

### Strongly Prefer GPT For:

- **Tool-heavy workflows** - Function calling is best-in-class
- **Multimodal tasks** - Vision capabilities are mature
- **Structured output needs** - JSON mode is reliable
- **Broad knowledge requirements** - Extensive training data
- **Existing OpenAI integration** - Ecosystem benefits

### Consider Alternatives For:

- **Code-heavy work** - Claude often produces cleaner code
- **Nuanced analysis** - Claude tends to be more thorough
- **Cost-sensitive high-volume** - Open source may be better
- **Privacy-critical** - Consider local models

---

## Working With GPT Effectively

### System Message Philosophy

GPT responds well to system messages that:
- Clearly establish the assistant's role
- Set explicit constraints and guidelines
- Specify output format requirements
- Include examples for complex formats

```
You are a technical assistant helping with software development.

Guidelines:
- Provide working code, not pseudocode
- Include error handling for common edge cases
- Explain your reasoning briefly
- If you're unsure about something, say so

Output format:
- Start with a brief explanation (2-3 sentences)
- Provide the code
- List any assumptions made
```

### Prompting Tips Specific to GPT

**Leverage JSON mode for structured tasks:**
```python
response = client.chat.completions.create(
    model="gpt-4o",
    response_format={"type": "json_object"},
    messages=[{
        "role": "user",
        "content": "Extract entities from this text as JSON with keys: 'people', 'places', 'organizations'. Text: ..."
    }]
)
```

**Use function calling for tool integration:**
```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get current weather for a location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {"type": "string"},
                "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
            },
            "required": ["location"]
        }
    }
}]
```

**Be explicit about response length:**
```
Provide a brief response (under 100 words).
```
```
Give a comprehensive analysis. Length is not a concern; thoroughness is.
```

**Reinforce instructions for long conversations:**
```
Remember: maintain the technical, direct tone established at the start. 
Continue with the analysis...
```

---

## API Basics

### Chat Completions

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Hello!"}
    ]
)

print(response.choices[0].message.content)
```

### Available Models

```
gpt-4o                    # Latest GPT-4o
gpt-4o-mini              # Efficient variant
gpt-4-turbo              # GPT-4 Turbo
o1-preview               # Advanced reasoning
o1-mini                  # Efficient reasoning
```

### Key Parameters

| Parameter | Description | Typical Values |
|-----------|-------------|----------------|
| `model` | Model identifier | See above |
| `max_tokens` | Maximum response length | Varies by model |
| `temperature` | Randomness (0-2) | 0-0.3 for deterministic, 0.7-1 for creative |
| `response_format` | Output format | `{"type": "json_object"}` |
| `tools` | Available functions | Array of function definitions |
| `tool_choice` | Function calling behavior | "auto", "required", or specific function |

---

## Integration Patterns

### With Function Calling

```python
import json
from openai import OpenAI

client = OpenAI()

def get_weather(location: str) -> dict:
    # Your implementation
    return {"location": location, "temperature": 22, "conditions": "sunny"}

tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get current weather for a location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {"type": "string", "description": "City name"}
            },
            "required": ["location"]
        }
    }
}]

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "What's the weather in Tokyo?"}],
    tools=tools,
    tool_choice="auto"
)

# Handle tool calls
if response.choices[0].message.tool_calls:
    tool_call = response.choices[0].message.tool_calls[0]
    args = json.loads(tool_call.function.arguments)
    result = get_weather(**args)
    
    # Continue conversation with result
    messages = [
        {"role": "user", "content": "What's the weather in Tokyo?"},
        response.choices[0].message,
        {"role": "tool", "tool_call_id": tool_call.id, "content": json.dumps(result)}
    ]
    
    final_response = client.chat.completions.create(
        model="gpt-4o",
        messages=messages
    )
```

### With Vision

```python
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "What's in this image?"},
            {"type": "image_url", "image_url": {"url": "https://example.com/image.jpg"}}
        ]
    }]
)
```

### With Assistants API

For stateful, multi-turn interactions with file handling:

```python
assistant = client.beta.assistants.create(
    name="Code Helper",
    instructions="You are a Python expert. Help with code questions.",
    tools=[{"type": "code_interpreter"}],
    model="gpt-4o"
)

thread = client.beta.threads.create()

message = client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="Write a function to find prime numbers"
)

run = client.beta.threads.runs.create_and_poll(
    thread_id=thread.id,
    assistant_id=assistant.id
)

messages = client.beta.threads.messages.list(thread_id=thread.id)
```

---

## Practical Examples

### Example 1: Structured Data Extraction

```python
response = client.chat.completions.create(
    model="gpt-4o",
    response_format={"type": "json_object"},
    messages=[{
        "role": "system",
        "content": """Extract structured data from resumes. Return JSON with:
        {
            "name": "string",
            "email": "string",
            "phone": "string or null",
            "skills": ["array", "of", "skills"],
            "experience_years": "number",
            "education": [{"degree": "string", "institution": "string", "year": "number"}]
        }"""
    }, {
        "role": "user",
        "content": "[resume text here]"
    }]
)
```

### Example 2: Multi-Step Analysis

```
Analyze this codebase for potential improvements. Work through this systematically:

Step 1: Identify the main components and their responsibilities
Step 2: Assess coupling between components
Step 3: Identify potential issues (bugs, performance, security)
Step 4: Prioritize improvements by impact/effort
Step 5: Provide specific recommendations for the top 3 priorities

[codebase excerpt]
```

### Example 3: Image Analysis

```python
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": "This is a screenshot of an error. What's the issue and how do I fix it?"
            },
            {
                "type": "image_url",
                "image_url": {"url": f"data:image/png;base64,{base64_image}"}
            }
        ]
    }]
)
```

---

## Model Selection Within GPT Family

| Use Case | Recommended | Reasoning |
|----------|-------------|-----------|
| General tasks | GPT-4o | Best balance of capability and speed |
| High volume | GPT-4o-mini | Cost-effective, still capable |
| Complex reasoning | o1-preview | Designed for multi-step reasoning |
| Math/logic puzzles | o1-mini | Efficient reasoning model |
| Tool integration | GPT-4o | Best function calling |
| Vision tasks | GPT-4o | Native multimodal |

---

## Further Reading

- [GPT Prompting Tips](prompting-tips.md) - Model-specific strategies
- [System Prompts for GPT](system-prompts.md) - Ready-to-use templates
- [API Reference](api-reference.md) - Complete API guide
- [Known Quirks](quirks.md) - Behaviors to be aware of

---

*GPT's strength is versatility and ecosystem. It handles most tasks well and integrates smoothly with existing workflows. Master its unique features, tool calling, JSON mode, vision, and you unlock significant capability.*

