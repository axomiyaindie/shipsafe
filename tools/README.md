
# ShipSafe Tools

This directory is intended to organize security tooling, automation resources and supporting utilities related to the ShipSafe project.

The goal is to help developers assess application security more effectively through established security tools, repeatable workflows and, potentially, future ShipSafe-developed utilities.

## Purpose

The tools directory aims to:

- Document useful application security tools.
- Explain where and when security tools should be used.
- Support security testing and vulnerability assessment.
- Encourage repeatable security review workflows.
- Explore opportunities for future automation.
- Help developers validate applications before production.

## Current Status

ShipSafe does not currently provide its own implemented security scanner or automated vulnerability detection engine.

This directory currently serves as a foundation for documenting tools and future tooling development.

External tools mentioned in future documentation will remain separate from software developed by ShipSafe.

## Planned Tool Categories

### 1. Static Application Security Testing (SAST)

Tools that analyze source code to identify potentially insecure coding patterns.

Potential capabilities:

- Detecting insecure functions.
- Identifying injection risks.
- Finding hardcoded credentials.
- Detecting insecure configurations.
- Reviewing source-code security issues.

### 2. Dynamic Application Security Testing (DAST)

Tools that test running applications for security weaknesses.

Potential capabilities:

- HTTP security testing.
- Common web vulnerability detection.
- Security header inspection.
- Authentication and session testing.
- API endpoint assessment.

### 3. Software Composition Analysis (SCA)

Tools that inspect third-party dependencies and identify known vulnerabilities.

Potential capabilities:

- Dependency vulnerability detection.
- Outdated package identification.
- License analysis.
- Software Bill of Materials (SBOM) generation.
- Dependency risk monitoring.

### 4. API Security Testing

Tools and utilities for reviewing API security.

Potential capabilities:

- Endpoint discovery.
- Authentication testing.
- Authorization testing.
- Rate-limit verification.
- Request and response inspection.
- API specification analysis.

### 5. Mobile Application Security

Tools for assessing Android and iOS application security.

Potential capabilities:

- Mobile application package inspection.
- Static analysis.
- Runtime analysis.
- Insecure storage detection.
- Network security testing.
- Mobile application configuration review.

### 6. Infrastructure and Cloud Security

Tools for evaluating infrastructure and cloud configurations.

Potential capabilities:

- Cloud configuration assessment.
- Container image scanning.
- Infrastructure-as-code analysis.
- Permission and access reviews.
- Network exposure assessment.
- Configuration compliance checks.

### 7. Secrets Detection

Tools for identifying accidentally exposed credentials and sensitive information.

Potential capabilities:

- Git repository scanning.
- Secret pattern detection.
- CI/CD secret checks.
- Credential exposure monitoring.
- Historical repository scanning.

### 8. Security Automation

Potential future ShipSafe-developed utilities may support:

- Repeatable security checklist workflows.
- Security assessment reporting.
- Configuration validation.
- Security review evidence collection.
- Integration with established security scanners.
- CI/CD security checks.

These are potential development directions, not currently implemented features.

## Tool Selection Principles

Tools documented by ShipSafe should be evaluated against the following criteria:

1. Security effectiveness.
2. Active maintenance.
3. Documentation quality.
4. Community adoption.
5. Transparency of findings.
6. Integration capabilities.
7. Privacy and data handling.
8. Licensing and usage restrictions.
9. False-positive and false-negative behavior.
10. Suitability for the intended environment.

A tool should not be recommended solely because it is popular or produces many findings.

## Important Limitations

Automated security tools cannot guarantee that an application is secure.

They may:

- Miss business logic vulnerabilities.
- Fail to identify complex authorization problems.
- Produce false positives.
- Miss vulnerabilities requiring authenticated access.
- Fail to detect issues outside their configured scope.
- Depend on outdated vulnerability databases.
- Require manual validation.

Security testing should combine automated tools, manual review, threat modeling and application-specific testing.

## Safe Usage Guidelines

Before running any security tool:

- Obtain authorization to test the target.
- Understand the tool's capabilities and limitations.
- Use isolated environments whenever possible.
- Avoid testing production systems without approval.
- Protect credentials and test data.
- Review scan configurations.
- Validate findings before reporting them.
- Avoid destructive testing unless explicitly authorized.
- Keep tools and vulnerability databases updated.

## Future Development Direction

ShipSafe may eventually introduce its own security assessment utilities.

Potential directions include:

- A local application security assessment CLI.
- Project configuration checks.
- Dependency security analysis integrations.
- Security checklist automation.
- Structured vulnerability reporting.
- AI-assisted security review workflows.
- CI/CD integration.

Any future implementation should prioritize transparency, reproducibility, privacy and safe execution.

A future scanner must clearly distinguish:

- Confirmed vulnerabilities.
- Potential vulnerabilities.
- Configuration warnings.
- Informational observations.
- Checks that were not performed.

It must never present an incomplete assessment as proof that an application is secure.

## Contributing

Contributions are welcome.

When documenting or proposing a tool:

1. Explain its purpose.
2. Identify the security domain it supports.
3. Include official documentation links.
4. Describe installation and configuration requirements where relevant.
5. Document limitations.
6. Explain safe usage.
7. Identify licensing considerations.
8. Keep information current.

See [CONTRIBUTING.md](../CONTRIBUTING.md).

## Related Resources

- [Security Knowledge Base](../security/README.md)
- [Security Checklists](../checklists/)
- [Project Documentation](../docs/README.md)
- [Examples](../examples/README.md)
- [Security Policy](../SECURITY.md)

## License

Documentation in this directory is generally covered by [CC BY 4.0](../LICENSE-DOCUMENTATION).

Software and code explicitly identified as MIT-licensed are covered by the [MIT License](../LICENSE).

Third-party tools and resources remain subject to their respective licenses and terms.

---

**Build fast. Audit deeply. Ship securely.**
