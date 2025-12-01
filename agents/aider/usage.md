# Aider Usage Patterns

## Effective Workflows for Terminal-Based AI Coding

Aider's strength is its git integration and terminal-native workflow. This guide covers practical patterns for getting the most out of it.

---

## Basic Workflow

### Starting a Session

```bash
# Start with specific files
aider src/auth.py src/models/user.py

# Start with a directory
aider src/

# Start with file pattern
aider "**/*.py"

# Start with no files (add later)
aider
```

### The Conversation Loop

```
$ aider main.py

> Add input validation to the process_order function

# Aider shows proposed changes
# Type 'y' to accept, 'n' to reject

> Actually, also handle the case where quantity is zero

# Aider builds on previous context

> /commit  # or auto-committed if enabled
```

---

## File Management

### Adding Files to Context

```bash
# In session, add more files
> /add src/utils.py

# Add multiple files
> /add tests/*.py

# Add read-only file (context but not edited)
> /read-only docs/api-spec.md
```

### Removing Files

```bash
# Remove file from session
> /drop src/old-file.py

# Clear all files
> /drop
```

### Viewing Current Files

```bash
# See what's in context
> /files

# See file tokens (context usage)
> /tokens
```

---

## Conversation Commands

### Essential Commands

| Command | Purpose |
|---------|---------|
| `/add <file>` | Add file to editing context |
| `/drop <file>` | Remove file from context |
| `/read-only <file>` | Add for context only (not editable) |
| `/files` | List current files |
| `/clear` | Clear conversation history |
| `/undo` | Undo last change |
| `/diff` | Show uncommitted changes |
| `/commit` | Commit pending changes |
| `/run <cmd>` | Run shell command |
| `/help` | Show all commands |

### Editing Commands

| Command | Purpose |
|---------|---------|
| `/architect` | Plan without implementing |
| `/ask` | Ask without making changes |
| `/code` | Default mode - make changes |

---

## Code Editing Patterns

### Implement Feature

```
> Add a caching layer to the getUserById function.
  Use Redis with a 5 minute TTL.
  Handle cache misses gracefully.
```

### Fix Bug

```
> This function throws an error when the input list is empty.
  Fix it to return an empty result instead.

# Or with the actual error
> I'm getting this error: [paste error]
  Fix the bug in process_data.py
```

### Refactor Code

```
> Refactor the PaymentProcessor class:
  1. Extract the validation logic into a separate method
  2. Use dependency injection for the payment gateway
  3. Add proper type hints
```

### Add Tests

```
> Write unit tests for the auth module.
  Cover the login, logout, and token refresh functions.
  Use pytest fixtures for test data.
```

---

## Context Management

### Strategic Context Building

```bash
# Start with core files
aider src/core/auth.py src/core/types.py

# Add related files as needed
> /add src/services/user.py

# Add docs for reference (read-only)
> /read-only docs/auth-flow.md
```

### Large Codebase Strategy

For big projects, be surgical:

```bash
# Don't add everything
# BAD: aider src/

# Add only what's relevant
aider src/auth/ src/models/user.py
```

### When Context Gets Confused

```bash
# Clear and restart
> /clear

# Or be explicit about focus
> Ignore previous discussion. 
  Focus only on the error handling in auth.py.
```

---

## Git Workflow Integration

### Auto-Commit Workflow

```bash
# Enable auto-commits (default)
aider --auto-commits

# Each change is committed automatically
> Add error handling to login function
# -> Committed: "feat: Add error handling to login function"
```

### Manual Commit Workflow

```bash
# Disable auto-commits for more control
aider --no-auto-commits

> Add feature X
> Add feature Y  
> Fix bug Z

# Review all changes
> /diff

# Commit when ready
> /commit "feat: Add X, Y, and fix Z"
```

### Undoing Changes

```bash
# Undo last aider change
> /undo

# Or use git directly
git checkout -- file.py
git reset --hard HEAD~1
```

### Branch Workflow

```bash
# Create feature branch first
git checkout -b feature/new-auth

# Work with aider
aider src/auth.py

# Multiple commits accumulate on branch
# Merge when complete
git checkout main
git merge feature/new-auth
```

---

## Advanced Patterns

### Architect Mode

Plan before implementing:

```
> /architect

> Design a notification system that:
  - Supports email, SMS, and push notifications
  - Uses a queue for async delivery
  - Has retry logic for failures

# Aider describes the design without coding

> /code

> Now implement the notification system based on that design
```

### Multi-File Changes

```
> Rename the User model to Account across the entire codebase.
  Update all imports, references, and tests.
```

Aider handles coordinated changes across files.

### Running Tests

```
> /run pytest tests/test_auth.py

# If tests fail
> The test_login test is failing with [error].
  Fix the implementation.
```

### Shell Integration

```
> /run npm test
> /run python -m pytest -v
> /run make build
```

Review output and iterate.

---

## Effective Prompting

### Be Specific

```
# Less effective
> Improve this code

# More effective
> Add input validation to process_order:
  - Validate order_id is a positive integer
  - Validate items is a non-empty list
  - Raise ValueError with descriptive messages for invalid input
```

### Provide Context

```
> The User model has these fields: id, email, name, created_at.
  Add a method to check if the account is older than 30 days.
```

### Reference Other Code

```
> Make the error handling in auth.py match the pattern used in database.py
```

### Iterative Refinement

```
> Add a cache to get_user

> Actually, use Redis instead of in-memory

> Add a cache invalidation method too

> Write tests for the caching behavior
```

---

## Common Workflows

### Bug Fix Workflow

```bash
# 1. Identify the issue
git bisect start
git bisect bad HEAD
git bisect good v1.0.0

# 2. Once found, start aider
aider problematic_file.py

# 3. Fix with context
> I found a bug introduced in commit abc123.
  The issue is [description].
  Here's the error: [error message]
  Fix it while maintaining existing behavior.

# 4. Verify
> /run pytest tests/

# 5. Done - aider already committed the fix
```

### Feature Development Workflow

```bash
# 1. Create branch
git checkout -b feature/user-preferences

# 2. Start aider with relevant files
aider src/models/ src/api/users.py

# 3. Build incrementally
> Create a UserPreferences model with fields for
  notification_enabled, theme, and language

> Add API endpoints for getting and updating preferences

> Write tests for the new endpoints

# 4. Review accumulated commits
git log --oneline

# 5. Merge when complete
```

### Refactoring Workflow

```bash
# 1. Ensure tests pass first
pytest tests/

# 2. Start aider
aider src/legacy_module.py

# 3. Refactor incrementally
> Extract the database logic into a separate repository class

> /run pytest tests/

> Now add type hints to all public methods

> /run pytest tests/

# Each step is committed, easy to rollback if needed
```

---

## Tips for Success

### Start Small

Begin with well-defined, limited tasks. Build confidence before tackling complex changes.

### Review Every Change

Even with auto-commit, review the diffs. Use `/diff` or `git diff` before proceeding.

### Use Read-Only Files

Add documentation, specs, or reference files as read-only. They inform without cluttering.

```
> /read-only docs/api-spec.yaml
> Implement the /users endpoint according to the spec
```

### Commit Often

Small, focused commits are easier to review and rollback. Auto-commits encourage this naturally.

### Know When to Stop

If aider struggles with something after a few attempts, it might be faster to do it manually or approach differently.

---

*Aider shines in its git-native workflow. Every change is tracked, reversible, and documented. Embrace the terminal and let version control be your safety net.*

