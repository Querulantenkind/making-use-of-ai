# Code Migration Project

## Migrating Between Languages or Frameworks with AI Assistance

This walkthrough demonstrates a systematic approach to code migration, whether changing languages (JavaScript to TypeScript), frameworks (Express to FastAPI), or major version upgrades that require significant refactoring.

---

## The Scenario

**Situation:** Your team has decided to migrate a JavaScript backend to TypeScript. The codebase is 15,000 lines with 50 files, reasonable test coverage, and active development continuing during migration.

**The challenge:**
- Can't stop development for weeks
- Must maintain working software throughout
- Need to preserve all functionality
- Want to improve code quality along the way

---

## Phase 1: Migration Assessment

### Understanding Scope

```
I'm planning a JavaScript to TypeScript migration.

Codebase:
- 50 files, ~15,000 lines
- Express.js backend
- MongoDB with Mongoose
- Jest tests (~60% coverage)

Help me assess:
1. What's the typical effort for this size migration?
2. What are the main risk areas?
3. What should I do before starting?
4. How should I sequence the work?
```

### Creating the Migration Plan

```
Based on the assessment, create a detailed migration plan.

Constraints:
- Team continues feature development
- Can't break production
- Want incremental rollouts

Include:
1. Phases with specific goals
2. Order of files/modules to migrate
3. Testing strategy at each phase
4. Rollback strategy
5. Estimated timeline
```

### Setting Up for Migration

```
What infrastructure changes do I need before migrating code?

Current setup:
- Node.js 18
- No build step (raw JS)
- ESLint for linting
- Jest for tests

What do I need to add/change to support TypeScript while keeping
existing JS working?
```

---

## Phase 2: Foundation Work

### TypeScript Configuration

```
Help me set up TypeScript for gradual migration.

Requirements:
- Existing JS files keep working unchanged
- New TS files can import from JS files
- JS files can import from TS files
- Strict mode eventually, but not initially

Generate:
1. tsconfig.json with appropriate settings
2. Explanation of key settings for migration
3. Build script that handles both JS and TS
```

### Type Definitions

```
I'm using these libraries without types. Help me handle them:

- mongoose (has @types)
- express (has @types)
- custom-internal-lib (no types)
- legacy-api-client (no types)

For each:
1. How to add type support
2. Recommended approach (install types vs create declarations)
3. What to do if types don't exist
```

### Shared Types Definition

```
Before migrating code, let's define core types.

Here are my main data structures (from Mongoose models):

```javascript
const userSchema = new Schema({
  email: String,
  name: String,
  role: { type: String, enum: ['admin', 'user', 'guest'] },
  createdAt: Date,
  settings: {
    notifications: Boolean,
    theme: String
  }
});
```

Create TypeScript interfaces for:
1. The domain types (User, etc.)
2. API request/response types
3. Database document types
```

---

## Phase 3: Incremental Migration

### Choosing Migration Order

```
Which files should I migrate first?

File types in my codebase:
- config/ (5 files) - Configuration loading
- utils/ (10 files) - Utility functions
- models/ (8 files) - Mongoose models
- services/ (12 files) - Business logic
- routes/ (10 files) - Express routes
- middleware/ (5 files) - Express middleware

What order minimizes risk and maximizes early benefit?
```

### Migrating a Simple File

Start with low-risk files:

```
Migrate this utility file to TypeScript:

```javascript
// utils/validation.js
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

function isValidEmail(email) {
  return emailRegex.test(email);
}

function isValidPhone(phone) {
  return /^\d{10,15}$/.test(phone);
}

function sanitizeInput(input) {
  if (typeof input !== 'string') return '';
  return input.trim().toLowerCase();
}

module.exports = { isValidEmail, isValidPhone, sanitizeInput };
```

Include:
1. Full TypeScript conversion
2. Proper type annotations
3. Explanation of type choices
```

### Migrating Complex Files

Progress to more complex files:

```
Migrate this service file. It's more complex:

```javascript
// services/userService.js
const User = require('../models/User');
const { hashPassword, verifyPassword } = require('../utils/crypto');
const { sendEmail } = require('./emailService');

async function createUser(data) {
  const existingUser = await User.findOne({ email: data.email });
  if (existingUser) {
    throw new Error('Email already exists');
  }
  
  const hashedPassword = await hashPassword(data.password);
  const user = await User.create({
    ...data,
    password: hashedPassword
  });
  
  await sendEmail(user.email, 'welcome', { name: user.name });
  
  return user;
}

// ... more functions
```

Handle:
1. Async function types
2. External dependencies (some migrated, some not)
3. Error types
4. Return types that may be null/undefined
```

### Handling Mixed JS/TS

```
I have this situation:

Migrated to TS:
- utils/validation.ts
- services/userService.ts

Still JavaScript:
- models/User.js
- routes/userRoutes.js

The TS files import from JS files. Show me:
1. How to write declaration files for the JS files
2. Best practices for the transition period
3. How to avoid type: any everywhere
```

---

## Phase 4: Testing During Migration

### Test Strategy

```
How should I handle tests during migration?

Current state:
- Jest tests in JavaScript
- Tests import from source files

Questions:
1. Should I migrate tests at the same time as source?
2. How to test TS files from JS test files?
3. When to migrate test files themselves?
```

### Migrating Tests

```
Migrate this test file alongside its source:

```javascript
// tests/userService.test.js
const { createUser, getUser } = require('../services/userService');
const User = require('../models/User');

jest.mock('../models/User');

describe('UserService', () => {
  describe('createUser', () => {
    it('should create a user with hashed password', async () => {
      User.findOne.mockResolvedValue(null);
      User.create.mockResolvedValue({ id: 1, email: 'test@test.com' });
      
      const result = await createUser({
        email: 'test@test.com',
        password: 'password123'
      });
      
      expect(result.email).toBe('test@test.com');
    });
  });
});
```

Include:
1. TypeScript conversion
2. Proper mock typing
3. Type-safe test data
```

---

## Phase 5: Handling Edge Cases

### Third-Party Libraries

```
I'm having trouble with library types:

Problem 1: Library has outdated @types
```typescript
// @types/legacy-lib says method returns string
// but it actually returns string | null
const result = legacyLib.getData(); // typed as string, but can be null
```

Problem 2: Library types don't match runtime
```typescript
// Types say config.timeout is number
// but runtime it's string from environment
```

How do I handle these mismatches safely?
```

### Type Assertions vs Type Guards

```
I'm using too many type assertions (as). Review this code:

```typescript
const user = await User.findById(id) as User; // unsafe!
const data = JSON.parse(response.body) as ApiResponse; // unsafe!
const config = process.env.CONFIG as string; // unsafe!
```

Show me how to handle each case properly with:
1. Type guards
2. Runtime validation
3. Proper null handling
```

### Complex Type Scenarios

```
I have complex type scenarios:

1. Function that returns different types based on options
```javascript
function fetchData(options) {
  if (options.raw) return rawBuffer;
  if (options.json) return parsedJson;
  return stringData;
}
```

2. Object with dynamic keys
```javascript
const handlers = {
  [EVENT_A]: handleA,
  [EVENT_B]: handleB,
};
```

3. Callback with multiple signatures
```javascript
function process(data, callback) {
  // callback might be (err) => void
  // or (err, result) => void
  // or (err, result, metadata) => void
}
```

Help me type each correctly.
```

---

## Phase 6: Strict Mode Transition

### Enabling Strict Checks

```
Migration is complete with loose TypeScript settings.
Now I want to enable strict mode.

Current tsconfig:
```json
{
  "compilerOptions": {
    "strict": false,
    "noImplicitAny": false,
    "strictNullChecks": false
  }
}
```

Create a plan to enable strict mode incrementally:
1. Which checks to enable first?
2. How to find violations?
3. How to fix common patterns?
```

### Fixing Strict Mode Violations

```
I enabled strictNullChecks and have 200+ errors. Example:

```typescript
async function getUser(id: string): Promise<User> {
  const user = await User.findById(id); // might be null
  return user; // Error: Type 'User | null' not assignable to 'User'
}
```

Show me patterns for fixing:
1. When null is expected (return null or throw)
2. When null should be impossible (assertion with reasoning)
3. When caller should handle null
```

---

## Migration Patterns

### The Strangler Pattern

```
For large migrations, use the strangler pattern:

1. Create new TS module alongside JS module
2. Route traffic to new module gradually
3. Monitor for issues
4. Remove old JS module when confident

How do I implement this for my Express app?
Show me a concrete example.
```

### The Bridge Pattern

```
During migration, I need bridges between JS and TS:

```typescript
// bridge/userService.ts
// Type-safe wrapper around untyped JS module

import untypedUserService from '../legacy/userService.js';
import { User, CreateUserInput } from '../types';

export async function createUser(data: CreateUserInput): Promise<User> {
  const result = await untypedUserService.createUser(data);
  return validateUser(result); // runtime validation
}
```

When should I use this pattern? Show me guidelines.
```

---

## Common Pitfalls

### Pitfall: Type Assertions Everywhere

**Problem:** Using `as` to silence TypeScript instead of fixing types.
**Solution:** Each `as` should be justified. Use type guards for runtime safety.

### Pitfall: Any Epidemic

**Problem:** Using `any` to make things compile, losing type safety.
**Solution:** Use `unknown` with type guards. Track `any` usage, reduce over time.

### Pitfall: Big Bang Migration

**Problem:** Trying to migrate everything at once.
**Solution:** Incremental migration with working software at each step.

### Pitfall: Ignoring Test Migration

**Problem:** Tests remain in JS, don't catch type errors.
**Solution:** Migrate tests alongside source, or at least use TS-aware test config.

### Pitfall: Fighting the Type System

**Problem:** Complex types that make code harder to understand.
**Solution:** Sometimes simpler types with runtime checks beat complex type gymnastics.

---

## Other Migration Types

### Framework Migration (Express to Fastify)

Same principles apply:
1. Assess scope and risk
2. Set up to support both frameworks
3. Migrate route by route
4. Test thoroughly at each step
5. Remove old framework

### Database Migration (MongoDB to PostgreSQL)

Additional considerations:
- Data migration tooling
- Schema mapping
- Query translation
- Dual-write period

### Major Version Upgrade (React 17 to 18)

Focus on:
- Breaking changes documentation
- Deprecation handling
- New feature adoption
- Incremental feature enablement

---

## Checklist

Before starting:
- [ ] Migration plan documented
- [ ] Team aligned on approach
- [ ] CI/CD supports mixed codebase
- [ ] Rollback strategy defined

During migration:
- [ ] Tests pass at each step
- [ ] No functionality regression
- [ ] Documentation updated
- [ ] Team knowledge shared

After migration:
- [ ] All code migrated
- [ ] Old code removed
- [ ] Strict mode enabled
- [ ] Documentation complete
- [ ] Team trained on new patterns

---

*Migrations are marathons, not sprints. AI accelerates each step, but success comes from systematic execution and continuous validation.*

