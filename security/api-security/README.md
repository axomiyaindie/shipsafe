
# API Security

APIs (Application Programming Interfaces) allow applications, services and systems to communicate and exchange data.

Modern applications depend on APIs for authentication, payments, user management, file uploads, mobile applications, integrations and internal services.

An insecure API can expose sensitive information, enable unauthorized operations, disrupt services or compromise connected systems.

This guide covers API security fundamentals, common vulnerabilities, secure implementation practices and verification procedures.

## 1. API Security Fundamentals

API security protects the confidentiality, integrity and availability of data and services exposed through APIs.

Common API architectures include:

- REST APIs.
- GraphQL APIs.
- SOAP services.
- WebSocket APIs.
- Internal service-to-service APIs.
- Third-party integration APIs.

### Core Security Principles

- Authenticate requests where authentication is required.
- Authorize every protected operation.
- Validate and normalize untrusted input.
- Minimize exposed data.
- Apply rate limits and resource consumption controls.
- Use HTTPS for data in transit.
- Protect credentials, tokens and secrets.
- Monitor suspicious activity.
- Keep API dependencies and infrastructure updated.

Never assume a request is trustworthy simply because it originates from your own frontend or mobile application.

## 2. REST API Security

REST APIs commonly expose resources through HTTP endpoints and methods.

Common HTTP methods include:

| Method | Typical purpose |
|---|---|
| GET | Retrieve resources |
| POST | Create resources or trigger operations |
| PUT | Replace a resource |
| PATCH | Modify a resource |
| DELETE | Delete a resource |

### Security Recommendations

- Enforce authentication and authorization on protected endpoints.
- Validate HTTP methods and request content types.
- Reject unexpected or unsupported methods.
- Validate request parameters and payloads.
- Avoid exposing internal database structures.
- Use appropriate HTTP status codes.
- Apply pagination and response-size limits.
- Avoid returning sensitive information unnecessarily.

HTTP methods must not be treated as authorization controls. Each operation requires an explicit permission check.

## 3. GraphQL Security

GraphQL allows clients to request specific data through a query language.

Its flexibility can introduce security risks when query complexity, permissions or exposed fields are not properly controlled.

### Common Risks

- Excessively expensive nested queries.
- Unauthorized access to object fields.
- Introspection exposing unnecessary schema information.
- Unrestricted query depth.
- Missing resolver-level authorization.
- Excessive data retrieval.
- Denial of service through expensive queries.

### Security Recommendations

- Enforce authorization in resolvers or equivalent trusted service layers.
- Apply query depth and complexity limits.
- Limit request size and execution time.
- Apply rate limiting.
- Restrict access to sensitive fields.
- Review introspection exposure according to the deployment's requirements.
- Prevent expensive nested queries from exhausting resources.

Disabling introspection alone does not make a GraphQL API secure.

## 4. API Authentication

Authentication verifies the identity of a user, client or service making an API request.

Common approaches include:

- Session-based authentication.
- OAuth 2.0 access tokens.
- OpenID Connect.
- JWT-based authentication.
- API keys for identifying and controlling API clients.
- Mutual TLS for suitable service-to-service environments.

### Security Recommendations

- Use HTTPS.
- Validate tokens on the server.
- Check token expiration, issuer and audience where applicable.
- Rotate credentials when required.
- Avoid placing tokens in URLs.
- Store secrets securely.
- Never expose privileged API credentials in frontend code.
- Use short-lived access tokens where appropriate.
- Implement secure revocation and credential recovery procedures.

An API key is not automatically proof of an end user's identity or authorization.

## 5. API Authorization

Authentication alone does not determine what a request is allowed to do.

Authorization must be enforced for individual operations and resources.

### Important Security Controls

- Object-level authorization.
- Function-level authorization.
- Property-level authorization.
- Role and permission validation.
- Resource ownership checks.
- Tenant isolation.

### Common Vulnerabilities

**Broken Object Level Authorization (BOLA)**

A user accesses another user's resource by changing an object identifier.

**Broken Function Level Authorization (BFLA)**

A user invokes a function or endpoint they are not authorized to use.

**Broken Object Property Level Authorization**

An API exposes sensitive object properties or permits unauthorized modification of protected fields.

### Prevention

- Verify permissions on every protected operation.
- Check ownership or explicit sharing permissions.
- Restrict access to administrative endpoints.
- Filter sensitive fields in responses.
- Prevent unauthorized modification of protected properties.
- Test using accounts with different roles and ownership boundaries.

## 6. Input Validation and Injection Attacks

API requests contain untrusted input, including query parameters, headers, JSON bodies, uploaded files and path variables.

### Common Risks

- SQL injection.
- NoSQL injection.
- Command injection.
- Cross-site scripting through stored or reflected API data.
- Path traversal.
- Unsafe deserialization.
- Server-side template injection.

### Prevention

- Validate input against explicit schemas.
- Enforce data types, length limits and allowed values.
- Use parameterized database queries.
- Avoid constructing shell commands from untrusted input.
- Encode output appropriately for its eventual context.
- Reject unexpected fields when appropriate.
- Normalize input carefully.
- Use established validation libraries.

Validation should happen on the server even when the frontend already validates the same data.

## 7. Rate Limiting and Resource Consumption

Unrestricted API usage can lead to brute-force attacks, automated abuse, service degradation and unexpected infrastructure costs.

### Security Recommendations

- Apply rate limits to sensitive endpoints.
- Set request-size limits.
- Restrict expensive operations.
- Configure pagination limits.
- Set execution timeouts.
- Apply upload-size limits.
- Monitor unusual request patterns.
- Consider quotas for API clients and tenants.
- Protect authentication and password recovery endpoints from automated abuse.

Rate limits should account for the operation, authenticated identity and infrastructure requirements.

Avoid relying exclusively on IP-based limits because multiple users can share an IP address.

## 8. CORS and Security Headers

Cross-Origin Resource Sharing (CORS) controls which browser origins can access resources through cross-origin requests.

CORS is a browser security mechanism, not an authentication or authorization system.

### Security Recommendations

- Allow only required origins.
- Avoid wildcard origins when credentialed requests are used.
- Restrict allowed methods and headers.
- Configure credential handling carefully.
- Validate origin behavior in production.
- Avoid reflecting arbitrary Origin headers.
- Review preflight request handling.

### Additional HTTP Security Controls

Depending on the application, consider:

- Strict-Transport-Security.
- X-Content-Type-Options.
- Content-Security-Policy.
- Referrer-Policy.
- Cache-Control for sensitive responses.

Security headers should be selected according to the application architecture and browser behavior.

## 9. Server-Side Request Forgery (SSRF)

SSRF occurs when an application can be manipulated into making unintended network requests.

An API may be vulnerable when it accepts user-controlled URLs for:

- Image processing.
- File imports.
- Webhook destinations.
- Document previews.
- URL previews.
- Remote data fetching.

### Potential Impact

- Access to internal services.
- Exposure of cloud instance metadata.
- Unauthorized network access.
- Access to sensitive internal resources.

### Prevention

- Avoid arbitrary server-side URL fetching where possible.
- Use strict destination allowlists.
- Restrict access to internal and reserved IP ranges.
- Validate DNS resolution and the final destination.
- Handle redirects safely.
- Apply outbound network restrictions.
- Use network-level controls in addition to application validation.

SSRF defenses must account for DNS rebinding, redirects and changes in resolved addresses.

## 10. API Keys, Tokens and Secrets

API credentials can grant access to application data, external services and infrastructure.

### Security Recommendations

- Never commit secrets to Git repositories.
- Keep privileged credentials on trusted backend systems.
- Store secrets using an appropriate secret manager.
- Rotate compromised or exposed credentials immediately.
- Scope credentials to the minimum required permissions.
- Separate development, staging and production credentials.
- Avoid logging access tokens, API keys and authorization headers.
- Monitor credential usage where possible.

If a credential is accidentally committed, removing it from the latest commit is not sufficient. Treat it as compromised and rotate or revoke it.

## 11. Pagination, Filtering and Data Exposure

APIs frequently return collections of records, search results and reports.

Incorrect response handling can expose excessive data or create resource exhaustion.

### Security Recommendations

- Apply maximum page sizes.
- Validate pagination parameters.
- Restrict filtering and sorting to approved fields.
- Enforce authorization on every returned record.
- Avoid exposing internal fields.
- Minimize sensitive response properties.
- Apply tenant boundaries to searches and exports.
- Protect bulk-download endpoints.
- Consider resource limits for expensive filters and queries.

Never assume that filtering data in the frontend protects information returned by the API.

## 12. Webhook Security

Webhooks allow external services to send event notifications to an application's endpoint.

An insecure webhook implementation can accept forged events or repeatedly process malicious requests.

### Security Recommendations

- Verify webhook signatures using the provider's documented method.
- Use HTTPS.
- Validate event structure and expected event types.
- Implement replay protection where supported.
- Handle duplicate event delivery safely.
- Use timestamps or event identifiers where appropriate.
- Avoid trusting sender-supplied identity fields without verification.
- Store webhook secrets securely.
- Log security-relevant events without exposing secrets.

Webhook verification must follow the provider's exact signing specification.

## 13. File Upload and Download Security

APIs that handle files introduce risks beyond ordinary request validation.

### Security Recommendations

- Enforce file-size limits.
- Validate file types using content inspection, not only filename extensions.
- Generate server-side filenames.
- Prevent path traversal.
- Store uploads outside executable application directories.
- Apply malware scanning where appropriate.
- Restrict access to private files.
- Use short-lived authorized download links where appropriate.
- Prevent unauthorized file replacement or deletion.

Do not assume that an uploaded file is safe because its extension appears valid.

## 14. API Error Handling and Logging

Errors and logs can unintentionally expose sensitive implementation details.

### Security Recommendations

- Return consistent error responses.
- Avoid exposing stack traces in production.
- Never return database credentials or internal secrets.
- Avoid revealing sensitive account information.
- Log authentication and authorization failures.
- Monitor unusual traffic and repeated failures.
- Protect logs from unauthorized access.
- Apply appropriate retention and privacy controls.
- Never log passwords, access tokens or secret keys.

Logs should provide enough context for investigation without becoming another source of sensitive data exposure.

## 15. API Security Testing

API security should be verified throughout development and before production deployment.

### Essential Test Scenarios

- Anonymous requests cannot access protected resources.
- Users cannot access another user's records.
- Users cannot invoke restricted administrative operations.
- Sensitive fields cannot be retrieved or modified without permission.
- Malformed input is rejected safely.
- Injection payloads do not execute.
- Rate limits work as intended.
- Oversized requests are rejected.
- CORS policies match intended origins.
- SSRF protections prevent unauthorized network access.
- Webhook signatures are verified.
- File upload restrictions work correctly.
- Error responses do not expose internal details.
- Secrets are not exposed in responses or logs.

### Testing Approaches

- Automated unit and integration tests.
- API contract and schema testing.
- Authorized dynamic security testing.
- Dependency and configuration scanning.
- Manual penetration testing.
- Negative authorization testing.

Test APIs using authorized test accounts and non-production environments wherever practical.

## 16. Common API Security Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| BOLA | Unauthorized access to objects |
| BFLA | Unauthorized execution of functions |
| Property-level authorization failures | Sensitive data exposure or modification |
| Injection | Database or command execution risks |
| Missing rate limits | Automated abuse and resource exhaustion |
| SSRF | Unauthorized server-side network access |
| Weak authentication | Account or service compromise |
| Excessive data exposure | Sensitive information disclosure |
| Insecure webhook handling | Forged or replayed events |
| Unrestricted file uploads | Malware or unauthorized file access |
| Security misconfiguration | Exposure of application functionality or data |

## 17. Remediation Priorities

When API security weaknesses are identified:

1. Restrict access to exposed sensitive endpoints.
2. Fix broken authentication and authorization controls.
3. Prevent unauthorized object and property access.
4. Correct injection and input-validation weaknesses.
5. Protect sensitive credentials and rotate compromised secrets.
6. Add rate limits and resource consumption controls.
7. Fix SSRF, webhook and file-handling weaknesses.
8. Review CORS, HTTP headers and deployment configuration.
9. Add automated regression tests.
10. Monitor and investigate suspicious API activity.

Prioritize remediation according to exploitability, exposure and potential impact.

## 18. References and Further Reading

### OWASP

- [OWASP API Security Top 10](https://owasp.org/API-Security/)
- [OWASP API Security Project](https://owasp.org/www-project-api-security/)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OWASP GraphQL Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html)
- [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

### Standards and Specifications

- [RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
- [RFC 9700: OAuth 2.0 Security Best Current Practice](https://www.rfc-editor.org/rfc/rfc9700)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

---

This guide is educational and does not replace application-specific security reviews, implementation testing or professional security assessments.
