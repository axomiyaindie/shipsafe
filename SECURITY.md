
# ShipSafe Security Policy

ShipSafe takes security seriously.

We welcome responsible security research and appreciate the efforts of developers, security researchers and community members who help identify and report vulnerabilities.

This document explains how to report security vulnerabilities affecting the ShipSafe project and how we approach responsible disclosure.

**Project:** ShipSafe\
**Repository:** https://github.com/rakibuli10/shipsafe

## 1. Reporting a Security Vulnerability

If you discover a potential security vulnerability affecting ShipSafe, please report it responsibly.

### Preferred Reporting Method

Use GitHub's private vulnerability reporting feature when it is enabled for this repository:

https://github.com/rakibuli10/shipsafe/security/advisories/new

Private reporting helps prevent sensitive vulnerability details from being exposed publicly before they can be reviewed.

If private vulnerability reporting is unavailable, contact the repository maintainers through an appropriate private GitHub communication channel.

**Do not publish sensitive vulnerability details in a public GitHub issue.**

### What to Include

A useful vulnerability report should contain:

- A clear vulnerability title.
- A description of the security issue.
- The affected file, component or functionality.
- The affected version or commit, if known.
- Steps to reproduce the issue.
- Proof-of-concept material, where safe and appropriate.
- The potential security impact.
- Suggested remediation, if known.
- Relevant logs or screenshots with sensitive information removed.

Provide enough information for maintainers to understand and validate the issue.

Avoid including real credentials, personal information, private keys, production secrets or confidential third-party data.

## 2. What Should Be Reported?

Security reports may include issues affecting the ShipSafe repository, its maintained code, documentation examples or project infrastructure.

Examples include:

### Repository and Source Code

- Exposed credentials or API keys.
- Accidentally committed private keys.
- Insecure scripts maintained by the project.
- Unsafe automation workflows.
- Vulnerable dependencies in maintained software.
- Insecure build or deployment configurations.

### Documentation and Examples

- Security examples that introduce exploitable vulnerabilities.
- Incorrect security guidance that could lead developers to deploy unsafe systems.
- Code examples that expose credentials or sensitive data.
- Dangerous configuration recommendations.
- Insecure instructions that materially increase security risk.

### Project Infrastructure

- Unauthorized access to project-controlled infrastructure.
- Repository permission or access-control weaknesses.
- Compromised project automation.
- Security weaknesses in project-managed services.

Reports should identify the affected component and explain the potential impact.

## 3. Scope and Limitations

ShipSafe is primarily an open-source security knowledge base and toolkit.

Not every technology, external service, package, framework or third-party project mentioned in its documentation is maintained or controlled by ShipSafe.

The following are generally outside the project's direct security scope:

- Vulnerabilities in unrelated third-party products.
- Security issues affecting systems not controlled by ShipSafe.
- Vulnerabilities in external services merely referenced in documentation.
- Issues requiring unauthorized access to third-party infrastructure.
- General feature requests without a security impact.
- Theoretical concerns without a plausible security consequence.

If a report concerns an external product, consider reporting it directly to that product's maintainers through their responsible disclosure process.

## 4. Responsible Research Guidelines

We support good-faith security research.

When investigating a potential vulnerability:

- Test only systems and accounts you own or have explicit authorization to assess.
- Do not access, modify or disclose another person's private information.
- Do not disrupt services or degrade availability.
- Do not perform destructive testing.
- Do not attempt unauthorized privilege escalation against third-party systems.
- Do not use discovered credentials to access unrelated services.
- Do not publish confidential vulnerability details before appropriate coordination.
- Stop testing if you discover unexpected access to sensitive information.

Do not conduct unauthorized penetration testing against project infrastructure or third-party services.

## 5. Vulnerability Assessment and Triage

Submitted reports may be reviewed to determine:

1. Whether the report concerns a ShipSafe-maintained component.
2. Whether the reported issue is reproducible.
3. Whether the issue has a security impact.
4. Which versions or components are affected.
5. The severity and potential consequences.
6. Whether remediation or additional investigation is necessary.

Reports may be classified as:

| Severity | Description |
|---|---|
| Critical | Severe compromise or exposure with substantial security impact |
| High | Significant vulnerability affecting important security boundaries |
| Medium | Meaningful vulnerability with limited or conditional impact |
| Low | Limited impact or defense-in-depth weakness |
| Informational | Security observation without a demonstrated vulnerability |

Severity depends on the actual impact, exploitability and affected environment.

Not every submitted report will necessarily be classified as a vulnerability.

## 6. Remediation and Disclosure

When a vulnerability is confirmed, maintainers may:

- Investigate the affected component.
- Prepare a correction or mitigation.
- Update affected documentation or examples.
- Review related components for similar weaknesses.
- Publish a security advisory when appropriate.
- Coordinate disclosure with the reporter where practical.

The timing of remediation and disclosure depends on severity, complexity, available resources and the potential impact on users.

Please avoid publishing exploit details or unpatched vulnerability information prematurely.

We cannot guarantee a particular response or remediation timeframe.

## 7. Security Advisories

Confirmed vulnerabilities may be documented through GitHub Security Advisories when appropriate.

An advisory may include:

- Vulnerability description.
- Affected components.
- Affected versions.
- Severity assessment.
- Remediation guidance.
- Fixed versions or commits, where applicable.
- Relevant references.

Security advisories should provide useful information without unnecessarily exposing users to additional risk.

## 8. Handling Exposed Secrets

If you discover an exposed credential, API key, token, private key or certificate:

- Do not use the credential.
- Do not attempt to access the associated service.
- Do not reproduce the secret in a public issue.
- Report the exposure privately.
- Provide the affected file or commit reference without unnecessarily copying the secret.

Maintainers should assess the exposure and arrange revocation or rotation through the appropriate service owner.

Removing a secret from the latest version of a repository does not automatically invalidate it or remove it from Git history.

## 9. Security of Third-Party Dependencies

ShipSafe may reference or recommend third-party libraries, frameworks and security tools.

The presence of a dependency or tool in project documentation does not mean ShipSafe maintains or guarantees its security.

For vulnerabilities affecting third-party software:

- Consult the relevant vendor or project security policy.
- Check official security advisories.
- Follow the affected project's disclosure process.
- Avoid publishing sensitive exploit details prematurely.

## 10. Researcher Recognition

We appreciate responsible security researchers and contributors who help improve ShipSafe.

Where appropriate, and with the researcher's permission, maintainers may acknowledge their contribution in a security advisory or related project documentation.

Recognition is not guaranteed and does not imply financial compensation or a formal bug bounty program.

ShipSafe does not currently promise monetary rewards for vulnerability reports.

## 11. Privacy and Confidentiality

Security reports may contain sensitive technical information.

Please share only the information necessary to explain and validate the vulnerability.

Maintainers should handle submitted reports responsibly and avoid unnecessarily disclosing reporter information or sensitive technical evidence.

Do not include third-party personal data or confidential customer information in security reports.

## 12. Security Policy Updates

This policy may be updated as ShipSafe grows and its infrastructure, maintainers and security practices evolve.

Check this document periodically for the latest reporting instructions.

## Final Note

Security is a continuous responsibility.

We encourage developers and researchers to report issues responsibly, share accurate security knowledge and help make the open-source ecosystem safer.

**Build fast. Audit deeply. Ship securely.**
