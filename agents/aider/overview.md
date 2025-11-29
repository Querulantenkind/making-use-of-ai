# Aider

## The Terminal-Native AI Coding Assistant

Aider is a command-line AI pair programmer that works directly with your Git repository. It's designed for developers who prefer working in the terminal and want AI assistance that integrates seamlessly with their existing workflow.

---

## What Makes Aider Different

### Git-Native

Aider treats Git as a first-class citizen:
- Automatic commits for AI changes
- Sensible commit messages generated
- Easy to revert AI modifications
- Works with your existing Git workflow

### Editor-Agnostic

Unlike IDE-integrated tools:
- Works with any editor (Vim, Emacs, VS Code, anything)
- No need to change your existing setup
- AI assistance happens in a separate terminal
- Files sync through the filesystem

### Transparent Operation

Everything Aider does is visible:
- See exactly what files are in context
- Watch the conversation in plain text
- Review diffs before they're applied
- Full control over what's committed

---

## Installation

### Via pip

```bash
pip install aider-chat

# Or with pipx for isolation
pipx install aider-chat
```

### API Key Setup

```bash
# OpenAI
export OPENAI_API_KEY=sk-...

# Anthropic (Claude)
export ANTHROPIC_API_KEY=sk-...

# Or use a config file
echo "ANTHROPIC_API_KEY=sk-..." >> ~/.aider.conf.yml
```

### Quick Start

```bash
cd your-project
aider

# Or specify files to work with
aider src/main.py src/utils.py
```

---

## Core Concepts

### The Chat Session

Aider runs as an interactive chat in your terminal:

```
$ aider src/app.py

Aider v0.50.0
Model: claude-3-5-sonnet with diff edit format
Git repo: .git
Added src/app.py to the chat.

> Add input validation to the process_data function
```

You describe what you want; Aider reads, edits, and commits.

### File Context

Aider needs to know which files to work with:

```bash
# Add files at start
aider file1.py file2.py

# Add files during session
> /add src/utils.py

# Remove files
> /drop src/utils.py

# Show current files
> /files
```

### Edit Modes

Aider supports different ways of receiving edits:

**Diff mode** (default, recommended):
- Shows changes as unified diffs
- More reliable for complex edits
- Better for reviewing changes

**Whole file mode**:
- Outputs entire files
- Simpler but uses more tokens
- Good for small files

---

## Essential Commands

### Session Control

| Command | Description |
|---------|-------------|
| `/help` | Show all commands |
| `/add <file>` | Add file to context |
| `/drop <file>` | Remove file from context |
| `/files` | List files in context |
| `/clear` | Clear conversation history |
| `/undo` | Undo last AI edit |
| `/quit` | Exit aider |

### Context Commands

| Command | Description |
|---------|-------------|
| `/read <file>` | Add file as read-only reference |
| `/ls` | List repository files |
| `/git <cmd>` | Run git command |
| `/run <cmd>` | Run shell command |

### Model Commands

| Command | Description |
|---------|-------------|
| `/model <name>` | Switch model |
| `/models` | List available models |
| `/tokens` | Show token usage |

---

## Working With Aider

### Basic Workflow

```bash
# Start session with files you'll modify
$ aider src/user_service.py

# Describe what you want
> Add a method to validate email addresses using regex

# Aider modifies the file and commits
# Review the diff it shows

# Continue the conversation
> Make the regex also accept plus-addressing like user+tag@email.com

# Check git log to see commits
$ git log --oneline -5
```

### Adding Context

```bash
# Reference files without editing them
> /read src/models.py

# Now ask questions that need that context
> How does the User model handle email validation?
```

### Complex Tasks

Break them down:

```bash
> First, let's understand the current authentication flow. 
  Explain how auth.py works.

> Now, let's add JWT support. What files will we need to modify?

> Start with adding the JWT utility functions.

> Now update the login endpoint to issue JWTs.
```

### Reviewing Changes

```bash
# Aider shows diffs before applying
# You can reject with:
> /undo

# Review what was changed
$ git diff HEAD~1

# Review the commit message
$ git log -1
```

---

## Model Configuration

### Default Models

```bash
# Use Claude (recommended)
aider --model claude-3-5-sonnet-20241022

# Use GPT-4
aider --model gpt-4o

# Use cheaper models
aider --model gpt-4o-mini
aider --model claude-3-5-haiku-20241022
```

### Configuration File

Create `~/.aider.conf.yml`:

```yaml
model: claude-3-5-sonnet-20241022
auto-commits: true
pretty: true
dark-mode: true

# API keys (better than environment variables)
anthropic-api-key: sk-ant-...

# Or use OpenAI
# openai-api-key: sk-...
```

### Local Models

Aider supports local models via Ollama:

```bash
# Start Ollama
ollama serve

# Use with Aider
aider --model ollama/llama3.1:70b
```

---

## Advanced Usage

### Repository Mapping

For large codebases, enable repository mapping:

```bash
aider --map-tokens 1024

# Or in config
# map-tokens: 1024
```

This gives Aider understanding of your entire codebase structure.

### Linting Integration

Have Aider respect your linter:

```bash
aider --lint-cmd "ruff check"

# Aider will run the linter after edits
# And attempt to fix any issues it introduces
```

### Testing Integration

```bash
aider --test-cmd "pytest tests/"

# Aider will run tests after changes
# And iterate if tests fail
```

### Watch Mode

For continuous assistance:

```bash
aider --watch

# Aider watches for file changes
# Automatically adds modified files to context
```

---

## Effective Patterns

### Pattern 1: Exploratory Coding

```bash
# Start with broad context
$ aider --read src/

# Ask about the codebase
> Explain the architecture of this project

# Then focus on specific files
> /add src/handlers/order.py
> Add pagination to the list_orders function
```

### Pattern 2: Test-Driven Development

```bash
$ aider src/calculator.py tests/test_calculator.py

> I want to add a divide function. 
  First, write a failing test for divide, 
  then implement the function.
```

### Pattern 3: Documentation Update

```bash
$ aider README.md docs/api.md src/api.py

> The API has changed. Update the documentation to match 
  the current implementation in src/api.py
```

### Pattern 4: Bug Investigation

```bash
$ aider src/problematic_file.py

> I'm getting this error: [paste error]
  Help me understand why and fix it.
```

### Pattern 5: Code Review Preparation

```bash
# After making changes
$ aider --read $(git diff --name-only main)

> Review these changes for:
  - Bugs
  - Security issues
  - Style violations
  
  Don't make changes, just report issues.
```

---

## Comparison With Other Tools

### Aider vs. Cursor

| Aspect | Aider | Cursor |
|--------|-------|--------|
| Interface | Terminal | Full IDE |
| Editor integration | Any editor | Built-in |
| Git integration | Native, automatic | Manual/assisted |
| Learning curve | Lower if CLI-comfortable | Lower for GUI users |
| Context control | Explicit | More automatic |
| Best for | Terminal workflows | Full IDE experience |

### When to Use Aider

- You prefer terminal-based workflows
- You want explicit control over context
- You work with multiple editors
- You value Git-native operation
- You want simpler, focused tool

---

## Troubleshooting

### "File not in context" errors

```bash
# Add the file
> /add path/to/file.py

# Or check what's in context
> /files
```

### Edits not applying correctly

```bash
# Try whole file mode for complex edits
> /whole-file

# Or undo and rephrase
> /undo
> [Rephrase your request more specifically]
```

### Token limit issues

```bash
# Drop files you don't need
> /drop unnecessary_file.py

# Use read-only for reference files
> /drop reference.py
> /read reference.py

# Clear conversation history
> /clear
```

### Model not working

```bash
# Check your API key
echo $ANTHROPIC_API_KEY  # or OPENAI_API_KEY

# Try explicit model
aider --model claude-3-5-sonnet-20241022

# Check available models
> /models
```

---

## Resources

- **Documentation**: https://aider.chat
- **GitHub**: https://github.com/paul-gauthier/aider
- **Discord**: Active community for questions

---

*Aider proves that effective AI coding assistance doesn't require a new IDE. If you live in the terminal, Aider meets you there.*

