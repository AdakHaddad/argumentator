# PRD — X Argument Orchestration Platform

## 1. Product Overview

**Working name:** ArgOS  
**Platform:** X-first  
**Status:** Draft  
**Document:** PRD

ArgOS is an argument strategy and publishing workspace for users who want to develop, test, and publish persuasive arguments on X.

The product combines:

- Argument mapping
- Evidence management
- Audience/context analysis
- Psychology-informed communication strategies
- Multi-account orchestration
- Draft generation
- Human review
- Publishing and performance tracking

The product is **not** designed to manufacture fake consensus, impersonate independent people, or covertly manipulate users through fabricated identities.

---

## 2. Problem

Current social-media argument workflows are fragmented:

1. A user encounters a claim on X.
2. They manually research it.
3. They formulate an argument.
4. They write several possible replies.
5. They switch between accounts.
6. They manually decide which account should say what.
7. They lose track of evidence, context, and previous replies.
8. They have little structured way to test whether their argument survives strong counterarguments.

Users need a workspace where an argument can be treated as a structured object rather than a single text box.

---

## 3. Product Goal

Allow a user to go from:

> **"I disagree with this argument."**

to:

> **"Here is my structured argument, its strongest counterarguments, the evidence supporting it, several communication strategies, and the reviewed messages I want to publish from my legitimate X accounts."**

in one workflow.

---

## 4. Non-Goals

ArgOS will not:

- Create fake identities to simulate independent people.
- Manufacture artificial grassroots consensus.
- Pretend multiple controlled accounts are unrelated people.
- Automatically harass, dogpile, or target an individual.
- Exploit sensitive personal vulnerabilities.
- Generate deceptive claims about identity, expertise, affiliation, or experience.
- Automatically publish large volumes of messages without user approval.
- Optimize solely for outrage, humiliation, or emotional distress.

---

# 5. Core Concepts

## 5.1 Argument

An argument is a structured collection of:

- Claim
- Premises
- Evidence
- Assumptions
- Counterarguments
- Rebuttals
- Confidence level
- Sources

Example:

```text
Claim
└── X is misleading
    ├── Premise A
    ├── Premise B
    ├── Evidence 1
    └── Evidence 2
```

---

## 5.2 Strategy

A strategy describes **how an argument is communicated**, not what conclusion the user must adopt.

Supported strategies:

### Steelman

Present the strongest reasonable version of the opposing argument before responding.

### Concession

Identify a point that is genuinely valid before explaining where the disagreement remains.

### Socratic Questioning

Use questions to expose assumptions or inconsistencies.

### Reframing

Change the conceptual framing while preserving factual accuracy.

### Evidence Sequencing

Present information in an order that makes the logical structure easier to understand.

### Prebunking

Address predictable misconceptions or objections before they appear.

### Perspective-Taking

Explain how the issue appears from another person's perspective.

### Red Team

Attack the user's argument as strongly as possible.

### Clarification

Identify ambiguity before attempting to rebut a claim.

---

# 6. Psychology Layer

## 6.1 Purpose

The psychology layer helps the user understand how people may process an argument.

It should be used for:

- Better communication
- Identifying reasoning errors
- Anticipating objections
- Reducing unnecessary defensiveness
- Improving clarity
- Testing whether an argument depends on cognitive biases

It should not be used to covertly exploit an individual's vulnerabilities.

---

## 6.2 Supported Psychological Analysis

The system may identify concepts such as:

- Confirmation bias
- Anchoring
- Availability heuristic
- In-group/out-group framing
- Reactance
- Social proof
- Loss framing
- Authority effects
- Status-quo bias
- Dunning-Kruger effects
- Motivated reasoning

The system should distinguish:

```text
Psychological observation
        ≠
Proof that the audience will behave a certain way
```

---

## 6.3 Bias Audit

For every major argument, the system can generate:

**Potential bias in my argument**
- Confirmation bias
- Cherry-picking
- Appeal to authority
- False dichotomy
- Overgeneralization

**Potential bias in opposing argument**
- Anchoring
- Availability heuristic
- Moral framing
- Appeal to popularity

The system should explain why each classification is plausible rather than presenting it as certainty.

---

# 7. X Account Orchestration

## 7.1 Account Management

Users can connect legitimate X accounts they control.

Each account has:

- Account ID
- Display name
- Handle
- Profile metadata
- Role
- Permission level
- Publishing status
- Connection status

Example:

```text
@main_account
Role: Primary

@research_account
Role: Evidence / Research

@technical_account
Role: Technical explanation
```

---

## 7.2 Account Roles

Roles are communication functions, not fake identities.

### Primary

Publishes the user's central argument.

### Evidence

Publishes or replies with sources and factual context.

### Technical

Explains technical details.

### Questioner

Asks genuine clarifying questions.

### Skeptic

Tests both sides of the argument.

### Context

Provides background information.

---

# 8. Argument Orchestration

A user can create an **Argument Campaign**.

Example:

```text
Campaign
│
├── Original X post
│
├── Main claim
│
├── Supporting evidence
│
├── Opposing claims
│
├── Psychological analysis
│
├── Strategy
│
└── Message queue
     ├── @main
     ├── @research
     └── @technical
```

Each message should have:

- Account
- Target post
- Intent
- Strategy
- Draft
- Evidence
- Approval status
- Publishing status

---

# 9. Message Pipeline

```text
Capture X Post
      ↓
Extract Claims
      ↓
Build Argument Map
      ↓
Research / Evidence
      ↓
Generate Counterarguments
      ↓
Red Team
      ↓
Select Communication Strategy
      ↓
Generate Drafts
      ↓
Assign Account
      ↓
Human Review
      ↓
Publish
      ↓
Track Results
```

---

# 10. Message Composer

The composer should allow:

### Strategy

```text
[ Steelman ▼ ]
```

### Tone

```text
[ Neutral ]
[ Friendly ]
[ Direct ]
[ Academic ]
[ Concise ]
```

### Intent

```text
[ Explain ]
[ Question ]
[ Rebut ]
[ Provide evidence ]
[ Correct misinformation ]
[ De-escalate ]
```

### Account

```text
[ @main ▼ ]
```

### Evidence

```text
[ Source A ]
[ Source B ]
```

---

# 11. Campaign Timeline

Users should be able to visualize the planned conversation.

Example:

```text
Original Post
      │
      ├── @main
      │    └── Main argument
      │
      ├── @research
      │    └── Supporting evidence
      │
      └── @technical
           └── Technical explanation
```

The timeline should make it obvious which messages are coordinated.

---

# 12. Human Approval

No message should be automatically published by default.

Every message passes through:

```text
Generated
   ↓
Review
   ↓
Approved
   ↓
Published
```

The user can:

- Edit
- Reject
- Regenerate
- Change strategy
- Change account
- Attach evidence
- Delay publication

---

# 13. Safety / Integrity Layer

The system should detect potentially deceptive coordination.

## Block or warn on:

- Fake personas
- Impersonation
- Fabricated expertise
- Fake personal experiences
- Manufactured consensus
- Pretending accounts are independent
- Coordinated harassment
- Targeted dogpiling
- Manipulation based on sensitive personal characteristics
- Deliberate misinformation

Example warning:

> These accounts appear to be coordinated around the same argument. Avoid presenting them as independent public consensus.

---

# 14. Strategy Evaluation

Instead of scoring an argument as "good" or "bad", the system provides dimensions:

| Dimension | Assessment |
|---|---|
| Logical consistency | High / Medium / Low |
| Evidence coverage | High / Medium / Low |
| Counterargument coverage | High / Medium / Low |
| Unsupported assumptions | Detected / None |
| Emotional language | Low / Medium / High |
| Ambiguity | Low / Medium / High |
| Source quality | High / Medium / Low |

The user remains responsible for the final judgment.

---

# 15. Red Team Mode

A major feature.

User:

> "Attack my argument as strongly as possible."

System generates:

1. Strongest opposing argument
2. Weakest premise
3. Missing evidence
4. Possible counterexamples
5. Alternative interpretation
6. Likely objections
7. Suggested improvements

Then:

> "Now defend the argument against those attacks."

This creates an iterative loop:

```text
ARGUMENT
   ↓
ATTACK
   ↓
REVISE
   ↓
ATTACK AGAIN
   ↓
FINAL ARGUMENT
```

---

# 16. Evidence System

Evidence objects contain:

- Title
- URL
- Source
- Publication date
- Relevant excerpt
- Claim supported
- Reliability notes
- Contradicting evidence

Evidence should be attached to claims rather than merely attached to messages.

---

# 17. Argument Graph

Internal representation:

```text
Claim
 ├── supports → Claim
 ├── contradicts → Claim
 ├── depends_on → Premise
 ├── supported_by → Evidence
 └── challenged_by → Counterargument
```

This allows the same research to generate multiple communication strategies.

---

# 18. Suggested Data Model

## users

```text
id
email
created_at
```

## social_accounts

```text
id
user_id
platform
handle
role
connection_status
created_at
```

## campaigns

```text
id
user_id
title
description
status
created_at
```

## claims

```text
id
campaign_id
text
type
confidence
```

## evidence

```text
id
campaign_id
claim_id
title
url
source
published_at
excerpt
```

## strategies

```text
id
campaign_id
name
description
```

## messages

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
```

## reviews

```text
id
message_id
reviewer_id
status
notes
created_at
```

---

# 19. Technical Architecture

## Frontend

- Next.js
- TypeScript
- Tailwind CSS
- shadcn/ui

## Backend

- Next.js API routes / server actions
- Supabase
- PostgreSQL
- Row Level Security

## AI Layer

LLM-based services for:

- Claim extraction
- Argument mapping
- Counterargument generation
- Strategy generation
- Bias analysis
- Draft generation
- Red teaming

Use structured JSON outputs internally.

## Search / Research

- Web search
- Source extraction
- Citation verification
- Evidence ranking

## X Integration

Use official X APIs / supported integrations where available.

Store OAuth credentials securely and never expose access tokens to the client.

---

# 20. MVP

### Phase 1 — Argument Workspace

- Create campaign
- Paste X post
- Extract claims
- Build argument tree
- Add evidence
- Generate counterarguments
- Red Team mode
- Generate response drafts

### Phase 2 — X Accounts

- Connect X account
- Account selector
- Account roles
- Draft-to-account assignment
- Publishing through supported API capabilities
- Publishing history

### Phase 3 — Orchestration

- Multi-account campaign
- Message queue
- Conversation timeline
- Strategy per message
- Approval workflow
- Scheduling where supported

### Phase 4 — Analytics

- Replies
- Engagement
- Conversation branches
- Which arguments generated responses
- Evidence usage
- Strategy comparison

Analytics should describe observed behavior rather than claim that a psychological tactic caused a particular outcome.

---

# 21. Example User Flow

### Step 1

User pastes an X post.

> "Claim X."

### Step 2

ArgOS extracts:

```text
Claim:
X

Assumptions:
A
B

Potential weaknesses:
C
D
```

### Step 3

User selects:

> Red Team

System generates the strongest opposing argument.

### Step 4

User selects:

> Socratic + Evidence

System generates several drafts.

### Step 5

User assigns:

```text
@main      → Core argument
@research  → Evidence
@technical → Explanation
```

### Step 6

User reviews the complete campaign.

```text
3 messages
2 sources
1 counterargument
3 accounts
```

### Step 7

User explicitly approves each message.

### Step 8

Messages are published through supported X functionality.

---

# 22. Success Metrics

## Product

- Time from captured post → approved response
- Percentage of drafts edited before publication
- Number of arguments successfully red-teamed
- Evidence attached per campaign
- Repeat usage

## Quality

- Unsupported-claim rate
- Citation accuracy
- Counterargument coverage
- User-rated clarity

## Integrity

Track:

- Rejected deceptive-persona attempts
- Blocked impersonation attempts
- Human-review completion rate
- Number of messages published without required approval: target = 0

---

# 23. Key Product Principle

> **Optimize the argument, not the manipulation.**

The system should help users understand:

- What is true?
- What is uncertain?
- What is the strongest opposing argument?
- Why might someone disagree?
- Which communication strategy makes the reasoning clearer?
- What evidence actually supports the claim?

It should not turn psychology into a mechanism for covertly manufacturing agreement.

---

# 24. Future Direction

Potential future features:

- Argument library
- Debate replay
- Conversation simulation
- Audience-specific explanations
- Fallacy detection
- Source credibility analysis
- Claim provenance
- Collaborative debate workspaces
- Browser extension for capturing X posts
- Mobile companion
- Team review
- Public argument pages

---

# 25. One-Sentence Product Definition

**ArgOS is an X-first argument intelligence and orchestration platform that helps users research, stress-test, strategically communicate, and publish evidence-backed arguments across the legitimate accounts they control.**
