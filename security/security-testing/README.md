
# Security Testing

Security testing is the process of evaluating an application, system or infrastructure to identify security weaknesses, validate security controls and reduce the risk of exploitation.

It helps developers determine whether an application protects sensitive information, enforces access restrictions, handles untrusted input safely and behaves securely under unexpected conditions.

Security testing should be integrated throughout the software development lifecycle rather than performed only before production deployment.

This guide covers security testing methodologies, tools, practical workflows, vulnerability validation, automation and reporting for web applications, mobile applications, APIs, backend services and cloud infrastructure.

## 1. What Is Security Testing?

Security testing evaluates whether an application protects its data, functionality and infrastructure against unauthorized access, misuse and attacks.

It examines both the presence of vulnerabilities and the effectiveness of security controls.

### Main Objectives

- Identify security vulnerabilities.
- Verify authentication mechanisms.
- Test authorization boundaries.
- Detect sensitive information exposure.
- Identify insecure configurations.
- Evaluate dependency security.
- Test API security.
- Assess mobile application protections.
- Validate infrastructure security.
- Verify remediation efforts.
- Reduce production security risks.

### Security Testing vs. Functional Testing

| Functional testing | Security testing |
|---|---|
| Checks whether features work | Checks whether features are secure |
| Tests expected workflows | Tests expected and unexpected behavior |
| Validates business functionality | Validates security requirements |
| Focuses on correctness | Focuses on protection and abuse resistance |
| Tests authorized operations | Also tests unauthorized operations |

Both are necessary for reliable software.

## 2. Types of Security Testing

Different testing approaches identify different categories of weaknesses.

### Static Application Security Testing (SAST)

Examines source code, bytecode or compiled artifacts without executing the application.

Common findings include:

- Hardcoded secrets.
- Unsafe functions.
- Potential injection vulnerabilities.
- Insecure cryptographic usage.
- Dangerous coding patterns.
- Missing security checks detectable by supported rules.

### Dynamic Application Security Testing (DAST)

Tests a running application by sending requests and analyzing its behavior.

Common findings include:

- Exposed endpoints.
- Insecure response headers.
- Authentication weaknesses.
- Reflected cross-site scripting.
- Misconfigured services.
- Some injection vulnerabilities.

DAST generally cannot see every internal code path.

### Software Composition Analysis (SCA)

Examines third-party dependencies and their known security vulnerabilities.

Common findings include:

- Vulnerable packages.
- Outdated libraries.
- Transitive dependency vulnerabilities.
- Insecure dependency versions.
- License policy concerns.

### Interactive Application Security Testing (IAST)

Combines application execution with instrumentation to observe behavior during testing.

It can help identify vulnerabilities in code paths exercised by tests.

### Manual Security Testing

A security reviewer manually examines application behavior, architecture, access controls and attack scenarios.

Manual testing is particularly important for:

- Business logic vulnerabilities.
- Complex authorization rules.
- Multi-tenant isolation.
- Payment workflows.
- Abuse scenarios.
- Application-specific security requirements.

### Penetration Testing

An authorized security assessment that attempts to exploit weaknesses within an agreed scope.

It may combine automated tools, manual analysis and controlled exploitation to determine practical impact.

### Configuration and Infrastructure Testing

Evaluates the security of servers, networks, containers, cloud services and deployment configurations.

## 3. Security Testing Methodology

A structured methodology makes security assessments repeatable and easier to review.

### Phase 1: Define Scope

Before testing, identify:

- Application or system being tested.
- Approved domains and IP addresses.
- API endpoints.
- Test environments.
- Authorized accounts.
- Allowed testing methods.
- Restricted operations.
- Testing time window.
- Responsible contacts.

Never assume that permission to test one application also grants permission to test its hosting provider, connected services or unrelated systems.

### Phase 2: Understand the Architecture

Review:

- Application architecture.
- Data flows.
- Authentication.
- Authorization.
- Database access.
- External integrations.
- Infrastructure.
- Sensitive data.
- Trust boundaries.

Use the threat model to identify important attack scenarios.

Reference: `../threat-modeling/README.md`

### Phase 3: Identify Attack Surfaces

List accessible entry points.

Examples:

- Login endpoints.
- Registration.
- Password recovery.
- File uploads.
- Search.
- API endpoints.
- Administrative dashboards.
- Webhooks.
- Mobile deep links.
- Cloud management interfaces.

### Phase 4: Select Testing Methods

Choose appropriate methods based on the application's technology and risk.

Examples:

- SAST for source code.
- SCA for dependencies.
- Secret scanning for credentials.
- DAST for running web applications.
- API testing for endpoint authorization.
- Manual testing for business logic.
- Infrastructure assessment for deployment systems.

### Phase 5: Execute Tests

Perform testing within the approved scope.

Record:

- Test objective.
- Tool or methodology.
- Target component.
- Observed behavior.
- Evidence.
- Potential impact.
- Reproduction steps.

### Phase 6: Validate Findings

Determine whether each finding represents a real security weakness.

Check:

- Is the behavior reproducible?
- Is the finding within scope?
- Is exploitation possible?
- What security boundary is affected?
- What information or functionality is exposed?
- Could the finding be a false positive?
- Does the issue already have an effective mitigation?

### Phase 7: Remediate

Recommend an appropriate fix and assign responsibility.

### Phase 8: Retest

Verify that the vulnerability is fixed and that the remediation has not introduced new problems.

## 4. SAST — Static Application Security Testing

SAST analyzes code without requiring a running application.

It can be integrated into development environments, pull requests and CI/CD pipelines.

### What SAST Can Detect

- Hardcoded credentials.
- Insecure cryptographic functions.
- Potential SQL injection.
- Unsafe deserialization patterns.
- Dangerous command execution.
- Path traversal patterns.
- Some missing security checks.
- Insecure coding practices.

### Common SAST Tools

| Tool | Description |
|---|---|
| Semgrep | Rule-based static analysis |
| CodeQL | Semantic code analysis |
| SonarQube | Code quality and security analysis |
| Bandit | Python security linting |
| ESLint security plugins | JavaScript and TypeScript checks |
| Gosec | Go security analysis |
| Brakeman | Ruby on Rails security analysis |

### Example: Semgrep

Install Semgrep according to its official documentation:

https://semgrep.dev/docs/

Run a scan:

```bash
semgrep scan --config auto .
```

Review the findings and confirm that the selected rules cover the application's languages and frameworks.

### Example: Bandit for Python

Install:

```bash
python -m pip install bandit
```

Scan a project:

```bash
bandit -r .
```

### SAST Limitations

SAST cannot reliably identify every vulnerability.

It may:

- Produce false positives.
- Miss runtime-dependent issues.
- Miss complex business logic flaws.
- Fail to understand custom authorization policies.
- Miss vulnerabilities outside its supported rules.

A clean SAST result does not prove that an application is secure.

## 5. DAST — Dynamic Application Security Testing

DAST evaluates an application while it is running.

It interacts with the application through its exposed interfaces and observes responses.

### Common DAST Targets

- Web applications.
- Public APIs.
- Authentication workflows.
- Application endpoints.
- HTTP response headers.
- Session handling.
- Input processing.

### Common DAST Tools

| Tool | Description |
|---|---|
| OWASP ZAP | Open-source web application security testing |
| Burp Suite | Web security testing platform |
| Nuclei | Template-based vulnerability scanning |
| Nikto | Web server security scanner |
| Wapiti | Web application vulnerability scanner |

### OWASP ZAP

Official website:

https://www.zaproxy.org/

ZAP can be used for:

- Passive scanning.
- Active scanning.
- Spidering.
- API testing.
- Authentication-assisted testing.
- Security report generation.

### Important Safety Considerations

Active scanning may submit unexpected requests and modify application data.

Before testing:

- Obtain explicit authorization.
- Prefer a staging environment.
- Use test accounts.
- Back up important data.
- Define scan limits.
- Avoid destructive tests unless specifically authorized.
- Coordinate with system owners.

DAST results should be reviewed manually before reporting vulnerabilities as confirmed.

## 6. SCA — Software Composition Analysis

SCA identifies known security vulnerabilities in application dependencies.

Modern applications frequently depend on hundreds or thousands of direct and transitive packages.

### What SCA Examines

- Package names.
- Installed versions.
- Dependency trees.
- Known vulnerability advisories.
- Vulnerable transitive dependencies.
- Sometimes package licensing and provenance.

### Common SCA Tools

- npm audit.
- Dependabot.
- Renovate.
- OSV-Scanner.
- Trivy.
- Snyk.
- OWASP Dependency-Check.

### Node.js Example

```bash
npm audit
```

To request a machine-readable report:

```bash
npm audit --json
```

Do not automatically apply every suggested update without reviewing compatibility and breaking changes.

### Python Example

Install pip-audit:

```bash
python -m pip install pip-audit
```

Audit installed packages:

```bash
pip-audit
```

### OSV-Scanner

Official project:

https://google.github.io/osv-scanner/

Example:

```bash
osv-scanner scan source -r .
```

Verify the command and supported options against the installed version's documentation.

### Important Considerations

A vulnerable dependency does not always mean the application is exploitable.

Review:

- Whether the vulnerable code is present.
- Whether the affected functionality is used.
- Whether the vulnerable code path is reachable.
- Whether a fix is available.
- Whether compensating controls exist.
- Whether the package can be safely upgraded.

Reference: `../dependencies/README.md`

## 7. Secret Scanning

Secret scanning identifies credentials and sensitive values accidentally exposed in source code or related files.

### Common Secrets

- API keys.
- Database passwords.
- Cloud credentials.
- OAuth secrets.
- Private keys.
- Access tokens.
- Signing keys.
- Deployment credentials.

### Common Secret Exposure Locations

- Source files.
- `.env` files.
- Git history.
- Configuration files.
- CI/CD logs.
- Build artifacts.
- Container images.
- Documentation.
- Debug output.

### Common Secret Scanning Tools

| Tool | Purpose |
|---|---|
| Gitleaks | Detect secrets in repositories and Git history |
| TruffleHog | Search for exposed credentials |
| GitHub Secret Scanning | Detect supported secret patterns |
| detect-secrets | Identify and manage potential secrets |

### Gitleaks Example

Official project:

https://github.com/gitleaks/gitleaks

Scan a Git repository:

```bash
gitleaks git .
```

Check the installed version's documentation for exact command options.

### Critical Remediation Rule

If a real credential is exposed:

1. Revoke or rotate the credential.
2. Investigate possible unauthorized usage.
3. Review logs and access history.
4. Remove the exposure from current files.
5. Review Git history and distribution channels.
6. Improve secret handling to prevent recurrence.

Removing a credential from the latest commit does not make the exposed credential safe again.

Reference: `../secrets-management/README.md`

## 8. Web Application Security Testing

Web application testing evaluates security weaknesses in browser-based applications and their backend services.

### Important Testing Areas

**Authentication**

- Login protection.
- Password reset.
- Session management.
- MFA behavior.
- Account enumeration.
- Session expiration.

**Authorization**

- Horizontal privilege escalation.
- Vertical privilege escalation.
- Resource ownership.
- Administrative endpoint protection.
- Tenant isolation.

**Input Handling**

- SQL injection.
- Cross-site scripting.
- Command injection.
- Path traversal.
- Unsafe deserialization.
- File upload validation.

**Browser Security**

- CSRF protection.
- CORS configuration.
- Content Security Policy.
- Cookie security.
- Clickjacking protection.
- Open redirects.

**Information Exposure**

- Debug responses.
- Sensitive API fields.
- Public files.
- Source maps.
- Error messages.
- Misconfigured storage.

### Testing Questions

- Can an unauthenticated visitor access private functionality?
- Can one user access another user's resources?
- Can normal users invoke administrative operations?
- Are sensitive values returned unnecessarily?
- Can malicious input execute in a browser context?
- Can uploaded files be accessed or executed improperly?
- Are security headers configured appropriately?

### Recommended Reference

OWASP Web Security Testing Guide:

https://owasp.org/www-project-web-security-testing-guide/

## 9. API Security Testing

API security testing evaluates how endpoints respond to authorized and unauthorized requests.

### Important Testing Areas

- Authentication.
- Object-level authorization.
- Function-level authorization.
- Property-level authorization.
- Input validation.
- Rate limiting.
- Resource consumption.
- CORS.
- Error handling.
- File uploads.
- Webhooks.
- GraphQL security.

### Authorization Testing Matrix

| Test scenario | Expected behavior |
|---|---|
| Unauthenticated request to protected endpoint | Denied |
| User accesses own resource | Allowed when authorized |
| User accesses another user's resource | Denied |
| Normal user invokes admin operation | Denied |
| User modifies protected ownership fields | Rejected or ignored |
| Expired token is reused | Denied |
| User accesses another tenant's data | Denied |

The expected behavior must reflect the application's actual security policy.

### API Testing Tools

- Postman.
- Bruno.
- Burp Suite.
- OWASP ZAP.
- Schemathesis.
- RESTler.
- Insomnia.

### Example: Testing Authorization

Assume a test API provides:

```text
GET /api/documents/123
```

Use two authorized test accounts:

- User A owns document 123.
- User B does not own document 123.

Verify:

1. User A can access document 123.
2. User B cannot access document 123.
3. An unauthenticated request is denied if authentication is required.
4. Changing the identifier does not bypass authorization.
5. The response does not reveal unnecessary private information.

Use only test accounts and resources within the approved scope.

## 10. Authentication and Authorization Testing

Authentication and authorization should be tested independently.

### Authentication Test Cases

- [ ] Invalid credentials are rejected.
- [ ] Expired tokens are rejected.
- [ ] Revoked sessions are rejected.
- [ ] Password reset tokens cannot be reused.
- [ ] Login attempts are appropriately restricted.
- [ ] MFA requirements are enforced where configured.
- [ ] Sensitive account recovery actions are protected.

### Authorization Test Cases

- [ ] Users cannot access unauthorized resources.
- [ ] Normal users cannot perform administrator operations.
- [ ] Tenant boundaries are enforced.
- [ ] Resource ownership is checked.
- [ ] Sensitive fields cannot be modified without permission.
- [ ] Background operations enforce appropriate access rules.
- [ ] Permissions are checked on the backend.

### Important Principle

A successful login test does not establish that authorization is correct.

A user may be authenticated but still lack permission to access a particular resource or perform an operation.

References:

- `../authentication/README.md`
- `../authorization/README.md`

## 11. Mobile Application Security Testing

Mobile applications introduce security concerns related to device storage, application binaries, permissions and communication.

### Android Testing Areas

- Application permissions.
- Exported components.
- Intent handling.
- Deep links.
- Local data storage.
- Network communication.
- WebViews.
- Backup configuration.
- Debug settings.
- Signing configuration.

### iOS Testing Areas

- Keychain usage.
- Data protection.
- Application permissions.
- URL schemes and universal links.
- Local storage.
- Network communication.
- WebViews.
- Entitlements.
- Application signing.

### Cross-Platform Testing

For Flutter, React Native and other cross-platform applications, assess both framework-specific behavior and underlying platform security.

### Mobile Security Tools

- MobSF.
- Frida.
- JADX.
- Apktool.
- Objection.
- OWASP MASTG testing resources.

### Mobile Security Testing Standard

OWASP Mobile Application Security Verification Standard:

https://mas.owasp.org/MASVS/

OWASP Mobile Application Security Testing Guide:

https://mas.owasp.org/MASTG/

Mobile testing should use applications and devices that are owned by the testing team or explicitly authorized for assessment.

Reference: `../mobile-security/README.md`

## 12. Infrastructure and Cloud Security Testing

Infrastructure testing examines the systems supporting application deployment and operation.

### Testing Areas

- Public network exposure.
- SSH configuration.
- Firewall rules.
- Server patch levels.
- Database exposure.
- TLS configuration.
- Container security.
- Cloud IAM permissions.
- Storage access policies.
- Backup protection.
- Infrastructure as Code.
- Administrative interfaces.

### Common Tools

| Tool | Purpose |
|---|---|
| Nmap | Network discovery and port scanning |
| Lynis | Linux security auditing |
| Trivy | Vulnerability and configuration scanning |
| Checkov | Infrastructure as Code scanning |
| OpenSCAP | Security configuration assessment |
| Greenbone/OpenVAS | Vulnerability management |

### Infrastructure Testing Principles

- Obtain authorization before scanning.
- Define network scope.
- Avoid disruptive tests against production.
- Review firewall exposure.
- Validate configuration findings.
- Assess the business impact.
- Document remediation.
- Retest after changes.

Reference: `../infrastructure/README.md`

## 13. Database Security Testing

Database security testing evaluates data access, permissions, query handling and storage protections.

### Testing Areas

- SQL injection.
- NoSQL injection.
- Excessive database privileges.
- Missing tenant isolation.
- Weak database authentication.
- Public network exposure.
- Insecure backup storage.
- Unencrypted connections.
- Sensitive information exposure.

### Testing Questions

- Can application users access data belonging to other users?
- Are queries parameterized?
- Are database accounts restricted?
- Can a compromised application account access unnecessary tables?
- Are administrative credentials isolated?
- Are backups protected?
- Are database network rules restrictive?

### Database-Specific Controls

For systems using PostgreSQL, review:

- Database roles.
- Schema permissions.
- Row-Level Security policies.
- Connection encryption.
- Administrative privileges.
- Backup permissions.

For Supabase applications, verify:

- Row-Level Security is enabled on relevant exposed tables.
- Policies cover required operations.
- Anonymous and authenticated roles have appropriate permissions.
- Service-role credentials remain server-side.
- Storage policies enforce intended access.
- Security is tested using realistic user identities.

Enabling Row-Level Security alone does not guarantee correct tenant isolation. Policies must be reviewed and tested.

Reference: `../database-security/README.md`

## 14. Business Logic Security Testing

Business logic vulnerabilities occur when an application allows an operation that violates its intended rules.

They are often difficult for generic scanners to identify.

### Common Examples

- Applying a discount multiple times.
- Purchasing an item without valid payment.
- Changing account ownership without authorization.
- Bypassing subscription restrictions.
- Reusing one-time actions.
- Manipulating transaction amounts.
- Skipping required workflow steps.
- Exploiting race conditions.

### Testing Questions

- Can a user bypass required workflow steps?
- Can a transaction be submitted twice?
- Can users modify protected values?
- Can free accounts access paid functionality?
- Can an operation be repeated beyond intended limits?
- Can concurrent requests create inconsistent state?
- Are business rules enforced on trusted backend components?

### Testing Recommendations

- Understand the intended business workflow.
- Identify important state transitions.
- Test boundary values.
- Test repeated operations.
- Test concurrent requests where relevant.
- Use separate test accounts.
- Validate financial and entitlement calculations.
- Verify server-side enforcement.

Business logic testing requires understanding the application's intended behavior.

## 15. Vulnerability Validation and False Positives

Security tools can identify suspicious patterns, but findings must be validated.

### What Is a False Positive?

A false positive occurs when a tool reports a potential vulnerability that is not actually present under the relevant conditions.

### What Is a False Negative?

A false negative occurs when a vulnerability exists but the testing method does not identify it.

### Why False Positives Occur

- Incomplete application context.
- Generic detection rules.
- Framework-specific behavior.
- Incorrect scanner assumptions.
- Unreachable vulnerable code.
- Existing security controls.
- Test environment differences.

### Why False Negatives Occur

- Unsupported technologies.
- Missing authentication context.
- Incomplete endpoint discovery.
- Complex business logic.
- Unexecuted code paths.
- Insufficient test coverage.
- Custom application behavior.

### Finding Validation Process

1. Reproduce the reported behavior.
2. Identify the affected component.
3. Review the relevant source code or configuration.
4. Determine whether the behavior violates a security requirement.
5. Evaluate realistic exploitation conditions.
6. Assess impact.
7. Document supporting evidence.
8. Classify the finding accurately.

### Finding Statuses

Use clear statuses:

- Confirmed.
- Potential.
- False positive.
- Accepted risk.
- Fixed.
- Retest required.
- Not reproducible.

Do not label a potential scanner result as a confirmed vulnerability without adequate evidence.

## 16. Vulnerability Severity and Prioritization

Security findings should be prioritized according to their actual risk.

### Important Factors

- Exploitability.
- Exposure.
- Required privileges.
- User interaction.
- Confidentiality impact.
- Integrity impact.
- Availability impact.
- Affected users.
- Business impact.
- Existing mitigations.

### CVSS

The Common Vulnerability Scoring System provides a standardized framework for describing vulnerability severity.

Official resource:

https://www.first.org/cvss/

CVSS scores are useful for prioritization but should not replace application-specific risk assessment.

### Suggested Priority Levels

| Priority | General description |
|---|---|
| Critical | Severe impact with a credible exploitation path |
| High | Significant security impact |
| Medium | Meaningful weakness requiring specific conditions |
| Low | Limited impact or difficult exploitation |
| Informational | Observation without a demonstrated vulnerability |

Define consistent criteria for each project.

## 17. Security Testing in CI/CD

CI/CD pipelines can automatically perform security checks when code is committed or submitted for review.

### Recommended Automated Checks

- Secret scanning.
- Dependency scanning.
- Static analysis.
- Unit tests.
- Security-specific integration tests.
- Container image scanning.
- Infrastructure as Code scanning.
- License policy checks where required.

### Example GitHub Actions Workflow

```yaml
name: ShipSafe Security Checks

on:
  pull_request:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  secret-scan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Run Gitleaks
        uses: gitleaks/gitleaks-action@v2

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

This is an illustrative workflow for a repository that uses Node.js and has a committed package lockfile.

Before adopting it:

- Review and pin third-party actions to verified commit SHAs where appropriate.
- Confirm action permissions.
- Review action maintenance and provenance.
- Configure the correct runtime version.
- Decide how findings should affect pull requests.
- Avoid exposing secrets to untrusted pull request code.
- Test the workflow in a non-production environment.

A pipeline passing its checks does not prove that an application is secure.

## 18. Security Testing for AI-Assisted Applications

AI-generated applications should undergo the same security testing as traditionally developed applications.

The speed of AI-assisted development can increase the volume of code changes, making repeatable security checks particularly important.

### Common Testing Targets

- Generated authentication code.
- API authorization.
- Database access.
- Generated SQL.
- Dependency additions.
- Environment configuration.
- Frontend secret exposure.
- File upload functionality.
- Infrastructure configuration.
- AI agent permissions.

### AI-Assisted Testing Workflow

1. Ask the agent to identify security-sensitive code.
2. Review the application's trust boundaries.
3. Run appropriate static analysis.
4. Scan dependencies.
5. Scan for secrets.
6. Test API authorization.
7. Test negative scenarios.
8. Review configuration changes.
9. Validate findings independently.
10. Record unresolved risks.

### Example Agent Testing Instruction

```text
Perform a security review of the application within the
authorized repository and test environment.

Review:

1. Authentication and session handling.
2. Authorization and resource ownership.
3. API endpoint exposure.
4. Database query safety.
5. Input validation and output encoding.
6. Secret exposure.
7. Dependency vulnerabilities.
8. File upload and download handling.
9. Security-sensitive configuration.
10. Multi-tenant isolation.

For every finding:

- Identify the affected file or endpoint.
- Explain the potential security impact.
- Provide supporting evidence.
- Distinguish confirmed issues from potential risks.
- Recommend a specific remediation.
- Suggest a safe validation test.

Do not claim a vulnerability is confirmed without
reproducible evidence.

Do not access systems outside the authorized scope.
Do not perform destructive tests or modify production
data.
```

AI-generated findings require independent validation.

## 19. Security Testing Report Template

A useful security report should communicate findings clearly and support remediation.

Use the following template:

```markdown
# Security Testing Report

## 1. Assessment Information

- Application:
- Version:
- Environment:
- Assessment date:
- Assessor:
- Authorized scope:

## 2. Assessment Overview

Describe the purpose and methodology of the assessment.

## 3. Testing Methods

- SAST:
- DAST:
- SCA:
- Secret scanning:
- Manual testing:
- Infrastructure testing:

## 4. Executive Summary

Summarize the overall security observations.

## 5. Findings Summary

| ID | Finding | Severity | Status |
|---|---|---|---|
| SHIP-001 | | | |

## 6. Detailed Findings

### SHIP-001: [Finding Title]

- Severity:
- Affected component:
- Status:
- Evidence:
- Reproduction steps:
- Potential impact:
- Recommended remediation:
- Retest requirements:

## 7. Positive Security Controls

Document effective controls observed during testing.

## 8. Testing Limitations

Document components, features or attack paths that were not tested.

## 9. Remediation Plan

| Finding | Action | Owner | Target date |
|---|---|---|---|
| | | | |

## 10. Retest Results

Document whether remediation was successful.

## 11. Residual Risks

List unresolved risks and accepted exceptions.
```

Never include real passwords, private keys or unnecessary sensitive information in a security report.

## 20. Security Retesting

Retesting verifies whether a previously identified security weakness has been corrected.

### Retesting Process

1. Locate the original finding.
2. Review the remediation.
3. Reproduce the original test conditions.
4. Verify the vulnerability is no longer present.
5. Test relevant negative scenarios.
6. Check for regressions.
7. Record the result.

### Retest Outcomes

- Fixed: The original issue is no longer reproducible.
- Partially fixed: Some attack paths remain.
- Not fixed: The original issue remains.
- Regression: The fix introduced another problem.
- Inconclusive: Available evidence is insufficient.

A fix should not be marked successful solely because the affected code has changed.

## 21. Security Testing Checklist

### Assessment Preparation

- [ ] Testing authorization is documented.
- [ ] Testing scope is defined.
- [ ] Environment is identified.
- [ ] Test accounts are available.
- [ ] Sensitive operations are restricted.
- [ ] Relevant architecture documentation is reviewed.
- [ ] Testing limitations are documented.

### Application Security

- [ ] Authentication is tested.
- [ ] Authorization is tested.
- [ ] Input validation is tested.
- [ ] Output encoding is reviewed.
- [ ] Database query safety is reviewed.
- [ ] Session handling is tested.
- [ ] File handling is tested.
- [ ] Sensitive data exposure is checked.
- [ ] Business logic is reviewed.

### Automated Security Checks

- [ ] SAST is performed.
- [ ] Dependency scanning is performed.
- [ ] Secret scanning is performed.
- [ ] Configuration scanning is performed.
- [ ] Container scanning is performed where applicable.
- [ ] DAST is performed where appropriate.

### API and Mobile

- [ ] API authentication is tested.
- [ ] Object-level authorization is tested.
- [ ] Administrative operations are protected.
- [ ] Rate limits are reviewed.
- [ ] Mobile storage is reviewed.
- [ ] Mobile permissions are reviewed.
- [ ] Deep links and WebViews are assessed where applicable.

### Reporting and Remediation

- [ ] Findings have supporting evidence.
- [ ] False positives are reviewed.
- [ ] Severity is justified.
- [ ] Remediation recommendations are documented.
- [ ] Owners are assigned.
- [ ] Retesting is performed.
- [ ] Unresolved risks are recorded.

## 22. Common Security Testing Mistakes

| Mistake | Consequence |
|---|---|
| Testing without authorization | Legal and operational risk |
| Scanning production carelessly | Service disruption |
| Trusting scanner output blindly | Incorrect vulnerability reports |
| Ignoring manual testing | Missed business logic flaws |
| Testing only authenticated users | Missed unauthenticated attack paths |
| Ignoring authorization | Cross-user data exposure |
| Skipping retesting | Unverified remediation |
| Ignoring false negatives | Incomplete security assessment |
| Using real user data unnecessarily | Privacy exposure |
| Treating passing CI checks as proof | False confidence |
| Testing without defined scope | Uncontrolled assessment |

## 23. Recommended Security Testing Resources

### OWASP

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP API Security Project](https://owasp.org/www-project-api-security/)
- [OWASP Mobile Application Security Testing Guide](https://mas.owasp.org/MASTG/)
- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)

### Static and Dependency Analysis

- [Semgrep](https://semgrep.dev/)
- [CodeQL](https://codeql.github.com/)
- [SonarQube](https://www.sonarsource.com/products/sonarqube/)
- [OSV-Scanner](https://google.github.io/osv-scanner/)
- [Gitleaks](https://github.com/gitleaks/gitleaks)

### Dynamic Testing

- [OWASP ZAP](https://www.zaproxy.org/)
- [Burp Suite](https://portswigger.net/burp)
- [Nuclei](https://projectdiscovery.io/nuclei)
- [Wapiti](https://wapiti-scanner.github.io/)

### Mobile Testing

- [MobSF](https://mobsf.github.io/)
- [Frida](https://frida.re/)
- [OWASP MASVS](https://mas.owasp.org/MASVS/)

### Infrastructure Testing

- [Nmap](https://nmap.org/)
- [Lynis](https://cisofy.com/lynis/)
- [Trivy](https://trivy.dev/)
- [Checkov](https://www.checkov.io/)

---

Security testing reduces risk but cannot prove that an application is completely secure. Results depend on assessment scope, test coverage, tool capabilities and the quality of manual validation.

All security testing must be performed against systems that you own or are explicitly authorized to assess.
