# ArgOS

X-first argument intelligence and orchestration platform.

ArgOS helps users capture arguments, research evidence, stress-test reasoning, develop communication strategies, and publish reviewed messages through legitimate social accounts they control.

## Core loop

```text
Capture
  ↓
Understand
  ↓
Research
  ↓
Steelman
  ↓
Red Team
  ↓
Improve
  ↓
Communicate
  ↓
Review
  ↓
Publish
```

## Documentation

Start here:

1. `AGENTS.md` — coding-agent instructions
2. `PRD.md` — product requirements
3. `ARCHITECTURE.md` — system architecture
4. `PRODUCT_RULES.md` — non-negotiable product rules
5. `AI.md` — AI architecture
6. `SECURITY.md` — security requirements
7. `DEVELOPMENT.md` — local development
8. `DATABASE.md` — database design
9. `API.md` — API contracts
10. `X_INTEGRATION.md` — X integration

Detailed domain documentation lives under `docs/`.

## Development

```bash
npm install
npm run dev
```

Copy `.env.example` to `.env.local` and configure the required services.

## Project principles

- Evidence before rhetoric.
- Human approval before publishing.
- External social content is untrusted input.
- AI output must be validated.
- Account ownership must be explicit.
- Coordinated accounts must not be presented as independent consensus.
- The product should improve reasoning, not manufacture agreement.
