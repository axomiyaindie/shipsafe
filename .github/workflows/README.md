
# ShipSafe GitHub Actions Workflows

This directory is reserved for GitHub Actions workflows that may automate repository validation, maintenance and security-related checks.

## Current Status

No GitHub Actions workflows have been implemented yet.

This README documents the intended direction. It does not indicate that automated checks are currently running.

## Planned Workflows

| Workflow | Intended purpose |
|---|---|
| Markdown Validation | Check Markdown formatting and consistency |
| Link Checking | Identify broken documentation links |
| Repository Validation | Verify expected files and directory structure |
| Dependency Checks | Identify vulnerable dependencies when applicable |
| Secret Detection | Help identify accidentally committed credentials |

These are planned capabilities, not active workflows.

## Security Requirements

Every future workflow must:

- Follow least-privilege permissions.
- Avoid exposing secrets in logs or artifacts.
- Use maintained and reviewed GitHub Actions.
- Pin third-party actions to commit SHAs where practical.
- Handle untrusted pull request code safely.
- Avoid unnecessary write permissions.
- Report failures clearly.
- Document limitations and skipped checks.

## Contribution Guidelines

Before introducing a workflow:

1. Explain its purpose.
2. Review its permissions.
3. Inspect third-party actions.
4. Test it in a controlled repository environment.
5. Document its limitations.
6. Update the project documentation.

See [GitHub Configuration](../README.md), [CONTRIBUTING.md](../../CONTRIBUTING.md) and [SECURITY.md](../../SECURITY.md).

---

**Build fast. Audit deeply. Ship securely.**
