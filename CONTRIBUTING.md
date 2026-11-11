
# Contributing to ShipSafe

Thank you for your interest in contributing to ShipSafe.

ShipSafe is an open-source application security knowledge base and toolkit designed to help developers understand, identify, prevent and reduce security risks throughout the software development lifecycle.

We welcome contributions from developers, security researchers, penetration testers, DevOps engineers, cloud engineers, technical writers, students and anyone interested in improving application security.

Our goal is to make practical security knowledge accessible, accurate, actionable and useful.

## 1. Our Mission

ShipSafe follows one principle:

**Build fast. Audit deeply. Ship securely.**

We aim to help developers build safer applications by providing:

- Practical application security guidance.
- Secure coding recommendations.
- Security review checklists.
- Threat modeling resources.
- Secure architecture patterns.
- Vulnerability prevention guidance.
- Security testing methodologies.
- Infrastructure and cloud security practices.
- Real-world security examples.
- Resources for AI-assisted software development.

Contributions should support this mission.

## 2. What Can You Contribute?

We welcome the following contributions.

### Security Documentation

Improve or add documentation covering:

- Authentication and authorization.
- API security.
- Web application security.
- Mobile application security.
- Database security.
- Cloud and infrastructure security.
- Cryptography.
- Secrets management.
- Dependency security.
- Threat modeling.
- Privacy and data protection.
- Logging and monitoring.
- Incident response.
- Secure development practices.
- Security testing.

### Checklists

Contribute practical security checklists for:

- Web applications.
- Mobile applications.
- REST and GraphQL APIs.
- Backend services.
- Cloud deployments.
- Databases.
- Authentication systems.
- Production readiness.
- AI-powered applications.

Checklists should contain actionable verification items rather than vague statements.

### Code Examples

We welcome secure implementation examples in languages and frameworks such as:

- JavaScript and TypeScript.
- Python.
- Go.
- Java and Kotlin.
- Swift.
- PHP.
- Rust.
- SQL.
- Shell scripting.

Examples should demonstrate secure implementation patterns and explain relevant limitations.

Never include real credentials, private keys, production secrets or sensitive personal information.

### Security Tools and Resources

You can contribute:

- Useful open-source security tools.
- Security testing references.
- Developer documentation.
- Standards and frameworks.
- Vulnerability databases.
- Learning resources.
- Secure development utilities.

Explain what each tool does, its limitations and when developers should use it.

### Bug Reports and Improvements

You can also report:

- Incorrect security guidance.
- Outdated documentation.
- Broken links.
- Missing security topics.
- Incomplete checklists.
- Typographical errors.
- Unclear explanations.
- Inconsistent recommendations.

## 3. Before You Start

Before making a contribution:

1. Read the project [README](README.md).
2. Review the [project structure](STRUCTURE.md).
3. Check the relevant documentation under `security/`.
4. Search existing issues and pull requests to avoid duplicate work.
5. Open an issue when proposing a major new topic or structural change.

For small documentation corrections, a pull request can be submitted directly.

## 4. Repository Structure

ShipSafe uses an organized documentation structure.

```text
shipsafe/
│
├── README.md
├── STRUCTURE.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── LICENSE
│
├── security/
│   ├── authentication/
│   ├── authorization/
│   ├── api-security/
│   ├── database-security/
│   ├── web-security/
│   ├── mobile-security/
│   ├── secrets-management/
│   ├── dependencies/
│   ├── infrastructure/
│   ├── threat-modeling/
│   ├── secure-development/
│   ├── security-testing/
│   ├── cloud-security/
│   ├── logging-monitoring/
│   ├── incident-response/
│   ├── privacy-data-protection/
│   └── cryptography/
│
├── checklists/
├── docs/
├── examples/
├── tools/
└── .github/
```

Follow the existing structure when adding documentation.

If a contribution requires a new directory, explain why it is necessary and update `STRUCTURE.md`.

## 5. Documentation Standards

All documentation should be:

- Technically accurate.
- Clearly written.
- Practical and actionable.
- Organized with meaningful headings.
- Appropriate for the intended audience.
- Consistent with existing documentation.
- Supported by trustworthy references where possible.

### Recommended Documentation Format

Use this general structure when creating a new security guide:

1. Overview
2. Why the topic matters
3. Common security risks
4. Attack scenarios and examples
5. Prevention and mitigation
6. Secure implementation guidance
7. Testing and verification
8. Common mistakes
9. Practical checklist
10. References and further reading
11. Disclaimer

Not every document requires every section. Adapt the structure to the subject.

### Writing Guidelines

- Use descriptive headings.
- Prefer short paragraphs.
- Explain technical terms when first introduced.
- Use code blocks for implementation examples.
- Use tables when they improve clarity.
- Include practical examples where useful.
- Avoid unsupported security claims.
- Distinguish recommendations from mandatory requirements.
- Explain important limitations and assumptions.
- Avoid copying large sections from external documentation.
- Cite original and authoritative sources.

### Markdown Guidelines

- Use standard Markdown.
- Use fenced code blocks with language identifiers.
- Use relative links for files within this repository.
- Keep file and directory names lowercase and descriptive.
- Avoid unnecessary duplication.
- Check links before submitting a pull request.

## 6. Technical Accuracy and Security Quality

Security guidance must be reviewed carefully.

Contributors should:

- Verify security recommendations against authoritative documentation.
- Prefer primary sources such as OWASP, NIST, IETF, official platform documentation and recognized standards.
- Include relevant version information when behavior depends on a particular release.
- Explain security trade-offs.
- Identify assumptions and limitations.
- Avoid presenting theoretical protections as guaranteed security.

Examples of important distinctions:

- CORS is not an authentication or authorization mechanism.
- UUIDs do not prevent insecure direct object references.
- Using an ORM does not automatically eliminate injection vulnerabilities.
- Environment variables are not a complete secret-management solution.
- A client-side permission check is not a substitute for server-side authorization.
- Passing an automated scan does not prove that an application is secure.

If guidance is uncertain, document the uncertainty instead of presenting assumptions as facts.

## 7. Secure Code Examples

Code examples should prioritize safety and clarity.

Every security-sensitive example should:

- Use safe defaults.
- Validate untrusted input.
- Enforce authorization on the server.
- Avoid hardcoded credentials.
- Use maintained libraries.
- Handle errors safely.
- Avoid leaking sensitive information.
- Explain configuration requirements.
- Identify relevant limitations.

Do not submit:

- Real API keys or passwords.
- Private keys or certificates.
- Production database credentials.
- Personal information.
- Live customer data.
- Unredacted security incident evidence.

Use clearly labeled placeholders for sensitive values.

## 8. AI-Assisted Contributions

ShipSafe welcomes contributions created with AI coding agents and AI writing assistants.

AI assistance does not remove the contributor's responsibility for correctness.

If you use AI to prepare a contribution:

- Review the complete generated content.
- Verify technical claims independently.
- Test code examples where practical.
- Check referenced documentation.
- Remove fabricated or unsupported references.
- Review generated code for insecure patterns.
- Ensure no private project data or credentials are exposed.
- Confirm that the contribution follows the project's standards.

Contributors remain responsible for the content they submit.

## 9. Responsible Security Research

ShipSafe supports responsible security research and education.

All security testing must be performed only on systems you own or have explicit authorization to assess.

Do not submit contributions that:

- Target real systems without authorization.
- Expose private user information.
- Publish live credentials or secrets.
- Include destructive exploit instructions targeting third parties.
- Encourage unauthorized access.
- Provide malware or credential theft functionality.
- Disclose unpatched vulnerabilities without appropriate coordination.

For security vulnerabilities affecting ShipSafe itself, follow [SECURITY.md](SECURITY.md).

## 10. Contribution Workflow

### Step 1: Fork the Repository

Fork the official repository:

https://github.com/rakibuli10/shipsafe

### Step 2: Clone Your Fork

```bash
git clone https://github.com/YOUR-USERNAME/shipsafe.git
cd shipsafe
```

### Step 3: Create a Branch

Use a descriptive branch name.

```bash
git checkout -b docs/improve-api-security
```

Other examples:

```text
docs/add-authentication-guide
fix/correct-security-reference
checklist/add-cloud-security
examples/secure-file-upload
docs/update-cryptography
```

### Step 4: Make Your Changes

- Follow the repository structure.
- Keep changes focused.
- Use clear file names.
- Update related documentation where necessary.
- Add references for technical claims.
- Avoid unrelated changes.

### Step 5: Review Your Changes

Before committing:

```bash
git status
git diff --check
git diff
```

Check that:

- No secrets were added.
- Markdown formatting is correct.
- Internal links work.
- Technical statements are accurate.
- Examples are safe.
- Existing content has not been unintentionally removed.

### Step 6: Commit Your Changes

Use clear commit messages.

```bash
git add .
git commit -m "docs: improve API security guidance"
```

Suggested commit prefixes:

| Prefix | Purpose |
|---|---|
| docs | Documentation changes |
| fix | Corrections and bug fixes |
| feat | New content or functionality |
| security | Security-related improvements |
| test | Testing improvements |
| refactor | Structural improvements |
| chore | Maintenance |

### Step 7: Push Your Branch

```bash
git push origin docs/improve-api-security
```

### Step 8: Open a Pull Request

Open a pull request against the official ShipSafe repository.

Include:

- A clear title.
- A summary of the changes.
- The reason for the contribution.
- Related issue numbers, if applicable.
- Sources and references.
- Testing or verification performed.
- Any limitations or outstanding concerns.

## 11. Pull Request Guidelines

Every pull request should:

- Have a descriptive title.
- Address a specific improvement.
- Follow the project's documentation standards.
- Avoid unrelated modifications.
- Include appropriate references.
- Contain no exposed secrets or private information.
- Be reviewed before merging.

A maintainer may request changes, additional evidence, clarification or revisions before approving a contribution.

Not every submitted contribution is guaranteed to be accepted.

## 12. Reporting Documentation Issues

If you discover an incorrect or potentially dangerous security recommendation:

1. Identify the affected file.
2. Explain the incorrect statement.
3. Provide authoritative supporting references.
4. Suggest a correction where possible.
5. Open an issue or submit a pull request.

Security accuracy takes priority over preserving existing documentation.

## 13. Review and Maintenance

Contributions may be reviewed for:

- Technical correctness.
- Security implications.
- Clarity and readability.
- Source reliability.
- Duplication.
- Compatibility with the repository structure.
- Long-term maintenance.

Documentation may be revised as technologies, standards and security practices evolve.

## 14. License and Contribution Terms

By submitting a contribution, you confirm that:

- You have the right to submit the contributed material.
- Your contribution does not knowingly violate another party's intellectual property rights.
- You have removed private information and secrets.
- You understand that accepted contributions will be distributed under the applicable repository license.

Review the repository's [LICENSE](LICENSE) before contributing.

## 15. Code of Conduct

All contributors are expected to follow the project's [Code of Conduct](CODE_OF_CONDUCT.md).

We want ShipSafe to remain a respectful, welcoming and collaborative open-source project.

## 16. Questions and Community

For questions, suggestions and project discussions, use the repository's GitHub Issues and Discussions where available.

For security-sensitive reports, do not publish confidential vulnerability details in a public issue. Follow the instructions in [SECURITY.md](SECURITY.md).

## Final Note

ShipSafe is built through shared knowledge, careful engineering and responsible collaboration.

Every accurate explanation, practical checklist, reviewed example and meaningful correction helps developers build safer software.

**Build fast. Audit deeply. Ship securely.**
