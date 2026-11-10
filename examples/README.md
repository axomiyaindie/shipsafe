
# ShipSafe Examples

This directory is intended to contain practical, security-focused implementation examples for developers building web applications, mobile applications, APIs, backend services and cloud infrastructure.

ShipSafe aims to help developers understand not only what security practices to follow, but also how to implement them correctly.

## Purpose

The examples directory is designed to:

- Demonstrate secure implementation patterns.
- Show common insecure coding patterns and their safer alternatives.
- Help developers understand application security in practice.
- Support traditional and AI-assisted development workflows.
- Provide reusable educational examples.
- Connect security concepts with real implementation decisions.

## Current Status

This directory is currently a structure for future examples.

No implementation examples are published here yet.

Examples will be added after technical review, testing and documentation.

## Planned Example Categories

### 1. Authentication

Potential examples:

- Secure password hashing.
- Login and registration flows.
- Multi-factor authentication.
- Session management.
- OAuth 2.0 and OpenID Connect integration.
- Secure password recovery.

### 2. Authorization

Potential examples:

- Role-based access control (RBAC).
- Resource ownership validation.
- Preventing IDOR and BOLA vulnerabilities.
- Multi-tenant authorization.
- Server-side permission enforcement.

### 3. API Security

Potential examples:

- Secure REST API endpoints.
- Request validation.
- Rate limiting.
- Secure API key handling.
- Webhook signature verification.
- GraphQL authorization.

### 4. Database Security

Potential examples:

- Parameterized database queries.
- Secure ORM usage.
- Database permission separation.
- Row-level security.
- Multi-tenant data isolation.
- Secure database migrations.

### 5. Web Application Security

Potential examples:

- Input validation and output encoding.
- XSS prevention.
- CSRF protection.
- Secure cookies.
- Content Security Policy.
- Secure file uploads.

### 6. Mobile Application Security

Potential examples:

- Secure token storage.
- Platform-backed key storage.
- Certificate validation.
- Secure deep links.
- Sensitive data protection.
- Secure API communication.

### 7. Secrets Management

Potential examples:

- Environment configuration.
- Backend-only secret handling.
- Secret rotation.
- CI/CD secret management.
- Preventing accidental credential exposure.

### 8. Cloud and Infrastructure Security

Potential examples:

- Secure Docker configurations.
- Least-privilege service identities.
- Secure deployment configuration.
- Infrastructure permission reviews.
- Network exposure reduction.

### 9. Security Testing

Potential examples:

- Automated security tests.
- Authorization test cases.
- API security testing.
- Dependency vulnerability checks.
- Secure CI/CD validation.

## Example Documentation Format

Future examples should follow a consistent structure.

Recommended format:

```text
example-name/
├── README.md
├── insecure-example/
└── secure-example/
```

Not every example needs both implementation directories. The structure should match the complexity and purpose of the example.

Each example should explain:

1. **Problem:** What security issue is being addressed?
2. **Context:** Where can this issue occur?
3. **Insecure approach:** What common mistake should developers avoid?
4. **Secure implementation:** What is the recommended approach?
5. **Explanation:** Why is the implementation safer?
6. **Testing:** How can developers verify the behavior?
7. **Limitations:** What does the example not protect against?
8. **References:** Which authoritative sources support the guidance?

## Secure Example Standards

Every submitted example should:

- Use supported and maintained technologies.
- Follow current security best practices.
- Avoid hardcoded credentials and secrets.
- Avoid insecure cryptographic implementations.
- Include appropriate validation and error handling.
- Enforce authorization on the server side.
- Avoid exposing sensitive information in logs.
- Include dependency and configuration considerations.
- Be tested where practical.
- Clearly document assumptions and limitations.

## AI-Assisted Development

ShipSafe recognizes that developers increasingly use AI coding agents to generate and modify application code.

Examples should help developers understand how to:

- Review AI-generated authentication logic.
- Identify missing authorization checks.
- Detect exposed secrets.
- Validate generated database queries.
- Review dependencies and package changes.
- Test generated API endpoints.
- Verify security-sensitive changes before deployment.

AI-generated code must never be considered secure merely because it compiles, passes basic tests or appears correct.

All security-sensitive examples require human review.

## Responsible Use

Examples are intended for education, secure development and authorized security testing.

They must not contain:

- Real credentials or private keys.
- Live personal or confidential data.
- Unnecessary destructive operations.
- Unexplained exploit payloads.
- Instructions encouraging unauthorized access.

Security demonstrations should use isolated test environments and clearly identified sample data.

## Contributing Examples

Contributions are welcome.

Before submitting an example:

1. Review the relevant security guide.
2. Follow the documentation standards.
3. Verify technical correctness.
4. Test the implementation where practical.
5. Explain security assumptions and limitations.
6. Include references.
7. Follow the project's contribution guidelines.

See [CONTRIBUTING.md](../CONTRIBUTING.md).

## Related Resources

- [Security Knowledge Base](../security/README.md)
- [Web Application Checklist](../checklists/web-app.md)
- [Mobile Application Checklist](../checklists/mobile-app.md)
- [API Security Checklist](../checklists/api.md)
- [Pre-launch Security Checklist](../checklists/pre-launch.md)
- [Project Documentation](../docs/README.md)

## License

Code examples explicitly identified as MIT-licensed are covered by the [MIT License](../LICENSE).

Explanatory documentation is generally covered by [CC BY 4.0](../LICENSE-DOCUMENTATION), subject to applicable third-party rights and license terms.

---

**Build fast. Audit deeply. Ship securely.**
```

Save it, then run:

```powershell
(Get-Content examples/README.md).Count
```

We'll build `tools/README.md` next, with a clear distinction between external security tools and any future ShipSafe-built scanner.