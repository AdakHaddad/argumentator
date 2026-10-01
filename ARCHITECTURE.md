# ARCHITECTURE.md

## 1. Purpose

ArgOS is an X-first argument intelligence and orchestration platform.

The architecture separates:

- Product/domain logic
- AI reasoning
- Social-platform integrations
- Publishing
- Evidence/research
- Authentication and authorization

The core argument system must work independently of X.

---

## 2. High-Level Architecture

```text
                         ┌──────────────────┐
                         │    Next.js UI    │
                         └────────┬─────────┘
                                  │
                         Server Actions /
                              Route API
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
      Campaign Domain       AI/Reasoning         Platform Layer
             │                    │                    │
             │              ┌─────┴─────┐       ┌─────┴─────┐
             │              │           │       │           │
             │          Argument     Strategy   X       Future
             │           Engine       Engine  Adapter    Adapters
             │              │           │       │
             └──────────────┼───────────┼───────┘
                            │
                            ▼
                     PostgreSQL/Supabase
```

---

## 3. Architectural Principles

### Domain first

Campaigns, arguments, evidence, strategies, messages, and reviews should not depend directly on X.

### Provider abstraction

Social platforms are adapters.

```ts
interface SocialPlatform {
  getPost(id: string): Promise<Post>;
  createReply(input: CreateReplyInput): Promise<PublishedPost>;
}
```

### Server-side secrets

OAuth credentials, LLM API keys, and platform tokens remain server-side.

### Explicit state transitions

Messages have explicit states:

```text
draft
review
approved
scheduled
publishing
published
failed
rejected
```

No implicit `draft → published`.

### Human approval

Publishing requires an explicit approval transition.

---

## 4. Repository Architecture

```text
src/
├── app/
│   ├── (auth)/
│   ├── dashboard/
│   ├── campaigns/
│   ├── settings/
│   └── api/
│
├── components/
│   ├── ui/
│   └── shared/
│
├── features/
│   ├── campaigns/
│   ├── arguments/
│   ├── evidence/
│   ├── strategies/
│   ├── messages/
│   ├── accounts/
│   └── reviews/
│
├── lib/
│   ├── auth/
│   ├── db/
│   ├── validation/
│   └── utils/
│
├── server/
│   ├── ai/
│   ├── platforms/
│   │   └── x/
│   ├── research/
│   └── publishing/
│
└── types/
```

---

## 5. Domain Boundaries

### Campaign

Owns the overall argument workspace.

### Argument

Owns:

- claims
- premises
- assumptions
- counterarguments
- relationships

### Evidence

Owns source material supporting or contradicting claims.

### Strategy

Owns communication strategy.

### Message

Owns a specific publishable artifact.

### Social Account

Owns connected platform identity and permissions.

### Review

Owns human approval state.

---

## 6. AI Boundary

AI should never directly mutate critical state.

Preferred flow:

```text
User request
    ↓
Server validates input
    ↓
AI generates structured proposal
    ↓
Zod validates proposal
    ↓
User reviews
    ↓
Server persists approved result
```

Avoid:

```text
User → LLM → unrestricted database mutation
```

---

## 7. Publishing Boundary

Publishing should be isolated behind:

```text
PublishingService
        │
        ├── XPublishingAdapter
        └── Future adapters
```

The campaign domain should not know how X API requests work.

---

## 8. Event Model

Future event types:

```text
campaign.created
claim.created
evidence.attached
argument.generated
strategy.generated
message.generated
message.approved
message.scheduled
message.published
message.failed
account.connected
account.disconnected
```

Use events for auditability and future analytics.

---

## 9. Observability

Log:

- request ID
- authenticated user ID
- campaign ID
- message ID
- platform
- operation
- latency
- result
- error category

Never log:

- OAuth access tokens
- refresh tokens
- API secrets
- passwords
- sensitive personal data

---

## 10. Scalability

MVP can use Next.js server actions and route handlers.

When publishing/research workloads grow, move long-running jobs to a queue:

```text
UI
 ↓
API
 ↓
Job Queue
 ↓
Worker
 ↓
Provider
```

Do not introduce distributed infrastructure before it is needed.

---

## 11. Testing Boundaries

Test each domain independently.

```text
Argument Engine
Strategy Engine
Evidence Service
Message Service
Review Service
X Adapter
```

Integration tests verify that these pieces work together.

---

## 12. Architecture Decision Rule

When uncertain:

1. Keep domain logic platform-independent.
2. Keep secrets server-side.
3. Prefer explicit state machines.
4. Validate all external data.
5. Require human approval for publishing.
6. Prefer simple infrastructure until scale requires more.
