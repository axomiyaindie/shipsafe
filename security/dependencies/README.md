
# Dependency Security

Dependency security focuses on identifying, evaluating and managing security risks introduced by external libraries, frameworks, packages, plugins, tools and other software components used by an application.

Modern applications rely heavily on third-party dependencies. A vulnerability or malicious update in even one package can affect the security of the entire application.

Dependency security is an essential part of application security, software supply-chain protection and secure software development.

This guide explains how to identify vulnerable dependencies, reduce supply-chain risks, audit packages and maintain a secure dependency lifecycle.

## 1. Understanding Software Dependencies

A software dependency is an external component that an application relies on to provide functionality.

### Common Dependency Types

| Dependency type | Examples |
|---|---|
| JavaScript packages | React, Next.js, Express |
| Python packages | Django, Flask, Requests |
| Java libraries | Spring, Apache Commons |
| PHP packages | Laravel packages, Composer libraries |
| Ruby gems | Rails dependencies |
| Mobile libraries | Android and iOS SDK dependencies |
| Native libraries | OpenSSL, SQLite |
| Development tools | Build systems, compilers, testing frameworks |
| CI/CD actions | GitHub Actions and deployment integrations |
| Container images | Base operating system and application images |

Dependencies may be direct or transitive.

- **Direct dependencies:** Packages explicitly included by an application.
- **Transitive dependencies:** Packages required by other dependencies.

A project can therefore contain vulnerable packages that developers never intentionally installed.

## 2. Understanding Dependency Risks

Third-party dependencies can introduce security weaknesses through vulnerable code, malicious updates, compromised maintainers or insecure package distribution.

### Common Risks

- Known vulnerabilities.
- Malicious packages.
- Typosquatting.
- Dependency confusion.
- Compromised maintainer accounts.
- Hijacked package versions.
- Abandoned or unmaintained packages.
- Insecure installation scripts.
- Untrusted package registries.
- Vulnerable transitive dependencies.
- Compromised build tools.
- Malicious CI/CD actions.

### Potential Consequences

- Remote code execution.
- Unauthorized data access.
- Credential theft.
- Server compromise.
- Application crashes.
- Supply-chain compromise.
- Unauthorized software modification.
- Lateral movement into infrastructure.

Dependency security must consider the complete software supply chain, not only the application's source code.

## 3. Direct and Transitive Dependency Auditing

Every application should maintain visibility into its dependency tree.

### Direct Dependency Review

Review packages explicitly declared in:

- `package.json`
- `requirements.txt`
- `pyproject.toml`
- `Pipfile`
- `pom.xml`
- `build.gradle`
- `composer.json`
- `Gemfile`
- Other ecosystem-specific manifests.

### Transitive Dependency Review

A dependency can introduce another package with a known vulnerability.

For example:

```text
Application
|
+-- Framework
|   |
|   +-- HTTP Client
|   |
|   +-- Utility Library
|
+-- Authentication Package
    |
    +-- Cryptography Library
```

Reviewing only the direct dependencies can miss vulnerabilities introduced by nested packages.

### Security Recommendations

- Inspect direct and transitive dependencies.
- Identify package ownership and maintainers.
- Review package maintenance activity.
- Check security advisories.
- Remove unnecessary dependencies.
- Monitor dependency changes.
- Investigate unexpected package additions.

## 4. Package Managers and Ecosystems

Different programming languages use different package management systems.

### JavaScript and Node.js

Common tools:

- npm
- pnpm
- Yarn

Important files:

```text
package.json
package-lock.json
pnpm-lock.yaml
yarn.lock
```

Audit dependencies:

```bash
npm audit
```

Review package metadata:

```bash
npm outdated
```

Install dependencies using the committed lockfile:

```bash
npm ci
```

`npm ci` is designed for clean, reproducible installation in automated environments when a compatible lockfile exists.

### Python

Common tools:

- pip
- Poetry
- uv
- Pipenv

Important files:

```text
requirements.txt
pyproject.toml
poetry.lock
uv.lock
Pipfile.lock
```

Review installed packages:

```bash
python -m pip list
```

Inspect package dependency relationships:

```bash
python -m pip show PACKAGE_NAME
```

Use a suitable vulnerability auditing tool, such as:

- pip-audit
- Safety

Example:

```bash
pip-audit
```

### Other Ecosystems

| Ecosystem | Common package management files |
|---|---|
| Java | pom.xml, build.gradle |
| PHP | composer.json, composer.lock |
| Ruby | Gemfile, Gemfile.lock |
| .NET | .csproj, packages.lock.json |
| Rust | Cargo.toml, Cargo.lock |
| Go | go.mod, go.sum |
| Swift | Package.swift, Package.resolved |

Use ecosystem-specific auditing tools and security advisories.

## 5. Lockfiles and Reproducible Builds

Lockfiles record the resolved versions of dependencies and, depending on the ecosystem, additional integrity information.

### Why Lockfiles Matter

Without a lockfile, dependency resolution can produce different package versions across machines or deployment environments.

This can introduce:

- Unexpected dependency changes.
- Unreviewed package updates.
- Inconsistent builds.
- Difficult vulnerability investigations.
- Unintended production changes.

### Security Recommendations

- Commit the appropriate application lockfile.
- Use reproducible installation commands.
- Review lockfile changes in pull requests.
- Avoid manually editing lockfiles unless necessary.
- Regenerate lockfiles using the intended package manager.
- Verify package integrity where supported.
- Test dependency updates before deployment.

Examples:

```bash
npm ci
```

```bash
pnpm install --frozen-lockfile
```

```bash
yarn install --immutable
```

Use the command appropriate for the project's package manager and version.

A lockfile improves reproducibility but does not guarantee that every dependency is safe.

## 6. Vulnerability Databases and Security Advisories

Dependency scanners use vulnerability databases and security advisories to identify known weaknesses.

### Important Resources

- [National Vulnerability Database (NVD)](https://nvd.nist.gov/)
- [CVE Program](https://www.cve.org/)
- [GitHub Advisory Database](https://github.com/advisories)
- [OSV Database](https://osv.dev/)
- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
- [Snyk Vulnerability Database](https://security.snyk.io/)

### Understanding Vulnerability Information

A vulnerability record may include:

- CVE identifier.
- Affected package.
- Vulnerable version ranges.
- Patched versions.
- Severity rating.
- Attack conditions.
- Potential impact.
- Available remediation.

### Important Considerations

A vulnerability scanner may produce:

- Confirmed matches.
- Potentially vulnerable packages.
- Unmatched or ambiguous versions.
- False positives.
- Findings requiring manual investigation.

A reported vulnerability does not automatically mean the application is exploitable.

Assess whether the affected package is present, reachable, configured and used in a vulnerable way.

## 7. Malicious Packages and Typosquatting

Attackers sometimes publish malicious packages that imitate legitimate software.

### Common Techniques

**Typosquatting**

An attacker creates a package with a name similar to a popular dependency.

**Dependency Confusion**

An attacker exploits package resolution behavior involving internal and public package registries.

**Account Compromise**

An attacker takes control of a legitimate maintainer account and publishes a malicious release.

**Malicious Updates**

A previously legitimate package receives a harmful version.

**Install Script Abuse**

A package executes unexpected commands during installation.

### Security Recommendations

- Verify package names before installation.
- Prefer official package maintainers.
- Review package repositories and release history.
- Investigate unexpected dependencies.
- Review installation scripts.
- Use trusted package registries.
- Restrict access to private registries.
- Monitor unusual package changes.
- Avoid installing packages simply because an AI agent recommends them.

A package's download count, popularity or presence in a public registry is not proof of safety.

## 8. Dependency Selection and Maintenance

Selecting a dependency is a long-term maintenance decision.

### Evaluate Before Adding a Package

Review:

- Project activity.
- Maintainer reputation.
- Release history.
- Security advisories.
- Documentation quality.
- License compatibility.
- Dependency count.
- Package ownership.
- Community support.
- Compatibility with the application.

### Prefer Dependencies That

- Have an identifiable maintainer.
- Receive appropriate security updates.
- Provide clear documentation.
- Follow responsible disclosure practices.
- Avoid unnecessary transitive dependencies.
- Have an established release process.

### Avoid

- Abandoned packages without a maintenance plan.
- Unverified package copies.
- Suspicious installation behavior.
- Unnecessary dependencies for trivial functionality.
- Packages with unresolved critical security issues.

Do not automatically assume that a smaller package is safer than a larger one. Evaluate the actual risk and maintenance requirements.

## 9. Dependency Update Strategy

Keeping dependencies updated reduces exposure to known vulnerabilities and unsupported software.

### Recommended Update Categories

| Update type | Recommended approach |
|---|---|
| Critical security fix | Prioritize immediate assessment and remediation |
| High-severity security fix | Assess promptly and schedule remediation |
| Minor update | Review changes and run regression tests |
| Major update | Review breaking changes and migration requirements |
| Abandoned dependency | Evaluate replacement or mitigation |

### Security Recommendations

- Establish a regular dependency review process.
- Prioritize security updates.
- Review release notes.
- Check compatibility.
- Test application behavior.
- Update lockfiles.
- Verify the deployed version.
- Document exceptions and accepted risks.

Avoid blindly updating every dependency to its newest version without reviewing the changes.

## 10. Automated Dependency Scanning

Automated tools help identify vulnerable packages during development and CI/CD.

### Useful Tools

| Tool | Purpose |
|---|---|
| npm audit | npm dependency vulnerability checks |
| pip-audit | Python package vulnerability checks |
| OSV-Scanner | Vulnerability scanning using OSV |
| Trivy | Vulnerability and configuration scanning |
| OWASP Dependency-Check | Software composition analysis |
| Snyk | Dependency and application security analysis |
| Dependabot | Dependency updates and security alerts |
| Renovate | Automated dependency update management |

### Security Recommendations

- Scan direct and transitive dependencies.
- Run scans regularly.
- Scan pull requests where practical.
- Scan production dependency lockfiles.
- Include container dependencies when relevant.
- Review scanner output.
- Track remediation progress.
- Retain scan results where useful.

Automated scanners identify known risks; they do not prove that an application has no dependency vulnerabilities.

## 11. GitHub Dependabot

Dependabot can monitor supported dependency manifests and create pull requests for dependency updates.

### Useful Capabilities

- Dependency version updates.
- Security update pull requests.
- Vulnerability alerts.
- Automated update workflows.

### Example Configuration

Create:

`.github/dependabot.yml`

```yaml
version: 2

updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

This is an example configuration. Add only ecosystems and directories that actually exist in the repository.

### Security Recommendations

- Enable appropriate repository security features.
- Review generated pull requests.
- Run tests before merging.
- Review major version updates carefully.
- Configure update groups where useful.
- Monitor security alerts.
- Avoid automatically merging high-impact changes without appropriate verification.

Automated update pull requests still require review and validation.

## 12. Software Bill of Materials (SBOM)

A Software Bill of Materials is a structured inventory of software components included in an application or software artifact.

An SBOM can help organizations understand what software they use and identify components affected by newly disclosed vulnerabilities.

### Common SBOM Formats

- SPDX.
- CycloneDX.

### Useful Tools

- [Syft](https://github.com/anchore/syft)
- [CycloneDX](https://cyclonedx.org/)
- [SPDX](https://spdx.dev/)

### Example: Generate an SBOM with Syft

After installing Syft using its official instructions:

```bash
syft . -o cyclonedx-json > sbom.json
```

### Security Recommendations

- Generate SBOMs for important application releases.
- Include relevant direct and transitive dependencies.
- Keep SBOMs associated with the corresponding release.
- Protect sensitive internal software inventory information.
- Update SBOMs when dependencies change.
- Use SBOMs during vulnerability investigations.

An SBOM provides component visibility. It does not independently prove that listed components are secure.

## 13. Container and Base Image Security

Applications built with Docker or other container technologies inherit risks from their base images and installed packages.

### Common Risks

- Outdated operating system packages.
- Vulnerable system libraries.
- Untrusted base images.
- Excessive packages.
- Unpinned image tags.
- Insecure build dependencies.
- Compromised container registries.

### Security Recommendations

- Prefer trusted and maintained base images.
- Use supported image versions.
- Pin images to verified digests where appropriate.
- Minimize unnecessary packages.
- Scan images before release.
- Rebuild images to incorporate security fixes.
- Review image provenance.
- Avoid running containers with unnecessary privileges.

### Example: Trivy Image Scan

```bash
trivy image YOUR_IMAGE_NAME
```

Review findings and confirm whether the affected components are actually present in the deployed image.

## 14. CI/CD and Software Supply-Chain Security

Dependency security must extend to automated build and deployment systems.

### Security Recommendations

- Use trusted package registries.
- Restrict package publishing permissions.
- Protect CI/CD credentials.
- Review third-party build actions.
- Pin CI/CD actions to verified commit SHAs.
- Use dependency lockfiles.
- Scan dependencies before release.
- Protect build artifacts.
- Verify software provenance where supported.
- Restrict access to production deployment workflows.

### Build Integrity

A secure application requires more than secure source code.

The build environment, package registry, dependency resolution process and release artifacts must also be protected.

## 15. AI-Assisted Development and Dependency Risks

AI coding agents can generate code and recommend external packages.

However, generated dependencies may be unnecessary, outdated, incorrectly named or vulnerable.

### Common Risks

- Installing an unrelated package with a similar name.
- Introducing unnecessary dependencies.
- Using outdated package versions.
- Copying vulnerable code from examples.
- Trusting unverified installation commands.
- Adding packages without reviewing their maintainers.
- Updating dependency versions without testing.
- Introducing malicious packages through generated instructions.

### Security Recommendations

- Review every newly introduced package.
- Verify package names and official documentation.
- Check package maintenance and security history.
- Prefer existing project dependencies where appropriate.
- Avoid unnecessary packages.
- Review generated lockfile changes.
- Run dependency audits after significant code generation.
- Require human approval for sensitive dependency changes.
- Restrict AI agents from publishing packages without authorization.

AI-generated code and dependency choices should be treated as untrusted until reviewed.

## 16. License and Legal Review

Dependency management also involves software licensing.

Different packages may have different distribution, modification and attribution requirements.

### Security and Maintenance Recommendations

- Review dependency licenses.
- Identify incompatible licenses.
- Track license changes.
- Maintain required notices.
- Review transitive dependency licenses.
- Consult legal expertise when distribution obligations are unclear.

Security approval does not automatically imply license approval.

## 17. Dependency Security Checklist

### Package Management

- [ ] Dependency manifests are maintained.
- [ ] Appropriate lockfiles are committed.
- [ ] Direct and transitive dependencies are reviewed.
- [ ] Unnecessary dependencies are removed.
- [ ] Package names and maintainers are verified.
- [ ] Package sources are trusted.

### Vulnerability Management

- [ ] Automated dependency scanning is configured.
- [ ] Vulnerability alerts are monitored.
- [ ] Critical findings receive priority.
- [ ] Vulnerable versions are identified.
- [ ] Fixes are tested before deployment.
- [ ] Unresolved risks are documented.

### Supply-Chain Security

- [ ] Installation scripts are reviewed when appropriate.
- [ ] Private registry access is restricted.
- [ ] Build dependencies are monitored.
- [ ] CI/CD actions are reviewed and pinned appropriately.
- [ ] Container images are scanned.
- [ ] Production build artifacts are protected.

### Maintenance

- [ ] Dependency update procedures are documented.
- [ ] Unsupported packages are identified.
- [ ] Security updates are prioritized.
- [ ] SBOM generation is considered.
- [ ] License requirements are reviewed.
- [ ] AI-generated dependencies receive human review.

## 18. Common Dependency Security Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Known vulnerable dependency | Exploitation of published weaknesses |
| Malicious package | Unauthorized code execution |
| Typosquatting | Installation of attacker-controlled software |
| Dependency confusion | Execution of unintended packages |
| Compromised maintainer account | Malicious legitimate-looking updates |
| Unpinned dependencies | Unexpected version changes |
| Abandoned dependencies | Unpatched vulnerabilities |
| Insecure CI/CD actions | Build and deployment compromise |
| Vulnerable container image | Runtime compromise |
| Missing dependency monitoring | Delayed vulnerability discovery |

## 19. Remediation Priorities

When dependency security weaknesses are discovered:

1. Identify affected packages and versions.
2. Determine whether the vulnerable code is present and reachable.
3. Check official security advisories.
4. Upgrade to a fixed version when available.
5. Remove malicious or unnecessary dependencies.
6. Review transitive dependency changes.
7. Test the application after remediation.
8. Rebuild affected application and container artifacts.
9. Review whether credentials or data may have been exposed.
10. Record the remediation and verify the deployed version.

If a package is confirmed malicious, investigate the affected environment and any code or credentials it could access. A normal version upgrade may not be sufficient.

## 20. References and Further Reading

### OWASP

- [OWASP Software Supply Chain Security](https://owasp.org/www-project-software-supply-chain-security/)
- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
- [OWASP CycloneDX](https://owasp.org/www-project-cyclonedx/)

### Vulnerability Databases

- [National Vulnerability Database](https://nvd.nist.gov/)
- [CVE Program](https://www.cve.org/)
- [GitHub Advisory Database](https://github.com/advisories)
- [Open Source Vulnerabilities Database](https://osv.dev/)

### Dependency Security Tools

- [Gitleaks](https://github.com/gitleaks/gitleaks)
- [OSV-Scanner](https://github.com/google/osv-scanner)
- [Trivy](https://github.com/aquasecurity/trivy)
- [Syft](https://github.com/anchore/syft)
- [Dependabot](https://docs.github.com/en/code-security/dependabot)
- [Renovate](https://docs.renovatebot.com/)

### Ecosystem Documentation

- [npm Audit](https://docs.npmjs.com/cli/commands/npm-audit)
- [pip-audit](https://github.com/pypa/pip-audit)
- [GitHub Dependency Graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)

---

This guide is educational and does not replace application-specific dependency audits, software supply-chain assessments or professional security reviews.
