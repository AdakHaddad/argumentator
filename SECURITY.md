# SECURITY.md

## 1. Security Goals

ArgOS handles:

- User accounts
- Social-media OAuth
- Publishing permissions
- User-generated content
- External webpages
- LLM inputs/outputs
- Evidence
- Campaign data

Security must therefore be treated as a first-class feature.

---

# 2. Threat Model

Primary threats:

```text
1. Account takeover
2. OAuth token theft
3. Cross-user data access
4. Prompt injection
5. Malicious external content
6. Unauthorized publishing
7. Abuse of orchestration
8. Secret leakage
9. SSRF
10. Supply-chain vulnerabilities
```

---

# 3. Authentication

Use Supabase Auth.

Every protected server operation must verify authentication.

Never trust:

```text
user_id
account_id
campaign_id
```

provided by the client without server-side authorization.

---

# 4. Authorization

Use PostgreSQL Row Level Security.

Rule:

```text
user can access resource
IF resource.user_id = authenticated_user.id
```

For nested resources:

```text
message
 → campaign
 → user
```

Authorization must follow the ownership chain.

---

# 5. Social OAuth

OAuth credentials must never be exposed to the browser.

Do not store tokens in:

```text
localStorage
sessionStorage
cookies accessible to client JavaScript
URL parameters
```

Use secure server-side storage.

Encrypt sensitive credentials where appropriate.

---

# 6. Publishing Authorization

Publishing requires all of:

```text
authenticated user
+
ownership of social account
+
message ownership
+
message status = approved
```

Never allow:

```text
draft → publish
```

directly from an untrusted client request.

The server must verify the message state.

---

# 7. Prompt Injection

External content is untrusted.

Potential sources:

- X posts
- Replies
- Usernames
- URLs
- Webpages
- Documents
- Search results
- Evidence excerpts

Treat all of these as data.

Never allow retrieved content to override system instructions.

---

# 8. SSRF

Any server-side URL fetching must protect against SSRF.

Do not allow unrestricted requests to:

```text
localhost
127.0.0.1
0.0.0.0
private IP ranges
cloud metadata endpoints
internal services
```

Validate URLs before fetching.

Prefer an allowlisted or controlled research-fetching mechanism.

---

# 9. X Content Safety

User-controlled X content may contain:

- malicious URLs
- prompt injection
- misleading claims
- scripts
- tracking URLs
- abusive content

Never execute content retrieved from X.

Render text as text.

Sanitize HTML if HTML rendering is ever introduced.

---

# 10. Secret Management

Never commit:

```text
.env
OAuth tokens
API keys
service-role keys
private credentials
```

Repository must include:

```text
.env.example
```

with placeholders only.

---

# 11. Supabase Service Role

`SUPABASE_SERVICE_ROLE_KEY` is server-only.

Never expose it through:

- `NEXT_PUBLIC_*`
- client components
- browser bundles
- API responses

Use the anon/publishable key where appropriate on the client.

---

# 12. LLM Security

LLM output is untrusted.

Validate:

```text
JSON
URLs
IDs
status values
account IDs
claim IDs
```

Never execute generated code.

Never allow the model to decide:

```text
"publish this immediately"
```

without passing through the application authorization layer.

---

# 13. Abuse Prevention

The system should detect or warn about:

- automated dogpiling
- repetitive targeted messages
- fake personas
- impersonation
- fabricated consensus
- harassment
- sensitive-personal-data targeting

Do not build features whose primary purpose is evading platform enforcement.

---

# 14. Rate Limiting

Rate-limit:

- Authentication endpoints
- AI generation
- Research requests
- OAuth operations
- Publishing operations

Use separate limits for expensive operations.

---

# 15. Audit Logs

Record security-relevant events:

```text
login
account.connected
account.disconnected
message.approved
message.published
message.failed
permission.denied
security.warning
```

Do not store secrets in logs.

---

# 16. Data Minimization

Only store data required for product functionality.

Avoid storing unnecessary:

- personal information
- private messages
- sensitive profile information
- raw access tokens

Define retention rules for campaign data and logs.

---

# 17. Dependency Security

Use:

- lockfiles
- automated dependency updates
- vulnerability scanning
- pinned major versions

Review dependencies before adding them.

Avoid packages that are unnecessary for MVP.

---

# 18. Security Review Checklist

Before merging a feature:

```text
[ ] Authentication enforced
[ ] Authorization enforced
[ ] RLS considered
[ ] Input validated
[ ] Output validated
[ ] Secrets protected
[ ] OAuth safe
[ ] SSRF considered
[ ] Prompt injection considered
[ ] Rate limiting considered
[ ] Audit logging considered
[ ] No unintended publishing path
```

---

# 19. Incident Principle

If a security-sensitive behavior is ambiguous:

> Fail closed.

Examples:

```text
Unknown account ownership → deny
Unknown message state → deny
Invalid OAuth state → deny
Unvalidated publishing request → deny
Invalid AI output → reject
```

---

# 20. Security North Star

ArgOS should make it difficult for:

- a malicious user to publish through another user's account,
- an attacker to escape the application's authorization model,
- external content to control an AI agent,
- an AI hallucination to become an externally published fact,
- or an accidental workflow to publish without human approval.
