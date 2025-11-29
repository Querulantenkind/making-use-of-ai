# Cursor Setup Guide

## Getting Started with the AI-First IDE

Cursor is a fork of VS Code rebuilt around AI assistance. It combines the familiar VS Code interface with deep AI integration that makes it the most capable AI coding environment currently available.

---

## Installation

### Download

1. Visit [cursor.com](https://cursor.com)
2. Download for your platform (macOS, Windows, Linux)
3. Run the installer

### Migration from VS Code

Cursor can import your VS Code configuration:

- **Extensions**: Most VS Code extensions work
- **Settings**: Settings.json imports cleanly
- **Keybindings**: Custom keybindings transfer
- **Themes**: Your themes work in Cursor

On first launch, Cursor offers to import these automatically.

### Initial Configuration

After installation:

1. **Sign in** - Required for AI features
2. **Choose plan** - Free tier available with limits
3. **Configure AI settings** - Models, privacy preferences
4. **Import settings** - If migrating from VS Code

---

## Essential Settings

### AI Configuration

Access via Settings > Cursor Settings (or `Cmd/Ctrl + Shift + J`):

**Models:**
```
Default Model: claude-3-5-sonnet (recommended)
Long Context Model: claude-3-5-sonnet
Fast Model: claude-3-5-haiku (for autocomplete)
```

**Privacy:**
- Enable/disable codebase indexing
- Choose what context is sent
- Configure privacy mode for sensitive projects

**Behavior:**
- Auto-accept suggestions threshold
- Context inclusion preferences
- Response style settings

### Recommended Settings

```json
{
  // Enable codebase indexing for better context
  "cursor.ai.indexCodebase": true,
  
  // Use gitignore patterns for privacy
  "cursor.ai.respectGitignore": true,
  
  // Include documentation in context
  "cursor.ai.includeDocumentation": true,
  
  // Auto-run formatting after AI edits
  "cursor.ai.formatOnApply": true
}
```

---

## Understanding Cursor's AI Features

### Chat (`Cmd/Ctrl + L`)

The AI chat panel for longer conversations:

- Ask questions about your code
- Discuss architecture decisions
- Get explanations of complex logic
- Plan implementations before coding

**Best practices:**
- Include relevant files with `@file`
- Reference specific functions with `@symbol`
- Use for planning, understanding, and complex questions

### Composer (`Cmd/Ctrl + I`)

Inline AI that can edit multiple files:

- Quick edits and refactors
- Multi-file changes
- Implements features across codebase
- More action-oriented than Chat

**Best practices:**
- Use for tasks you want done, not discussed
- Review diff before applying
- Good for "write X" and "change Y" requests

### Autocomplete (`Tab`)

Intelligent code completion:

- Context-aware suggestions
- Multi-line completions
- Learns from your patterns
- Fast, inline assistance

**Best practices:**
- Let it load (slight delay for better suggestions)
- Tab to accept, Esc to dismiss
- Partial accept with `Cmd/Ctrl + Right Arrow`

### Inline Edit (`Cmd/Ctrl + K`)

Edit selected code:

- Select code, describe change
- AI modifies in place
- Good for targeted refactors
- Preview before accepting

---

## Project Setup

### The `.cursorrules` File

Create `.cursorrules` in your project root to give Cursor persistent context:

```markdown
# Project Context

This is a TypeScript/React application using:
- React 18 with hooks
- TypeScript strict mode
- TailwindCSS for styling
- React Query for data fetching
- Zod for validation

## Code Style

- Use functional components exclusively
- Prefer named exports over default exports
- Use TypeScript strict mode conventions
- Error messages should be user-friendly

## Conventions

- Components go in /src/components
- API calls go in /src/api
- Types go in /src/types
- Tests colocated with source files (*.test.ts)

## Current Focus

Working on the user authentication system. Key files:
- /src/features/auth/
- /src/api/auth.ts
```

### The `.cursorignore` File

Exclude files from AI context:

```
# Don't send these to AI
.env*
secrets/
*.pem
*.key

# Large generated files
node_modules/
dist/
build/
*.min.js

# Binary files
*.png
*.jpg
*.pdf
```

### Codebase Indexing

For large projects, enable indexing:

1. Open Command Palette (`Cmd/Ctrl + Shift + P`)
2. "Cursor: Index Codebase"
3. Wait for indexing to complete

This enables:
- Semantic code search
- Better context selection
- Cross-file understanding

---

## Keyboard Shortcuts

### Essential Shortcuts

| Action | macOS | Windows/Linux |
|--------|-------|---------------|
| Open Chat | `Cmd + L` | `Ctrl + L` |
| Open Composer | `Cmd + I` | `Ctrl + I` |
| Inline Edit | `Cmd + K` | `Ctrl + K` |
| Accept Suggestion | `Tab` | `Tab` |
| Reject Suggestion | `Esc` | `Esc` |
| Add to Chat | `Cmd + Shift + L` | `Ctrl + Shift + L` |

### Context Shortcuts

| Action | Shortcut |
|--------|----------|
| Add file to context | `@filename` in chat |
| Add symbol to context | `@symbolname` in chat |
| Add folder to context | `@foldername/` in chat |
| Add codebase context | `@codebase` in chat |
| Add docs | `@docs` in chat |

---

## Workflow Integration

### Git Integration

Cursor works seamlessly with Git:

- Review AI changes before committing
- Use Git to revert problematic AI edits
- Commit frequently when working with AI
- Branch for experimental AI-driven changes

**Recommended workflow:**
```bash
# Before starting AI-assisted work
git add . && git commit -m "checkpoint before AI changes"

# After significant AI changes
git diff  # Review changes
git add . && git commit -m "feat: implement X (AI-assisted)"
```

### Terminal Integration

Use Cursor's terminal:

- AI can read terminal output
- Commands can be referenced in chat
- Error messages provide context
- Test results inform next steps

### Extensions

Most VS Code extensions work. Particularly useful:

- **Error Lens** - Inline error display
- **GitLens** - Git history and blame
- **ESLint/Prettier** - Format AI-generated code
- **TODO Highlight** - Track AI-generated TODOs

---

## Model Selection

### When to Use Which Model

**Claude 3.5 Sonnet (Default):**
- Code generation
- Bug fixing
- Most development tasks
- Best balance of quality and speed

**Claude 3 Opus:**
- Complex architectural questions
- Difficult debugging
- When Sonnet produces inadequate results
- Worth the slower speed for hard problems

**GPT-4o:**
- Alternative perspective
- Multimodal tasks (with images)
- When Claude-specific issues occur

**Fast Models (Haiku, GPT-4o-mini):**
- Autocomplete (automatic)
- Simple questions
- High-volume tasks

### Switching Models

In Chat or Composer:
- Click model name in the interface
- Select alternative model
- Model persists for that conversation

---

## Troubleshooting

### Common Issues

**"Context too long" errors:**
- Remove unnecessary files from context
- Use more specific file references
- Ask about specific functions, not entire files

**Poor quality suggestions:**
- Add more context (`.cursorrules`, relevant files)
- Be more specific in your request
- Try rephrasing the question
- Switch models

**Slow responses:**
- Check internet connection
- Reduce context size
- Use faster model for simple tasks

**AI doesn't understand the codebase:**
- Rebuild the index
- Check `.cursorignore` isn't too aggressive
- Add project context in `.cursorrules`

### Getting Help

- Cursor documentation: [docs.cursor.com](https://docs.cursor.com)
- Community forum: [forum.cursor.com](https://forum.cursor.com)
- Discord: Active community for questions
- GitHub Issues: Bug reports and feature requests

---

## Next Steps

Now that Cursor is set up:

1. **Read [Rules and Context](rules-and-context.md)** - Master context management
2. **Study [Workflows](workflows.md)** - Learn effective patterns
3. **Explore [Tips and Tricks](tips.md)** - Optimize your usage

---

*Cursor is a tool. Like any tool, its effectiveness depends on how you wield it. Start with the basics, observe what works, and gradually expand your usage.*

