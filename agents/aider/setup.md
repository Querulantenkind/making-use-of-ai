# Aider Setup Guide

## Installation and Configuration

Aider is a terminal-based AI coding assistant that works with your existing editor. It integrates deeply with git and provides a powerful command-line interface for AI-assisted development.

---

## Prerequisites

Before installing Aider, ensure you have:

- **Python 3.9+** - Aider is a Python package
- **Git** - Required for Aider's version control integration
- **API Key** - For your chosen AI provider (OpenAI, Anthropic, etc.)
- **A code editor** - Aider works alongside your preferred editor

---

## Installation

### Using pip (Recommended)

```bash
# Install aider
pip install aider-chat

# Or with pipx for isolated installation
pipx install aider-chat
```

### Using Homebrew (macOS)

```bash
brew install aider
```

### Verify Installation

```bash
aider --version
aider --help
```

---

## API Key Configuration

### OpenAI

```bash
# Set environment variable
export OPENAI_API_KEY=sk-your-key-here

# Or create a .env file in your project
echo "OPENAI_API_KEY=sk-your-key-here" >> .env
```

### Anthropic (Claude)

```bash
export ANTHROPIC_API_KEY=sk-ant-your-key-here
```

### Other Providers

Aider supports multiple providers:

```bash
# Azure OpenAI
export AZURE_API_KEY=your-key
export AZURE_API_BASE=https://your-resource.openai.azure.com
export AZURE_API_VERSION=2024-02-01

# OpenRouter (access to many models)
export OPENROUTER_API_KEY=your-key

# Local models via Ollama
# No API key needed, just have Ollama running
```

### Persistent Configuration

Create `~/.aider.conf.yml` for global settings:

```yaml
# Default model
model: claude-3-5-sonnet-20241022

# API keys (alternatively use environment variables)
openai-api-key: sk-your-key
anthropic-api-key: sk-ant-your-key

# Editor integration
editor: code  # or vim, nvim, etc.

# Auto-commits
auto-commits: true
```

---

## Model Selection

### Recommended Models

| Model | Best For | Flag |
|-------|----------|------|
| Claude 3.5 Sonnet | General coding (recommended) | `--model claude-3-5-sonnet-20241022` |
| GPT-4o | Broad knowledge, multimodal | `--model gpt-4o` |
| Claude 3 Opus | Complex reasoning | `--model claude-3-opus-20240229` |
| GPT-4o-mini | Fast, cost-effective | `--model gpt-4o-mini` |
| DeepSeek Coder | Budget coding | `--model deepseek/deepseek-coder` |

### Setting Default Model

```bash
# Via command line
aider --model claude-3-5-sonnet-20241022

# Via config file (~/.aider.conf.yml)
model: claude-3-5-sonnet-20241022

# Via environment variable
export AIDER_MODEL=claude-3-5-sonnet-20241022
```

### Using Local Models

```bash
# With Ollama
ollama pull codellama:34b
aider --model ollama/codellama:34b

# With LM Studio
aider --model openai/local --openai-api-base http://localhost:1234/v1
```

---

## Git Integration Setup

Aider works best with git. Initialize a repository if you haven't:

```bash
cd your-project
git init
git add .
git commit -m "Initial commit"
```

### Auto-commit Behavior

By default, Aider commits each change with a descriptive message:

```bash
# Disable auto-commits (manual control)
aider --no-auto-commits

# Specify commit message style
aider --commit-prompt "conventional"  # feat:, fix:, etc.
```

### Working with Existing Repos

```bash
# Start aider in your repo
cd your-project
aider

# Add specific files to the session
aider src/main.py src/utils.py

# Add all Python files
aider *.py
```

---

## Editor Integration

### VS Code

Aider is terminal-based, so it runs alongside VS Code:

1. Open integrated terminal (`Ctrl + ``)
2. Run `aider` with your files
3. Edit in VS Code, discuss in terminal

**Workflow tip**: Use split view with terminal on one side.

### Vim/Neovim

```bash
# Set vim as your diff editor
export AIDER_EDITOR=vim

# Or in config
editor: vim
```

### JetBrains IDEs

Run aider in the built-in terminal. Changes appear in the IDE immediately.

---

## Project Configuration

### Per-Project Config

Create `.aider.conf.yml` in your project root:

```yaml
# Project-specific model
model: gpt-4o

# Files to always include in context
read:
  - README.md
  - docs/architecture.md

# Files to never edit
ignore:
  - "*.lock"
  - "node_modules/**"
  - ".env*"

# Custom conventions
conventions: |
  - Use TypeScript strict mode
  - All functions must have JSDoc comments
  - Prefer functional programming patterns
```

### Gitignore Integration

Aider respects `.gitignore`. Files ignored by git won't be edited.

Add aider-specific ignores:

```gitignore
# .gitignore
.aider*
```

---

## Testing Your Setup

### Quick Test

```bash
# Start aider with a test file
echo "def hello(): pass" > test.py
aider test.py

# Try a simple command
> Add a docstring to the hello function

# Verify the change was made and committed
git log --oneline -1
cat test.py

# Clean up
rm test.py
git reset --hard HEAD~1
```

### Verify Model Access

```bash
# Test with your configured model
aider --model gpt-4o --message "Say hello"

# If using Claude
aider --model claude-3-5-sonnet-20241022 --message "Say hello"
```

---

## Troubleshooting

### "API key not found"

```bash
# Check environment variable
echo $OPENAI_API_KEY

# Set it if missing
export OPENAI_API_KEY=your-key

# Or check .env file in project directory
cat .env
```

### "Model not found"

```bash
# List available models
aider --list-models

# Use exact model name
aider --model claude-3-5-sonnet-20241022
```

### Git Issues

```bash
# Ensure you're in a git repo
git status

# If not initialized
git init
git add .
git commit -m "Initial commit"
```

### Slow Performance

```bash
# Use a faster model
aider --model gpt-4o-mini

# Or use streaming (usually default)
aider --stream
```

---

## Next Steps

Once setup is complete:

1. Read the [Usage Guide](usage.md) for workflows
2. Review [Configuration](config.md) for customization
3. Practice with a toy project before using on real code

---

*Aider setup is straightforward once you have your API key. The power is in the workflow, which we cover in the usage guide.*

