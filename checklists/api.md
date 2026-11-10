
# API Security Checklist

A practical security assessment checklist for REST APIs, GraphQL APIs, backend services, webhooks and AI-powered application interfaces.

**Project:**\
**API name:**\
**Base URL:**\
**Reviewer:**\
**Review date:**\
**API version:**\
**Environment:** Development / Staging / Production\
**API architecture:** REST / GraphQL / gRPC / WebSocket / Other\
**Authentication method:**\

---

## How to Use This Checklist

Review each applicable control against the actual API implementation and its infrastructure.

Use these statuses:

- [ ] Not reviewed
- [x] Verified and implemented
- [ ] Not applicable (document the reason)

A control should be marked complete only after its implementation and behavior have been verified.

### Severity Guidance

| Severity | Meaning | Expected action |
|---|---|---|
| Critical | Severe compromise, unrestricted data exposure or major unauthorized access | Stop release until resolved |
| High | Significant weakness affecting authentication, authorization or sensitive operations | Resolve before production |
| Medium | Meaningful security weakness with limited or conditional impact | Resolve or formally accept risk |
| Low | Limited weakness or defense-in-depth improvement | Track and prioritize |
| Informational | Documentation or improvement opportunity | Review and document |

---

# 1. API Architecture and Attack Surface

- [ ] API architecture is documented.
- [ ] All public endpoints are inventoried.
- [ ] Internal endpoints are inventoried.
- [ ] Administrative endpoints are identified.
- [ ] API versions are documented.
- [ ] Deprecated endpoints are identified.
- [ ] Authentication requirements are documented.
- [ ] Authorization requirements are documented.
- [ ] Sensitive data flows are documented.
- [ ] Third-party integrations are inventoried.
- [ ] External service dependencies are identified.
- [ ] Trust boundaries are documented.
- [ ] API gateways and reverse proxies are reviewed.
- [ ] Internal services are not unnecessarily exposed.
- [ ] Threat modeling has been performed for sensitive API functionality.
- [ ] API documentation is maintained and reviewed.

# 2. Authentication

- [ ] Protected endpoints require authentication.
- [ ] Authentication is performed server-side.
- [ ] Passwords are never stored in plaintext.
- [ ] Password hashing uses a suitable algorithm.
- [ ] Authentication tokens are generated securely.
- [ ] Token signatures are verified.
- [ ] Token expiration is enforced.
- [ ] Token issuer and audience are validated where applicable.
- [ ] Token algorithm handling is secure.
- [ ] Unsupported or unexpected token algorithms are rejected.
- [ ] Refresh tokens are protected.
- [ ] Refresh token rotation is implemented where appropriate.
- [ ] Token revocation is supported where required.
- [ ] API keys are scoped and restricted.
- [ ] API keys have appropriate expiration or rotation procedures.
- [ ] Authentication failures do not reveal sensitive information.
- [ ] Login and token issuance endpoints are rate-limited.
- [ ] Multi-factor authentication is supported for sensitive accounts where appropriate.
- [ ] Service-to-service authentication is implemented securely.
- [ ] Default or test credentials are removed.

# 3. Authorization and Access Control

- [ ] Authorization is enforced on the server.
- [ ] Every protected endpoint checks permissions.
- [ ] Object-level authorization is implemented.
- [ ] Function-level authorization is implemented.
- [ ] Property-level authorization is implemented.
- [ ] Users cannot access another user's private resources.
- [ ] Users cannot modify resources they do not own.
- [ ] Administrative endpoints require appropriate privileges.
- [ ] Role-based access control is tested.
- [ ] Attribute-based access control is tested where applicable.
- [ ] Multi-tenant isolation is enforced.
- [ ] Tenant identity is derived from trusted authentication context.
- [ ] Client-provided tenant IDs are not blindly trusted.
- [ ] Privilege escalation is tested.
- [ ] Authorization is checked for background operations.
- [ ] Authorization is checked during bulk operations.
- [ ] Authorization is checked for nested resources.
- [ ] Permission changes take effect appropriately.
- [ ] Default access is denied unless explicitly granted.

# 4. Broken Object Level Authorization (BOLA / IDOR)

- [ ] Every object access verifies the caller's authorization.
- [ ] Object ownership is checked server-side.
- [ ] Resource IDs are not treated as authorization controls.
- [ ] UUIDs are not relied upon as a replacement for authorization.
- [ ] Read operations are tested against unauthorized object IDs.
- [ ] Update operations are tested against unauthorized object IDs.
- [ ] Delete operations are tested against unauthorized object IDs.
- [ ] Nested resource access is tested.
- [ ] Bulk object operations verify authorization for every object.
- [ ] Cross-tenant object access is tested.
- [ ] File and document access verifies ownership or permission.
- [ ] Indirect references and relationship endpoints are reviewed.
- [ ] Authorization tests cover multiple user roles.

# 5. Broken Function Level Authorization (BFLA)

- [ ] Administrative functions are protected.
- [ ] Privileged operations require the correct role.
- [ ] Users cannot invoke restricted HTTP methods.
- [ ] Hidden frontend controls are not treated as security controls.
- [ ] Internal functions are not publicly accessible.
- [ ] Role changes cannot be performed by unauthorized users.
- [ ] Privileged API routes are tested directly.
- [ ] Function permissions are enforced independently of frontend navigation.
- [ ] Sensitive actions require suitable authorization.

# 6. Broken Object Property Level Authorization

- [ ] API responses expose only necessary fields.
- [ ] Sensitive fields are excluded from public responses.
- [ ] Password hashes are never returned.
- [ ] Internal account flags are not unnecessarily exposed.
- [ ] Users cannot modify protected properties.
- [ ] Mass assignment risks are reviewed.
- [ ] Input schemas explicitly define writable fields.
- [ ] Output schemas explicitly define readable fields.
- [ ] Administrative fields cannot be changed by ordinary users.
- [ ] Sensitive profile properties have appropriate access restrictions.
- [ ] Database models are not blindly serialized into API responses.

# 7. Input Validation and Injection Prevention

- [ ] All untrusted request input is validated.
- [ ] Validation occurs server-side.
- [ ] Request body size is limited.
- [ ] Query parameters are validated.
- [ ] Path parameters are validated.
- [ ] Headers are validated where relevant.
- [ ] Input types and formats are enforced.
- [ ] Unexpected properties are rejected or safely handled.
- [ ] SQL queries use parameterized statements.
- [ ] NoSQL queries are protected against injection.
- [ ] Command execution is avoided for untrusted input.
- [ ] LDAP injection is reviewed where applicable.
- [ ] Template injection is reviewed.
- [ ] XML external entity risks are addressed where XML is accepted.
- [ ] Deserialization of untrusted data is avoided or safely constrained.
- [ ] Regular expressions cannot cause avoidable denial of service.
- [ ] URL parsing and validation use trusted libraries.
- [ ] File paths are normalized and validated.
- [ ] Input validation does not rely solely on client-side checks.

# 8. Rate Limiting and Resource Consumption

- [ ] Rate limiting is enabled for public endpoints.
- [ ] Authentication endpoints have stricter limits where appropriate.
- [ ] Password reset endpoints are rate-limited.
- [ ] OTP and verification endpoints are rate-limited.
- [ ] Expensive operations have appropriate limits.
- [ ] Rate limits consider authenticated users and relevant abuse dimensions.
- [ ] IP-based controls are not the only protection where distributed abuse is possible.
- [ ] Request body size limits are enforced.
- [ ] Response size limits are considered.
- [ ] Pagination limits are enforced.
- [ ] Maximum query complexity is enforced for GraphQL where appropriate.
- [ ] Database query timeouts are configured.
- [ ] Expensive background jobs are protected.
- [ ] Concurrent request limits are considered.
- [ ] Abuse patterns are monitored.
- [ ] Rate-limit responses do not expose internal details.

# 9. API Key and Token Security

- [ ] API keys are generated using secure randomness.
- [ ] API keys are stored securely.
- [ ] API keys are not committed to Git.
- [ ] API keys are not exposed in frontend bundles.
- [ ] API keys are not placed in URLs.
- [ ] API keys are scoped to required operations.
- [ ] API keys have appropriate expiration policies.
- [ ] Key rotation procedures are documented.
- [ ] Compromised keys can be revoked.
- [ ] Service credentials are separated by environment.
- [ ] Tokens are not logged.
- [ ] Tokens are transmitted only over secure connections.
- [ ] Token revocation behavior is tested.
- [ ] Refresh token replay risks are addressed.
- [ ] Privileged credentials remain on trusted server-side systems.

# 10. GraphQL Security

For APIs that use GraphQL:

- [ ] GraphQL queries require appropriate authentication.
- [ ] Authorization is enforced at the resolver or service layer.
- [ ] Object-level authorization is enforced.
- [ ] Field-level access restrictions are implemented.
- [ ] Introspection exposure is reviewed.
- [ ] Query depth limits are configured.
- [ ] Query complexity limits are configured.
- [ ] Expensive nested queries are controlled.
- [ ] Pagination limits are enforced.
- [ ] Batching abuse is considered.
- [ ] Alias-based abuse is considered.
- [ ] Mutations validate all input.
- [ ] Mutations enforce authorization.
- [ ] Sensitive fields are not exposed unnecessarily.
- [ ] Error messages do not disclose internal details.
- [ ] GraphQL subscriptions authenticate and authorize connections.
- [ ] Subscription access is rechecked where necessary.
- [ ] Rate limiting applies to GraphQL operations.

# 11. REST API Security

- [ ] HTTP methods are restricted appropriately.
- [ ] GET requests do not perform sensitive state-changing actions.
- [ ] POST, PUT, PATCH and DELETE operations enforce authorization.
- [ ] Request content types are validated.
- [ ] Response content types are appropriate.
- [ ] Unsupported methods are rejected.
- [ ] HTTP status codes are used consistently.
- [ ] API versioning is controlled.
- [ ] Pagination is bounded.
- [ ] Sorting and filtering parameters are validated.
- [ ] Bulk endpoints enforce per-object authorization.
- [ ] Response fields are minimized.
- [ ] Error responses do not expose stack traces.
- [ ] Cache behavior is reviewed for sensitive responses.
- [ ] Idempotency is implemented where required for important operations.

# 12. Cross-Origin Resource Sharing (CORS)

- [ ] CORS is configured intentionally.
- [ ] Trusted origins are explicitly identified.
- [ ] Unnecessary wildcard origins are avoided.
- [ ] Credentialed requests use specific trusted origins.
- [ ] Allowed methods are restricted.
- [ ] Allowed headers are restricted.
- [ ] Preflight requests are handled correctly.
- [ ] Origin validation avoids unsafe substring matching.
- [ ] Development origins are not unnecessarily allowed in production.
- [ ] CORS behavior is tested from untrusted origins.

**Important:** CORS is not authentication or authorization. It does not prevent direct API requests from non-browser clients.

# 13. Cross-Site Request Forgery (CSRF)

- [ ] Cookie-authenticated state-changing endpoints are assessed for CSRF.
- [ ] Appropriate CSRF protection is implemented.
- [ ] State-changing operations do not rely on GET requests.
- [ ] SameSite cookie settings are appropriate.
- [ ] Origin or Referer validation is used where appropriate.
- [ ] Sensitive operations are tested for CSRF.
- [ ] Token-based authentication behavior is reviewed for browser-specific risks.
- [ ] Cross-origin credential behavior is reviewed.

# 14. Server-Side Request Forgery (SSRF)

- [ ] URL-fetching functionality is inventoried.
- [ ] User-controlled URLs are validated.
- [ ] Internal IP ranges are blocked where not required.
- [ ] Cloud metadata endpoints are protected.
- [ ] Redirect destinations are revalidated.
- [ ] DNS rebinding risks are considered.
- [ ] Outbound network access is restricted where practical.
- [ ] URL parsing uses trusted libraries.
- [ ] Webhook destination URLs are validated.
- [ ] Image and document URL import features are reviewed.
- [ ] Server-side integrations cannot access unintended internal services.

# 15. File Uploads and Downloads

- [ ] Upload endpoints require appropriate authorization.
- [ ] File size limits are enforced.
- [ ] File types are validated using content inspection where appropriate.
- [ ] File extensions are not blindly trusted.
- [ ] Filenames are safely generated.
- [ ] Uploaded files are stored securely.
- [ ] Public access is disabled by default for private files.
- [ ] Download endpoints enforce authorization.
- [ ] Signed URLs expire appropriately.
- [ ] Malware scanning is considered for relevant file types.
- [ ] Archive extraction is protected against path traversal.
- [ ] File processing libraries are maintained.
- [ ] Uploaded content cannot execute as application code.
- [ ] Temporary files are protected.
- [ ] File deletion behavior is tested.

# 16. Webhook Security

- [ ] Webhook endpoints are documented.
- [ ] Webhook signatures are verified.
- [ ] Signature verification uses constant-time comparison where appropriate.
- [ ] Webhook signing secrets are protected.
- [ ] Timestamp validation is implemented where supported.
- [ ] Replay attacks are considered.
- [ ] Duplicate webhook events are handled safely.
- [ ] Event identifiers are tracked where appropriate.
- [ ] Webhook payloads are validated.
- [ ] Webhook sender identity is verified.
- [ ] Webhook endpoints are rate-limited.
- [ ] Webhook secrets can be rotated.
- [ ] Failed webhook processing is handled safely.
- [ ] Sensitive webhook payloads are not unnecessarily logged.

# 17. Database and Backend Security

- [ ] Database access follows least privilege.
- [ ] Database credentials are not exposed to API consumers.
- [ ] Parameterized queries are used.
- [ ] Database network access is restricted.
- [ ] Sensitive data is protected at rest where appropriate.
- [ ] Row-level security is tested where applicable.
- [ ] Tenant isolation is enforced.
- [ ] Database error messages are not returned directly.
- [ ] Transaction boundaries are appropriate.
- [ ] Sensitive changes are audited.
- [ ] Database backups are protected.
- [ ] Database migrations are reviewed.
- [ ] Privileged database credentials are restricted to trusted services.
- [ ] Production and development database access are separated.

# 18. Security Headers and Transport Security

- [ ] HTTPS is enforced.
- [ ] TLS configuration is reviewed.
- [ ] Invalid TLS certificates are rejected by clients.
- [ ] HSTS is configured where appropriate.
- [ ] API gateway security headers are reviewed where applicable.
- [ ] Sensitive responses use suitable cache-control directives.
- [ ] Server version disclosure is minimized.
- [ ] Request and response content types are appropriate.
- [ ] Proxy and forwarded headers are trusted only from known infrastructure.
- [ ] TLS termination and internal service communication are reviewed.

# 19. Error Handling and Information Disclosure

- [ ] Errors do not reveal stack traces.
- [ ] Errors do not expose database queries.
- [ ] Errors do not reveal internal file paths.
- [ ] Authentication errors do not expose unnecessary account details.
- [ ] Authorization errors do not reveal private resource contents.
- [ ] Debug responses are disabled in production.
- [ ] Sensitive values are redacted.
- [ ] Unexpected exceptions are handled safely.
- [ ] Internal diagnostics are logged securely.
- [ ] Error responses follow a consistent format.
- [ ] Sensitive headers are not returned unnecessarily.

# 20. Logging, Monitoring and Auditing

- [ ] Authentication events are logged.
- [ ] Authorization failures are logged appropriately.
- [ ] Privileged operations are audited.
- [ ] Sensitive data modifications are logged.
- [ ] API abuse patterns are monitored.
- [ ] Rate-limit violations are monitored.
- [ ] Webhook failures are monitored.
- [ ] Logs exclude passwords.
- [ ] Logs exclude tokens and API secrets.
- [ ] Personal information is minimized or redacted.
- [ ] Log access is restricted.
- [ ] Log integrity is considered.
- [ ] Retention policies are defined.
- [ ] Security alerts are configured where appropriate.
- [ ] Incident investigation procedures are documented.

# 21. Multi-Tenant API Security

- [ ] Tenant identity is derived from trusted authentication context.
- [ ] Client-supplied tenant identifiers are validated.
- [ ] Tenant isolation is enforced at the service layer.
- [ ] Database isolation controls are tested.
- [ ] Cross-tenant object access is tested.
- [ ] Search and filtering respect tenant boundaries.
- [ ] File storage respects tenant boundaries.
- [ ] Background jobs preserve tenant context.
- [ ] Webhooks cannot access another tenant's resources.
- [ ] Tenant-specific administrative operations are protected.
- [ ] Cross-tenant data leakage is tested.
- [ ] Tenant deletion and export workflows are reviewed.

# 22. AI and LLM API Security

For APIs that integrate large language models, agents, retrieval systems or external AI services:

- [ ] AI provider credentials remain server-side.
- [ ] API requests do not expose privileged model credentials.
- [ ] User input is treated as untrusted.
- [ ] Prompt injection risks are assessed.
- [ ] Retrieved documents are treated as untrusted input.
- [ ] Retrieval access controls enforce user permissions.
- [ ] Cross-tenant retrieval is tested.
- [ ] Sensitive information is minimized before external model processing.
- [ ] AI output is not blindly executed.
- [ ] Tool calls require appropriate authorization.
- [ ] High-impact actions require suitable approval.
- [ ] Tool permissions follow least privilege.
- [ ] Model usage is rate-limited.
- [ ] Token and request costs are monitored.
- [ ] Abuse and automated misuse are monitored.
- [ ] Provider retention and data handling are reviewed.
- [ ] AI-generated queries are validated before execution.
- [ ] Model output cannot bypass backend authorization.

# 23. Infrastructure and Deployment

- [ ] API production configuration is reviewed.
- [ ] Debug mode is disabled.
- [ ] Default credentials are removed.
- [ ] Unnecessary endpoints are disabled.
- [ ] Firewall rules are reviewed.
- [ ] API gateway policies are reviewed.
- [ ] Database access is restricted.
- [ ] Secrets are supplied securely.
- [ ] CI/CD permissions follow least privilege.
- [ ] Deployment credentials are protected.
- [ ] Production and development environments are separated.
- [ ] Infrastructure configuration is reviewed.
- [ ] Container images are scanned where applicable.
- [ ] Dependencies are checked before release.
- [ ] Backup and recovery procedures are documented.
- [ ] Rollback procedures are tested.

# 24. Security Testing

- [ ] API endpoint inventory is complete.
- [ ] Authentication testing is performed.
- [ ] Authorization testing is performed.
- [ ] BOLA/IDOR testing is performed.
- [ ] BFLA testing is performed.
- [ ] Property-level authorization testing is performed.
- [ ] Injection testing is performed.
- [ ] Rate limiting is tested.
- [ ] Input validation is tested.
- [ ] CORS behavior is tested.
- [ ] CSRF is tested where applicable.
- [ ] SSRF is tested where applicable.
- [ ] File upload security is tested.
- [ ] Webhook signature validation is tested.
- [ ] GraphQL-specific controls are tested where applicable.
- [ ] Sensitive response exposure is tested.
- [ ] Multi-tenant isolation is tested.
- [ ] Dependency vulnerabilities are assessed.
- [ ] API fuzzing is considered.
- [ ] Security findings are documented.
- [ ] Fixes are retested.

# 25. Production Readiness

- [ ] Production endpoints are verified.
- [ ] API authentication is enforced.
- [ ] Authorization policies are tested.
- [ ] API secrets are protected.
- [ ] Rate limiting is operational.
- [ ] TLS is enforced.
- [ ] Error handling is reviewed.
- [ ] Logging and monitoring are operational.
- [ ] Sensitive response data is minimized.
- [ ] Deprecated endpoints are addressed.
- [ ] Security documentation is available.
- [ ] Incident response procedures are documented.
- [ ] Backup and recovery procedures are reviewed.
- [ ] Known security risks are documented.
- [ ] Critical and high-severity findings are resolved or formally handled before release.

---

# 26. Security Findings Register

| ID | Finding | Severity | Endpoint / Component | Status | Owner |
|---|---|---|---|---|---|
| API-001 | | | | Open | |
| API-002 | | | | Open | |
| API-003 | | | | Open | |

### Finding Status

- Open
- In progress
- Fixed
- Retest required
- Accepted risk
- Not applicable

For each finding, document:

**Finding ID:**\
**Description:**\
**Affected endpoint:**\
**HTTP method:**\
**Affected role or user type:**\
**Evidence:**\
**Reproduction steps:**\
**Impact:**\
**Severity:**\
**Recommended remediation:**\
**Assigned owner:**\
**Retest date:**\
**Retest result:**\

---

# 27. Final Security Review

**Total items reviewed:**\

**Verified controls:**\

**Items not applicable:**\

**Items not tested:**\

**Open findings:**\

**Critical findings:**\

**High findings:**\

**Medium findings:**\

**Low findings:**\

**Overall review result:** Pass / Conditional / Fail

**Reviewer notes:**

**Approved by:**

**Approval date:**

---

## Related ShipSafe Documentation

- [Authentication](../security/authentication/README.md)
- [Authorization](../security/authorization/README.md)
- [API Security](../security/api-security/README.md)
- [Database Security](../security/database-security/README.md)
- [Secrets Management](../security/secrets-management/README.md)
- [Web Security](../security/web-security/README.md)
- [Threat Modeling](../security/threat-modeling/README.md)
- [Security Testing](../security/security-testing/README.md)
- [Logging and Monitoring](../security/logging-monitoring/README.md)
- [Cryptography](../security/cryptography/README.md)

## Important Disclaimer

This checklist is an educational and practical security review aid. Completing it does not guarantee that an API is secure, free of vulnerabilities or legally compliant.

Automated scanners cannot identify every security issue. API security requires server-side authorization, architecture review, threat modeling, manual testing and continuous assessment.

A secure frontend does not make an insecure API safe. Every protected operation must enforce its own authentication, authorization and validation requirements.
