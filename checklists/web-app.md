
# Web Application Security Checklist

A practical security checklist for reviewing web applications before development milestones, staging deployment and production release.

**Project:**\
**Application URL:**\
**Reviewer:**\
**Review date:**\
**Environment:** Development / Staging / Production\
**Application version:**\

---

## How to Use This Checklist

Review each item against the actual implementation and available evidence.

Mark each item as:

- [ ] Not reviewed
- [x] Verified and implemented
- [ ] Not applicable (document the reason)

Do not mark an item complete simply because a feature exists. Verify that it works correctly and securely.

### Severity Guidance

| Severity | Meaning | Expected action |
|---|---|---|
| Critical | Vulnerability enables severe compromise, such as unrestricted account takeover or major data exposure | Stop release until resolved |
| High | Significant security weakness affecting sensitive data, access or application integrity | Resolve before production |
| Medium | Meaningful weakness with limited or conditional impact | Resolve or formally accept the risk |
| Low | Limited security weakness or defense-in-depth improvement | Track and prioritize |
| Informational | Documentation or security improvement | Review and document |

Severity must be based on actual exploitability, impact and application context.

---

# 1. Application Architecture and Attack Surface

- [ ] Application architecture has been documented.
- [ ] Frontend and backend responsibilities are clearly separated.
- [ ] Public and private application components are identified.
- [ ] Sensitive application functionality is handled server-side.
- [ ] Administrative interfaces are identified and protected.
- [ ] External integrations and third-party services are inventoried.
- [ ] Public endpoints are documented.
- [ ] Internal endpoints are not unintentionally exposed.
- [ ] Trust boundaries have been identified.
- [ ] Sensitive data flows have been documented.
- [ ] Threat modeling has been performed for important features.
- [ ] Security requirements are included in development planning.

# 2. Authentication and Account Security

- [ ] Authentication is implemented using a trusted, maintained solution.
- [ ] Passwords are never stored in plaintext.
- [ ] Passwords are hashed using Argon2id, bcrypt, scrypt or appropriately configured PBKDF2.
- [ ] Password hashing parameters are appropriate.
- [ ] Password reset tokens are generated securely.
- [ ] Password reset tokens expire appropriately.
- [ ] Password reset tokens cannot be reused after successful use.
- [ ] Email verification is implemented where required.
- [ ] Authentication attempts are rate-limited.
- [ ] Brute-force protection is implemented.
- [ ] Account enumeration risks have been reviewed.
- [ ] Multi-factor authentication is available where appropriate.
- [ ] Sensitive account changes require appropriate verification.
- [ ] Session invalidation works after logout.
- [ ] Password changes invalidate or appropriately review existing sessions.
- [ ] Account recovery does not bypass security controls.
- [ ] Authentication errors do not reveal unnecessary sensitive information.

# 3. Session Management

- [ ] Session identifiers are generated securely.
- [ ] Session identifiers are not predictable.
- [ ] Session identifiers are not exposed in URLs.
- [ ] Session cookies use `Secure` where HTTPS is required.
- [ ] Session cookies use `HttpOnly` where appropriate.
- [ ] Appropriate `SameSite` settings are configured.
- [ ] Session expiration is implemented.
- [ ] Idle session timeout is considered.
- [ ] Sensitive actions require appropriate reauthentication.
- [ ] Session identifiers are rotated after authentication or privilege changes where appropriate.
- [ ] Logout invalidates server-side sessions where applicable.
- [ ] Session fixation risks have been addressed.
- [ ] Concurrent session behavior is understood.
- [ ] Authentication tokens are not unnecessarily stored in browser-accessible storage.
- [ ] Refresh tokens are protected and rotated where appropriate.

# 4. Authorization and Access Control

- [ ] Authorization is enforced on the server.
- [ ] Every sensitive endpoint checks the caller's permissions.
- [ ] Users can access only resources they are authorized to access.
- [ ] Object-level authorization is implemented.
- [ ] Function-level authorization is implemented.
- [ ] Administrative operations require appropriate privileges.
- [ ] Role-based permissions are tested.
- [ ] Tenant isolation is enforced where applicable.
- [ ] Users cannot access another tenant's information.
- [ ] Privilege escalation risks have been tested.
- [ ] Resource identifiers are not treated as authorization controls.
- [ ] Direct object reference attacks have been tested.
- [ ] Ownership is verified before sensitive updates or deletion.
- [ ] Authorization is checked on background and asynchronous operations.
- [ ] Permission changes take effect appropriately.
- [ ] Default permissions follow least privilege.

# 5. Input Validation and Output Encoding

- [ ] All untrusted input is validated server-side.
- [ ] Client-side validation is not treated as a security boundary.
- [ ] Input types and formats are validated.
- [ ] Maximum input lengths are enforced.
- [ ] Unexpected fields are rejected or safely handled.
- [ ] HTML output is encoded appropriately.
- [ ] JavaScript output contexts are handled safely.
- [ ] URL output is validated and encoded appropriately.
- [ ] SQL injection risks have been tested.
- [ ] NoSQL injection risks have been tested where applicable.
- [ ] Command injection risks have been reviewed.
- [ ] Path traversal risks have been tested.
- [ ] Template injection risks have been reviewed.
- [ ] Regular expressions cannot cause avoidable denial of service.
- [ ] Untrusted HTML is sanitized using a maintained sanitizer when HTML input is required.

# 6. Cross-Site Scripting (XSS)

- [ ] Reflected XSS has been tested.
- [ ] Stored XSS has been tested.
- [ ] DOM-based XSS has been tested.
- [ ] Framework escaping behavior is understood.
- [ ] Unsafe HTML rendering is avoided or properly sanitized.
- [ ] Untrusted data is not inserted into executable JavaScript.
- [ ] URL-based DOM manipulation is reviewed.
- [ ] Third-party scripts are reviewed.
- [ ] Content Security Policy is configured where appropriate.
- [ ] Security testing covers user-generated content.
- [ ] Rich-text editors and Markdown renderers are reviewed.

# 7. Cross-Site Request Forgery (CSRF)

- [ ] CSRF risk has been assessed for cookie-authenticated operations.
- [ ] Appropriate CSRF protection is implemented.
- [ ] State-changing operations do not rely on GET requests.
- [ ] SameSite cookie settings are appropriate.
- [ ] Origin or Referer validation is used where appropriate.
- [ ] Sensitive state-changing actions are tested.
- [ ] Authentication and account recovery flows are reviewed for CSRF risks.

# 8. HTTP Security Headers

- [ ] HTTPS is enforced.
- [ ] HTTP requests are redirected to HTTPS.
- [ ] HSTS is configured where appropriate.
- [ ] Content Security Policy is configured and tested.
- [ ] `X-Content-Type-Options: nosniff` is configured.
- [ ] Clickjacking protections are configured using CSP `frame-ancestors` or suitable alternatives.
- [ ] Referrer Policy is configured.
- [ ] Permissions Policy is reviewed.
- [ ] Sensitive responses use suitable cache-control directives.
- [ ] Server information disclosure is minimized.
- [ ] Headers do not conflict with application requirements.

# 9. Cross-Origin Resource Sharing (CORS)

- [ ] CORS is configured intentionally.
- [ ] Trusted origins are explicitly identified.
- [ ] Unnecessary wildcard origins are avoided.
- [ ] Credentialed requests use specific trusted origins.
- [ ] Allowed methods are restricted.
- [ ] Allowed request headers are restricted.
- [ ] Preflight behavior is tested.
- [ ] Origin validation does not rely on unsafe substring matching.
- [ ] Development origins are not unnecessarily allowed in production.

**Important:** CORS is a browser-enforced cross-origin policy, not an authentication or authorization mechanism. It does not prevent direct API requests from other clients.

# 10. API and Backend Security

- [ ] All private API endpoints require authentication.
- [ ] Authorization is checked independently for each protected resource.
- [ ] API input is validated.
- [ ] API response fields are minimized.
- [ ] Sensitive fields are not returned unnecessarily.
- [ ] Rate limits are configured.
- [ ] Request body sizes are limited.
- [ ] Pagination limits are enforced.
- [ ] Bulk export functionality is restricted.
- [ ] HTTP methods are restricted appropriately.
- [ ] API errors do not expose stack traces.
- [ ] API versioning is controlled.
- [ ] Deprecated endpoints are removed or protected.
- [ ] Webhook signatures are verified.
- [ ] Replay protection is considered for sensitive signed requests.
- [ ] API documentation does not expose private endpoints or credentials.
- [ ] Debug and test endpoints are disabled or protected in production.

# 11. Database Security

- [ ] Database credentials are not exposed in frontend code.
- [ ] Database access is restricted to trusted services.
- [ ] Parameterized queries are used.
- [ ] Database accounts follow least privilege.
- [ ] Administrative database access is restricted.
- [ ] Database network exposure is minimized.
- [ ] Sensitive information is encrypted where appropriate.
- [ ] Backups are protected.
- [ ] Database migrations are reviewed.
- [ ] Database error messages do not expose sensitive information.
- [ ] Tenant isolation is enforced where applicable.
- [ ] Row-level security is configured and tested where applicable.
- [ ] Production data is not unnecessarily copied into development environments.
- [ ] Database audit logging is considered for sensitive operations.

# 12. Secrets and Environment Variables

- [ ] Secrets are not hardcoded in source code.
- [ ] Secrets are not committed to Git.
- [ ] `.env` files containing secrets are excluded from source control.
- [ ] Previously committed secrets have been removed and rotated.
- [ ] Frontend environment variables contain no privileged secrets.
- [ ] Public environment variable prefixes are understood.
- [ ] Production secrets are managed through appropriate secret management.
- [ ] Credentials are separated by environment.
- [ ] Secret access follows least privilege.
- [ ] API keys are scoped where possible.
- [ ] Secret rotation procedures are documented.
- [ ] Secrets are not printed in logs.
- [ ] CI/CD credentials are protected.

# 13. File Uploads and Downloads

- [ ] File uploads require appropriate authorization.
- [ ] File size limits are enforced.
- [ ] File types are validated using content inspection where appropriate.
- [ ] File extensions are not blindly trusted.
- [ ] Uploaded filenames are safely generated.
- [ ] Files are stored outside publicly accessible directories where appropriate.
- [ ] Private uploads are not publicly accessible by default.
- [ ] Download access is authorized.
- [ ] Signed download URLs expire appropriately.
- [ ] Malware scanning is considered for relevant file types.
- [ ] Archive extraction is protected against path traversal and resource exhaustion.
- [ ] Image processing libraries are maintained.
- [ ] Uploaded content is not executed as application code.
- [ ] Sensitive file metadata is reviewed.
- [ ] File deletion behavior is tested.

# 14. Server-Side Request Forgery (SSRF)

- [ ] Server-side URL fetching functionality has been reviewed.
- [ ] User-controlled URLs are validated.
- [ ] Internal IP ranges are blocked where not required.
- [ ] Cloud metadata endpoints are protected.
- [ ] Redirect behavior is restricted and revalidated.
- [ ] DNS resolution and rebinding risks are considered.
- [ ] Outbound network access is restricted where practical.
- [ ] URL parsing uses trusted libraries.
- [ ] Untrusted webhook destinations are reviewed.

# 15. Business Logic Security

- [ ] Sensitive workflows are reviewed for abuse.
- [ ] Payment amounts are verified server-side.
- [ ] Discounts and pricing are calculated server-side.
- [ ] Users cannot manipulate their own authorization level.
- [ ] Subscription and entitlement checks are enforced server-side.
- [ ] Important operations cannot be repeated improperly.
- [ ] Race conditions are considered.
- [ ] Transaction integrity is enforced.
- [ ] Account ownership is verified before sensitive changes.
- [ ] Workflow steps cannot be bypassed.
- [ ] Abuse of invitation, referral and reward systems is reviewed.
- [ ] Sensitive administrative operations are audited.

# 16. Frontend and Browser Security

- [ ] Production source maps are reviewed for sensitive information.
- [ ] Frontend bundles contain no privileged secrets.
- [ ] Third-party JavaScript is inventoried.
- [ ] External scripts use integrity protection where appropriate.
- [ ] Dependencies are reviewed.
- [ ] Browser storage does not unnecessarily contain sensitive information.
- [ ] Sensitive pages use appropriate cache controls.
- [ ] Open redirect vulnerabilities are tested.
- [ ] Dangerous URL schemes are rejected where appropriate.
- [ ] DOM manipulation is reviewed.
- [ ] Client-side permissions are not trusted by the backend.
- [ ] Error messages do not expose internal implementation details.

# 17. Dependencies and Supply Chain

- [ ] Direct dependencies are inventoried.
- [ ] Transitive dependencies are reviewed.
- [ ] Lockfiles are committed.
- [ ] Vulnerable dependencies are identified.
- [ ] Unsupported packages are replaced or isolated.
- [ ] Package names are checked for typosquatting risks.
- [ ] Dependency updates are reviewed.
- [ ] Automated dependency alerts are enabled where available.
- [ ] Software composition analysis is performed where appropriate.
- [ ] Build scripts are reviewed.
- [ ] Third-party packages are obtained from trusted sources.
- [ ] Dependency licenses are reviewed.
- [ ] Unused dependencies are removed.

# 18. Infrastructure and Deployment

- [ ] Production configuration is reviewed.
- [ ] Debug mode is disabled.
- [ ] Development endpoints are not publicly exposed.
- [ ] Server permissions follow least privilege.
- [ ] Unnecessary ports and services are disabled.
- [ ] Firewall rules are reviewed.
- [ ] Reverse proxy configuration is secure.
- [ ] TLS certificates are valid.
- [ ] Cloud storage permissions are reviewed.
- [ ] Database access is restricted.
- [ ] Administrative access is protected.
- [ ] Infrastructure configuration is version-controlled where appropriate.
- [ ] Production and development environments are separated.
- [ ] Deployment permissions are restricted.
- [ ] Backups and recovery procedures are tested.

# 19. Logging, Monitoring and Error Handling

- [ ] Authentication events are logged appropriately.
- [ ] Privilege changes are logged.
- [ ] Sensitive administrative operations are logged.
- [ ] Security failures are monitored.
- [ ] Logs do not contain passwords.
- [ ] Logs do not contain session tokens or API secrets.
- [ ] Sensitive personal information is minimized or redacted.
- [ ] Log access is restricted.
- [ ] Log retention is defined.
- [ ] Security alerts are configured where appropriate.
- [ ] Error responses do not expose stack traces.
- [ ] Internal errors are recorded securely.
- [ ] Incident investigation procedures are documented.

# 20. Privacy and Data Protection

- [ ] Personal information collected by the application is inventoried.
- [ ] Each data category has a documented purpose.
- [ ] Unnecessary data collection is avoided.
- [ ] Privacy notices accurately describe actual processing.
- [ ] Applicable legal requirements have been assessed.
- [ ] Sensitive personal information is identified.
- [ ] Access to personal information is restricted.
- [ ] Data retention periods are defined.
- [ ] Account deletion behavior is documented and tested.
- [ ] Third-party data sharing is reviewed.
- [ ] Analytics collection is minimized.
- [ ] Cookie and tracking behavior is reviewed.
- [ ] Data export and correction workflows are considered where applicable.
- [ ] Backup retention and deletion limitations are documented.

# 21. AI and AI-Assisted Development

For applications built with AI coding agents or applications that integrate AI services:

- [ ] AI-generated security-sensitive code is reviewed.
- [ ] AI coding agents do not receive unnecessary production secrets.
- [ ] AI integrations use appropriately scoped credentials.
- [ ] Sensitive prompts are minimized.
- [ ] Model provider data handling is reviewed.
- [ ] Conversation history is protected.
- [ ] AI-generated SQL and queries are reviewed.
- [ ] Tool and agent permissions follow least privilege.
- [ ] Retrieval systems enforce authorization.
- [ ] Cross-user and cross-tenant retrieval is tested.
- [ ] AI output is not treated as trusted executable input.
- [ ] Prompt injection risks are assessed.
- [ ] Sensitive AI actions require suitable authorization and approval.

# 22. Security Testing

- [ ] Static application security testing is performed where appropriate.
- [ ] Dependency vulnerability scanning is performed.
- [ ] Dynamic application security testing is considered.
- [ ] Authentication flows are tested.
- [ ] Authorization boundaries are tested.
- [ ] IDOR and broken access control tests are performed.
- [ ] XSS testing is performed.
- [ ] CSRF testing is performed where applicable.
- [ ] SQL injection testing is performed.
- [ ] File upload functionality is tested.
- [ ] API rate limiting is tested.
- [ ] Security headers are inspected.
- [ ] TLS configuration is inspected.
- [ ] Business logic abuse cases are tested.
- [ ] Security findings are documented.
- [ ] Critical and high-severity findings are resolved or formally handled before release.
- [ ] Fixes are retested.

# 23. Production Readiness

- [ ] Production environment variables are reviewed.
- [ ] Production secrets are configured securely.
- [ ] Debug mode is disabled.
- [ ] Default credentials have been removed.
- [ ] Administrative accounts are protected.
- [ ] HTTPS is enforced.
- [ ] Security headers are verified.
- [ ] Database access is restricted.
- [ ] Backups are configured.
- [ ] Recovery procedures have been tested.
- [ ] Monitoring is operational.
- [ ] Incident response contacts are identified.
- [ ] Security contact information is available.
- [ ] Privacy documentation is available.
- [ ] Third-party integrations are approved.
- [ ] Known security risks are documented.
- [ ] Release approval includes a security review.

---

# 24. Security Findings Register

Record vulnerabilities and unresolved risks discovered during the review.

| ID | Finding | Severity | Affected component | Status | Owner |
|---|---|---|---|---|---|
| WEB-001 | | | | Open | |
| WEB-002 | | | | Open | |
| WEB-003 | | | | Open | |

### Finding Status

- Open
- In progress
- Fixed
- Retest required
- Accepted risk
- Not applicable

Every accepted risk should include a documented rationale, owner and review date.

# 25. Final Security Review

**Total items reviewed:**\

**Verified controls:**\

**Items not applicable:**\

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

## Important Disclaimer

This checklist is an educational and practical review aid. Completing it does not guarantee that an application is secure, free of vulnerabilities or legally compliant.

Automated scanners cannot identify every security issue. Some controls require manual verification, architecture review, threat modeling and testing by qualified security professionals.

ShipSafe encourages continuous security testing throughout development rather than relying on a single review before production.
