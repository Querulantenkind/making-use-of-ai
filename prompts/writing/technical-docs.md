# Technical Documentation Prompts

## Prompts for Creating Clear, Useful Documentation

Technical documentation bridges the gap between code and understanding. These prompts help you generate documentation that developers actually want to read.

---

## README Generation

### Project README

```
Generate a README.md for my project:

Project name: [name]
Purpose: [what it does]
Tech stack: [languages, frameworks, key dependencies]
Main features: [list key features]

Include these sections:
1. Title and brief description
2. Features list
3. Installation instructions
4. Quick start / basic usage
5. Configuration options
6. Contributing guidelines placeholder
7. License

Target audience: [developers, end users, both]
Tone: [professional, casual, technical]
```

### Library README

```
Generate a README for this library/package:

Library code:
[paste main module or API surface]

Include:
1. What problem it solves
2. Installation (npm/pip/etc.)
3. Quick start example
4. API reference summary
5. Common use cases with examples
6. Comparison to alternatives (brief)
7. Contributing and license

Keep it concise - link to detailed docs where appropriate.
```

---

## API Documentation

### Function/Method Documentation

```
Write documentation for this function:

[paste function code]

Include:
1. Brief description of what it does
2. Parameters with types and descriptions
3. Return value with type and description
4. Exceptions/errors it may raise
5. Usage example
6. Edge cases or important notes

Format: [JSDoc/docstring/XML comments/etc.]
```

### REST API Endpoint Documentation

```
Document this API endpoint:

Endpoint: [METHOD /path]
Handler code:
[paste handler]

Request/response examples:
[paste examples if available]

Generate documentation including:
1. Endpoint description
2. URL parameters
3. Query parameters
4. Request body schema with field descriptions
5. Response schema with field descriptions
6. Status codes and their meanings
7. Example request and response
8. Authentication requirements
9. Rate limiting (if applicable)

Format: [OpenAPI/Markdown/etc.]
```

### Generate OpenAPI Spec

```
Generate an OpenAPI 3.0 specification for these endpoints:

[paste route definitions or describe endpoints]

Include:
- Endpoint descriptions
- Request/response schemas
- Parameter descriptions
- Example values
- Error responses
- Security schemes

Output as YAML.
```

---

## Code Documentation

### Add Inline Comments

```
Add appropriate inline comments to this code:

[paste code]

Guidelines:
- Explain WHY, not WHAT (the code shows what)
- Comment complex algorithms or non-obvious logic
- Document assumptions and constraints
- Note any gotchas or edge cases
- Don't over-comment obvious code

Keep comments concise and valuable.
```

### Generate Module Documentation

```
Generate module-level documentation for this file:

[paste file contents]

Include:
1. Module purpose and responsibility
2. Key classes/functions with brief descriptions
3. Usage examples
4. Dependencies and requirements
5. Related modules (if mentioned in imports)

Format as a docstring/header comment appropriate for [language].
```

### Document Complex Algorithm

```
Document this algorithm:

[paste algorithm code]

Provide:
1. High-level description of what it accomplishes
2. Step-by-step explanation of the approach
3. Time and space complexity analysis
4. Why this approach was chosen (trade-offs)
5. Example walkthrough with sample input
6. Edge cases handled

Make it understandable to someone unfamiliar with this specific algorithm.
```

---

## Architecture Documentation

### System Architecture Overview

```
Generate architecture documentation for this system:

Components:
[list main components/services]

Relationships:
[describe how they interact]

Include:
1. High-level system overview
2. Component descriptions and responsibilities
3. Data flow description
4. Integration points
5. Technology choices and rationale
6. ASCII diagram of architecture

Target reader: [new team member, architect, stakeholder]
```

### Component Design Document

```
Write a design document for this component:

Component: [name]
Purpose: [what it does]
Code:
[paste relevant code or interface]

Document:
1. Overview and purpose
2. Design decisions and rationale
3. Interface/API surface
4. Internal structure
5. Dependencies
6. Error handling strategy
7. Testing approach
8. Future considerations

Keep it practical and maintainable.
```

### Database Schema Documentation

```
Document this database schema:

Schema:
[paste CREATE TABLE statements or schema definition]

For each table, include:
1. Purpose and what entity it represents
2. Column descriptions
3. Relationships to other tables
4. Indexes and their purpose
5. Constraints and validation rules
6. Example queries

Generate as Markdown with clear formatting.
```

---

## User-Facing Documentation

### User Guide Section

```
Write user documentation for this feature:

Feature: [name]
What it does: [description]
UI/interface: [describe how users interact with it]

Include:
1. Feature overview
2. Step-by-step instructions
3. Screenshots placeholders [describe where screenshots should go]
4. Tips and best practices
5. Common issues and solutions
6. Related features

Audience: [technical/non-technical]
Tone: [formal/friendly/instructional]
```

### FAQ Generation

```
Generate a FAQ section based on this documentation/feature:

[paste existing documentation or feature description]

Create 8-12 frequently asked questions covering:
1. Getting started questions
2. Common "how do I..." questions
3. Troubleshooting questions
4. Advanced usage questions

Format: Question as heading, concise answer below.
Keep answers focused and actionable.
```

### Error Message Documentation

```
Document these error messages for end users:

[list error codes/messages]

For each error:
1. What the error means (non-technical explanation)
2. Common causes
3. How to resolve it
4. When to contact support

Write in a helpful, non-blaming tone.
```

---

## Process Documentation

### Development Setup Guide

```
Write a development environment setup guide:

Project: [name]
Tech stack: [languages, tools, services]
Prerequisites: [what they need installed first]

Include:
1. Prerequisites checklist
2. Repository clone and setup
3. Environment configuration
4. Dependency installation
5. Database setup (if applicable)
6. Running the application
7. Running tests
8. Common setup issues and solutions

Assume the reader is a new developer on the team.
```

### Deployment Runbook

```
Create a deployment runbook for:

Application: [name]
Environment: [production/staging/etc.]
Infrastructure: [cloud provider, containers, etc.]

Include:
1. Pre-deployment checklist
2. Step-by-step deployment process
3. Verification steps
4. Rollback procedure
5. Monitoring and alerts to watch
6. Emergency contacts

Format for easy following during deployment.
```

### Incident Response Template

```
Create an incident response template for:

System: [name]
Type of incidents: [describe common incident types]

Include sections for:
1. Incident detection and initial response
2. Assessment and severity classification
3. Communication plan
4. Investigation steps
5. Resolution actions
6. Post-incident review
7. Follow-up tasks

Make it actionable under pressure.
```

---

## Documentation Improvement

### Improve Existing Documentation

```
Improve this documentation:

[paste existing docs]

Issues to address:
- [list specific issues, or ask for general improvement]

Improvements needed:
1. Clarity and readability
2. Completeness
3. Better organization
4. Updated examples
5. Consistent formatting

Maintain the existing structure where it works well.
```

### Convert Code to Documentation

```
This code is self-explanatory but undocumented. Generate documentation:

[paste well-written code]

Extract:
1. Purpose and functionality
2. API/interface documentation
3. Usage examples (derived from code patterns)
4. Important behaviors and edge cases

The code is the source of truth - document what it actually does.
```

### Technical to Non-Technical Translation

```
Translate this technical documentation for non-technical stakeholders:

[paste technical docs]

Maintain accuracy while:
1. Removing jargon (or explaining it)
2. Using analogies where helpful
3. Focusing on outcomes and benefits
4. Simplifying technical details
5. Keeping it concise

Target audience: [executives, clients, users]
```

---

## Quick Reference

### Documentation Principles

1. **Write for the reader** - Who will read this? What do they need?
2. **Start with why** - Purpose before details
3. **Use examples** - Show, don't just tell
4. **Keep it current** - Outdated docs are worse than no docs
5. **Make it scannable** - Headers, lists, code blocks

### Good Documentation Checklist

```
Before publishing documentation, verify:

[ ] Purpose is clear from the first paragraph
[ ] Target audience is appropriate
[ ] Steps are in logical order
[ ] Examples are tested and work
[ ] No broken links
[ ] Consistent formatting
[ ] Spelling and grammar checked
[ ] Technical accuracy verified
```

---

*Documentation is a gift to your future self and your teammates. Write what you wish existed when you started.*
