# Contributing

## Before contributing

Read:

- `README.md`
- `AGENTS.md`
- `PRD.md`
- `ARCHITECTURE.md`
- `PRODUCT_RULES.md`
- `SECURITY.md`

## Branches

Use descriptive branches:

```text
feat/argument-graph
fix/oauth-state
refactor/ai-provider
docs/security-model
```

## Commits

Prefer small, focused commits.

Examples:

```text
feat: add campaign creation
feat: add claim extraction
fix: enforce campaign ownership
fix: validate AI strategy output
docs: document X OAuth flow
```

## Pull requests

A PR should explain:

- What changed
- Why it changed
- How it was tested
- Any migration required
- Any security implications
- Any known limitations

## Code standards

Prefer:

- TypeScript
- Explicit types
- Small modules
- Server-side authorization
- Zod validation
- Reusable components

Avoid:

- `any`
- duplicated domain logic
- client-only authorization
- secrets in source
- giant components
- unnecessary dependencies

## Database changes

Database changes require migrations.

Never ask another developer to manually edit production tables.

## AI changes

Document:

- Prompt changes
- Output schema changes
- New failure cases
- Evaluation coverage

## Security

Report security issues privately rather than opening a public issue containing exploitable details.
