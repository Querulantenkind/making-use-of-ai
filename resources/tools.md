# Tools

## Software and Utilities for AI Work

A curated collection of tools that enhance AI-assisted workflows. Organized by category with honest assessments of each tool's strengths.

---

## AI Coding Assistants

### Cursor
**The AI-first IDE**

- **Website:** https://cursor.com
- **Pricing:** Free tier available, Pro $20/month
- **Best for:** Full IDE AI integration, code generation, refactoring

Cursor rebuilds VS Code around AI assistance. It's currently the most capable integrated AI coding experience.

**Strengths:**
- Deep integration (Chat, Composer, inline edits)
- Excellent codebase understanding
- Multiple model support
- Familiar VS Code interface

**Limitations:**
- Requires IDE switch (though imports VS Code config)
- Subscription for full features

---

### Aider
**Terminal-native AI coding**

- **Website:** https://aider.chat
- **Pricing:** Free (open source), API costs only
- **Best for:** Terminal workflows, Git integration, editor-agnostic

Aider runs in your terminal and works with any editor through the filesystem.

**Strengths:**
- Git-native (automatic commits)
- Works with any editor
- Full control over context
- Open source

**Limitations:**
- Terminal only (no GUI)
- Steeper learning curve

---

### GitHub Copilot
**The original AI assistant**

- **Website:** https://github.com/features/copilot
- **Pricing:** $10-39/month depending on plan
- **Best for:** Autocomplete, inline suggestions, familiar interface

Copilot integrates into existing IDEs as an extension.

**Strengths:**
- Wide IDE support
- Mature, battle-tested
- Good autocomplete
- GitHub ecosystem integration

**Limitations:**
- Less powerful than dedicated AI IDEs
- Model choice limited

---

### Codeium
**Free alternative**

- **Website:** https://codeium.com
- **Pricing:** Free for individuals
- **Best for:** Cost-conscious users, basic AI assistance

A free option with solid capabilities.

**Strengths:**
- Completely free for individuals
- Multiple IDE support
- Fast autocomplete

**Limitations:**
- Less capable than paid alternatives
- Limited advanced features

---

## Local Model Tools

### Ollama
**Easiest local setup**

- **Website:** https://ollama.ai
- **Pricing:** Free (open source)
- **Best for:** Getting started with local models

One command to download and run models locally.

```bash
ollama run llama3.1:8b
```

**Strengths:**
- Trivial setup
- Great model library
- OpenAI-compatible API
- Cross-platform

**Limitations:**
- Less configurable than alternatives
- Some models not available

---

### LM Studio
**GUI for local models**

- **Website:** https://lmstudio.ai
- **Pricing:** Free
- **Best for:** Visual model management, experimentation

A desktop application for downloading and running local models.

**Strengths:**
- User-friendly interface
- Built-in model browser
- OpenAI-compatible server
- No command line needed

**Limitations:**
- Less efficient than CLI tools
- Limited automation

---

### llama.cpp
**Maximum efficiency**

- **Website:** https://github.com/ggerganov/llama.cpp
- **Pricing:** Free (open source)
- **Best for:** Production deployments, maximum performance

The reference implementation for efficient LLM inference.

**Strengths:**
- Highly optimized
- Extensive quantization support
- Cross-platform
- Active development

**Limitations:**
- Requires building from source (usually)
- More complex setup

---

### vLLM
**Production inference**

- **Website:** https://vllm.ai
- **Pricing:** Free (open source)
- **Best for:** High-throughput production serving

Fast inference engine for production deployments.

**Strengths:**
- Very high throughput
- PagedAttention for memory efficiency
- OpenAI-compatible API
- Continuous batching

**Limitations:**
- Requires GPU
- More complex deployment

---

## Development Frameworks

### LangChain
**LLM application framework**

- **Website:** https://langchain.com
- **Pricing:** Free (open source)
- **Best for:** Building complex LLM applications

The most popular framework for building with LLMs.

**Strengths:**
- Extensive integrations
- Chain and agent abstractions
- Active community
- Good documentation

**Limitations:**
- Can be over-abstracted for simple use cases
- Rapid changes can break code

---

### LlamaIndex
**Data-focused LLM framework**

- **Website:** https://llamaindex.ai
- **Pricing:** Free (open source)
- **Best for:** RAG applications, document Q&A

Focused on connecting LLMs to data sources.

**Strengths:**
- Excellent for RAG
- Many data connectors
- Good indexing options

**Limitations:**
- More specialized than LangChain
- Less general-purpose

---

### LiteLLM
**Unified API**

- **Website:** https://litellm.ai
- **Pricing:** Free (open source)
- **Best for:** Multi-provider applications

Call any LLM provider with the same OpenAI-format API.

**Strengths:**
- Single API for all providers
- Easy provider switching
- Fallback support
- Good for A/B testing models

**Limitations:**
- Additional abstraction layer
- Some provider-specific features not exposed

---

## Prompt Development

### Anthropic Workbench
**Claude development environment**

- **Website:** https://console.anthropic.com/workbench
- **Pricing:** API costs only
- **Best for:** Developing and testing Claude prompts

Official interface for prompt development with Claude.

**Strengths:**
- Direct Claude access
- Variable substitution
- Save and share prompts

---

### OpenAI Playground
**GPT development environment**

- **Website:** https://platform.openai.com/playground
- **Pricing:** API costs only
- **Best for:** Developing and testing GPT prompts

OpenAI's interface for prompt experimentation.

**Strengths:**
- All GPT models available
- Parameter tuning
- Chat and completion modes

---

### Google AI Studio
**Gemini development environment**

- **Website:** https://aistudio.google.com
- **Pricing:** Free (with limits)
- **Best for:** Gemini prompt development

Google's interface for Gemini experimentation.

**Strengths:**
- Free tier generous
- Multimodal support
- Easy to start

---

## Utilities

### tiktoken
**Token counting**

- **Website:** https://github.com/openai/tiktoken
- **Pricing:** Free (open source)
- **Best for:** Accurate token counting for OpenAI models

```python
import tiktoken
enc = tiktoken.encoding_for_model("gpt-4o")
tokens = enc.encode("Hello world")
```

---

### Hugging Face Hub
**Model repository**

- **Website:** https://huggingface.co
- **Pricing:** Free (paid tiers for compute)
- **Best for:** Finding and downloading models

The GitHub of machine learning models.

**Strengths:**
- Massive model library
- Datasets available
- Community and discussions
- Inference endpoints

---

## IDE Extensions

### Error Lens (VS Code)
Shows errors inline in the editor. Useful for seeing AI-introduced issues immediately.

### GitLens (VS Code)
Enhanced Git capabilities. Useful for tracking AI changes.

### Thunder Client (VS Code)
API testing. Useful for testing LLM API calls.

---

## Comparison Matrices

### Coding Assistants

| Tool | IDE | Price | Offline | Multi-model |
|------|-----|-------|---------|-------------|
| Cursor | Own IDE | $20/mo | No | Yes |
| Aider | Any | API only | Optional | Yes |
| Copilot | Multiple | $10-39/mo | No | Limited |
| Codeium | Multiple | Free | No | Limited |

### Local Running

| Tool | Ease | Performance | GUI | Automation |
|------|------|-------------|-----|------------|
| Ollama | Easy | Good | No | Yes |
| LM Studio | Easy | Good | Yes | Limited |
| llama.cpp | Medium | Best | No | Yes |
| vLLM | Hard | Best | No | Yes |

---

*Tools evolve rapidly. Check for updates and new options regularly. The best tool is the one that fits your workflow.*

