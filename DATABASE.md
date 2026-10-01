# DATABASE

## Database

ArgOS uses PostgreSQL through Supabase.

The database is the source of truth for application state.

## Core relationships

```text
auth.users
    │
    ├── campaigns
    │      ├── claims
    │      ├── evidence
    │      ├── strategies
    │      └── messages
    │               └── reviews
    │
    └── social_accounts
```

## Tables

### campaigns

```text
id
user_id
title
description
status
created_at
updated_at
```

### claims

```text
id
campaign_id
text
type
confidence
created_at
```

### evidence

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

### strategies

```text
id
campaign_id
name
description
created_at
```

### social_accounts

```text
id
user_id
platform
platform_user_id
handle
display_name
role
connection_status
created_at
updated_at
```

Sensitive OAuth material must not be exposed through normal client queries.

### messages

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
external_post_id
created_at
updated_at
```

### reviews

```text
id
message_id
reviewer_id
status
notes
created_at
```

## RLS

Every user-owned table must have RLS enabled.

Ownership must be checked through the appropriate relationship.

Example:

```text
messages
→ campaign
→ campaign.user_id = auth.uid()
```

## Status constraints

Message status:

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

Only valid transitions should be accepted.

## Migrations

All schema changes require migration files.

Migration names should describe the change:

```text
202610010001_create_campaigns.sql
202610010002_create_claims.sql
```

Never edit an already-applied migration to change production behavior.

Create a new migration instead.

## Indexes

At minimum:

```text
campaigns(user_id)
claims(campaign_id)
evidence(campaign_id)
evidence(claim_id)
strategies(campaign_id)
messages(campaign_id)
messages(social_account_id)
messages(status)
social_accounts(user_id)
reviews(message_id)
```

## Data retention

Store only information needed for product functionality.

Define explicit retention policies before collecting large amounts of social content or audit data.
