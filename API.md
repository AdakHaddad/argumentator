# API

## API principles

- All protected endpoints require authentication.
- Validate every request.
- Authorize every resource.
- Return structured errors.
- Never expose secrets.
- Publishing requires an approved message.

## Resource groups

```text
/campaigns
/claims
/evidence
/strategies
/messages
/accounts
/reviews
```

## Campaigns

### Create

```http
POST /api/campaigns
```

Request:

```json
{
  "title": "Example campaign",
  "description": "Argument workspace"
}
```

### Get

```http
GET /api/campaigns/:id
```

### Update

```http
PATCH /api/campaigns/:id
```

### Delete

```http
DELETE /api/campaigns/:id
```

All operations must verify ownership.

## Claims

```http
POST /api/campaigns/:id/claims
GET /api/campaigns/:id/claims
```

## Evidence

```http
POST /api/campaigns/:id/evidence
GET /api/campaigns/:id/evidence
```

## Strategies

```http
POST /api/campaigns/:id/strategies/generate
```

AI-generated output must be schema-validated before persistence.

## Messages

```http
POST /api/campaigns/:id/messages
GET /api/campaigns/:id/messages
PATCH /api/messages/:id
```

## Review

```http
POST /api/messages/:id/approve
POST /api/messages/:id/reject
```

Approval must be performed server-side.

## Publishing

```http
POST /api/messages/:id/publish
```

Server requirements:

```text
authenticated
AND owns message
AND owns connected account
AND message.status == approved
```

The server must re-check all conditions even if the UI already checked them.

## Accounts

```http
GET /api/accounts
POST /api/accounts/x/connect
POST /api/accounts/:id/disconnect
```

OAuth state must be validated.

## Error format

Prefer:

```json
{
  "error": {
    "code": "MESSAGE_NOT_APPROVED",
    "message": "Message must be approved before publishing."
  }
}
```

Do not expose stack traces or internal provider errors directly to users.

## Versioning

When public API compatibility becomes important, use versioned routes:

```text
/api/v1/...
```

Do not introduce versioning prematurely for internal MVP APIs.
