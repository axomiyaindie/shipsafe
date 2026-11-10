
# Secure Development

Secure development is the practice of designing, writing, reviewing, testing and maintaining software with security built into every stage of the development lifecycle.

It focuses on preventing vulnerabilities before they reach production rather than relying exclusively on security testing after implementation.

Secure development applies to traditional software engineering, AI-assisted coding, automated development agents, web applications, mobile applications, APIs, backend systems and infrastructure.

This guide provides practical principles and workflows for building software securely.

## 1. What Is Secure Development?

Secure development means treating security as a fundamental engineering requirement.

A feature is not complete simply because it works as expected. It should also protect user data, enforce permissions, handle untrusted input and fail safely.

### Core Objectives

- Prevent common application vulnerabilities.
- Protect sensitive information.
- Enforce authentication and authorization.
- Reduce insecure coding patterns.
- Protect application dependencies.
- Secure development environments.
- Review security-sensitive changes.
- Integrate security into CI/CD.
- Reduce the risk of AI-generated vulnerabilities.
- Maintain security throughout the application's lifetime.

### Security by Design

Security by design means considering security requirements before implementation.

For example, before developing a file-sharing feature, define:

- Who can upload files?
- Who can access uploaded files?
- Who owns each file?
- Can files be shared with other users?
- How are file permissions enforced?
- Where are files stored?
- How are malicious uploads handled?
- How are unauthorized access attempts detected?

These decisions should influence the architecture and implementation from the beginning.

## 2. Secure Software Development Lifecycle

The Secure Software Development Lifecycle (SSDLC) integrates security into software planning, development, testing, deployment and maintenance.

### Typical Development Stages

| Stage | Security activities |
|---|---|
| Planning | Identify security requirements |
| Architecture | Threat modeling and trust boundaries |
| Development | Secure coding and validation |
| Review | Code review and security checks |
| Testing | Security and abuse-case testing |
| Deployment | Secure configuration and secret handling |
| Maintenance | Patching, monitoring and incident response |

### Phase 1: Planning

Identify the security requirements of the application and its features.

Questions to ask:

- What data will the application collect?
- Which information is sensitive?
- Who should access each feature?
- What are the application's trust boundaries?
- Which external services will be integrated?
- What regulatory or contractual requirements apply?

### Phase 2: Architecture

Design the system with appropriate security boundaries.

Consider:

- Authentication architecture.
- Authorization model.
- Database access.
- Tenant isolation.
- API exposure.
- Encryption requirements.
- Secrets management.
- Third-party integrations.
- Infrastructure boundaries.

Use threat modeling to identify potential attacks before implementation.

Reference: `../threat-modeling/README.md`

### Phase 3: Development

Apply secure coding practices consistently.

- Validate untrusted input.
- Enforce authorization on the server.
- Use parameterized database queries.
- Protect sensitive information.
- Handle errors safely.
- Avoid hardcoded secrets.
- Use maintained dependencies.
- Follow secure framework guidance.

### Phase 4: Review

Review code changes for functional correctness and security weaknesses.

Security-sensitive changes deserve additional scrutiny.

### Phase 5: Testing

Validate that security requirements work as intended.

Include:

- Unit tests.
- Integration tests.
- Authorization tests.
- Negative tests.
- Dependency scanning.
- Static analysis.
- Dynamic security testing where appropriate.

### Phase 6: Deployment

Protect production configuration, deployment credentials and infrastructure.

### Phase 7: Maintenance

Monitor vulnerabilities, update dependencies, investigate incidents and reassess security after significant changes.

## 3. Secure Coding Principles

Secure coding principles provide a foundation for writing reliable and maintainable software.

### Least Privilege

Every user, service and process should receive only the permissions necessary to perform its task.

Examples:

- Use restricted database accounts.
- Avoid running applications as root.
- Limit cloud IAM permissions.
- Give background workers only the required access.
- Avoid granting ordinary users administrative permissions.

### Defense in Depth

Use multiple independent security controls instead of relying on one mechanism.

For example, protecting an API may involve:

- Authentication.
- Authorization.
- Input validation.
- Rate limiting.
- Secure transport.
- Monitoring.
- Database constraints.

No individual control should be assumed to eliminate every possible attack.

### Secure Defaults

Applications should use safe configurations by default.

Examples:

- Private resources remain private.
- New accounts receive minimal permissions.
- Debug mode is disabled in production.
- Unnecessary services are disabled.
- Secure cookie attributes are configured.
- Sensitive endpoints require appropriate authentication.

### Fail Securely

When a security check fails, the system should deny the protected operation.

For example, if the application cannot establish whether a user owns a document, it should not grant access.

### Minimize Attack Surface

Reduce unnecessary functionality, exposed services, permissions and dependencies.

### Never Trust Client-Side Controls Alone

Frontend validation improves usability but does not provide authoritative security enforcement.

Attackers can modify requests, bypass interface restrictions and interact directly with backend endpoints.

The backend must independently validate security-sensitive operations.

## 4. Input Validation

Input validation ensures that application data meets expected requirements before processing.

All external input should be treated as untrusted.

### Common Sources of Untrusted Input

- Form submissions.
- URL parameters.
- HTTP headers.
- Cookies.
- API request bodies.
- File uploads.
- Mobile application requests.
- Webhook payloads.
- Third-party service responses.
- Imported files.
- Message queues.

### Validation Recommendations

- Define expected data types.
- Enforce length restrictions.
- Validate numeric ranges.
- Validate formats.
- Restrict accepted values where possible.
- Reject malformed input.
- Validate nested objects.
- Enforce file size limits.
- Validate data on the server.
- Apply context-specific output encoding.

### Allowlist Validation

Prefer explicitly defining accepted values instead of trying to enumerate every possible malicious value.

Example:

```javascript
const allowedStatuses = ["pending", "approved", "rejected"];

function isValidStatus(status) {
  return allowedStatuses.includes(status);
}
```

### Validate Business Rules

Validation is not limited to data types.

For example, an application may need to verify that:

- A user is eligible for an operation.
- A requested quantity is available.
- A transaction is within permitted limits.
- A resource belongs to the requesting tenant.
- A state transition is allowed.

Security-sensitive business rules should be enforced on trusted backend components.

## 5. Output Encoding

Output encoding reduces the risk of untrusted content being interpreted as executable code.

The correct encoding depends on the output context.

### Common Contexts

- HTML content.
- HTML attributes.
- JavaScript strings.
- CSS values.
- URL components.
- JSON responses.

### Recommendations

- Use framework-supported escaping.
- Avoid unsafe HTML rendering.
- Avoid building JavaScript using untrusted strings.
- Validate URL schemes.
- Use safe DOM APIs.
- Apply context-appropriate encoding.
- Review third-party rendering libraries.

### Unsafe Example

```javascript
element.innerHTML = userInput;
```

If `userInput` contains malicious HTML or script-capable content, this may introduce cross-site scripting.

### Safer Example

```javascript
element.textContent = userInput;
```

For applications that intentionally support rich HTML, use a maintained HTML sanitization library with an appropriate configuration.

Encoding and sanitization are not interchangeable. Select the correct protection for the context.

## 6. SQL Injection Prevention

SQL injection occurs when untrusted input changes the meaning of a database query.

### Unsafe Example

```javascript
const query =
  "SELECT * FROM users WHERE email = '" + email + "'";
```

Concatenating untrusted input into SQL can allow an attacker to alter the query.

### Safer Example

```javascript
const result = await db.query(
  "SELECT * FROM users WHERE email = $1",
  [email]
);
```

Use parameterized queries supported by the database driver.

### ORM Considerations

ORMs can reduce the need to write SQL manually, but they do not automatically eliminate injection risks.

Review:

- Raw SQL queries.
- Dynamic query construction.
- User-controlled sorting and filtering.
- Unsafe query fragments.
- Stored procedures.
- Database permissions.

Reference: `../database-security/README.md`

## 7. Authentication Security in Code

Authentication code determines how the application verifies user identity.

### Secure Development Practices

- Use established authentication libraries or providers.
- Avoid implementing custom cryptographic protocols.
- Hash passwords using appropriate password-hashing algorithms.
- Protect password reset workflows.
- Use secure session management.
- Validate token signatures and claims.
- Enforce token expiration.
- Support session revocation where appropriate.
- Apply rate limits to sensitive authentication endpoints.
- Avoid revealing unnecessary account information.

### Password Storage

Never store passwords as plaintext or using fast general-purpose hashes alone.

Use a dedicated password-hashing algorithm such as Argon2id, scrypt or appropriately configured bcrypt.

### Session Security

Session identifiers should be protected from unauthorized access.

For browser-based applications, use appropriate cookie settings such as:

- HttpOnly.
- Secure.
- SameSite.

The exact configuration depends on the application's authentication and cross-site request requirements.

Reference: `../authentication/README.md`

## 8. Authorization Security in Code

Authorization verifies whether an authenticated identity can perform a requested action on a particular resource.

### Common Implementation Mistake

Checking only whether a user is authenticated:

```javascript
if (user) {
  return getDocument(documentId);
}
```

This does not establish whether the user owns or can access the requested document.

### Better Approach

```javascript
const document = await getDocument(documentId);

if (!document || document.ownerId !== user.id) {
  throw new Error("Access denied");
}

return document;
```

This is a simplified illustration. Production applications should use consistent authorization policies, safe error handling and appropriate tenant checks.

### Authorization Recommendations

- Enforce permissions on the server.
- Check resource ownership.
- Validate tenant membership.
- Restrict administrative operations.
- Protect sensitive state transitions.
- Recheck authorization for background operations.
- Avoid trusting role values supplied by clients.
- Test access across users and tenants.

Reference: `../authorization/README.md`

## 9. Secure API Development

APIs should enforce security independently of the frontend that calls them.

### Secure API Practices

- Require authentication where appropriate.
- Enforce object-level authorization.
- Validate request bodies and parameters.
- Restrict response fields.
- Apply rate limits.
- Configure CORS deliberately.
- Validate webhook signatures.
- Protect file upload endpoints.
- Avoid exposing internal errors.
- Apply request size limits.
- Restrict expensive operations.

### API Response Security

Return only the information necessary for the operation.

Avoid exposing:

- Password hashes.
- Authentication tokens.
- Internal database identifiers when unnecessary.
- Private user attributes.
- Internal infrastructure details.
- Debug traces.
- Secrets.

Reference: `../api-security/README.md`

## 10. Secure File Handling

File handling can introduce security problems involving malicious content, path traversal, unauthorized access and resource exhaustion.

### Security Recommendations

- Restrict accepted file formats.
- Validate actual file content, not only extensions.
- Enforce upload size limits.
- Generate server-side storage names.
- Prevent path traversal.
- Store uploads outside executable application directories.
- Apply appropriate access controls.
- Scan files when justified by the risk.
- Avoid trusting client-supplied MIME types.
- Protect download endpoints.
- Apply retention and deletion policies.

### Important Consideration

A file extension such as `.jpg` does not prove that the uploaded content is a valid or safe image.

## 11. Secure Error and Exception Handling

Errors are necessary for debugging and application reliability, but poorly handled errors can disclose sensitive information.

### Information That Should Not Be Exposed

- Database credentials.
- API keys.
- Access tokens.
- Passwords.
- Internal stack traces.
- Sensitive SQL queries.
- Private filesystem paths.
- Internal infrastructure details.

### Recommendations

- Return safe, understandable error messages.
- Log appropriate diagnostic details securely.
- Use consistent error handling.
- Avoid leaking implementation details.
- Ensure errors do not bypass authorization.
- Avoid silently ignoring security failures.
- Monitor unexpected application exceptions.

### Example

A public response might be:

```json
{
  "error": "Unable to process the request",
  "requestId": "req_12345"
}
```

The corresponding internal log can contain additional diagnostic context, provided it does not expose unnecessary sensitive information.

## 12. Cryptography and Sensitive Data

Cryptography protects information against unauthorized disclosure and modification.

### Secure Development Practices

- Use maintained cryptographic libraries.
- Avoid custom encryption algorithms.
- Use authenticated encryption where appropriate.
- Protect encryption keys separately from encrypted data.
- Avoid hardcoded cryptographic keys.
- Use secure random number generators.
- Validate certificates correctly.
- Apply encryption requirements based on data sensitivity.

### Common Mistakes

- Using outdated algorithms.
- Reusing nonces where prohibited by the algorithm.
- Hardcoding encryption keys.
- Storing keys alongside encrypted data without appropriate protection.
- Disabling TLS certificate validation.
- Using encryption as a substitute for authorization.

Reference: `../cryptography/README.md`

## 13. Secure Secrets Handling

Secrets include credentials and values that provide access to protected systems.

### Examples

- Database passwords.
- API keys.
- OAuth client secrets.
- Signing keys.
- Cloud credentials.
- Deployment tokens.
- Encryption keys.

### Recommendations

- Keep secrets out of source code.
- Avoid committing `.env` files containing real credentials.
- Use appropriate secret management systems.
- Restrict secret access.
- Rotate exposed credentials.
- Prevent secrets from appearing in logs.
- Use short-lived credentials where possible.
- Separate development and production secrets.

An environment variable can help separate configuration from code, but it is not automatically a secure secret manager.

Reference: `../secrets-management/README.md`

## 14. Dependency Security During Development

Modern applications depend on external packages, libraries, frameworks and tools.

A vulnerable or compromised dependency can introduce security risks even when application code is well written.

### Recommendations

- Use trusted package registries.
- Review new dependencies.
- Maintain lockfiles.
- Scan dependencies.
- Remove unused packages.
- Update vulnerable versions.
- Review package maintainer and provenance information where available.
- Avoid blindly installing AI-recommended packages.
- Monitor transitive dependencies.

### Useful Tools

- npm audit.
- Dependabot.
- Renovate.
- OSV-Scanner.
- Trivy.
- Snyk.

Tool findings require review. A scanner does not guarantee that all vulnerabilities have been identified.

Reference: `../dependencies/README.md`

## 15. Secure Configuration Management

Application configuration controls runtime behavior and security boundaries.

### Common Configuration Risks

- Debug mode enabled in production.
- Weak CORS policies.
- Insecure cookie settings.
- Public database access.
- Excessive application permissions.
- Default credentials.
- Exposed administration endpoints.
- Insecure TLS configuration.

### Recommendations

- Separate configuration from application code.
- Maintain environment-specific settings.
- Validate required configuration at startup.
- Use secure defaults.
- Restrict access to configuration files.
- Review production configuration changes.
- Avoid exposing configuration values through frontend bundles.
- Document required security settings.

## 16. Frontend Security

Frontend applications run in environments controlled by their users.

Frontend code must not be treated as a trusted security boundary.

### Recommendations

- Do not embed privileged server secrets.
- Avoid storing sensitive information unnecessarily.
- Validate data on the backend.
- Avoid unsafe HTML rendering.
- Configure appropriate Content Security Policy.
- Protect authentication sessions.
- Review third-party scripts.
- Avoid exposing private API responses.
- Remove development-only debugging functionality from production builds.

### Important Principle

Hiding a button does not prevent a user from calling the corresponding API endpoint directly.

The backend must enforce the actual permission requirements.

## 17. Backend Security

Backend systems control important application operations and frequently access sensitive data.

### Recommendations

- Authenticate requests appropriately.
- Enforce authorization for every protected operation.
- Validate all untrusted inputs.
- Use secure database access patterns.
- Apply resource limits.
- Restrict internal service communication.
- Protect background workers.
- Secure file processing.
- Avoid running processes with unnecessary privileges.
- Log important security events safely.

### Background Job Security

Background jobs should not automatically be trusted simply because they are executed internally.

Verify:

- Job origin.
- Required permissions.
- Tenant context.
- Input validity.
- Resource ownership.
- Retry behavior.
- Access to sensitive credentials.

## 18. Secure Development for Mobile Applications

Mobile application code operates on devices that developers cannot fully control.

### Recommendations

- Do not embed privileged backend credentials.
- Use platform-supported secure storage.
- Validate API requests on the backend.
- Protect authentication tokens.
- Avoid logging sensitive information.
- Review application permissions.
- Validate deep links.
- Keep SDKs updated.
- Protect production signing keys.
- Remove debug functionality from release builds.

Reference: `../mobile-security/README.md`

## 19. Secure Multi-Tenant Development

Multi-tenant applications must prevent one tenant from accessing another tenant's data.

### Recommendations

- Resolve tenant identity using trusted server-side context.
- Verify tenant membership.
- Apply tenant-aware authorization.
- Scope database queries.
- Protect file storage.
- Isolate cache entries.
- Validate background job tenant context.
- Review administrative access.
- Test cross-tenant access explicitly.

Never treat a client-provided tenant ID as sufficient authorization.

## 20. Code Review for Security

Code review is an important opportunity to identify security weaknesses before changes reach production.

### Security-Sensitive Changes

Give additional attention to changes involving:

- Authentication.
- Authorization.
- Payment processing.
- Database queries.
- File uploads.
- Secrets management.
- Cryptography.
- Tenant isolation.
- Administrative functionality.
- Infrastructure configuration.
- Dependency updates.

### Security Code Review Questions

- Does the change introduce new public endpoints?
- Is authentication required where expected?
- Are permissions enforced server-side?
- Is user input validated?
- Can sensitive information be exposed?
- Are database queries safe?
- Are secrets included in the change?
- Are dependencies trustworthy?
- Are errors handled safely?
- Are security tests included?
- Could an attacker abuse this feature in an unexpected way?

### Review Principles

- Review actual behavior, not only comments.
- Inspect authorization checks.
- Review data flows.
- Examine error paths.
- Check configuration changes.
- Verify tests cover negative scenarios.
- Do not approve changes solely because automated checks pass.

## 21. Security Testing During Development

Security testing should be integrated into normal engineering workflows.

### Unit Testing

Test individual security-related functions.

Examples:

- Input validation.
- Permission checks.
- Token expiration.
- Password reset rules.
- Business logic restrictions.

### Integration Testing

Test how security controls work across components.

Examples:

- API authorization.
- Database access permissions.
- Tenant isolation.
- File access restrictions.
- Authentication provider integration.

### Negative Testing

Test that unauthorized operations fail.

Examples:

- Accessing another user's document.
- Invoking an administrator endpoint as a normal user.
- Submitting malformed input.
- Uploading oversized files.
- Using expired tokens.
- Reusing one-time tokens.

### Automated Security Testing

Possible checks include:

- Static Application Security Testing (SAST).
- Software Composition Analysis (SCA).
- Secret scanning.
- Infrastructure as Code scanning.
- Dynamic Application Security Testing (DAST).

No single test category provides complete security coverage.

Reference: `../security-testing/README.md`

## 22. CI/CD Security

Continuous Integration and Continuous Deployment pipelines can access source code, dependencies, signing keys and deployment credentials.

They must be protected as part of the application's security architecture.

### Common CI/CD Risks

- Exposed deployment tokens.
- Untrusted build scripts.
- Excessive pipeline permissions.
- Unreviewed production deployments.
- Compromised dependencies.
- Insecure build artifacts.
- Unprotected workflow configuration.

### Recommendations

- Protect CI/CD credentials.
- Use least-privilege pipeline permissions.
- Require review for sensitive workflow changes.
- Restrict production deployment access.
- Scan dependencies.
- Run appropriate security checks.
- Protect build artifacts.
- Review third-party pipeline actions.
- Separate development and production credentials.
- Audit deployment activity.

### Example GitHub Actions Security Checks

```yaml
name: Security Checks

on:
  pull_request:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  dependency-audit:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm ci

      - name: Audit dependencies
        run: npm audit --audit-level=high
```

This is an illustrative workflow for a Node.js project with a committed lockfile.

For production use, review action versions, pin third-party actions to verified commit SHAs where appropriate, configure the correct Node.js version and understand how audit findings affect the build.

A successful dependency audit does not establish that the application is secure.

## 23. Secure AI-Assisted Development

AI coding assistants can generate, modify, test and explain application code.

They can also introduce security mistakes when generated output is accepted without review.

### Common AI-Assisted Development Risks

- Missing authorization checks.
- Insecure authentication logic.
- Hardcoded credentials.
- Unsafe database queries.
- Vulnerable dependencies.
- Insecure cryptographic implementations.
- Incomplete input validation.
- Unsafe shell commands.
- Unreviewed configuration changes.
- Exposure of private project information.

### Secure AI Coding Workflow

**Before asking an AI agent to implement a feature:**

- Define security requirements.
- Explain relevant trust boundaries.
- Identify sensitive data.
- Specify authorization requirements.
- Provide approved architectural patterns.
- Avoid sharing unnecessary secrets.

**While the agent is working:**

- Review proposed architecture changes.
- Inspect commands that modify files or systems.
- Restrict access to production credentials.
- Check dependency additions.
- Verify security-sensitive implementation decisions.

**After implementation:**

- Inspect the complete code diff.
- Review authentication and authorization.
- Check database queries.
- Scan dependencies.
- Run automated tests.
- Test unauthorized access scenarios.
- Review generated configuration.
- Require human approval before privileged deployment actions.

### AI Agent Security Principles

- Treat generated code as untrusted until reviewed.
- Treat repository content and external instructions as potentially untrusted.
- Give agents only the permissions they need.
- Keep production credentials outside unnecessary agent contexts.
- Require approval for destructive or privileged actions.
- Never assume that an AI-generated security explanation proves the implementation is safe.

AI-assisted development should follow the same engineering standards as manually written code.

## 24. Secure Development for Vibe Coding

Vibe coding often involves describing a feature in natural language and relying on AI tools to generate much of the implementation.

This can accelerate development, but it may also make it easier to overlook important security decisions.

### Common Vibe-Coding Risks

- Accepting large code changes without reviewing them.
- Building features without a permission model.
- Trusting frontend-only validation.
- Installing packages without checking them.
- Exposing API keys in client-side code.
- Allowing broad database access.
- Using insecure default configurations.
- Deploying without testing negative scenarios.
- Giving autonomous agents excessive permissions.

### Safer Vibe-Coding Workflow

1. Define the feature and its security requirements.
2. Identify sensitive data and access rules.
3. Ask the AI agent to explain its proposed architecture.
4. Review the implementation plan.
5. Implement the feature in small changes.
6. Inspect security-sensitive code.
7. Run tests and dependency checks.
8. Test unauthorized access.
9. Review the final diff.
10. Deploy only after appropriate validation.

### Example Prompt for an AI Coding Agent

```text
Implement this feature using secure development practices.

Before coding:

1. Identify sensitive data and trust boundaries.
2. Explain the authentication requirements.
3. Define the authorization rules.
4. Identify likely abuse cases.
5. List relevant security risks.

During implementation:

1. Validate all untrusted input on the server.
2. Enforce authorization for every protected operation.
3. Use parameterized database queries.
4. Do not expose privileged credentials.
5. Use existing maintained security libraries.
6. Follow the project's established architecture.

After implementation:

1. Review the complete code diff.
2. Identify security-sensitive changes.
3. Add tests for unauthorized access.
4. Check dependency changes.
5. Report unresolved security concerns.

Do not claim that the implementation is secure merely
because it compiles or passes functional tests.
```

This prompt is a development aid, not a substitute for human security review.

## 25. Secure Development Checklist

### Planning and Architecture

- [ ] Security requirements are defined.
- [ ] Sensitive assets are identified.
- [ ] Trust boundaries are documented.
- [ ] Threat modeling is performed for significant features.
- [ ] Authentication architecture is reviewed.
- [ ] Authorization requirements are documented.
- [ ] External integrations are assessed.

### Secure Coding

- [ ] Untrusted input is validated.
- [ ] Output is encoded appropriately.
- [ ] Database queries use safe parameterization.
- [ ] Authentication follows established practices.
- [ ] Authorization is enforced server-side.
- [ ] Errors do not expose sensitive information.
- [ ] Secrets are not hardcoded.
- [ ] File handling is restricted appropriately.
- [ ] Sensitive data is protected.
- [ ] Application privileges follow least privilege.

### Dependencies and Configuration

- [ ] Dependencies are reviewed.
- [ ] Lockfiles are maintained.
- [ ] Vulnerability checks are performed.
- [ ] Production settings use secure defaults.
- [ ] Debug features are disabled in production.
- [ ] Frontend bundles contain no privileged secrets.
- [ ] Configuration changes are reviewed.

### Code Review and Testing

- [ ] Security-sensitive code receives additional review.
- [ ] Negative test cases are included.
- [ ] Authorization tests are included.
- [ ] Dependency checks are performed.
- [ ] Secret scanning is enabled where appropriate.
- [ ] Security findings are tracked.
- [ ] Unresolved risks are documented.

### AI-Assisted Development

- [ ] Generated code is reviewed.
- [ ] AI agents have limited permissions.
- [ ] Secrets are not unnecessarily shared.
- [ ] Generated dependencies are checked.
- [ ] Security-sensitive changes receive human review.
- [ ] Deployment actions are appropriately controlled.

## 26. Common Secure Development Mistakes

| Mistake | Potential consequence |
|---|---|
| Frontend-only authorization | Unauthorized API access |
| SQL string concatenation | SQL injection |
| Hardcoded credentials | Secret exposure |
| Unsafe HTML rendering | Cross-site scripting |
| Excessive permissions | Privilege escalation |
| Detailed public errors | Information disclosure |
| Unreviewed dependencies | Supply chain compromise |
| Missing negative tests | Undetected access control failures |
| Running applications as root | Increased compromise impact |
| Trusting AI-generated code | Unreviewed security vulnerabilities |
| Insecure defaults | Production exposure |
| Missing security reviews | Vulnerabilities reaching production |

## 27. Recommended Development Resources

### OWASP

- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OWASP Software Assurance Maturity Model](https://owaspsamm.org/)

### NIST

- [NIST Secure Software Development Framework (SSDF)](https://csrc.nist.gov/Projects/ssdf)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

### Secure Development Tools

- [Semgrep](https://semgrep.dev/)
- [CodeQL](https://codeql.github.com/)
- [GitHub Dependabot](https://docs.github.com/en/code-security/dependabot)
- [Gitleaks](https://github.com/gitleaks/gitleaks)
- [Trivy](https://trivy.dev/)
- [OSV-Scanner](https://google.github.io/osv-scanner/)

### Development Guidance

- [GitHub Security Documentation](https://docs.github.com/en/code-security)
- [Google Secure Coding Practices](https://security.googleblog.com/)
- [Microsoft Security Development Lifecycle](https://www.microsoft.com/en-us/securityengineering/sdl)

---

Secure development is a continuous engineering responsibility. This guide is educational and does not guarantee that an application is free from vulnerabilities. Security decisions should be validated against the application's actual architecture, threat model and operational requirements.
