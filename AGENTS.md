# AGENTS.md

## Project: ArgOS

ArgOS is an X-first argument intelligence and orchestration platform.

The product helps users:

1. Capture an X post or argument.
2. Extract and structure claims.
3. Research and attach evidence.
4. Generate and test counterarguments.
5. Analyze communication strategies and possible cognitive biases.
6. Generate multiple response drafts.
7. Assign drafts to legitimate X accounts controlled by the user.
8. Review and approve messages.
9. Publish through supported X APIs/integrations.
10. Track conversations and outcomes.

Read `prd.md` before making architectural decisions.

---

# 1. Development Principles

## 1.1 Optimize the argument, not manipulation

The system may analyze psychology and cognitive biases to improve communication, but must not be designed around:

- Fake consensus
- Sockpuppet identities
- Impersonation
- Fabricated personal experiences
- Fabricated expertise
- Coordinated harassment
- Dogpiling individuals
- Exploiting sensitive personal vulnerabilities
- Deliberate misinformation

Multiple accounts are allowed only as legitimate accounts controlled by the user.

The UI should make account ownership and campaign coordination explicit.

## 1.2 Human approval is mandatory

Never automatically publish generated messages without explicit user approval.

Required flow:

```text
Generated
  ↓
Review
  ↓
Approved
  ↓
Published
```

There must be no hidden auto-publishing path.

## 1.3 Evidence before persuasion

When a response contains factual claims:

```text
Claim → Evidence → Draft
```

Do not silently invent sources.

## 1.4 Separate facts from interpretation

Internally distinguish:

- Fact
- Claim
- Inference
- Opinion
- Hypothesis
- Counterargument

Do not present model-generated interpretations as established facts.

---

# 2. Initial Tech Stack

Use:

- Next.js
- TypeScript
- Tailwind CSS
- shadcn/ui
- Supabase
- PostgreSQL
- Supabase Auth
- PostgreSQL Row Level Security
- Zod for runtime validation
- TanStack Query where client-side server-state management is useful

Use the latest stable versions available when initializing the project unless compatibility requires otherwise.

Prefer server-side operations for:

- OAuth
- X API calls
- LLM calls
- Secret handling
- Database mutations requiring authorization

Never expose OAuth client secrets or API secrets to the browser.

---

# 3. Repository Structure

Start with:

```text
/
├── AGENTS.md
├── README.md
├── prd.md
├── package.json
├── .env.example
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   ├── server/
│   └── types/
├── supabase/
│   └── migrations/
└── tests/
```

Suggested feature structure:

```text
src/features/
├── campaigns/
├── arguments/
├── evidence/
├── strategies/
├── messages/
├── accounts/
├── reviews/
└── analytics/
```

Keep domain logic out of UI components.

---

# 4. Core Domain Model

Implement these entities first.

## User

```text
id
email
created_at
```

## Social Account

```text
id
user_id
platform
handle
display_name
role
connection_status
created_at
```

## Campaign

```text
id
user_id
title
description
status
created_at
updated_at
```

## Claim

```text
id
campaign_id
text
type
confidence
created_at
```

## Evidence

```text
id
campaign_id
claim_id
title
url
source
published_at
excerpt
created_at
```

## Strategy

```text
id
campaign_id
name
description
created_at
```

## Message

```text
id
campaign_id
social_account_id
target_post_id
strategy_id
content
status
scheduled_at
published_at
created_at
```

## Review

```text
id
message_id
reviewer_id
status
notes
created_at
```

---

# 5. Database Rules

Every user-owned table must have appropriate RLS policies.

A user must only be able to access:

```text
their campaigns
their claims
their evidence
their strategies
their messages
their reviews
their connected accounts
```

Never rely exclusively on client-side filtering for authorization.

Add indexes for:

- user_id
- campaign_id
- social_account_id
- message status
- created_at

Use foreign keys and appropriate cascading behavior.

---

# 6. First MVP

Do NOT attempt to build every PRD feature immediately.

Implement this vertical slice first:

```text
Authentication
    ↓
Dashboard
    ↓
Create Campaign
    ↓
Paste X Post
    ↓
Extract Claim
    ↓
Create Argument
    ↓
Generate Draft
    ↓
Review
    ↓
Save Draft
```

Do not implement real X publishing until the internal campaign/message model works correctly.

---

# 7. MVP Pages

Build:

```text
/login
/dashboard
/campaigns/new
/campaigns/[id]
/campaigns/[id]/argument
/campaigns/[id]/messages
/settings/accounts
```

## Dashboard

Show:

- Recent campaigns
- Draft messages
- Connected accounts
- Campaign status

## Campaign

Show:

- Original X post
- Claims
- Evidence
- Counterarguments
- Strategies
- Messages

## Message Review

Show:

```text
Account
Target
Strategy
Draft
Evidence
Risk / integrity warnings

[Edit]
[Regenerate]
[Reject]
[Approve]
```

---

# 8. Argument Engine

Represent an argument as structured data.

Example:

```ts
type Argument = {
  claim: Claim;
  premises: Premise[];
  evidence: Evidence[];
  assumptions: Assumption[];
  counterarguments: Counterargument[];
};
```

The LLM must return validated structured output.

Never trust arbitrary LLM JSON.

Use Zod validation.

---

# 9. Strategy Engine

Implement these strategies initially:

```text
steelman
concession
socratic_question
reframe
evidence_first
prebunk
perspective_taking
clarification
red_team
```

A strategy should produce:

```ts
type StrategyResult = {
  strategy: string;
  rationale: string;
  draft: string;
  assumptions: string[];
  evidenceNeeded: string[];
};
```

The system should explain why a strategy was selected.

---

# 10. Psychology / Bias Analysis

Implement a `bias_audit` capability.

It may identify:

```text
confirmation_bias
anchoring
availability_heuristic
ingroup_outgroup_framing
reactance
social_proof
loss_framing
authority_effect
status_quo_bias
motivated_reasoning
```

The result must use uncertainty:

```text
{
  "bias": "confirmation_bias",
  "confidence": 0.62,
  "reason": "...",
  "evidence": [...]
}
```

Never claim that a person definitely has a psychological bias.

The purpose is argument analysis, not psychological profiling.

Do not infer sensitive personal traits.

---

# 11. Red Team

Create a dedicated action:

```text
Attack my argument
```

Output:

```text
Strongest opposing argument
Weakest premise
Missing evidence
Counterexample
Alternative interpretation
Likely objection
Recommended revision
```

Then allow:

```text
Defend revised argument
```

The red-team agent should genuinely attempt to defeat the argument instead of merely generating superficial objections.

---

# 12. X Integration

Start with an abstraction:

```ts
interface SocialPlatform {
  connect(): Promise<void>;
  getPost(id: string): Promise<Post>;
  createReply(input: CreateReplyInput): Promise<Post>;
}
```

Then implement:

```text
XPlatform
```

Do not couple the entire application directly to X API calls.

This makes future support for other platforms possible.

Use official X APIs and supported OAuth mechanisms.

Store tokens server-side in encrypted/secure storage.

Never put access tokens in:

- localStorage
- URL parameters
- client-side JavaScript
- database fields accessible to ordinary client queries

---

# 13. Account Roles

Initial roles:

```text
primary
evidence
technical
questioner
context
skeptic
```

These are functional publishing roles.

Do not represent accounts as fictional independent people.

The campaign UI should make it clear which connected account is being used.

---

# 14. Message Orchestration

Represent a campaign's messages as a queue.

Example:

```text
Campaign
│
├── Message 1
│   └── @main
│
├── Message 2
│   └── @research
│
└── Message 3
    └── @technical
```

Each message must have an independent review status.

Statuses:

```text
draft
review
approved
scheduled
published
failed
rejected
```

Never automatically transition:

```text
draft → published
```

without explicit approval.

---

# 15. Integrity Checks

Before approval, run checks for:

- Unsupported factual claims
- Missing citations
- Impersonation
- Fake personal experience
- Fake authority
- Manufactured consensus
- Targeted harassment
- Excessive repetitive posting
- Sensitive-personal-data exploitation

Warnings should explain the problem and suggest a safer alternative.

---

# 16. UI Design

The UI should feel like a modern research/debate workspace.

Priorities:

1. Clear hierarchy
2. Fast navigation
3. Minimal clutter
4. Strong keyboard support
5. Excellent mobile layout
6. Visible campaign state
7. Easy review before publishing

Important components:

```text
CampaignSidebar
ArgumentGraph
ClaimCard
EvidenceCard
CounterargumentCard
StrategySelector
MessageComposer
AccountSelector
ReviewPanel
CampaignTimeline
```

Do not build giant monolithic components.

---

# 17. API / Server Actions

Group operations by domain:

```text
campaigns
arguments
evidence
strategies
messages
accounts
reviews
```

Validate all external input.

Example:

```ts
const CreateCampaignSchema = z.object({
  title: z.string().min(1).max(200),
  description: z.string().max(5000).optional(),
});
```

Return typed errors.

Never expose internal stack traces to users.

---

# 18. Testing

Minimum tests:

### Unit

- Argument parsing
- Strategy selection
- Zod schemas
- Permission checks
- Bias analysis schema
- Message status transitions

### Integration

- Create campaign
- Add claim
- Attach evidence
- Generate message
- Review message
- Approve message

### Security

Verify:

- User A cannot access User B's campaign
- User A cannot access User B's social account
- Unapproved messages cannot publish
- OAuth secrets never reach client
- API endpoints enforce authentication

---

# 19. Environment Variables

Create `.env.example`.

Expected categories:

```text
NEXT_PUBLIC_APP_URL=

SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

X_CLIENT_ID=
X_CLIENT_SECRET=

LLM_API_KEY=
```

Never commit real credentials.

---

# 20. Development Workflow

For every feature:

```text
1. Read PRD
2. Inspect existing architecture
3. Define domain types
4. Define database changes
5. Implement server logic
6. Implement UI
7. Add validation
8. Add tests
9. Run lint/typecheck/tests
10. Review security
```

Do not make broad unrelated refactors.

Prefer small, reviewable commits.

---

# 21. Definition of Done

A feature is complete only when:

- TypeScript passes
- Lint passes
- Tests pass
- RLS is considered
- Authentication is enforced
- User input is validated
- Errors are handled
- Loading states exist
- Empty states exist
- Mobile layout works
- No secrets are exposed
- No unintended auto-publishing exists

---

# 22. Agent Behavior

When working on this repository:

### Before coding

Inspect:

```text
README.md
prd.md
AGENTS.md
package.json
src/
supabase/
```

Understand existing code before replacing it.

### While coding

Prefer:

```text
small changes
typed interfaces
server-side authorization
reusable components
domain-driven modules
```

Avoid:

```text
giant components
any types
duplicated API logic
client-side authorization
hardcoded credentials
unvalidated LLM output
```

### After coding

Run the relevant checks.

Report:

```text
What changed
Files changed
Tests run
Known limitations
Next recommended task
```

---

# 23. First Agent Task

Start by initializing the application foundation.

### Task

Implement:

1. Next.js application structure
2. TypeScript configuration
3. Tailwind/shadcn setup
4. Supabase client/server utilities
5. Authentication
6. Database migrations for:
   - campaigns
   - claims
   - evidence
   - strategies
   - social_accounts
   - messages
   - reviews
7. RLS policies
8. Dashboard
9. Campaign creation page
10. Basic campaign detail page
11. `.env.example`
12. README development instructions

Do not implement X publishing yet.

### Acceptance Criteria

A new developer can:

```text
clone
→ install
→ configure .env
→ run migrations
→ start dev server
→ register/login
→ create campaign
→ view campaign
→ add a claim
→ add evidence
```

with no manual database edits.

---

# 24. Recommended Implementation Order

```text
Phase 1
├── Project setup
├── Auth
├── Database
└── RLS

Phase 2
├── Campaigns
├── Claims
├── Evidence
└── Argument graph

Phase 3
├── LLM abstraction
├── Claim extraction
├── Counterarguments
├── Strategy engine
└── Red Team

Phase 4
├── Message composer
├── Review workflow
└── Campaign timeline

Phase 5
├── X OAuth
├── Account management
└── X post/reply integration

Phase 6
├── Multi-account orchestration
├── Scheduling
└── Analytics
```

Do not skip directly to Phase 5.

The internal argument system must work independently of X first.

---

# 25. Product North Star

ArgOS should make the user better at **thinking and communicating arguments**, not merely better at producing more posts.

The core loop is:

```text
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

The system should optimize for **clarity, evidence, intellectual honesty, and deliberate communication**.
