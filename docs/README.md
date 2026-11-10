
# ShipSafe Documentation

Welcome to the ShipSafe project documentation.

This directory contains project-level documentation, development standards, architectural decisions, onboarding resources and guidance for contributors.

While the [`security/`](../security/) directory focuses on application security concepts and vulnerabilities, `docs/` focuses on understanding, navigating, maintaining and extending the ShipSafe project itself.

## Purpose

The documentation directory aims to:

- Explain how ShipSafe is organized.
- Establish consistent documentation standards.
- Help developers understand the project architecture.
- Guide new contributors.
- Maintain technical and architectural decisions.
- Document future development directions.
- Make the project easier to maintain and extend.

## Documentation Categories

The following categories define the intended organization of this directory.

### 1. Project Overview

Explains the mission, objectives, scope and direction of ShipSafe.

Related resources:

- [Main README](../README.md)
- [Repository Structure](../STRUCTURE.md)

### 2. Documentation Standards

Defines how security guides, checklists, examples and technical documentation should be written and maintained.

Expected topics:

- Markdown formatting.
- Documentation structure.
- Technical accuracy.
- Source attribution.
- Internal linking.
- Version and framework references.
- Documentation review.

### 3. Architecture and Design

Explains important architectural decisions and the organization of the repository.

Expected topics:

- Repository architecture.
- Security domain organization.
- Content relationships.
- Future tooling architecture.
- Integration design.

### 4. Developer Onboarding

Helps contributors understand how to work with ShipSafe.

Expected topics:

- Repository setup.
- Development workflow.
- Contribution process.
- Git conventions.
- Pull request preparation.
- Local documentation validation.

### 5. Security Terminology and Glossary

Provides clear explanations of commonly used application security terminology.

Expected topics:

- Authentication and authorization.
- Vulnerability and exploitability.
- Threat modeling.
- Risk assessment.
- Encryption and hashing.
- Security testing terminology.
- Application security standards.

### 6. Project Roadmap

Documents potential development directions and project priorities.

Potential areas:

- Expanded security documentation.
- More application-specific checklists.
- Secure implementation examples.
- Security tool integrations.
- AI coding agent security guidance.
- Automated security assessment capabilities.

Planned capabilities must be clearly distinguished from implemented functionality.

## Existing Documentation

ShipSafe currently maintains its core project and security documentation in these locations:

| Resource | Purpose |
|---|---|
| [README.md](../README.md) | Project introduction and navigation |
| [STRUCTURE.md](../STRUCTURE.md) | Repository architecture |
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Contribution standards |
| [SECURITY.md](../SECURITY.md) | Responsible vulnerability reporting |
| [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) | Community standards |
| [Security Knowledge Base](../security/README.md) | Application security documentation |
| [Security Checklists](../checklists/) | Practical security reviews |

## Documentation Contribution Guidelines

When adding documentation:

1. Use clear and descriptive filenames.
2. Organize content under appropriate headings.
3. Prefer practical explanations over vague statements.
4. Include authoritative references where relevant.
5. Use relative links for repository resources.
6. Avoid unnecessary duplication.
7. Identify assumptions and limitations.
8. Distinguish implemented features from planned features.
9. Review technical accuracy before submitting.
10. Update related documentation when project structure changes.

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the complete contribution process.

## Documentation Licensing

Project documentation is generally covered by the Creative Commons Attribution 4.0 International license (CC BY 4.0), subject to the terms and exclusions described in [LICENSE-DOCUMENTATION](../LICENSE-DOCUMENTATION).

Software and code examples explicitly identified as MIT-licensed are covered by the [MIT License](../LICENSE).

Third-party materials remain subject to their respective licenses.

## Important Note

ShipSafe is an evolving open-source project.

This directory provides the foundation for project-level documentation. Individual guides and supporting documents will be added as they are developed and reviewed.

**Build fast. Audit deeply. Ship securely.**
