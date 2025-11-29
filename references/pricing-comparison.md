# Pricing Comparison

## Understanding AI Model Costs

AI pricing directly impacts which models are practical for different use cases. This reference provides current pricing and strategies for cost optimization.

---

## Pricing by Provider (Late 2024)

### Anthropic (Claude)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|------------------------|
| Claude 3.5 Sonnet | $3.00 | $15.00 |
| Claude 3 Opus | $15.00 | $75.00 |
| Claude 3.5 Haiku | $0.25 | $1.25 |

**Notes:**
- Cached prompts: Reduced pricing for repeated content
- Batch API: ~50% discount for non-realtime

### OpenAI (GPT)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|------------------------|
| GPT-4o | $2.50 | $10.00 |
| GPT-4o-mini | $0.15 | $0.60 |
| GPT-4 Turbo | $10.00 | $30.00 |
| o1-preview | $15.00 | $60.00 |
| o1-mini | $3.00 | $12.00 |

**Notes:**
- Cached tokens: 50% discount on input
- Batch API: 50% discount overall

### Google (Gemini)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|------------------------|
| Gemini 1.5 Pro | $1.25-3.50 | $5.00-10.50 |
| Gemini 1.5 Flash | $0.075 | $0.30 |

**Notes:**
- Pricing varies by context length
- <128K tokens is cheaper than 128K+

### Open Source (Inference Providers)

| Provider | Model | Input (per 1M tokens) | Output (per 1M tokens) |
|----------|-------|----------------------|------------------------|
| Together AI | Llama 3.1 70B | $0.88 | $0.88 |
| Together AI | Llama 3.1 8B | $0.18 | $0.18 |
| Replicate | Llama 3.1 70B | ~$0.65 | ~$0.65 |
| Groq | Llama 3.1 70B | Free tier / $0.59 | $0.79 |

**Self-hosted:**
- Infrastructure cost only
- GPU cost: $0.50-4.00/hour depending on hardware
- Amortize across usage volume

---

## Cost Estimation

### Per-Request Estimates

| Task | Input Tokens | Output Tokens | Cost (Sonnet) |
|------|-------------|---------------|---------------|
| Simple question | 500 | 200 | $0.0045 |
| Code review | 3,000 | 1,000 | $0.024 |
| Full file analysis | 5,000 | 2,000 | $0.045 |
| Codebase query | 20,000 | 1,000 | $0.075 |
| Long conversation | 50,000 | 5,000 | $0.225 |

### Monthly Usage Estimates

| Usage Level | Requests/Day | Est. Monthly Cost (Sonnet) |
|-------------|-------------|---------------------------|
| Light | 10-20 | $15-40 |
| Moderate | 50-100 | $75-200 |
| Heavy | 200-500 | $300-800 |
| Very Heavy | 1000+ | $1,500+ |

---

## Cost Optimization Strategies

### Model Selection

**Use the right tier:**
| Task Complexity | Recommended | Savings vs Premium |
|-----------------|-------------|-------------------|
| Simple queries | Haiku / GPT-4o-mini | 90%+ |
| Standard tasks | Sonnet / GPT-4o | Baseline |
| Complex reasoning | Opus / o1-preview | N/A (pay for quality) |

**Route by complexity:**
```python
def select_model(task_complexity: str) -> str:
    if task_complexity == "simple":
        return "claude-3-5-haiku"
    elif task_complexity == "complex":
        return "claude-3-opus"
    else:
        return "claude-3-5-sonnet"
```

### Context Optimization

**Minimize input tokens:**
- Include only relevant code
- Summarize documentation
- Use file references instead of full content
- Compress verbose prompts

**Control output tokens:**
- Set appropriate `max_tokens`
- Request concise responses
- Use structured formats
- Ask for specific outputs, not explanations

### Caching Strategies

**Prompt caching (where available):**
- Claude: Automatic caching of repeated prefixes
- OpenAI: Cached input tokens at 50% discount

**Application-level caching:**
```python
import hashlib
from functools import lru_cache

@lru_cache(maxsize=1000)
def cached_completion(prompt_hash: str, model: str) -> str:
    # Only calls API if not in cache
    return call_api(prompt_hash, model)

def get_completion(prompt: str, model: str) -> str:
    prompt_hash = hashlib.md5(prompt.encode()).hexdigest()
    return cached_completion(prompt_hash, model)
```

### Batch Processing

**Use batch APIs for non-urgent work:**
- 50% discount on batch processing
- Good for: analysis, data processing, bulk generation
- Not for: interactive use, real-time needs

### Local Models for High Volume

**Break-even analysis:**
| Volume (M tokens/month) | Cloud Cost (Sonnet) | Local Cost (70B) |
|-------------------------|--------------------|--------------------|
| 1M | $18 | ~$100 (infra) |
| 10M | $180 | ~$100 |
| 100M | $1,800 | ~$200 |
| 1B | $18,000 | ~$500 |

Local becomes economical at high volume (typically 50M+ tokens/month).

---

## Budget Planning

### Setting Limits

**API-level limits:**
- Set spending caps in provider dashboards
- Configure alerts at thresholds

**Application-level limits:**
```python
class BudgetTracker:
    def __init__(self, monthly_budget: float):
        self.monthly_budget = monthly_budget
        self.spent = 0.0
    
    def check_budget(self, estimated_cost: float) -> bool:
        if self.spent + estimated_cost > self.monthly_budget:
            raise BudgetExceededError()
        return True
    
    def record_spend(self, actual_cost: float):
        self.spent += actual_cost
```

### Cost Monitoring

**Track by:**
- Model used
- Task type
- User/team (for multi-user)
- Time period

**Key metrics:**
- Cost per request
- Cost per successful task
- Input/output token ratio
- Cache hit rate

---

## Cost Comparison Scenarios

### Scenario 1: Individual Developer

**Usage:** 50 code reviews/day, 20 Q&A interactions

| Option | Monthly Cost |
|--------|-------------|
| All Claude 3.5 Sonnet | ~$200 |
| Haiku for Q&A, Sonnet for review | ~$100 |
| GPT-4o-mini for Q&A, GPT-4o for review | ~$80 |

### Scenario 2: Small Team (5 devs)

**Usage:** 1,000 requests/day total

| Option | Monthly Cost |
|--------|-------------|
| Claude 3.5 Sonnet | ~$800-1,200 |
| Mixed tiers | ~$400-600 |
| Self-hosted Llama | ~$200-400 (infra) |

### Scenario 3: High-Volume Processing

**Usage:** 10M tokens/day for document processing

| Option | Monthly Cost |
|--------|-------------|
| Claude 3.5 Sonnet | ~$5,400 |
| Claude Haiku | ~$450 |
| Self-hosted Llama 70B | ~$300-500 (infra) |
| Gemini Flash | ~$225 |

---

## Pricing Trends

### Historical Patterns

- Prices generally decrease over time
- New efficient models introduced regularly
- Capabilities increase at same price points

### Planning Considerations

- Lock in commitments cautiously
- Build model-agnostic systems where possible
- Monitor pricing announcements
- Re-evaluate periodically

---

## Hidden Costs

**Beyond API pricing:**
- Development time for integration
- Monitoring and observability
- Error handling and retries
- Quality assurance
- Infrastructure for local models

**The real cost equation:**
```
Total Cost = API Costs 
           + Development Time
           + Operational Overhead
           + Quality/Review Costs
           + Opportunity Cost of Errors
```

---

*Pricing is just one factor. The cheapest model that doesn't do the job well isn't actually cheap. Balance cost against quality requirements for your specific use case.*

