
# ShipSafe GitHub Configuration

This directory contains GitHub-specific configuration and collaboration resources for the ShipSafe repository.

It is intended to support consistent contributions, project maintenance, automation and repository security.

## Purpose

The `.github/` directory may contain:

- GitHub Actions workflows.
- Issue templates.
- Pull request templates.
- Contribution automation.
- Repository-specific guidance.
- Security and maintenance configuration.

## Current Status

ShipSafe currently uses the root-level project policies:

- [Contribution Guidelines](../CONTRIBUTING.md)
- [Security Policy](../SECURITY.md)
- [Code of Conduct](../CODE_OF_CONDUCT.md)

GitHub Actions workflows and additional repository templates have not yet been implemented.

## Planned Organization

```text
.github/
├── README.md
├── ISSUE_TEMPLATE/
│   ├── bug_report.md
│   ├── documentation_issue.md
│   └── feature_request.md
├── PULL_REQUEST_TEMPLATE.md
└── workflows/
    ├── markdown-validation.yml
    ├── link-check.yml
    └── security-checks.yml
```

This is a proposed structure, not a list of files currently implemented.

## Planned Automation

Future GitHub Actions workflows may support:

### Documentation Validation

- Markdown formatting checks.
- Broken internal link detection.
- Documentation consistency checks.

### Repository Quality

- Pull request validation.
- Basic repository structure checks.
- Automated contribution checks.

### Security Automation

- Dependency vulnerability monitoring.
- Secret detection.
- Static analysis where applicable.
- Workflow permission reviews.

Automation should use maintained actions, minimal permissions and pinned action versions where practical.

## Security Principles

GitHub configuration must follow these principles:

1. Use least-privilege workflow permissions.
2. Never expose secrets in logs.
3. Avoid executing untrusted pull request code with privileged credentials.
4. Review third-party GitHub Actions before using them.
5. Protect sensitive deployment credentials.
6. Keep workflows maintained.
7. Review security-related changes before merging.

## Contributing

Changes to GitHub configuration should be reviewed carefully because workflows can execute code and access repository resources.

See [CONTRIBUTING.md](../CONTRIBUTING.md) and [SECURITY.md](../SECURITY.md).

---

**Build fast. Audit deeply. Ship securely.**
