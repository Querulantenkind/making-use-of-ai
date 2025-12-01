# Aider Configuration Reference

## Complete Guide to Customizing Aider

Aider is highly configurable. This reference covers all configuration options and how to use them effectively.

---

## Configuration Sources

Aider reads configuration from multiple sources (in order of precedence):

1. **Command line arguments** - Highest priority
2. **Environment variables** - `AIDER_*` prefix
3. **Project config** - `.aider.conf.yml` in current directory
4. **User config** - `~/.aider.conf.yml`

---

## Configuration File Format

### YAML Configuration

```yaml
# ~/.aider.conf.yml or .aider.conf.yml

# Model settings
model: claude-3-5-sonnet-20241022
weak-model: gpt-4o-mini
editor-model: gpt-4o

# API keys
openai-api-key: sk-...
anthropic-api-key: sk-ant-...

# Git behavior  
auto-commits: true
dirty-commits: false
commit-prompt: conventional

# Editor
editor: code

# Context
read:
  - README.md
  - docs/conventions.md

# Files to never touch
ignore:
  - "*.lock"
  - "node_modules/**"
  - ".env*"

# UI preferences
dark-mode: true
stream: true
pretty: true
```

### Environment Variables

```bash
# Equivalent environment variables
export AIDER_MODEL=claude-3-5-sonnet-20241022
export AIDER_AUTO_COMMITS=true
export AIDER_DARK_MODE=true
export OPENAI_API_KEY=sk-...
export ANTHROPIC_API_KEY=sk-ant-...
```

---

## Model Configuration

### Model Selection

```yaml
# Primary model for code generation
model: claude-3-5-sonnet-20241022

# Weaker model for simpler tasks (commit messages, etc.)
weak-model: gpt-4o-mini

# Model for editor integration
editor-model: gpt-4o
```

### Model-Specific Settings

```yaml
# Use specific model features
model: gpt-4o
# GPT-4 specific
edit-format: diff  # or whole, diff-fenced

# For Claude
model: claude-3-5-sonnet-20241022
# Claude tends to work well with default settings
```

### API Configuration

```yaml
# OpenAI
openai-api-key: sk-...
openai-api-base: https://api.openai.com/v1  # or custom endpoint

# Anthropic
anthropic-api-key: sk-ant-...

# Azure OpenAI
azure-api-key: ...
azure-api-base: https://your-resource.openai.azure.com
azure-api-version: 2024-02-01
azure-api-deployment-id: your-deployment

# OpenRouter
openrouter-api-key: sk-or-...
```

### Local Models

```yaml
# Ollama
model: ollama/codellama:34b

# LM Studio
model: openai/local-model
openai-api-base: http://localhost:1234/v1

# Any OpenAI-compatible server
openai-api-base: http://localhost:8000/v1
```

---

## Git Configuration

### Commit Behavior

```yaml
# Auto-commit after each change (default: true)
auto-commits: true

# Allow commits when repo has uncommitted changes (default: true)
dirty-commits: true

# Attribute commits to aider
attribute-author: true
attribute-committer: true

# Commit message generation
attribute-commit-message-author: false
attribute-commit-message-committer: false
```

### Commit Message Style

```yaml
# Commit message prompt
commit-prompt: conventional

# Or custom prompt
commit-prompt: |
  Generate a commit message for the following changes.
  Use the format: [type]: [description]
  Types: feat, fix, docs, refactor, test, chore
```

### Git Ignore Patterns

```yaml
# Additional patterns beyond .gitignore
aiderignore:
  - "*.generated.ts"
  - "build/**"
  - "dist/**"
```

---

## Context Configuration

### Default Files

```yaml
# Always include these files (read-only context)
read:
  - README.md
  - docs/architecture.md
  - src/types.ts
  - .cursorrules  # reuse cursor rules

# Files to edit by default
files:
  - src/main.py
```

### File Handling

```yaml
# Maximum file size to include (in tokens)
map-tokens: 1024

# Enable repository map for context
map-multiplier-no-files: 3

# Show file diffs
show-diffs: true
```

### Ignore Patterns

```yaml
# Files to never edit or include
ignore:
  - "node_modules/**"
  - "*.lock"
  - "*.min.js"
  - ".env*"
  - "**/__pycache__/**"
```

---

## UI Configuration

### Display Settings

```yaml
# Use dark mode colors
dark-mode: true

# Colorize output
pretty: true

# Stream responses in real-time
stream: true

# Show model in prompt
show-model-warnings: true
```

### Input Settings

```yaml
# Editor for multi-line input
editor: vim  # or code, nvim, nano, etc.

# Use /edit command to open editor
input-history-file: ~/.aider.history

# Vim mode for input
vim: false
```

### Output Settings

```yaml
# Show diffs of changes
show-diffs: true

# Verbose output
verbose: false

# Log to file
log-file: ~/.aider.logs/aider.log
```

---

## Project-Specific Configuration

### Conventions

```yaml
# Project coding conventions (included in context)
conventions: |
  This is a TypeScript project using:
  - React 18 with functional components
  - Tailwind CSS for styling
  - Zustand for state management
  
  Coding standards:
  - Use async/await, never .then()
  - All exports should be named, not default
  - Components go in PascalCase.tsx files
  - Utilities go in camelCase.ts files
  - Always add JSDoc comments to public functions
```

### Test Commands

```yaml
# Commands to run tests
test-cmd: npm test

# Lint command
lint-cmd: npm run lint

# Auto-run tests after changes
auto-test: false
```

---

## Performance Configuration

### Token Management

```yaml
# Maximum tokens in context
max-chat-history-tokens: 4096

# Token limit for file map
map-tokens: 1024
```

### Cache Settings

```yaml
# Enable caching for faster responses
cache-prompts: true
```

---

## Command Line Quick Reference

### Most Used Flags

```bash
# Model selection
aider --model claude-3-5-sonnet-20241022
aider --model gpt-4o

# Git behavior
aider --no-auto-commits
aider --dirty-commits

# Context
aider --read README.md
aider --file src/main.py

# One-shot mode
aider --message "Add error handling" --yes

# Dry run (no changes)
aider --dry-run

# Show help
aider --help
```

### Useful Combinations

```bash
# Quick fix with auto-accept
aider --message "Fix the typo in line 42" --yes

# Add file and make change
aider src/auth.py --message "Add input validation"

# Use specific model for one session
aider --model gpt-4o-mini  # budget mode

# No git integration
aider --no-git
```

---

## Example Configurations

### TypeScript Web Project

```yaml
# .aider.conf.yml
model: claude-3-5-sonnet-20241022
weak-model: gpt-4o-mini

auto-commits: true
commit-prompt: conventional

read:
  - README.md
  - tsconfig.json
  - package.json

ignore:
  - "node_modules/**"
  - "dist/**"
  - "*.lock"
  - ".env*"

conventions: |
  TypeScript React project with:
  - Strict TypeScript
  - Functional components with hooks
  - CSS Modules for styling
  
  Always:
  - Use explicit return types on functions
  - Handle loading and error states
  - Write tests for new components
```

### Python Backend Project

```yaml
# .aider.conf.yml
model: claude-3-5-sonnet-20241022

auto-commits: true
dirty-commits: false

read:
  - README.md
  - pyproject.toml
  - docs/api.md

ignore:
  - "**/__pycache__/**"
  - "*.pyc"
  - ".venv/**"
  - ".env*"

test-cmd: pytest tests/ -v

conventions: |
  Python 3.11+ project using:
  - FastAPI for web framework
  - SQLAlchemy for ORM
  - Pydantic for validation
  
  Standards:
  - Type hints on all functions
  - Docstrings in Google format
  - Async where appropriate
```

### Minimal Configuration

```yaml
# Bare minimum
model: claude-3-5-sonnet-20241022
auto-commits: true
```

---

## Troubleshooting Configuration

### Check Current Config

```bash
# See what config is loaded
aider --verbose

# Test specific setting
aider --model gpt-4o --message "hello" --yes
```

### Config Not Loading

1. Check file location: `.aider.conf.yml` in project root or `~/.aider.conf.yml`
2. Verify YAML syntax: use a YAML validator
3. Check for typos in option names
4. Try command line flag to verify option works

### Override for Testing

```bash
# Command line always wins
aider --model gpt-4o-mini  # overrides config file
aider --no-auto-commits    # overrides auto-commits: true
```

---

*Configuration is about matching Aider to your workflow. Start simple, add options as you discover needs, and maintain project-specific configs for team consistency.*

