# Rules and Context in Cursor

## Teaching Your AI Collaborator About Your Project

The quality of AI assistance depends directly on context quality. Cursor can analyze your code, but it cannot read your mind. Rules and context bridge that gap, giving the AI understanding of your conventions, preferences, and project structure.

---

## The `.cursorrules` File

### Purpose

The `.cursorrules` file provides persistent context that Cursor includes in every interaction. It's your primary tool for shaping how the AI behaves in your project.

### Location

Place `.cursorrules` in your project root:
```
my-project/
├── .cursorrules      <-- Here
├── .cursorignore
├── src/
├── package.json
└── ...
```

### Anatomy of Effective Rules

A well-structured `.cursorrules` file includes:

```markdown
# Project Overview

Brief description of what this project is and does.
Mention the domain, purpose, and any critical context.

## Tech Stack

List of technologies with versions:
- Language and version
- Framework and version
- Key libraries
- Database
- Infrastructure

## Architecture

High-level architecture description:
- Main components and their responsibilities
- How they interact
- Key patterns in use (MVC, Clean Architecture, etc.)

## Code Conventions

Specific coding standards:
- Naming conventions
- File organization
- Import ordering
- Error handling patterns
- Testing approach

## Style Preferences

Your team's preferences:
- Prefer X over Y
- Always/Never do Z
- Specific patterns to follow

## Current Work Context

What you're currently working on:
- Active feature or bug
- Relevant files and areas
- Known issues or constraints

## Don'ts

Things the AI should avoid:
- Deprecated patterns
- Security anti-patterns
- Style violations
```

---

## Example `.cursorrules` Files

### TypeScript/React Project

```markdown
# E-Commerce Frontend

A React-based e-commerce frontend for a SaaS platform.
Users browse products, manage carts, and complete purchases.

## Tech Stack

- React 18.2 with TypeScript 5.3 (strict mode)
- Vite for build tooling
- TanStack Query v5 for server state
- Zustand for client state
- TailwindCSS 3.4 for styling
- React Hook Form + Zod for forms
- Vitest + Testing Library for tests

## Architecture

Feature-based folder structure:
```
src/
├── features/         # Feature modules
│   └── [feature]/
│       ├── api/      # API calls
│       ├── components/
│       ├── hooks/
│       └── types/
├── shared/           # Shared utilities
│   ├── components/   # Shared UI components
│   ├── hooks/
│   └── utils/
└── app/              # App shell, routing, providers
```

## Code Conventions

- Functional components only (no classes)
- Named exports only (no default exports)
- Colocate tests with source (*.test.tsx)
- Use absolute imports from @/ prefix
- Queries in /api folders use TanStack Query
- Forms use React Hook Form + Zod schemas

## Style Preferences

- Prefer early returns for guard clauses
- Destructure props in function signature
- Use semantic HTML elements
- Tailwind classes in className, not @apply
- Extract components at ~100 lines
- Custom hooks for reusable logic

## API Patterns

All API calls follow this pattern:
```typescript
// features/products/api/getProducts.ts
export const getProducts = async (): Promise<Product[]> => {
  const response = await apiClient.get('/products');
  return ProductArraySchema.parse(response.data);
};

// Hook using TanStack Query
export const useProducts = () => {
  return useQuery({
    queryKey: ['products'],
    queryFn: getProducts,
  });
};
```

## Don'ts

- Don't use any type (use unknown + type guards)
- Don't use index.ts barrel files (causes import issues)
- Don't inline Tailwind in render (extract to const if >3 classes)
- Don't use useEffect for data fetching (use TanStack Query)
- Don't mutate state directly

## Current Focus

Building the checkout flow. Key files:
- src/features/checkout/
- src/features/cart/
```

### Python/FastAPI Backend

```markdown
# Order Processing Service

FastAPI microservice handling order lifecycle.
Receives orders from frontend, processes payments, manages inventory.

## Tech Stack

- Python 3.12 with strict typing
- FastAPI 0.109
- SQLAlchemy 2.0 (async)
- PostgreSQL 15
- Redis for caching
- Celery for background tasks
- Pytest for testing

## Architecture

Clean Architecture layers:
```
src/
├── api/              # FastAPI routes (thin controllers)
├── domain/           # Business entities and logic
├── services/         # Application services (use cases)
├── repositories/     # Data access (SQLAlchemy)
├── schemas/          # Pydantic schemas (API contracts)
└── core/             # Config, dependencies, exceptions
```

## Code Conventions

- Type hints on all functions
- Pydantic models for all data shapes
- Dependency injection via FastAPI Depends
- Repository pattern for data access
- Service layer for business logic
- Routes only handle HTTP concerns

## Style Preferences

- Prefer explicit over implicit
- Return types always specified
- Use structural pattern matching (match/case)
- Single quotes for strings
- Ruff for formatting and linting

## Error Handling

```python
# Custom exceptions in core/exceptions.py
class OrderError(AppException):
    """Base exception for order domain."""
    pass

class OrderNotFoundError(OrderError):
    """Raised when order doesn't exist."""
    pass

# Handler converts to HTTP response
@app.exception_handler(OrderNotFoundError)
async def order_not_found_handler(request, exc):
    return JSONResponse(
        status_code=404,
        content={"detail": str(exc)}
    )
```

## Testing Approach

- Unit tests for domain logic (no DB)
- Integration tests for services (with test DB)
- API tests for routes (full stack)
- Fixtures in conftest.py
- Factory pattern for test data

## Don'ts

- Don't import from api/ into domain/
- Don't use ORM models in API responses (use schemas)
- Don't catch generic Exception
- Don't put business logic in routes
- Don't use sync database operations
```

---

## Dynamic Context with @ References

Beyond static rules, Cursor supports dynamic context references:

### File References

```
@src/services/auth.ts      # Include specific file
@src/features/auth/        # Include directory
@package.json              # Include config files
```

### Symbol References

```
@UserService               # Include class/function by name
@handleSubmit              # Reference specific function
@OrderStatus               # Reference type/enum
```

### Special References

```
@codebase                  # Search entire codebase
@docs                      # Include documentation
@git                       # Git context (recent commits, diffs)
@web                       # Web search for current info
```

---

## The `.cursorignore` File

### Purpose

Prevent specific files from being included in AI context. Essential for:
- Privacy (credentials, secrets)
- Efficiency (large generated files)
- Relevance (binary files, dependencies)

### Format

Uses gitignore syntax:

```gitignore
# Secrets and credentials
.env
.env.*
*.pem
*.key
secrets/
credentials/

# Dependencies (too large, not helpful)
node_modules/
vendor/
.venv/
__pycache__/

# Build outputs
dist/
build/
out/
.next/
*.min.js
*.bundle.js

# Binary and media files
*.png
*.jpg
*.jpeg
*.gif
*.pdf
*.zip
*.woff
*.woff2

# Generated files
*.generated.ts
coverage/
.nyc_output/

# IDE and system
.idea/
.vscode/
.DS_Store
*.log
```

---

## Context Strategies

### For New Projects

Start with comprehensive rules:
1. Document your tech stack completely
2. Establish conventions before writing code
3. Include example patterns you want followed
4. Update as project evolves

### For Existing Projects

Extract patterns from code:
1. Identify consistent patterns in your best code
2. Document conventions that aren't obvious
3. Note any technical debt to avoid propagating
4. Add architectural context

### For Legacy Projects

Focus on constraints:
1. Document what can't change (APIs, databases)
2. Identify modernization goals
3. Note deprecated patterns to avoid
4. Be explicit about incremental improvement strategy

---

## Context Quality Indicators

### Signs of Good Context

- AI follows your conventions consistently
- Code suggestions fit your architecture
- Less need for correction and retry
- AI references project patterns appropriately

### Signs Context Needs Work

- AI uses different conventions in each response
- Suggestions conflict with project patterns
- AI doesn't understand relationships between files
- Frequent need to explain project basics

---

## Maintaining Context

### Regular Updates

```markdown
## Recent Changes (Update Weekly)

- Migrated auth from sessions to JWT (2024-01)
- Added Redis caching layer (2024-02)
- Deprecated old API routes /v1/* (2024-03)
```

### Per-Session Context

In chat, add temporary context:

```
For this session, I'm focused on:
- Optimizing database queries
- The Order and OrderItem models
- Performance over readability tradeoffs
```

### Context Checkpoints

Periodically verify:
1. Are the documented patterns still accurate?
2. Has the tech stack changed?
3. Are there new conventions to document?
4. Should any patterns be deprecated?

---

## Advanced Patterns

### Multiple Rule Sets

For mono-repos or complex projects:

```
project/
├── .cursorrules              # Root rules (shared)
├── frontend/
│   └── .cursorrules          # Frontend-specific
├── backend/
│   └── .cursorrules          # Backend-specific
└── shared/
    └── .cursorrules          # Shared library rules
```

### Team Rules Templates

Create shareable templates:

```markdown
# Team Cursor Rules Template

## Required Sections
1. Tech stack with versions
2. Folder structure diagram
3. Naming conventions
4. Error handling patterns
5. Testing requirements

## Project-Specific Sections
[Add project-unique information here]

## Don'ts (Add your team's anti-patterns)
- ...
```

---

*Good context is an investment. Spend time crafting it well, keep it updated, and the AI becomes dramatically more useful. Poor context means fighting the AI's assumptions in every conversation.*

