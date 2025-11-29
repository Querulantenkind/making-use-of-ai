# Local Models

## AI That Runs on Your Hardware

Running AI models locally represents the ultimate in privacy and control. Your data never leaves your machine, there are no API costs, and you can work entirely offline. This guide covers the practical aspects of local AI deployment.

---

## Why Run Models Locally?

### Complete Privacy

- Data never transmitted to external servers
- No logging by third parties
- Full compliance with any data restriction
- Ideal for sensitive or proprietary information

### Zero Marginal Cost

Once infrastructure is set up:
- No per-token charges
- Unlimited usage
- Predictable expenses
- Test and iterate freely

### Offline Capability

- Works without internet
- No dependency on external services
- No outages from provider issues
- Deploy in air-gapped environments

### Full Control

- Choose exact model versions
- Customize and fine-tune
- No sudden deprecations
- Integrate deeply into workflows

---

## Hardware Requirements

### GPU Recommendations

| Use Case | Minimum | Recommended | Notes |
|----------|---------|-------------|-------|
| Experimentation | 8GB VRAM | 16GB+ VRAM | RTX 3070/4070 |
| Serious local use | 16GB VRAM | 24GB VRAM | RTX 4090 |
| Production | 24GB+ VRAM | 48GB+ | A6000, multiple GPUs |
| Large models | 48GB+ | 80GB+ | A100, H100 |

### Consumer GPUs

**NVIDIA RTX 4090 (24GB)**
- Best consumer option
- Runs 7-8B models excellently
- 13B models with quantization
- 70B models with heavy quantization (slow)

**NVIDIA RTX 4080/4070 (16GB/12GB)**
- Good for 7-8B models
- Smaller models run well
- Limited for larger models

**AMD GPUs**
- ROCm support improving
- Generally behind NVIDIA in AI workloads
- MI series competitive for datacenter

### Apple Silicon

**M1/M2/M3 Pro/Max/Ultra**
- Unified memory is a major advantage
- 32GB+ models can run 70B quantized
- Metal acceleration via llama.cpp
- Good performance/watt ratio

```bash
# Example: Ollama on Mac
ollama run llama3.1:8b
# Works out of the box with Metal acceleration
```

### CPU-Only

Running on CPU is possible but slow:
- llama.cpp supports CPU inference
- Expect 1-5 tokens/second for 7B models
- Usable for batch processing, not interactive
- Consider for privacy when GPU unavailable

---

## Setting Up Local Inference

### Ollama (Recommended for Beginners)

The easiest way to run models locally:

```bash
# Install (Linux/Mac)
curl https://ollama.ai/install.sh | sh

# Pull and run a model
ollama run llama3.1:8b

# List available models
ollama list

# Pull specific model
ollama pull codellama:7b

# API access
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.1:8b",
  "prompt": "Write a Python function to sort a list"
}'
```

**Available models via Ollama:**
- `llama3.1:8b`, `llama3.1:70b`
- `codellama:7b`, `codellama:13b`
- `mistral:7b`
- `phi3:mini`
- `gemma2:9b`
- And many more

### llama.cpp (Maximum Control)

For power users who want full control:

```bash
# Clone and build
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
make -j

# Download a model (GGUF format)
# From Hugging Face or convert yourself

# Run interactive
./main -m models/llama-3.1-8b.gguf -n 256 --interactive

# Start server
./server -m models/llama-3.1-8b.gguf --port 8080
```

### LM Studio (GUI Option)

For those preferring graphical interfaces:
- Download from lmstudio.ai
- Browse and download models within app
- Chat interface built-in
- OpenAI-compatible API server

### Text Generation WebUI

Full-featured web interface:

```bash
# Clone
git clone https://github.com/oobabooga/text-generation-webui
cd text-generation-webui

# Run installer
./start_linux.sh  # or start_windows.bat

# Access at http://localhost:7860
```

---

## Model Formats and Quantization

### GGUF Format

The standard format for local inference:
- Optimized for CPU and GPU inference
- Supports various quantization levels
- Used by llama.cpp, Ollama, LM Studio

### Quantization Levels

| Format | Bits | Quality | Speed | VRAM for 7B |
|--------|------|---------|-------|-------------|
| F16 | 16 | Best | Slow | ~14GB |
| Q8_0 | 8 | Excellent | Good | ~7GB |
| Q6_K | 6 | Very good | Better | ~5.5GB |
| Q5_K_M | 5 | Good | Better | ~4.5GB |
| Q4_K_M | 4 | Acceptable | Fast | ~4GB |
| Q3_K_M | 3 | Degraded | Fastest | ~3GB |
| Q2_K | 2 | Poor | Fastest | ~2.5GB |

**Recommendation:** Q4_K_M or Q5_K_M for best quality/size balance.

### Finding Quantized Models

**Hugging Face:**
- TheBloke's quantizations (extensive collection)
- Official GGUF releases from model creators
- Community uploads

```bash
# Example: Download from Hugging Face
huggingface-cli download TheBloke/Llama-3.1-8B-GGUF \
    llama-3.1-8b.Q4_K_M.gguf
```

---

## Optimizing Performance

### Context Length Management

Longer context = more memory:

```
Context 2048: Uses ~4GB (7B model, Q4)
Context 4096: Uses ~5GB
Context 8192: Uses ~7GB
Context 16384: Uses ~11GB
```

Set context appropriately for your task and hardware.

### Batch Size Tuning

For throughput (not interactive):

```bash
# llama.cpp
./main -m model.gguf -b 512  # Larger batch, higher throughput
```

### Layer Offloading

Split model between GPU and CPU:

```bash
# Ollama (automatic)
# Will use GPU when available

# llama.cpp
./main -m model.gguf -ngl 35  # Offload 35 layers to GPU
```

### Flash Attention

If supported, enables longer context with less memory:

```bash
# Ollama: automatic when available
# llama.cpp: compile with LLAMA_FLASH_ATTN=1
```

---

## Integration Patterns

### OpenAI-Compatible API

Most local servers provide OpenAI-compatible endpoints:

```python
from openai import OpenAI

# Point to local server
client = OpenAI(
    base_url="http://localhost:11434/v1",  # Ollama
    api_key="not-needed"
)

response = client.chat.completions.create(
    model="llama3.1:8b",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

### LangChain Integration

```python
from langchain_community.llms import Ollama

llm = Ollama(model="llama3.1:8b")
response = llm.invoke("Explain quantum computing")
```

### Direct HTTP Requests

```bash
# Ollama API
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.1:8b",
  "prompt": "Why is the sky blue?",
  "stream": false
}'
```

---

## Use Cases for Local Models

### Development and Testing

- Rapid iteration without API costs
- Test prompt variations freely
- Debug integrations locally

### Sensitive Data Processing

- Medical records
- Legal documents
- Proprietary code
- Personal information

### Offline Environments

- Air-gapped systems
- Unreliable connectivity
- Field deployments

### High-Volume Processing

- Batch processing jobs
- Document analysis pipelines
- Code analysis across large codebases

---

## Practical Recommendations

### Starting Out

1. **Install Ollama** - Easiest path to running models
2. **Try llama3.1:8b** - Good balance of capability and speed
3. **Use Q4_K_M quantization** - Best quality/size ratio
4. **Start with 4096 context** - Increase if needed

### Hardware Investment

If buying hardware for local AI:

1. **NVIDIA GPU** - Best software support
2. **Maximum VRAM** - Most important spec
3. **RTX 4090** - Best consumer option
4. **Apple M-series** - Excellent if in Apple ecosystem

### When Local Makes Sense

| Situation | Recommendation |
|-----------|----------------|
| Privacy critical | Local |
| High volume (>$100/month API) | Consider local |
| Simple tasks | Local works well |
| Complex reasoning | Use API (for now) |
| Offline required | Local only option |
| Quick prototyping | API faster to start |

---

## Troubleshooting Common Issues

### Out of Memory

```bash
# Reduce context length
ollama run llama3.1:8b --ctx-size 2048

# Use smaller quantization
ollama pull llama3.1:8b-q4_0

# Reduce batch size
./main -m model.gguf -b 128
```

### Slow Generation

- Check GPU is being used (nvidia-smi)
- Reduce context length
- Use more aggressive quantization
- Consider smaller model

### Quality Issues

- Try larger model or less quantization
- Use better prompting (chain-of-thought)
- Consider API for complex tasks

---

## Resources

- **Ollama**: https://ollama.ai
- **llama.cpp**: https://github.com/ggerganov/llama.cpp
- **Hugging Face Models**: https://huggingface.co/models
- **LocalLLaMA Reddit**: https://reddit.com/r/LocalLLaMA
- **LM Studio**: https://lmstudio.ai

---

*Local AI is not a compromise; it's a choice. For the right use cases, running models on your own hardware provides privacy, control, and freedom that cloud APIs cannot match.*

