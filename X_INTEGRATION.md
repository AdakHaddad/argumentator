# X INTEGRATION

## Purpose

X is the first social-platform integration.

The rest of ArgOS must not depend directly on X-specific implementation details.

Use an adapter:

```text
SocialPlatform
      │
      └── XPlatform
```

## Authentication

Use the supported X OAuth mechanism appropriate for the required API capabilities.

OAuth flow:

```text
User
 ↓
ArgOS
 ↓
X authorization
 ↓
Callback
 ↓
Validate state
 ↓
Exchange authorization
 ↓
Securely store credentials
 ↓
Connected account
```

Validate OAuth state to prevent CSRF.

## Account model

Store:

```text
platform
platform_user_id
handle
display_name
role
connection_status
```

Do not use the handle as the permanent identity key.

Use X's stable platform user ID.

## Account roles

Initial roles:

```text
primary
evidence
technical
questioner
context
skeptic
```

Roles describe how the account is used within a campaign.

They must not be used to fabricate fictional identities.

## Post capture

When importing an X post, preserve:

```text
post_id
author_id
author_handle
text
created_at
conversation_id
```

Treat imported text as untrusted external content.

## Publishing

Publishing flow:

```text
Draft
 ↓
Review
 ↓
Approved
 ↓
Publish request
 ↓
Ownership verification
 ↓
X API
 ↓
Published
```

The server must verify approval again at publish time.

## Idempotency

Publishing must protect against duplicate posts.

Store:

```text
external_post_id
```

and use an idempotency strategy where supported.

## Rate limits

Respect X API rate limits.

Do not implement mechanisms intended to evade platform rate limits or enforcement.

## Errors

Normalize provider errors:

```text
AUTH_EXPIRED
RATE_LIMITED
FORBIDDEN
NOT_FOUND
INVALID_REQUEST
PROVIDER_ERROR
```

Do not expose raw provider internals unnecessarily.

## Development

Use mocks/fixtures for automated tests.

Never run automated tests against a real account that could publish publicly.

## Multi-account orchestration

A campaign may contain messages assigned to multiple legitimate connected accounts.

The UI must show:

```text
which account
which message
which strategy
which approval state
```

Do not represent coordinated accounts as independent public consensus.

## Future compatibility

Keep the platform adapter generic enough to support future integrations without leaking X-specific types into the campaign domain.
