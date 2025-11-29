# API Quick Reference

## Common Patterns for AI API Integration

Quick reference for the most common API operations across major providers.

---

## Anthropic (Claude)

### Basic Completion

```python
from anthropic import Anthropic

client = Anthropic()

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    system="You are a helpful assistant.",
    messages=[
        {"role": "user", "content": "Hello, Claude!"}
    ]
)

print(message.content[0].text)
```

### Streaming

```python
with client.messages.stream(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Write a story."}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

### With Images

```python
import base64

with open("image.png", "rb") as f:
    image_data = base64.b64encode(f.read()).decode()

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {"type": "image", "source": {
                "type": "base64",
                "media_type": "image/png",
                "data": image_data
            }},
            {"type": "text", "text": "What's in this image?"}
        ]
    }]
)
```

### Tool Use

```python
tools = [{
    "name": "get_weather",
    "description": "Get current weather for a location",
    "input_schema": {
        "type": "object",
        "properties": {
            "location": {"type": "string", "description": "City name"}
        },
        "required": ["location"]
    }
}]

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "What's the weather in Paris?"}]
)

# Check if tool was called
if message.stop_reason == "tool_use":
    tool_use = next(b for b in message.content if b.type == "tool_use")
    # Execute tool and continue conversation
```

---

## OpenAI (GPT)

### Basic Completion

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

### Streaming

```python
stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Write a story."}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

### JSON Mode

```python
response = client.chat.completions.create(
    model="gpt-4o",
    response_format={"type": "json_object"},
    messages=[{
        "role": "user",
        "content": "Return a JSON object with name and age fields."
    }]
)

import json
data = json.loads(response.choices[0].message.content)
```

### Function Calling

```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get current weather",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {"type": "string"}
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
    # Execute function and continue
```

### Vision

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

---

## Google (Gemini)

### Basic Completion

```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")
model = genai.GenerativeModel('gemini-1.5-pro')

response = model.generate_content("Hello!")
print(response.text)
```

### With Images

```python
import PIL.Image

image = PIL.Image.open("image.png")
response = model.generate_content(["Describe this image:", image])
print(response.text)
```

### Streaming

```python
response = model.generate_content("Write a story.", stream=True)
for chunk in response:
    print(chunk.text, end="", flush=True)
```

### Chat Session

```python
chat = model.start_chat()
response = chat.send_message("Hello!")
print(response.text)

response = chat.send_message("Follow-up question")
print(response.text)
```

---

## Local Models (Ollama)

### Python Client

```python
import ollama

response = ollama.chat(
    model='llama3.1:8b',
    messages=[{'role': 'user', 'content': 'Hello!'}]
)
print(response['message']['content'])
```

### OpenAI-Compatible API

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="not-needed"
)

response = client.chat.completions.create(
    model="llama3.1:8b",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

### Streaming

```python
for chunk in ollama.chat(
    model='llama3.1:8b',
    messages=[{'role': 'user', 'content': 'Write a story.'}],
    stream=True
):
    print(chunk['message']['content'], end='', flush=True)
```

---

## Common Patterns

### Error Handling

```python
from anthropic import APIError, RateLimitError
import time

def with_retry(func, max_retries=3):
    for attempt in range(max_retries):
        try:
            return func()
        except RateLimitError:
            if attempt < max_retries - 1:
                time.sleep(2 ** attempt)  # Exponential backoff
                continue
            raise
        except APIError as e:
            if e.status_code >= 500:  # Server error, retry
                if attempt < max_retries - 1:
                    time.sleep(1)
                    continue
            raise
```

### Conversation Management

```python
class Conversation:
    def __init__(self, system_prompt: str = None):
        self.messages = []
        if system_prompt:
            self.system = system_prompt
    
    def add_user(self, content: str):
        self.messages.append({"role": "user", "content": content})
    
    def add_assistant(self, content: str):
        self.messages.append({"role": "assistant", "content": content})
    
    def get_messages(self):
        return self.messages
    
    def clear(self):
        self.messages = []
```

### Token Counting (OpenAI)

```python
import tiktoken

def count_tokens(text: str, model: str = "gpt-4o") -> int:
    encoding = tiktoken.encoding_for_model(model)
    return len(encoding.encode(text))

def estimate_cost(input_tokens: int, output_tokens: int, model: str) -> float:
    pricing = {
        "gpt-4o": {"input": 2.50, "output": 10.00},
        "claude-3-5-sonnet": {"input": 3.00, "output": 15.00},
    }
    rates = pricing.get(model, {"input": 0, "output": 0})
    return (input_tokens * rates["input"] + output_tokens * rates["output"]) / 1_000_000
```

### Provider Abstraction

```python
class LLMClient:
    def __init__(self, provider: str):
        self.provider = provider
        self._init_client()
    
    def _init_client(self):
        if self.provider == "anthropic":
            from anthropic import Anthropic
            self.client = Anthropic()
        elif self.provider == "openai":
            from openai import OpenAI
            self.client = OpenAI()
    
    def complete(self, prompt: str, model: str) -> str:
        if self.provider == "anthropic":
            response = self.client.messages.create(
                model=model,
                max_tokens=1024,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.content[0].text
        elif self.provider == "openai":
            response = self.client.chat.completions.create(
                model=model,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.choices[0].message.content
```

---

## Environment Setup

### Environment Variables

```bash
# Anthropic
export ANTHROPIC_API_KEY=sk-ant-...

# OpenAI
export OPENAI_API_KEY=sk-...

# Google
export GOOGLE_API_KEY=...
```

### Python Requirements

```txt
anthropic>=0.18.0
openai>=1.12.0
google-generativeai>=0.4.0
ollama>=0.1.0
tiktoken>=0.6.0
```

### .env File

```
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...
GOOGLE_API_KEY=...
```

```python
from dotenv import load_dotenv
load_dotenv()
```

---

## Quick Reference Card

### Endpoints

| Provider | Base URL |
|----------|----------|
| Anthropic | `https://api.anthropic.com` |
| OpenAI | `https://api.openai.com` |
| Google | `https://generativelanguage.googleapis.com` |
| Ollama | `http://localhost:11434` |

### Model Identifiers

| Provider | Current Models |
|----------|----------------|
| Anthropic | `claude-3-5-sonnet-20241022`, `claude-3-opus-20240229` |
| OpenAI | `gpt-4o`, `gpt-4o-mini`, `o1-preview` |
| Google | `gemini-1.5-pro`, `gemini-1.5-flash` |
| Ollama | `llama3.1:8b`, `llama3.1:70b`, `codellama` |

---

*Keep this reference handy during development. For detailed documentation, refer to official provider docs.*

