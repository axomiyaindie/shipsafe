
# Pre-Launch Security Checklist

The final security review checklist for applications preparing for staging approval, production deployment or public release.

Designed for web applications, mobile applications, REST and GraphQL APIs, backend services, SaaS platforms and AI-powered applications.

**Project:**\
**Application name:**\
**Application URL:**\
**Release version:**\
**Review owner:**\
**Review date:**\
**Target release date:**\
**Deployment environment:** Staging / Production\
**Technology stack:**\
**Release type:** Initial launch / Major update / Minor update / Security patch\

---

## How to Use This Checklist

Complete this checklist before approving a production release.

Review the actual implementation and available evidence. Do not assume that a security control works simply because it is configured or documented.

Use these statuses:

- [ ] Not reviewed
- [x] Verified and implemented
- [ ] Not applicable (document the reason)
- [ ] Not tested (requires follow-up)

**Important:** Not applicable and not tested are different from verified. Unresolved findings must remain visible in the final release decision.

### Severity Classification

| Severity | Description | Release recommendation |
|---|---|---|
| Critical | Severe compromise, major data exposure, unrestricted account takeover or system compromise | Block release |
| High | Significant weakness affecting sensitive data, authentication, authorization or infrastructure | Resolve before release |
| Medium | Meaningful weakness with limited or conditional impact | Resolve or formally accept the risk |
| Low | Limited security weakness or defense-in-depth improvement | Track and prioritize |
| Informational | Documentation or improvement opportunity | Review and document |

Severity must reflect the actual impact, exploitability and application context.

---

# 1. Release Scope and Architecture

- [ ] Release scope is documented.
- [ ] Application architecture is documented.
- [ ] Frontend and backend components are identified.
- [ ] Public endpoints are inventoried.
- [ ] Internal services are identified.
- [ ] Administrative interfaces are identified.
- [ ] Third-party integrations are inventoried.
- [ ] External dependencies are documented.
- [ ] Sensitive data flows are identified.
- [ ] Trust boundaries are reviewed.
- [ ] Important assets and business-critical functions are identified.
- [ ] Threat modeling has been performed for sensitive features.
- [ ] Security requirements have been reviewed.
- [ ] Known security limitations are documented.
- [ ] Changes since the previous release have been reviewed for security impact.

# 2. Authentication and Account Security

- [ ] Authentication works correctly in production configuration.
- [ ] Default credentials have been removed.
- [ ] Passwords are never stored in plaintext.
- [ ] Password hashing uses a suitable maintained algorithm.
- [ ] Password reset functionality is secure.
- [ ] Email or phone verification works where required.
- [ ] Login attempts are rate-limited.
- [ ] Brute-force protection is configured.
- [ ] Multi-factor authentication is available where appropriate.
- [ ] Session expiration is enforced.
- [ ] Logout invalidates the appropriate session.
- [ ] Token expiration and refresh behavior are verified.
- [ ] Refresh tokens are protected.
- [ ] Account recovery cannot bypass security controls.
- [ ] Sensitive account changes require appropriate verification.
- [ ] Administrative accounts have appropriate protection.
- [ ] Account enumeration risks have been assessed.

# 3. Authorization and Access Control

- [ ] All protected operations enforce server-side authorization.
- [ ] Users can access only resources they are permitted to access.
- [ ] Object-level authorization is tested.
- [ ] Function-level authorization is tested.
- [ ] Property-level authorization is tested.
- [ ] Administrative operations are restricted.
- [ ] Role-based permissions are verified.
- [ ] Privilege escalation risks are tested.
- [ ] IDOR and BOLA vulnerabilities are tested.
- [ ] Multi-tenant isolation is verified where applicable.
- [ ] Users cannot access another tenant's data.
- [ ] Client-side permission checks are not treated as security boundaries.
- [ ] Direct API access cannot bypass protected frontend workflows.
- [ ] Background jobs enforce appropriate authorization.
- [ ] Default access follows least privilege.

# 4. API and Backend Security

- [ ] All protected API endpoints require authentication.
- [ ] Authorization is checked for every sensitive operation.
- [ ] Input validation is enforced server-side.
- [ ] SQL and NoSQL injection risks are tested.
- [ ] API response fields are minimized.
- [ ] Sensitive fields are not unnecessarily exposed.
- [ ] API rate limits are configured.
- [ ] Request body sizes are restricted.
- [ ] Pagination limits are enforced.
- [ ] Bulk operations enforce authorization.
- [ ] Webhook signatures are verified where applicable.
- [ ] Replay risks are addressed where relevant.
- [ ] Deprecated endpoints are disabled or protected.
- [ ] Debug endpoints are disabled or protected.
- [ ] API errors do not expose stack traces.
- [ ] CORS is configured intentionally.
- [ ] CSRF protection is implemented where applicable.
- [ ] SSRF risks have been reviewed.
- [ ] GraphQL query complexity and depth limits are configured where applicable.
- [ ] Sensitive operations are audited.

# 5. Database Security

- [ ] Production database configuration is reviewed.
- [ ] Database credentials are protected.
- [ ] Database access follows least privilege.
- [ ] Database network exposure is restricted.
- [ ] Parameterized queries are used.
- [ ] Database permissions are reviewed.
- [ ] Row-level security is configured and tested where applicable.
- [ ] Multi-tenant isolation is tested.
- [ ] Sensitive information is encrypted where appropriate.
- [ ] Database backups are configured.
- [ ] Backup access is restricted.
- [ ] Database restoration has been tested.
- [ ] Production data is not unnecessarily copied into development environments.
- [ ] Migrations are reviewed and tested.
- [ ] Database error messages do not expose internal details.
- [ ] Database audit logging is considered for sensitive operations.

# 6. Secrets and Credentials

- [ ] No production secrets are hardcoded in source code.
- [ ] No secrets are committed to Git.
- [ ] `.env` files containing secrets are excluded from source control.
- [ ] Previously committed secrets have been rotated where necessary.
- [ ] Frontend bundles contain no privileged credentials.
- [ ] Public environment variable prefixes are understood.
- [ ] Production secrets are managed securely.
- [ ] Secrets are separated by environment.
- [ ] API keys follow least privilege.
- [ ] Database credentials are restricted.
- [ ] Cloud credentials are protected.
- [ ] CI/CD credentials are protected.
- [ ] Secret rotation procedures are documented.
- [ ] Compromised credentials can be revoked.
- [ ] Logs do not expose secrets or tokens.

# 7. Web Application Security

For web applications:

- [ ] HTTPS is enforced.
- [ ] HTTP redirects to HTTPS.
- [ ] TLS certificates are valid.
- [ ] Secure cookie settings are configured.
- [ ] Session management is reviewed.
- [ ] XSS risks are tested.
- [ ] CSRF risks are tested where applicable.
- [ ] Clickjacking protections are configured.
- [ ] Content Security Policy is reviewed.
- [ ] Security headers are verified.
- [ ] CORS is restricted to intended origins.
- [ ] Open redirects are tested.
- [ ] File upload security is reviewed.
- [ ] Sensitive browser storage is minimized.
- [ ] Third-party scripts are reviewed.
- [ ] Production source maps are reviewed for sensitive information.
- [ ] Debug mode is disabled.
- [ ] Error pages do not expose internal details.

# 8. Mobile Application Security

For Android and iOS applications:

- [ ] Release builds are used.
- [ ] Debugging features are disabled.
- [ ] Application signing is verified.
- [ ] Signing credentials are protected.
- [ ] Sensitive local storage is protected.
- [ ] Authentication tokens use appropriate secure storage.
- [ ] Android Keystore or iOS Keychain is used where appropriate.
- [ ] Network communication uses validated TLS.
- [ ] Application permissions are minimized.
- [ ] Sensitive information is not unnecessarily exposed in notifications.
- [ ] Deep links are validated.
- [ ] WebViews are reviewed.
- [ ] Backup behavior is reviewed.
- [ ] Crash reports do not contain sensitive information.
- [ ] Third-party SDKs are reviewed.
- [ ] Embedded credentials are absent.
- [ ] API authorization is enforced server-side.
- [ ] Mobile-specific security testing is completed where appropriate.

# 9. Infrastructure and Cloud Security

- [ ] Production infrastructure is reviewed.
- [ ] Firewall rules are reviewed.
- [ ] Unnecessary ports are closed.
- [ ] Unnecessary services are disabled.
- [ ] Administrative access is restricted.
- [ ] SSH access is secured where applicable.
- [ ] Server permissions follow least privilege.
- [ ] Reverse proxy configuration is reviewed.
- [ ] TLS termination is configured securely.
- [ ] Cloud IAM permissions are reviewed.
- [ ] Public storage permissions are reviewed.
- [ ] Object storage access is restricted.
- [ ] Container images are reviewed where applicable.
- [ ] Container privileges are minimized.
- [ ] Kubernetes permissions are reviewed where applicable.
- [ ] Infrastructure-as-code configurations are reviewed.
- [ ] Development and production environments are separated.
- [ ] Cloud audit logging is enabled where appropriate.
- [ ] Infrastructure recovery procedures are documented.

# 10. Dependencies and Software Supply Chain

- [ ] Direct dependencies are inventoried.
- [ ] Transitive dependencies are reviewed.
- [ ] Lockfiles are committed.
- [ ] Known vulnerable dependencies are identified.
- [ ] Unsupported packages are reviewed.
- [ ] Unused dependencies are removed.
- [ ] Package sources are trusted.
- [ ] Typosquatting risks are considered.
- [ ] Dependency update procedures are documented.
- [ ] Automated vulnerability alerts are enabled where available.
- [ ] Software composition analysis is performed where appropriate.
- [ ] Container dependencies are reviewed where applicable.
- [ ] Build scripts are reviewed.
- [ ] Third-party licenses are reviewed.
- [ ] Software bill of materials is considered where appropriate.

# 11. File Uploads and Storage

- [ ] Upload endpoints require authorization.
- [ ] File size limits are enforced.
- [ ] File types are validated.
- [ ] Uploaded filenames are safely generated.
- [ ] Files are stored securely.
- [ ] Private files are not publicly accessible by default.
- [ ] File download access is authorized.
- [ ] Signed URLs expire appropriately.
- [ ] Archive extraction is protected.
- [ ] Malware scanning is considered where appropriate.
- [ ] Sensitive temporary files are protected.
- [ ] File deletion behavior is tested.
- [ ] Backup copies are protected.

# 12. Privacy and Data Protection

- [ ] Personal data collected by the application is inventoried.
- [ ] Data collection has a documented purpose.
- [ ] Unnecessary data collection is avoided.
- [ ] Privacy notices accurately describe actual data processing.
- [ ] Applicable privacy obligations have been assessed.
- [ ] Sensitive information is identified.
- [ ] Access to personal data is restricted.
- [ ] Data retention periods are defined.
- [ ] Account deletion behavior is documented and tested.
- [ ] Data export and correction workflows are considered where applicable.
- [ ] Third-party data sharing is reviewed.
- [ ] Analytics collection is minimized.
- [ ] Cookie and tracking behavior is reviewed.
- [ ] Cross-border processing is assessed where relevant.
- [ ] Data breach procedures are documented.
- [ ] Backup retention and deletion limitations are documented.

# 13. Logging, Monitoring and Alerting

- [ ] Authentication events are logged appropriately.
- [ ] Authorization failures are monitored.
- [ ] Sensitive administrative operations are audited.
- [ ] Security-relevant application events are recorded.
- [ ] API abuse patterns are monitored.
- [ ] Infrastructure monitoring is operational.
- [ ] Database monitoring is operational where appropriate.
- [ ] Logs do not contain passwords.
- [ ] Logs do not contain access tokens or API secrets.
- [ ] Personal information is minimized or redacted.
- [ ] Log access is restricted.
- [ ] Log retention is defined.
- [ ] Security alerts are configured where appropriate.
- [ ] Alert ownership is assigned.
- [ ] Incident investigation procedures are documented.

# 14. Backup and Disaster Recovery

- [ ] Critical application data is backed up.
- [ ] Database backups are scheduled.
- [ ] Backup access is restricted.
- [ ] Backup encryption is configured where appropriate.
- [ ] Backup retention is defined.
- [ ] Backup integrity is checked.
- [ ] Restoration has been tested.
- [ ] Recovery procedures are documented.
- [ ] Recovery responsibilities are assigned.
- [ ] Recovery time objectives are considered.
- [ ] Recovery point objectives are considered.
- [ ] Backup systems are protected against unauthorized deletion.
- [ ] Disaster recovery dependencies are documented.

# 15. AI and AI-Assisted Development Security

For applications developed using AI coding agents or applications that integrate AI services:

- [ ] AI-generated security-sensitive code is reviewed.
- [ ] AI-generated authentication code is tested.
- [ ] AI-generated authorization code is tested.
- [ ] AI-generated database queries are reviewed.
- [ ] AI coding agents do not receive unnecessary production secrets.
- [ ] AI provider credentials remain server-side.
- [ ] AI service data handling is reviewed.
- [ ] Prompt injection risks are assessed where applicable.
- [ ] Retrieved content is treated as untrusted input.
- [ ] Retrieval systems enforce user permissions.
- [ ] Cross-user and cross-tenant data access is tested.
- [ ] AI outputs are not blindly executed.
- [ ] Agent tools follow least privilege.
- [ ] High-impact actions require appropriate authorization.
- [ ] AI usage and costs are monitored.
- [ ] Human review is applied to sensitive changes.

# 16. Security Testing and Assessment

- [ ] Security test scope is documented.
- [ ] Static application security testing is performed where appropriate.
- [ ] Dependency scanning is performed.
- [ ] Dynamic application security testing is considered.
- [ ] Authentication tests are completed.
- [ ] Authorization tests are completed.
- [ ] IDOR/BOLA tests are completed.
- [ ] Input validation tests are completed.
- [ ] Injection tests are completed.
- [ ] File upload tests are completed.
- [ ] API security tests are completed.
- [ ] Mobile security tests are completed where applicable.
- [ ] Infrastructure security checks are completed.
- [ ] Privacy-related tests are performed where appropriate.
- [ ] Business logic abuse cases are tested.
- [ ] Critical findings are resolved.
- [ ] High findings are resolved or formally blocked from release.
- [ ] Fixes have been retested.
- [ ] Remaining risks are documented.
- [ ] Testing limitations are documented.

# 17. CI/CD and Deployment Security

- [ ] Build pipelines are reviewed.
- [ ] Deployment permissions follow least privilege.
- [ ] CI/CD secrets are protected.
- [ ] Production deployment requires appropriate authorization.
- [ ] Code review requirements are enforced where appropriate.
- [ ] Dependency checks are integrated where practical.
- [ ] Security tests run at appropriate development stages.
- [ ] Build artifacts are protected.
- [ ] Release artifacts are verified.
- [ ] Deployment logs do not expose secrets.
- [ ] Production environment variables are reviewed.
- [ ] Rollback procedures are documented.
- [ ] Database migration procedures are reviewed.
- [ ] Deployment failure behavior is understood.
- [ ] Emergency patch procedures are documented.

# 18. Incident Response Readiness

- [ ] Security incident response procedures are documented.
- [ ] Incident response contacts are identified.
- [ ] Security reporting channels are available.
- [ ] Credential compromise procedures are documented.
- [ ] API key revocation procedures are documented.
- [ ] Database breach response procedures are documented.
- [ ] Account compromise response procedures are documented.
- [ ] Backup restoration procedures are documented.
- [ ] Evidence preservation procedures are considered.
- [ ] Incident communication responsibilities are assigned.
- [ ] Relevant legal and regulatory reporting obligations are assessed.
- [ ] Incident response procedures have been reviewed or exercised.

# 19. Documentation and Operational Readiness

- [ ] Application architecture documentation is current.
- [ ] API documentation is current.
- [ ] Deployment instructions are documented.
- [ ] Environment configuration is documented without exposing secrets.
- [ ] Security contacts are identified.
- [ ] Privacy documentation is available.
- [ ] User-facing security guidance is available where appropriate.
- [ ] Administrative procedures are documented.
- [ ] Known limitations are documented.
- [ ] Security findings are recorded.
- [ ] Accepted risks have named owners.
- [ ] Support and escalation procedures are documented.

---

# 20. Security Findings Register

Record every unresolved security issue identified during the release review.

| ID | Finding | Severity | Affected component | Status | Owner |
|---|---|---|---|---|---|
| PRE-001 | | | | Open | |
| PRE-002 | | | | Open | |
| PRE-003 | | | | Open | |

### Finding Status

- Open
- In progress
- Fixed
- Retest required
- Accepted risk
- Not applicable

For every finding, document:

**Finding ID:**\
**Title:**\
**Description:**\
**Affected component:**\
**Evidence:**\
**Reproduction steps:**\
**Impact:**\
**Severity:**\
**Recommended remediation:**\
**Assigned owner:**\
**Target resolution date:**\
**Retest result:**\
**Risk acceptance rationale, if applicable:**\

---

# 21. Final Release Assessment

Complete this section after reviewing all applicable controls.

**Total checklist items reviewed:**\

**Verified controls:**\

**Not applicable items:**\

**Not tested items:**\

**Open findings:**\

**Critical findings:**\

**High findings:**\

**Medium findings:**\

**Low findings:**\

**Security testing completed:** Yes / No / Partial

**Threat modeling completed:** Yes / No / Not applicable

**Backup restoration tested:** Yes / No / Not applicable

**Incident response reviewed:** Yes / No

### Release Decision

Select one:

- [ ] Approved for production
- [ ] Approved with documented conditions and accepted risks
- [ ] Not approved
- [ ] Further security testing required

**Release decision rationale:**

**Accepted risks and owners:**

**Outstanding remediation tasks:**

**Reviewer name:**

**Reviewer signature / approval:**

**Approval date:**

**Next security review date:**

---

# 22. Related ShipSafe Documentation

## Application Security

- [Web Application Security](../security/web-security/README.md)
- [Mobile Application Security](../security/mobile-security/README.md)
- [API Security](../security/api-security/README.md)
- [Authentication](../security/authentication/README.md)
- [Authorization](../security/authorization/README.md)

## Infrastructure and Operations

- [Infrastructure Security](../security/infrastructure/README.md)
- [Cloud Security](../security/cloud-security/README.md)
- [Database Security](../security/database-security/README.md)
- [Secrets Management](../security/secrets-management/README.md)
- [Dependency Security](../security/dependencies/README.md)

## Security Engineering

- [Threat Modeling](../security/threat-modeling/README.md)
- [Secure Development](../security/secure-development/README.md)
- [Security Testing](../security/security-testing/README.md)
- [Cryptography](../security/cryptography/README.md)

## Privacy and Operations

- [Privacy and Data Protection](../security/privacy-data-protection/README.md)
- [Logging and Monitoring](../security/logging-monitoring/README.md)
- [Incident Response](../security/incident-response/README.md)

---

## Important Disclaimer

This checklist is an educational and practical security review aid. Completing it does not guarantee that an application is secure, free of vulnerabilities or legally compliant.

A production release should be evaluated using the application's architecture, threat model, risk profile and actual security evidence.

Automated scanners cannot identify every vulnerability. Manual verification, code review, security testing and professional assessment may also be necessary.

A release decision must account for unresolved vulnerabilities, untested areas, known limitations and formally accepted risks.

ShipSafe promotes continuous security assessment throughout the software development lifecycle, not just a final review before deployment.
