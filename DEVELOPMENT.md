# Development

## Requirements

Recommended:

- Node.js LTS
- npm
- Git
- Supabase project or local Supabase CLI environment

## Setup

```bash
git clone <repository>
cd <repository>
npm install
cp .env.example .env.local
```

Configure environment variables before starting the application.

## Run

```bash
npm run dev
```

Open the local development URL shown by Next.js.

## Quality checks

Run before committing:

```bash
npm run lint
npm run typecheck
npm test
```

If a command does not exist yet, add it as part of the project setup rather than silently skipping the check.

## Database

Create and apply migrations through the project's migration workflow.

Do not modify production data manually.

## Local development principles

- Keep secrets in `.env.local`.
- Never commit credentials.
- Use test accounts for social integrations.
- Do not publish real social content during development.
- Prefer mocked X responses in automated tests.

## AI development

AI calls should be isolated behind an application service.

Use deterministic fixtures for tests whenever possible.

Do not make the test suite depend on an external LLM being available.

## Debugging

Log useful identifiers:

```text
request_id
user_id
campaign_id
message_id
```

Never log:

```text
access_token
refresh_token
API keys
passwords
```

## Production-like testing

Before enabling real X publishing:

1. Test OAuth.
2. Test ownership checks.
3. Test approval transitions.
4. Test failed publishing.
5. Test duplicate publishing protection.
6. Test rate-limit handling.
7. Verify audit logs.
