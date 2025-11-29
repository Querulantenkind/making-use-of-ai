# Terminology

## A Glossary of AI and LLM Concepts

Understanding the language helps you navigate the AI landscape confidently. This glossary covers terms you'll encounter when working with large language models and AI agents.

---

## Core Concepts

### Token
The fundamental unit of text that language models process. Roughly corresponds to 3/4 of a word or about 4 characters in English. Models read, process, and generate text token by token.

**Why it matters:** Context windows, pricing, and limits are measured in tokens. Understanding tokenization helps you estimate costs and work within limits.

### Context Window
The maximum amount of text (measured in tokens) a model can process in a single interaction. Includes both input (your prompt, system message, files) and output (the model's response).

**Typical sizes:**
- 128K tokens: ~100,000 words or ~200 pages
- 200K tokens: ~150,000 words or ~300 pages
- 1M+ tokens: ~750,000 words or ~1,500 pages

### Prompt
The input you provide to a language model. Can range from a simple question to a complex multi-part instruction with examples and context.

### System Prompt/Message
Special instructions provided at the start of a conversation that shape the model's behavior throughout. Used to establish persona, constraints, and consistent behaviors.

### Completion
The model's response to your prompt. Sometimes called "generation" or "output."

---

## Model Behavior

### Temperature
A parameter controlling randomness in model outputs. Range typically 0-1 (sometimes 0-2).

| Temperature | Behavior | Use Cases |
|-------------|----------|-----------|
| 0 | Deterministic, focused | Code, factual answers |
| 0.3-0.5 | Balanced | General tasks |
| 0.7-1.0 | Creative, varied | Brainstorming, creative writing |

### Top-p (Nucleus Sampling)
Alternative to temperature for controlling output randomness. Limits token selection to the smallest set of tokens whose cumulative probability exceeds p. Often used with temperature.

### Max Tokens
The maximum number of tokens the model will generate in its response. Helps control response length and costs.

### Stop Sequences
Strings that, when generated, cause the model to stop producing output. Useful for controlling response format.

---

## Prompting Techniques

### Few-Shot Learning
Providing examples of desired input-output pairs before the actual task. The model learns the pattern and applies it to new inputs.

```
Example 1: Input -> Output
Example 2: Input -> Output
Now: New Input -> ?
```

### Zero-Shot
Asking the model to perform a task without providing examples, relying only on its pre-trained knowledge and your instructions.

### Chain-of-Thought (CoT)
Instructing the model to reason step-by-step before providing a final answer. Improves performance on complex reasoning tasks.

### Role Prompting
Assigning the model a specific role or persona (e.g., "You are a senior software engineer") to influence how it approaches tasks.

### Prompt Chaining
Breaking complex tasks into multiple sequential prompts, where each step's output informs the next step's input.

---

## Training & Fine-Tuning

### Pre-training
The initial training phase where a model learns general language patterns from vast amounts of text data. Creates the foundation of the model's capabilities.

### Fine-tuning
Additional training on specific data to specialize a model for particular tasks or domains. Can improve performance on targeted use cases.

### RLHF (Reinforcement Learning from Human Feedback)
A training technique where human preferences guide model behavior. Used to make models more helpful, harmless, and honest.

### Instruction Tuning
Training a model to better follow instructions by exposing it to many example instructions and appropriate responses.

### LoRA (Low-Rank Adaptation)
An efficient fine-tuning method that trains small adapter weights rather than modifying the full model. Enables customization with less compute.

### QLoRA
LoRA applied to a quantized model, enabling fine-tuning on consumer hardware.

---

## Model Architecture

### Large Language Model (LLM)
A neural network trained on vast text data that can generate, understand, and manipulate text. "Large" refers to the billions of parameters.

### Parameters
The trained values (weights) in a neural network. More parameters generally means more capability but requires more compute. Models range from millions to trillions of parameters.

### Transformer
The neural network architecture underlying most modern LLMs. Uses attention mechanisms to process relationships between all tokens in the input.

### Attention
The mechanism that allows transformers to weigh the relevance of different parts of the input when generating each output token.

### Mixture of Experts (MoE)
An architecture where multiple specialized sub-networks (experts) handle different aspects of the task. Enables larger models with efficient inference.

---

## Inference & Deployment

### Inference
The process of generating output from a trained model. What happens when you send a prompt and receive a response.

### Quantization
Reducing the precision of model weights (e.g., from 16-bit to 4-bit) to decrease memory requirements and speed up inference. Trades some quality for efficiency.

| Quantization | Memory | Quality |
|--------------|--------|---------|
| FP16 | 100% | 100% |
| INT8 | ~50% | ~99% |
| INT4 | ~25% | ~95-97% |

### Latency
The time between sending a request and receiving a response. Important for interactive applications.

- **Time to First Token (TTFT)**: Delay before the first token appears
- **Tokens per Second**: Generation speed after starting

### Throughput
The volume of requests or tokens a system can process per unit time. Important for production deployments.

---

## Agent Concepts

### Agent
An AI system that can take actions in an environment, not just generate text. Agents can read files, execute code, browse the web, etc.

### Tool Use / Function Calling
The ability of a model to invoke external functions or tools based on the conversation. Enables actions like searching, calculating, or accessing APIs.

### ReAct (Reasoning + Acting)
A prompting pattern where agents alternate between reasoning about what to do and taking actions, observing results between steps.

### Autonomous Agent
An agent that can work independently toward a goal, making decisions about what actions to take without human approval for each step.

---

## Evaluation

### Hallucination
When a model generates plausible-sounding but incorrect or fabricated information. A known limitation requiring verification of outputs.

### Grounding
Techniques to anchor model outputs to factual sources, reducing hallucination. Examples include retrieval augmentation and search integration.

### Benchmark
A standardized test used to evaluate and compare model capabilities. Examples: MMLU, HumanEval, GSM8K.

### Perplexity
A metric measuring how well a model predicts text. Lower perplexity indicates better language modeling, though it doesn't directly measure usefulness.

---

## Privacy & Safety

### Data Retention
How long a provider stores data from your interactions. Important for privacy-sensitive applications.

### Training Data Opt-Out
The ability to prevent your data from being used to train future model versions. Most enterprise tiers include this.

### Constitutional AI
Anthropic's approach to AI safety, training models with principles that guide helpful, harmless, and honest behavior.

### Red Teaming
Deliberately testing models to find harmful or problematic behaviors. Used to improve safety before deployment.

---

## API Terminology

### API Key
Secret credential authenticating your requests to an AI provider. Keep secure; never commit to version control.

### Rate Limiting
Restrictions on how many requests you can make per minute/hour. Prevents abuse and ensures fair access.

### Streaming
Receiving the model's response token-by-token as it's generated, rather than waiting for the complete response. Enables faster perceived responses.

### Embedding
A numerical representation of text that captures semantic meaning. Used for search, clustering, and similarity comparisons.

---

## Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| LLM | Large Language Model |
| GPT | Generative Pre-trained Transformer |
| RAG | Retrieval-Augmented Generation |
| CoT | Chain of Thought |
| RLHF | Reinforcement Learning from Human Feedback |
| API | Application Programming Interface |
| TTFT | Time to First Token |
| MoE | Mixture of Experts |
| SFT | Supervised Fine-Tuning |
| PEFT | Parameter-Efficient Fine-Tuning |
| LoRA | Low-Rank Adaptation |

---

*Vocabulary is foundation. Understanding these terms helps you navigate AI documentation, compare providers, and communicate effectively about AI systems.*

