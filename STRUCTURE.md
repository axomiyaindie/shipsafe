
# ShipSafe Repository Structure

This document explains the architecture, responsibilities, organization, and intended contents of every important directory and file in ShipSafe.

Its purpose is to maintain a consistent, scalable, understandable, and maintainable repository as the project grows.

ShipSafe is organized around three primary objectives:

1. Build a comprehensive application security knowledge base.
2. Provide practical security assessment checklists and implementation examples.
3. Develop a foundation for future security tools and developer integrations.

---

# 1. Repository Overview

The following tree represents the current repository organization.

```text
shipsafe/
│
├── README.md
├── STRUCTURE.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── LICENSE
├── LICENSE-DOCUMENTATION
│
├── docs/
│
├── security/
│   ├── README.md
│   │
│   ├── authentication/
│   │   └── README.md
│   │
│   ├── authorization/
│   │   └── README.md
│   │
│   ├── api-security/
│   │   └── README.md
│   │
│   ├── database-security/
│   │   └── README.md
│   │
│   ├── web-security/
│   │   └── README.md
│   │
│   ├── mobile-security/
│   │   └── README.md
│   │
│   ├── secrets-management/
│   │   └── README.md
│   │
│   ├── dependencies/
│   │   └── README.md
│   │
│   ├── infrastructure/
│   │   └── README.md
│   │
│   ├── threat-modeling/
│   │   └── README.md
│   │
│   ├── secure-development/
│   │   └── README.md
│   │
│   ├── security-testing/
│   │   └── README.md
│   │
│   ├── cloud-security/
│   │   └── README.md
│   │
│   ├── logging-monitoring/
│   │   └── README.md
│   │
│   ├── incident-response/
│   │   └── README.md
│   │
│   ├── privacy-data-protection/
│   │   └── README.md
│   │
│   └── cryptography/
│       └── README.md
│
├── checklists/
│   ├── web-app.md
│   ├── mobile-app.md
│   ├── api.md
│   └── pre-launch.md
│
├── examples/
│
├── tools/
│
└── .github/
    └── workflows/
```

**Note:** Some directories are structural foundations for future contributions. Git does not track empty directories, so they will appear in the repository once files are added.

---

# 2. Root-Level Files

Root-level files provide the project's identity, navigation, policies, architecture and licensing information.

## README.md

**Purpose:** Main entry point to ShipSafe.

Contains:

- Project introduction.
- Mission and objectives.
- Security coverage.
- Repository navigation.
- Intended audience.
- Core principles.
- Contribution information.
- Security reporting information.
- Disclaimer.
- Licensing information.

The README should remain concise and help visitors understand the project quickly.

Detailed architectural explanations belong in STRUCTURE.md.

## STRUCTURE.md

**Purpose:** Complete repository architecture documentation.

Contains:

- Repository overview.
- Directory and file descriptions.
- Responsibilities of each section.
- Content organization standards.
- Documentation guidelines.
- Licensing structure.
- Future architectural expansion.

This document should be updated whenever significant repository structures or responsibilities change.

## CONTRIBUTING.md

**Purpose:** Explain how developers and community members can contribute.

Contains:

- Contribution workflow.
- Repository organization.
- Documentation standards.
- Secure code example requirements.
- AI-assisted contribution guidelines.
- Pull request requirements.
- Responsible security research practices.
- Quality expectations.

## CODE_OF_CONDUCT.md

**Purpose:** Establish community standards for respectful and constructive collaboration.

Defines:

- Expected community behavior.
- Acceptable and unacceptable conduct.
- Technical discussion guidelines.
- Reporting procedures.
- Maintainer responsibilities.
- Community enforcement principles.

## SECURITY.md

**Purpose:** Explain how to responsibly report vulnerabilities affecting ShipSafe itself.

Contains:

- Private vulnerability reporting instructions.
- Reporting requirements.
- Project security scope.
- Responsible research guidelines.
- Vulnerability assessment and severity.
- Remediation and disclosure expectations.
- Security advisory guidance.

This policy concerns ShipSafe-maintained components, not every third-party technology referenced in its documentation.

## LICENSE

**Purpose:** Define the licensing terms for software and code identified as MIT-licensed.

Applies to:

- Software source code.
- Scripts.
- Utilities.
- Code examples explicitly identified as MIT-licensed.

The license permits reuse, modification and redistribution under its stated conditions.

## LICENSE-DOCUMENTATION

**Purpose:** Define the licensing terms for ShipSafe's written educational content.

Uses the Creative Commons Attribution 4.0 International license (CC BY 4.0).

Applies to:

- Written security guides.
- Educational documentation.
- Security checklists.
- Explanatory content.
- Other documentation covered by the license notice.

Users may share and adapt covered material, including commercially, subject to attribution requirements.

Third-party materials remain subject to their own licensing terms.

---

# 3. docs/ — Project Documentation

**Mission:** Maintain general documentation required to understand, navigate, contribute to and use ShipSafe.

This directory contains project-level documentation rather than individual vulnerability guides.

### Intended contents

- Project overview and roadmap.
- Documentation standards.
- Security terminology and glossary.
- Supported technologies and frameworks.
- Architecture decisions.
- Developer onboarding guides.
- Frequently asked questions.
- Project development guidelines.
- Documentation contribution standards.

### Organization

Documentation should be divided into meaningful topics as the project expands.

For example:

```text
docs/
├── README.md
├── standards.md
├── glossary.md
├── roadmap.md
└── development-guide.md
```

These are proposed future files, not claims that they already exist.

---

# 4. security/ — Security Knowledge Base

**Mission:** Provide comprehensive, practical and technically accurate security knowledge for application developers.

This is the central knowledge repository of ShipSafe.

Each security domain contains a dedicated guide explaining relevant vulnerabilities, prevention strategies, secure implementation practices, testing methods and references.

## security/README.md

**Purpose:** Central navigation page for all security domains.

Contains:

- Security domain overview.
- Links to individual guides.
- Knowledge base organization.
- Documentation standards.
- Security coverage and scope.

## 4.1 authentication/

**Mission:** Explain how applications establish and verify user identities.

Topics:

- Password security.
- Multi-factor authentication.
- OAuth 2.0 and OpenID Connect.
- JSON Web Token security.
- Session management.
- Password reset vulnerabilities.
- Account recovery security.
- Brute-force protection.
- Authentication implementation mistakes.

## 4.2 authorization/

**Mission:** Explain how applications control access to resources and operations.

Topics:

- Role-based access control.
- Attribute-based access control.
- Broken access control.
- Insecure direct object references.
- Broken object-level authorization.
- Privilege escalation.
- Multi-tenant isolation.
- Permission validation.
- Function-level authorization.

## 4.3 api-security/

**Mission:** Protect APIs and communication between application components.

Topics:

- API authentication.
- API authorization.
- Rate limiting.
- Input validation.
- Request and response security.
- CORS configuration.
- GraphQL security.
- REST API vulnerabilities.
- WebSocket security.
- API abuse prevention.
- Webhook security.
- File upload security.

## 4.4 database-security/

**Mission:** Protect databases and stored application data.

Topics:

- SQL injection.
- NoSQL injection.
- Database permissions.
- Row-level security.
- Data encryption.
- Secure database connections.
- Backup protection.
- Database exposure prevention.
- Multi-tenant data isolation.
- Secure database migrations.
- Query and access auditing.

## 4.5 web-security/

**Mission:** Explain browser-based application security.

Topics:

- Cross-site scripting.
- Cross-site request forgery.
- Content Security Policy.
- Clickjacking.
- Secure HTTP headers.
- HTTPS configuration.
- File upload security.
- Input validation and output encoding.
- Server-side request forgery.
- Client-side security.
- Browser storage security.
- Cookie and session protection.

## 4.6 mobile-security/

**Mission:** Protect Android and iOS applications and their communication with backend services.

Topics:

- Secure local storage.
- Secure network communication.
- Certificate validation.
- Deep-link security.
- Authentication on mobile devices.
- Sensitive information exposure.
- Android application security.
- iOS application security.
- Mobile reverse engineering risks.
- WebView security.
- Application signing.
- Mobile SDK security.

## 4.7 secrets-management/

**Mission:** Explain the secure handling of credentials and sensitive configuration.

Topics:

- API key protection.
- Environment variables.
- Secret storage.
- Credential rotation.
- Git history secret exposure.
- CI/CD secrets.
- Cloud secret managers.
- Production configuration security.
- Private key protection.
- AI coding agent secret exposure.

## 4.8 dependencies/

**Mission:** Protect applications against vulnerable and malicious third-party software.

Topics:

- Vulnerable dependencies.
- Dependency auditing.
- Software composition analysis.
- Lockfile security.
- Malicious packages.
- Package registry security.
- Software Bill of Materials.
- Dependency update strategies.
- Supply-chain attacks.
- Container dependency security.

## 4.9 infrastructure/

**Mission:** Secure hosting environments, servers and deployment infrastructure.

Topics:

- Docker security.
- Container hardening.
- Server configuration.
- Network security.
- CI/CD security.
- Deployment permissions.
- Storage bucket exposure.
- Infrastructure as Code security.
- Production environment hardening.
- SSH security.
- Reverse proxy configuration.

## 4.10 threat-modeling/

**Mission:** Identify and evaluate potential security threats before and during application development.

Topics:

- Threat modeling methodologies.
- STRIDE threat modeling.
- Attack surface analysis.
- Trust boundaries.
- Data flow diagrams.
- Abuse cases.
- Risk assessment.
- Threat mitigation planning.
- Threat modeling templates.
- AI application threat modeling.

## 4.11 secure-development/

**Mission:** Establish secure software development practices throughout the application lifecycle.

Topics:

- Secure coding standards.
- Input validation.
- Output encoding.
- Error handling.
- Secure architecture patterns.
- AI-assisted development security.
- Code review practices.
- Secure development lifecycle.
- Secure configuration.
- Security requirements.

## 4.12 security-testing/

**Mission:** Help developers systematically test applications for security weaknesses.

Topics:

- Static Application Security Testing (SAST).
- Dynamic Application Security Testing (DAST).
- Interactive Application Security Testing (IAST).
- Penetration testing.
- Fuzz testing.
- Security regression testing.
- Vulnerability verification.
- Security testing automation.
- False-positive assessment.
- Security findings and remediation.

## 4.13 cloud-security/

**Mission:** Protect applications, services, infrastructure and data hosted on cloud platforms.

Topics:

- AWS security.
- Microsoft Azure security.
- Google Cloud security.
- Cloud identity and access management.
- Cloud storage permissions.
- Serverless security.
- Cloud network security.
- Cloud configuration auditing.
- Cloud logging and monitoring.
- Cloud backup and recovery.

## 4.14 logging-monitoring/

**Mission:** Help developers detect suspicious activities and investigate security incidents.

Topics:

- Application security logging.
- Authentication event monitoring.
- Audit trails.
- Suspicious activity detection.
- Log integrity.
- Alert configuration.
- Security monitoring.
- Sensitive data redaction.
- Security event correlation.
- SIEM integration.

## 4.15 incident-response/

**Mission:** Provide practical guidance for identifying, containing, investigating and recovering from security incidents.

Topics:

- Incident response planning.
- Credential compromise.
- Data breach response.
- Account takeover response.
- Malware incidents.
- Incident investigation.
- Recovery procedures.
- Evidence preservation.
- Incident communication.
- Post-incident reviews.

## 4.16 privacy-data-protection/

**Mission:** Protect user information and establish responsible data handling practices.

Topics:

- Personal data protection.
- Data minimization.
- Data retention.
- Data deletion.
- Privacy by design.
- Consent management.
- Sensitive information handling.
- Data encryption.
- Privacy regulations and compliance.
- Data processing and third-party sharing.

## 4.17 cryptography/

**Mission:** Explain the correct use of cryptographic techniques to protect application data and communications.

Topics:

- Encryption and decryption.
- Symmetric cryptography.
- Asymmetric cryptography.
- Password hashing.
- Digital signatures.
- Key management.
- TLS and certificate security.
- Cryptographic implementation mistakes.
- Secure random number generation.
- Cryptographic algorithm selection.

---

# 5. checklists/ — Security Assessment Checklists

**Mission:** Turn security knowledge into actionable verification steps.

Checklists help developers systematically review applications, identify missing controls and document unresolved risks.

## 5.1 web-app.md

**Purpose:** Security review for browser-based applications.

Covers:

- Authentication and sessions.
- Authorization.
- Input validation.
- XSS and CSRF.
- Security headers.
- APIs and backend services.
- Database security.
- File uploads.
- Secrets and dependencies.
- Infrastructure.
- Privacy and monitoring.
- Production readiness.

## 5.2 mobile-app.md

**Purpose:** Security review for Android and iOS applications.

Covers:

- Secure local storage.
- Authentication.
- Network security.
- Platform permissions.
- Deep links.
- WebViews.
- Mobile application signing.
- Sensitive data exposure.
- Third-party SDKs.
- Backend integration.
- Release security.

## 5.3 api.md

**Purpose:** Security review for REST, GraphQL and other API implementations.

Covers:

- API authentication.
- Authorization.
- BOLA and IDOR.
- Input validation.
- Injection vulnerabilities.
- Rate limiting.
- Webhooks.
- File upload and download.
- CORS and CSRF.
- GraphQL security.
- Multi-tenant isolation.
- API testing.

## 5.4 pre-launch.md

**Purpose:** Final security review before production deployment.

Covers:

- Release scope and architecture.
- Authentication and authorization.
- API and backend security.
- Database protection.
- Secrets and credentials.
- Web and mobile security.
- Infrastructure and cloud.
- Dependencies and software supply chain.
- Privacy and data protection.
- Logging and monitoring.
- Backup and disaster recovery.
- AI-assisted development.
- Security testing.
- CI/CD and deployment.
- Incident response.
- Final release decision and sign-off.

### Checklist Standards

Checklists should clearly identify:

- What must be checked.
- Why the check matters.
- How to verify it.
- What a secure result looks like.
- How to remediate failures.
- Any limitations or exceptions.

An unchecked item must not be interpreted as a verified security control.

---

# 6. examples/ — Secure Implementation Examples

**Mission:** Demonstrate how to implement security recommendations correctly.

This directory is intended to contain practical, reusable and educational implementation examples.

### Intended contents

- Secure authentication implementations.
- Authorization examples.
- Secure API patterns.
- Database permission examples.
- Secure configuration examples.
- Framework-specific implementations.
- Vulnerable versus secure code comparisons.
- Secure file upload implementations.
- Cryptographic usage examples.
- Cloud configuration examples.

### Example Organization

```text
examples/
├── README.md
├── authentication/
├── authorization/
├── api-security/
├── database-security/
├── web-security/
├── mobile-security/
├── secrets-management/
└── infrastructure/
```

These directories represent a possible future organization.

Examples should be tested where practical and clearly identify:

- Programming language and framework.
- Supported versions.
- Required configuration.
- Security assumptions.
- Limitations.
- Relevant references.

Never include real credentials, private keys or production data.

---

# 7. tools/ — Security Tools and Integrations

**Mission:** Help developers discover, configure and use tools for application security assessment.

This directory provides a foundation for future security utilities and tool integrations.

### Intended contents

- Static application security testing.
- Dynamic application security testing.
- Dependency scanners.
- Secret detection.
- Container scanners.
- Infrastructure scanners.
- Security automation.
- CLI utilities.
- Tool configuration templates.
- Security reporting utilities.

### Possible Future Organization

```text
tools/
├── README.md
├── dependency-scanning/
├── secret-detection/
├── code-analysis/
├── api-testing/
├── container-security/
└── infrastructure-auditing/
```

These are proposed organizational areas, not currently implemented scanning capabilities.

Tools should clearly document their purpose, requirements, limitations, usage and security implications.

---

# 8. .github/ — GitHub Project Configuration

**Mission:** Maintain repository-level GitHub configuration and project automation.

This directory may contain:

- Issue templates.
- Pull request templates.
- Contribution-related GitHub configuration.
- Repository automation.
- GitHub Actions workflows.

## .github/workflows/

**Purpose:** Store GitHub Actions automation workflows.

### Intended workflows

- Markdown validation.
- Link checking.
- Secret scanning.
- Dependency auditing.
- Security linting.
- Pull request validation.
- Documentation checks.

Automation must be tested and configured to avoid exposing credentials or executing untrusted code with excessive permissions.

Only claim automated capabilities that are actually implemented and working.

---

# 9. Content Organization Standards

Every security topic should follow a consistent structure whenever applicable.

Recommended guide structure:

1. Overview.
2. Why the issue matters.
3. How the vulnerability works.
4. Common implementation mistakes.
5. How to identify the issue.
6. Prevention strategies.
7. Secure implementation examples.
8. Testing and verification.
9. Remediation guidance.
10. References and further reading.
11. Disclaimer.

Not every topic requires all sections. Contributors should include only relevant and useful material.

## Documentation Quality

All contributions should:

- Use clear and technically accurate language.
- Prefer practical explanations over vague advice.
- Include secure code examples when useful.
- Reference established standards such as OWASP and NIST.
- Identify framework and version assumptions.
- Distinguish verified facts from recommendations.
- Never publish real credentials or sensitive data.
- Use authorized testing environments.
- Avoid duplicated documentation.
- Keep content maintainable and up to date.

## Markdown and Linking

- Use standard Markdown.
- Use descriptive headings.
- Prefer relative links for repository resources.
- Use meaningful filenames.
- Use fenced code blocks with language identifiers.
- Check links before submitting changes.
- Update related indexes when adding new guides.

---

# 10. Licensing Architecture

ShipSafe uses a dual-licensing model.

| Material | License | License file |
|---|---|---|
| Written documentation | CC BY 4.0 | LICENSE-DOCUMENTATION |
| Educational guides | CC BY 4.0 | LICENSE-DOCUMENTATION |
| Security checklists | CC BY 4.0 | LICENSE-DOCUMENTATION |
| Software and scripts | MIT | LICENSE |
| Identified MIT-licensed code examples | MIT | LICENSE |

Third-party content remains subject to its original license.

Contributors should ensure they have the right to submit material and provide appropriate attribution.

When adding code examples inside documentation, clearly identify the applicable license.

For further information, see [CONTRIBUTING.md](CONTRIBUTING.md).

---

# 11. Future Expansion

ShipSafe is designed to grow beyond a documentation repository.

Potential future components include:

- Command-line security auditing.
- Automated application security checks.
- AI coding agent security instructions.
- Framework-specific security rule sets.
- CI/CD integrations.
- Security assessment reports.
- Technology-specific pre-launch audits.
- Developer security learning resources.
- Security configuration validation.
- Application security assessment workflows.

These are planned directions, not existing capabilities.

Any future implementation should be documented according to its actual functionality, limitations and testing requirements.

---

# 12. Repository Maintenance

To maintain a consistent repository:

1. Update STRUCTURE.md when adding or removing important directories.
2. Update security/README.md when adding security domains.
3. Update README.md when project navigation changes.
4. Update relevant checklists when security recommendations change.
5. Keep internal Markdown links valid.
6. Review external references periodically.
7. Avoid duplicating existing security guidance.
8. Clearly distinguish implemented features from planned features.
9. Preserve licensing and attribution requirements.
10. Review technical accuracy before merging contributions.

---

# 13. Project Philosophy

ShipSafe exists to make secure application development easier to understand, easier to implement and easier to verify.

It aims to bridge the gap between rapid software development and responsible security engineering.

Whether an application is developed by an individual developer, an engineering team, an AI coding agent or a combination of these approaches, security must remain part of the development process.

**Build fast. Audit deeply. Ship securely.**
