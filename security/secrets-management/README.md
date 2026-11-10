
# Secrets Management

Secrets management is the practice of securely creating, storing, accessing, distributing, rotating and revoking sensitive credentials used by applications and infrastructure.

Secrets include API keys, database passwords, authentication tokens, private keys, encryption keys, cloud credentials, signing certificates and other information that grants access to protected systems.

Poor secrets management can lead to account compromise, unauthorized access, data breaches, infrastructure takeover and financial loss.

This guide explains how to manage application secrets throughout development, testing, deployment and production.

## 1. Understanding Secrets

A secret is any sensitive value that must remain confidential because its exposure could allow unauthorized access or compromise security.

### Common Types of Secrets

| Secret type | Example |
|---|---|
| API keys | Third-party service credentials |
| Database credentials | Database usernames and passwords |
| Access tokens | Service and application access tokens |
| Refresh tokens | Long-lived authentication credentials |
| Cloud credentials | AWS, Azure or Google Cloud credentials |
| Private keys | SSH, TLS and signing private keys |
| Encryption keys | Keys used to encrypt sensitive information |
| Webhook secrets | Values used to authenticate webhook requests |
| Service account credentials | Credentials for automated services |
| CI/CD credentials | Deployment and automation tokens |

### Important Distinction

Not every configuration value is a secret.

Examples of generally non-secret configuration include:

- Application display names.
- Public API base URLs.
- Feature flags.
- Public analytics identifiers.
- Non-sensitive environment names.

However, a value should be treated as sensitive whenever its exposure could grant access, reveal protected information or weaken a security boundary.

## 2. Secret Exposure Risks

Secrets can be exposed through multiple parts of the software development lifecycle.

### Common Exposure Locations

- Source code repositories.
- Git commit history.
- `.env` files.
- Application configuration files.
- Frontend JavaScript bundles.
- Mobile application packages.
- Docker images.
- Container environment configuration.
- CI/CD pipeline logs.
- Build artifacts.
- Debugging output.
- Error reporting systems.
- Cloud storage.
- Developer workstations.
- Shared documents and messaging systems.

### Potential Consequences

- Unauthorized API usage.
- Database compromise.
- Cloud infrastructure takeover.
- Account impersonation.
- Data theft.
- Financial charges from resource abuse.
- Unauthorized deployment.
- Privilege escalation.
- Service disruption.

Assume that any secret exposed to an untrusted user or public artifact may be compromised.

## 3. Environment Variables

Environment variables provide a common way to supply configuration and secrets to applications without hardcoding values directly into source code.

### Example

A backend application might read configuration like this:

```javascript
const databaseUrl = process.env.DATABASE_URL;
const apiSecret = process.env.PAYMENT_SECRET_KEY;

if (!databaseUrl || !apiSecret) {
  throw new Error("Required server configuration is missing");
}
```

### Security Recommendations

- Keep production secrets outside source code.
- Provide secrets through a trusted deployment environment.
- Validate required configuration at application startup.
- Avoid printing environment variables to logs.
- Restrict access to deployment configuration.
- Use separate credentials for development, testing and production.
- Avoid sharing production secrets with developers who do not need them.
- Rotate credentials when exposure is suspected.
- Never assume that environment variables are automatically encrypted or access-controlled.

Environment variables are a delivery mechanism, not a complete secrets management solution.

## 4. Secure `.env` File Management

Many development frameworks use `.env` files to store local configuration.

Example:

```dotenv
DATABASE_URL=your_local_database_connection
JWT_SECRET=replace_with_a_secure_local_secret
PAYMENT_SECRET_KEY=replace_with_a_test_credential
```

### Recommended Files

A typical project may contain:

```text
project/
├── .env
├── .env.local
├── .env.development
├── .env.example
├── .gitignore
└── src/
```

The actual filenames and loading behavior depend on the framework.

### Security Recommendations

- Add actual secret-bearing `.env` files to `.gitignore`.
- Commit only sanitized example configuration.
- Never copy production credentials into public examples.
- Restrict filesystem access to local secret files.
- Avoid sending `.env` files to AI tools or external services unnecessarily.
- Review framework-specific environment loading behavior.
- Ensure local environment files are excluded from deployment artifacts when not needed.
- Do not assume that `.env.example` is safe without reviewing its contents.

### Example `.gitignore`

```gitignore
# Environment files
.env
.env.*
!.env.example

# Private keys and certificates
*.pem
*.key
*.p12
*.pfx

# Local secret files
secrets/
.private/
```

Review ignore patterns carefully. Some projects intentionally require specific certificates or configuration files, and exclusions should match the actual deployment architecture.

A `.gitignore` rule does not remove a file that Git already tracks.

## 5. Frontend and Backend Secret Separation

One of the most important secrets management rules is understanding where application code executes.

### Backend Secrets

Backend applications can access protected server-side credentials when their execution environment and access controls are properly secured.

Examples:

- Database passwords.
- Payment provider secret keys.
- Private API credentials.
- Signing keys.
- Cloud service credentials.

### Frontend Configuration

Frontend code executes in an environment controlled by the user.

Examples include:

- Browser JavaScript.
- Public web application bundles.
- Static site assets.
- Mobile application packages.
- Client-side configuration files.

Values included in these environments can generally be inspected.

### Security Rules

- Never expose privileged backend credentials to frontend code.
- Never place database passwords in browser applications.
- Never embed cloud administrator credentials in mobile applications.
- Never rely on minification or obfuscation to protect secrets.
- Use backend endpoints for operations requiring privileged credentials.
- Restrict public API keys according to provider capabilities.
- Enforce authorization on the backend.

A public key is safe to distribute only when the service explicitly designs it for public client-side use and appropriate restrictions are applied.

## 6. Secret Managers and Vaults

Dedicated secret management services provide centralized storage, access controls, auditing and rotation capabilities.

### Common Solutions

| Service | Typical use |
|---|---|
| HashiCorp Vault | Centralized secret storage and access policies |
| AWS Secrets Manager | AWS application and infrastructure secrets |
| AWS Systems Manager Parameter Store | Configuration and sensitive parameters |
| Azure Key Vault | Secrets, keys and certificates |
| Google Cloud Secret Manager | Application and infrastructure secrets |
| Doppler | Centralized application configuration and secrets |
| 1Password Secrets Automation | Shared application and automation secrets |
| Infisical | Development and deployment secrets management |

Select a solution based on the deployment platform, team requirements, security model and operational complexity.

### Security Recommendations

- Use centralized secret storage for production systems where appropriate.
- Restrict access through identity-based policies.
- Separate secret access by application and environment.
- Enable audit logging.
- Use short-lived credentials where supported.
- Rotate secrets according to risk and provider capabilities.
- Avoid distributing shared administrator credentials.
- Review access policies periodically.
- Protect the secret manager itself with strong authentication and authorization.

A secret manager reduces exposure risks but must itself be configured and monitored securely.

## 7. Secret Access Control

Not every developer, application or service should have access to every secret.

### Principle of Least Privilege

Grant each identity only the permissions necessary to perform its intended tasks.

For example:

| Identity | Appropriate access |
|---|---|
| Frontend application | Only intentionally public configuration |
| Backend service | Secrets required by that service |
| Database migration job | Required migration credentials |
| CI/CD deployment job | Deployment credentials for its target |
| Developer workstation | Local development credentials |
| Administrator | Explicitly authorized administrative access |

### Security Recommendations

- Assign permissions to individual identities.
- Avoid shared credentials across unrelated services.
- Separate production and development access.
- Review access permissions regularly.
- Revoke access when employees or services no longer require it.
- Prefer workload identities and managed identities where available.
- Use temporary access where practical.
- Log sensitive secret access.

Avoid giving an entire application or team unrestricted access to a global collection of production secrets.

## 8. Secret Rotation and Revocation

Secret rotation means replacing an existing credential with a new one.

Revocation means invalidating a credential so that it can no longer be used.

### When to Rotate Secrets

- A credential is accidentally committed to Git.
- A developer workstation is compromised.
- An employee leaves and retains access to credentials.
- A third-party provider reports a security incident.
- Suspicious API activity is detected.
- A credential reaches its expiration date.
- A security policy requires periodic replacement.

### Recommended Rotation Process

1. Identify the affected credential.
2. Determine which applications and services depend on it.
3. Generate a replacement credential.
4. Update the secret manager or deployment configuration.
5. Deploy the updated application configuration.
6. Verify that the replacement works.
7. Revoke the previous credential.
8. Monitor for unexpected failures or continued use of the old credential.
9. Record the incident and remediation.

When supported, use overlapping credential validity during migration to reduce downtime.

Do not delay revocation of a known compromised credential merely to preserve compatibility.

## 9. Git and Repository Secret Protection

Git repositories frequently become a source of accidental credential exposure.

Deleting a secret from the latest version of a file does not necessarily remove it from the repository's previous commit history.

### Prevention

- Review files before staging.
- Maintain appropriate `.gitignore` rules.
- Use secret scanning.
- Avoid committing `.env` files.
- Keep private keys outside source repositories.
- Review pull requests for credential exposure.
- Use pre-commit scanning where appropriate.

### Useful Secret Scanning Tools

- [Gitleaks](https://github.com/gitleaks/gitleaks)
- [TruffleHog](https://github.com/trufflesecurity/trufflehog)
- [GitHub Secret Scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
- [detect-secrets](https://github.com/Yelp/detect-secrets)

### Example: Gitleaks

Install Gitleaks using its official installation instructions.

Scan a working directory:

```bash
gitleaks dir .
```

Scan Git history:

```bash
gitleaks git .
```

Command syntax can vary by version. Check the installed version's help and official documentation before integrating it into automation.

### If a Secret Is Committed

1. Revoke or rotate the exposed credential immediately.
2. Determine whether it was accessed or abused.
3. Identify every system that trusted the credential.
4. Remove the secret from the current source files.
5. Review repository history and access logs.
6. Clean Git history when necessary.
7. Coordinate history rewriting with repository collaborators.
8. Add preventive scanning.
9. Document the incident.

History rewriting does not make an exposed credential safe again. Anyone who accessed the previous commit may still have a copy.

## 10. CI/CD Secrets Management

Continuous integration and deployment systems often require credentials to build, test and deploy applications.

Examples include:

- Repository access tokens.
- Cloud deployment credentials.
- Container registry credentials.
- Signing credentials.
- Infrastructure deployment secrets.
- Package publishing tokens.

### Security Recommendations

- Store credentials in the CI/CD platform's protected secret storage.
- Never hardcode credentials in workflow files.
- Restrict secret access to trusted workflows.
- Avoid exposing secrets to untrusted pull requests.
- Limit token permissions.
- Prefer short-lived credentials and workload identity federation where supported.
- Separate deployment credentials by environment.
- Protect production deployment approvals.
- Prevent secrets from appearing in logs.
- Audit workflow changes and secret access.

### GitHub Actions Example

```yaml
name: Deploy Application

on:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Deploy application
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: ./scripts/deploy.sh
```

This is an illustrative example. Production workflows should pin third-party actions to verified full commit SHAs, use appropriately scoped permissions and review deployment environment protections.

Do not print secret values in workflow output.

## 11. Docker and Container Secrets

Containerized applications frequently require database passwords, API credentials and service tokens.

### Common Mistakes

- Baking secrets into Docker images.
- Copying `.env` files into build contexts.
- Including credentials in Docker build arguments.
- Storing passwords in image layers.
- Running containers with unnecessarily broad permissions.
- Exposing secrets through container logs or inspection.

### Security Recommendations

- Use `.dockerignore` to exclude sensitive local files.
- Do not embed production credentials in Dockerfiles.
- Use platform-supported runtime secret mechanisms.
- Restrict access to container orchestration secrets.
- Avoid unnecessarily exposing environment configuration.
- Use separate credentials for individual services.
- Review image layers and build artifacts.
- Rotate credentials when images or environments expose them.

### Example `.dockerignore`

```dockerignore
.env
.env.*
!.env.example

.git/
.gitignore

secrets/
.private/
```

Review build contexts and Dockerfile copy instructions to ensure secret files are not included accidentally.

## 12. Cloud Credentials and Workload Identity

Cloud applications often need to access storage, databases, queues and other managed services.

Long-lived cloud access keys create unnecessary exposure when safer identity mechanisms are available.

### Security Recommendations

- Prefer managed identities or workload identity federation.
- Avoid using root or administrator credentials in applications.
- Restrict permissions to required resources.
- Separate identities by workload.
- Avoid sharing access keys between services.
- Monitor authentication and authorization events.
- Rotate long-lived credentials where they cannot be eliminated.
- Restrict access to cloud credential configuration.

Applications should receive only the cloud permissions required for their intended operations.

## 13. Database and Third-Party Service Credentials

Applications often depend on databases and external services.

### Database Credentials

- Keep database passwords on trusted backend systems.
- Use separate credentials for different environments.
- Restrict database users to required operations.
- Avoid exposing connection strings in frontend applications.
- Use encrypted database connections.
- Rotate credentials when compromise is suspected.
- Avoid using administrative database accounts for normal application operations.

### Third-Party API Credentials

- Keep privileged API credentials on backend systems.
- Apply provider-supported restrictions.
- Use separate credentials for development and production.
- Monitor usage and unusual request patterns.
- Apply quotas and rate limits.
- Revoke unused credentials.
- Review provider access permissions.

Some services intentionally provide public client keys. Understand the provider's security model before using such keys in a browser or mobile application.

## 14. Cryptographic Keys and Certificates

Cryptographic keys require additional protection because they may be used to encrypt data, sign software, authenticate services or establish trust.

### Security Recommendations

- Generate keys using trusted cryptographic libraries.
- Use appropriate key sizes and algorithms.
- Store private keys in secure key management systems where practical.
- Restrict access to signing keys.
- Protect certificate private keys.
- Define key rotation and expiration procedures.
- Maintain secure backups when required.
- Document key ownership and intended use.
- Never publish private keys in repositories or logs.

### Important Distinction

Encryption keys, signing keys, passwords and API tokens have different security requirements.

Do not use one credential for unrelated cryptographic purposes.

## 15. Logging and Secret Redaction

Logs help developers investigate application behavior, but they can also expose credentials and personal information.

### Never Log

- Passwords.
- Authentication tokens.
- Session identifiers.
- Private API keys.
- Database connection strings.
- Private cryptographic keys.
- Sensitive authentication headers.
- Unnecessary personal information.

### Security Recommendations

- Redact sensitive fields before logging.
- Avoid logging entire request headers.
- Review error reporting configuration.
- Protect access to logs.
- Define retention periods.
- Monitor unusual authentication and secret access activity.
- Test redaction behavior.

Masking a secret in a dashboard is not sufficient if the original value remains stored in the underlying logs.

## 16. AI-Assisted Development and Secret Safety

AI coding agents can help developers create applications, configure environments and automate deployment tasks.

However, sending secrets to an AI tool can expose them to additional systems and processing environments.

### Security Recommendations

- Do not paste production API keys into prompts.
- Do not upload `.env` files containing real credentials.
- Use placeholders in examples.
- Review generated code for hardcoded secrets.
- Check whether an AI agent can access local secret files.
- Restrict agent access to sensitive directories.
- Avoid granting agents unrestricted production credentials.
- Use temporary, narrowly scoped credentials when an agent needs access.
- Review generated CI/CD workflows for unsafe secret handling.
- Scan generated code and commits before publishing.

### Example: Safe Prompt

```text
Configure the application to read PAYMENT_SECRET_KEY
from a server-side environment variable.

Do not hardcode the credential.
Do not expose it to frontend code.
Use a placeholder in examples.
```

AI-generated code should be reviewed before receiving access to production credentials or systems.

## 17. Secret Management Checklist

### Development

- [ ] No real credentials are hardcoded in source code.
- [ ] Actual `.env` files are excluded from Git.
- [ ] Example configuration contains placeholders only.
- [ ] Development credentials are separate from production.
- [ ] Secret scanning is available.
- [ ] Debug logs do not expose sensitive values.

### Application Architecture

- [ ] Privileged secrets are accessible only to trusted backend components.
- [ ] Frontend and mobile applications contain no privileged credentials.
- [ ] Secret access follows least privilege.
- [ ] Database credentials have restricted permissions.
- [ ] Third-party API credentials are appropriately restricted.
- [ ] Cryptographic keys are protected separately from encrypted data.

### Deployment

- [ ] Production secrets are supplied through protected deployment configuration.
- [ ] CI/CD secrets are restricted to trusted workflows.
- [ ] Cloud workloads use managed identities where appropriate.
- [ ] Docker images do not contain production credentials.
- [ ] Production and staging credentials are separated.
- [ ] Secret access and deployment activity are auditable.

### Incident Readiness

- [ ] Credential rotation procedures are documented.
- [ ] Compromised credentials can be revoked.
- [ ] Secret ownership is documented.
- [ ] Access permissions are reviewed periodically.
- [ ] Git history exposure is considered during incident response.
- [ ] Relevant teams know how to respond to leaked credentials.

## 18. Common Secrets Management Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Hardcoded credentials | Unauthorized system access |
| Public repository exposure | Credential compromise |
| Exposed frontend secrets | Unauthorized API usage |
| Weak secret access policies | Privilege escalation |
| Long-lived credentials | Extended compromise window |
| Missing credential rotation | Continued unauthorized access |
| Secrets in logs | Accidental information disclosure |
| Insecure CI/CD configuration | Deployment compromise |
| Secrets in container images | Credential exposure |
| Shared administrator credentials | Broad infrastructure compromise |

## 19. Remediation Priorities

When secrets management weaknesses are discovered:

1. Revoke or rotate exposed credentials.
2. Identify affected systems and possible unauthorized access.
3. Remove exposed secrets from active application code.
4. Replace hardcoded credentials with secure configuration.
5. Restrict access to secret storage.
6. Separate production and non-production credentials.
7. Review CI/CD and container build configurations.
8. Implement secret scanning.
9. Improve logging and redaction.
10. Document rotation and incident response procedures.

Prioritize exposed production credentials and credentials with broad administrative privileges.

## 20. References and Further Reading

### Security Standards and Guidance

- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
- [OWASP CI/CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST SP 800-57: Key Management](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final)

### Secret Management Platforms

- [HashiCorp Vault Documentation](https://developer.hashicorp.com/vault/docs)
- [AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/)
- [Azure Key Vault Documentation](https://learn.microsoft.com/en-us/azure/key-vault/)
- [Google Cloud Secret Manager Documentation](https://cloud.google.com/secret-manager/docs)
- [Doppler Documentation](https://docs.doppler.com/)
- [Infisical Documentation](https://infisical.com/docs)

### Secret Scanning

- [Gitleaks](https://github.com/gitleaks/gitleaks)
- [TruffleHog](https://github.com/trufflesecurity/trufflehog)
- [GitHub Secret Scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)

---

This guide is educational and does not replace application-specific security reviews, professional security assessments or incident response procedures.
