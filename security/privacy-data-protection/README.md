
# Privacy and Data Protection

Privacy and data protection are fundamental parts of application security.

Modern applications collect and process information about users, customers, employees and other individuals. This information may include names, contact details, account identifiers, location data, financial information, health information, private communications and behavioral data.

Improper collection, storage, processing or sharing of personal information can expose individuals to identity theft, fraud, discrimination, unwanted surveillance and other forms of harm.

Developers must therefore consider privacy throughout the application lifecycle, from initial product design to deployment, maintenance and eventual data deletion.

This guide explains practical privacy and data protection principles for web applications, mobile applications, APIs, databases, cloud services and AI-powered systems.

## 1. Understanding Privacy and Data Protection

Privacy concerns how information about individuals is collected, used, accessed, shared and controlled.

Data protection refers to the technical, organizational and procedural safeguards used to protect information against unauthorized access, disclosure, alteration, loss or destruction.

### Privacy vs. Security

| Concept | Focus |
|---|---|
| Privacy | Appropriate and lawful handling of information about individuals |
| Data protection | Safeguarding personal information throughout its lifecycle |
| Information security | Protecting information and systems against security threats |
| Confidentiality | Preventing unauthorized disclosure |
| Data governance | Managing data ownership, quality, access and lifecycle |

Security supports privacy, but security alone does not guarantee privacy.

For example, an application may securely store every user interaction while collecting far more personal information than necessary. That may still create significant privacy risks.

### Privacy Should Be Considered From the Beginning

Privacy should not be treated as a feature added immediately before launch.

It should influence:

- Product requirements.
- Application architecture.
- Database design.
- User interface design.
- Authentication and authorization.
- Analytics and tracking.
- Third-party integrations.
- Data retention.
- Application monitoring.
- AI functionality.

## 2. Types of Personal and Sensitive Data

Applications may process several categories of information.

### Personal Information

Information that identifies or can reasonably be linked to an individual.

Examples:

- Full name.
- Email address.
- Phone number.
- Postal address.
- Account identifier.
- Identification number.
- Online identifiers.
- Location information.

Whether a particular identifier is personal data depends on context and applicable law.

### Sensitive Personal Information

Some information can create greater harm if exposed or misused.

Examples include:

- Health information.
- Financial information.
- Biometric information.
- Authentication credentials.
- Precise location information.
- Private communications.
- Information about children.

Legal definitions and additional protections differ across jurisdictions.

### Technical and Behavioral Data

Applications may also collect:

- IP addresses.
- Device identifiers.
- Browser information.
- Session identifiers.
- Usage patterns.
- Search history.
- Application activity.
- Crash reports.
- Diagnostic information.

Technical data can become personal data when it identifies or can reasonably be linked to an individual.

### Data Classification

A practical classification system can help teams apply appropriate controls.

| Classification | Example | Handling |
|---|---|---|
| Public | Public documentation | Normal publication controls |
| Internal | Internal operational information | Restrict internal access |
| Confidential | Customer records | Strong access controls |
| Highly sensitive | Authentication secrets or sensitive personal records | Strict access, encryption and monitoring |

Classification should reflect actual risks, organizational policy and legal requirements.

## 3. Privacy Principles

Privacy programs commonly rely on several foundational principles.

### Data Minimization

Collect only the information necessary for a defined and legitimate purpose.

For example, a basic newsletter subscription may require an email address but not a residential address, date of birth or government identification number.

### Purpose Limitation

Use information for clearly defined purposes.

Do not automatically reuse collected information for unrelated activities.

### Transparency

Explain what information is collected, why it is needed, how it is used, who receives it and how long it is retained.

### Accuracy

Take reasonable steps to keep personal information accurate and correct when appropriate.

### Storage Limitation

Retain information only for as long as necessary for the relevant purpose or applicable legal obligations.

### Confidentiality and Integrity

Protect information from unauthorized access, modification, disclosure and destruction.

### Accountability

Organizations should be able to demonstrate how they manage privacy obligations and implement appropriate safeguards.

### Privacy by Design

Build privacy considerations into system architecture and development decisions rather than relying only on policies.

## 4. Privacy by Design and Default

Privacy by design means considering privacy throughout the development process.

Privacy by default means that the default application settings should provide an appropriate level of privacy without requiring users to discover and disable unnecessary data collection.

### Practical Implementation

- Collect fewer fields during registration.
- Make optional information genuinely optional.
- Use private defaults for user-generated content.
- Restrict access to personal records.
- Avoid public exposure of user profiles by default.
- Provide clear privacy controls.
- Minimize third-party tracking.
- Apply retention limits.
- Protect sensitive information in logs.
- Avoid unnecessary data replication.

### Example: Registration Form

A basic account may require:

- Email address.
- Password or supported authentication method.

Additional information such as date of birth, phone number or profile details should have a clearly justified purpose.

Do not collect information simply because it may become useful someday.

### Privacy Review Questions

- What personal information does this feature collect?
- Why is each field necessary?
- Can the feature work with less information?
- Who can access the information?
- Is the information shared externally?
- How long is it retained?
- How can users exercise applicable rights?
- What happens when an account is deleted?

## 5. Data Mapping and Inventory

A data inventory identifies the personal information an application processes and where it travels.

Without a data inventory, teams may overlook information stored in logs, analytics platforms, backups or third-party services.

### What to Document

| Field | Description |
|---|---|
| Data category | Type of information |
| Collection source | Where information comes from |
| Purpose | Why it is processed |
| Storage location | Database, object storage or service |
| Access roles | Who can access it |
| Recipients | External parties receiving it |
| Retention | How long it is kept |
| Protection | Encryption and access controls |
| Deletion | How it is removed |

### Example Data Inventory

| Data | Purpose | Storage | Access |
|---|---|---|---|
| Email address | Account communication | User database | Authorized application services |
| Password hash | Authentication | Authentication store | Authentication service |
| Profile image | User profile | Object storage | Controlled application access |
| Session identifier | Session management | Session store | Authentication service |
| Diagnostic event | Troubleshooting | Monitoring platform | Authorized operators |

These examples are illustrative. Actual storage and access depend on application architecture.

### Data Flow Mapping

Map how information moves between components.

```text
User
 |
 v
Application Interface
 |
 v
Backend API
 |
 +-------> Authentication Provider
 |
 +-------> Application Database
 |
 +-------> Object Storage
 |
 +-------> Analytics Provider
 |
 +-------> Email Service
 |
 v
Logging and Monitoring
```

For every connection, determine:

- Which information is transferred?
- Why is it transferred?
- Is the recipient authorized?
- Is the transfer protected?
- Is the information retained?
- Can the transfer be disabled?
- Are users appropriately informed?

## 6. Lawful and Appropriate Data Collection

Applications should have a legitimate and documented reason for collecting and processing personal information.

The applicable legal basis depends on the jurisdiction, processing activity and relationship with the individual.

### Common Privacy Concepts

Depending on the applicable law, processing may involve:

- Consent.
- Contractual necessity.
- Legal obligations.
- Legitimate interests or comparable lawful grounds.
- Other legally recognized grounds.

Do not assume that consent is always required or that consent automatically makes every processing activity lawful.

### Consent Design

When consent is the appropriate legal basis:

- Explain what the user is agreeing to.
- Identify the relevant processing purpose.
- Avoid misleading language.
- Avoid preselected optional consent.
- Make withdrawal practical.
- Keep records of consent where required.
- Do not combine unrelated purposes unnecessarily.

### Example Consent Interface

A privacy-conscious interface might distinguish:

- Required account processing.
- Optional product analytics.
- Optional marketing communication.
- Optional personalized recommendations.

Do not make unrelated optional processing appear mandatory for basic account functionality.

### Important Principle

Consent must be meaningful under the applicable legal framework. A button click alone does not establish that all legal requirements have been met.

## 7. Authentication and Privacy

Authentication protects user accounts, but authentication systems also process sensitive information.

### Password Handling

- Never store plaintext passwords.
- Use appropriate password hashing algorithms.
- Avoid logging passwords.
- Protect password reset mechanisms.
- Limit authentication attempts.
- Use MFA where appropriate.
- Secure credential recovery.

### Session Privacy

- Use secure session identifiers.
- Avoid placing session secrets in URLs.
- Apply suitable expiration policies.
- Revoke sessions when appropriate.
- Protect session storage.
- Avoid exposing session information through analytics.

### User Enumeration

Authentication and recovery flows should avoid unnecessarily revealing whether an account exists.

For example, a password reset response should not disclose private account registration details to an unauthorized requester.

### Authentication Provider Considerations

When using external authentication services:

- Understand what account information they process.
- Review their privacy documentation.
- Configure data collection appropriately.
- Restrict administrative access.
- Review account deletion behavior.
- Understand session and identity data retention.

Reference: `../authentication/README.md`

## 8. Authorization and Personal Data Access

Personal information should only be accessible to identities with an appropriate authorization decision.

### Access Control Requirements

- Enforce authorization on the server.
- Restrict access by role and resource ownership.
- Apply tenant isolation.
- Use least privilege.
- Review administrator permissions.
- Audit sensitive operations.
- Revoke unnecessary access.

### Common Privacy Failures

**Broken object-level authorization**

A user can access another user's private record by changing a resource identifier.

**Broken function-level authorization**

A normal user can access an administrative operation.

**Excessive data exposure**

An API returns private fields that the current user does not need.

**Cross-tenant data exposure**

A tenant can access another tenant's information.

### Practical Example

An endpoint returning a profile should expose only the fields necessary for the requested operation.

Do not return internal account metadata, recovery information, private identifiers or unrelated personal records merely because they exist in the database.

### Important Principle

Hiding a user interface element does not provide data protection. Authorization must be enforced at the appropriate backend boundary.

Reference: `../authorization/README.md`

## 9. Secure Data Storage

Personal information should be stored in systems with appropriate security controls.

### Database Security

- Restrict database network access.
- Use dedicated database identities.
- Apply least privilege.
- Use parameterized queries.
- Enable appropriate audit logging.
- Encrypt sensitive data where appropriate.
- Protect backups.
- Review database permissions.
- Monitor unusual access.

### Object Storage

Applications often store:

- Profile pictures.
- Documents.
- Uploaded files.
- Reports.
- Identity documents.
- Private media.

Apply:

- Private storage by default.
- Restricted access policies.
- Short-lived signed URLs where appropriate.
- Upload validation.
- File access authorization.
- Retention and deletion policies.
- Protection against public bucket exposure.

### Backups

Backups are part of the personal data lifecycle.

Consider:

- Backup encryption.
- Access restrictions.
- Retention periods.
- Geographic storage.
- Restoration procedures.
- Deletion limitations.
- Backup expiration.

Deleting a record from the primary database does not necessarily delete it immediately from backups.

Document how backup retention and eventual expiration work.

## 10. Encryption and Data Protection

Encryption helps protect personal information against unauthorized disclosure.

### Encryption in Transit

Use appropriately configured TLS for network communication.

Examples:

- HTTPS.
- Secure database connections.
- Encrypted service-to-service communication.
- Secure file transfers.

### Encryption at Rest

Protect stored information using suitable encryption mechanisms.

Examples:

- Database encryption.
- Disk encryption.
- Object storage encryption.
- Backup encryption.

### Field-Level Encryption

Some sensitive fields may require application-level encryption beyond the underlying storage encryption.

This can reduce exposure when database access is compromised, depending on key architecture and application design.

### Key Management

- Keep encryption keys separate from encrypted data where practical.
- Restrict key access.
- Use appropriate key management services.
- Define rotation procedures.
- Protect backup and recovery keys.
- Monitor key usage.
- Avoid hardcoding keys.

### Hashing vs. Encryption

| Method | Purpose |
|---|---|
| Encryption | Reversible protection using a key |
| Hashing | One-way transformation |
| Password hashing | Specialized slow hashing for password verification |
| Tokenization | Replacing sensitive values with controlled substitutes |

Passwords should generally be stored using dedicated password hashing algorithms, not reversible encryption.

References:

- `../cryptography/README.md`
- `../secrets-management/README.md`

## 11. Data Retention and Deletion

Personal information should not be retained indefinitely without an appropriate purpose or legal basis.

### Retention Policy

Define:

- Data category.
- Purpose.
- Retention period.
- Applicable legal requirements.
- Deletion process.
- Backup behavior.
- Responsible owner.

### Data Deletion

Deletion may involve:

- Primary database records.
- Related application records.
- Object storage.
- Search indexes.
- Caches.
- Analytics systems.
- Third-party processors.
- Scheduled exports.
- Backup expiration.

### Account Deletion

An account deletion workflow should consider:

1. Verify the requester's authority.
2. Explain relevant consequences.
3. Identify associated personal data.
4. Determine applicable retention obligations.
5. Delete or anonymize eligible records.
6. Revoke sessions and credentials.
7. Remove associated files where appropriate.
8. Notify relevant processors when required.
9. Record completion without retaining unnecessary personal data.
10. Explain any information that must be retained and why, where appropriate.

### Deletion vs. Anonymization

**Deletion** removes information from the relevant system according to its deletion process.

**Anonymization** transforms information so that individuals are no longer identifiable, considering reasonably available means and applicable standards.

Pseudonymization is not the same as anonymization. Pseudonymized information may still be personal data.

### Important Backup Consideration

A deletion request may not require immediate removal from every immutable backup, depending on applicable law and circumstances.

However, organizations should define backup retention and ensure deleted information is not unnecessarily reintroduced into active systems during restoration.

## 12. User Privacy Rights and Controls

Applicable privacy laws may provide individuals with rights over their personal information.

These rights differ across jurisdictions.

Depending on the relevant legal framework, individuals may have rights relating to:

- Access.
- Correction.
- Erasure or deletion.
- Withdrawal of consent.
- Restriction of processing.
- Data portability.
- Objection.
- Grievance or complaint mechanisms.

### Developer Responsibilities

Build appropriate technical capabilities to support applicable rights.

Examples:

- Account data export.
- Profile correction.
- Account deletion.
- Consent management.
- Communication preferences.
- Privacy settings.
- Request tracking.
- Identity verification.

### Identity Verification

Privacy requests must be handled securely.

An organization should verify that the requester is authorized without collecting excessive additional information.

### Request Workflow

```text
User Submits Request
        |
        v
Identity Verification
        |
        v
Request Classification
        |
        v
Legal and Policy Review
        |
        v
Authorized Processing
        |
        v
Completion Verification
        |
        v
User Notification
```

### Important Principle

Do not implement a universal privacy-rights workflow without checking the laws applicable to the organization and processing activity.

## 13. Cookies and Browser Storage

Web applications use browser technologies to maintain sessions, store preferences and collect analytics.

These mechanisms can introduce privacy and security risks.

### Cookies

Cookies may be used for:

- Authentication sessions.
- Preferences.
- Shopping carts.
- Analytics.
- Advertising.
- Tracking.

### Secure Cookie Settings

For sensitive session cookies, consider:

- `Secure`
- `HttpOnly`
- Appropriate `SameSite` settings.
- Suitable expiration.
- Restricted domain and path scope.

These controls improve cookie security but do not independently establish privacy compliance.

### Browser Storage

Applications may use:

- Local Storage.
- Session Storage.
- IndexedDB.
- Browser Cache.

Avoid storing long-lived authentication secrets or unnecessary sensitive information in browser-accessible storage.

### Third-Party Tracking

Review:

- Analytics scripts.
- Advertising pixels.
- Embedded content.
- Social media integrations.
- Session replay tools.
- External fonts and scripts.

Determine what information they collect and where it is sent.

### Cookie Consent

Where applicable, provide transparent controls for optional cookies and similar technologies.

Do not assume that every cookie requires identical consent treatment. Essential functionality and applicable legal requirements must be considered.

## 14. Analytics and User Tracking

Analytics can improve product quality, but excessive tracking creates privacy risk.

### Common Analytics Data

- Page views.
- Feature usage.
- Device information.
- Session activity.
- Navigation patterns.
- Search interactions.
- Conversion events.
- Crash information.

### Privacy-Conscious Analytics

- Collect only useful metrics.
- Avoid unnecessary identifiers.
- Exclude passwords and sensitive fields.
- Avoid recording private user content.
- Restrict access to analytics data.
- Define retention periods.
- Review third-party sharing.
- Respect applicable consent requirements.
- Provide appropriate user controls.

### Session Replay Risks

Session replay tools may capture:

- Form inputs.
- User navigation.
- Interface content.
- Private messages.
- Personal information.

Use appropriate masking and exclusion controls.

Test them using realistic but non-sensitive test data before production deployment.

## 15. API Privacy and Data Exposure

APIs often transfer personal information between application components and external services.

### API Privacy Requirements

- Authenticate requests appropriately.
- Authorize access to individual records.
- Return only necessary fields.
- Validate input.
- Restrict bulk exports.
- Protect file access.
- Use TLS.
- Avoid sensitive URL parameters.
- Apply rate limits.
- Audit sensitive operations.

### Data Minimization in API Responses

An endpoint should return only the information needed for its purpose.

For example, a public profile endpoint should not return:

- Password hashes.
- Internal administrative notes.
- Authentication recovery information.
- Private contact information.
- Internal risk scores.
- Unrelated personal metadata.

### API Logging

Avoid recording complete request and response bodies by default.

Use structured event records with carefully selected metadata.

Reference: `../api-security/README.md`

## 16. File Uploads and Personal Information

Uploaded files can contain sensitive information beyond their visible content.

Examples:

- Identity documents.
- Medical reports.
- Financial records.
- Contracts.
- Photographs.
- Private communications.
- Location metadata.

### Secure Upload Practices

- Restrict accepted file types.
- Validate file signatures.
- Apply size limits.
- Generate safe storage names.
- Scan files where appropriate.
- Store private files outside public web roots.
- Restrict download access.
- Use short-lived access links.
- Define deletion policies.

### Metadata Considerations

Images and documents may contain:

- GPS coordinates.
- Device information.
- Author details.
- Creation timestamps.
- Embedded metadata.

Remove or preserve metadata according to the application's legitimate purpose and privacy requirements.

Do not automatically expose original uploaded files or their metadata publicly.

## 17. Mobile Application Privacy

Mobile applications may collect information through device permissions, operating system APIs and third-party SDKs.

### Common Permission Categories

- Camera.
- Microphone.
- Location.
- Contacts.
- Photos and files.
- Bluetooth.
- Notifications.
- Motion and activity sensors.

### Permission Best Practices

- Request permissions only when necessary.
- Explain why the permission is needed.
- Request permissions at an appropriate point in the user journey.
- Handle denied permissions safely.
- Avoid unnecessary background access.
- Respect operating system privacy controls.
- Provide appropriate settings and alternatives.

### Location Privacy

Location information can reveal sensitive behavioral patterns.

Consider:

- Approximate vs. precise location.
- Foreground vs. background access.
- Collection frequency.
- Storage duration.
- Third-party sharing.
- User visibility and controls.

### Mobile SDKs

Review third-party SDKs for:

- Information collected.
- Permission requirements.
- Analytics behavior.
- Network destinations.
- Data retention.
- Privacy documentation.
- Security practices.

### Platform Resources

- [Apple Privacy](https://developer.apple.com/privacy/)
- [Apple App Privacy Details](https://developer.apple.com/app-store/app-privacy-details/)
- [Android Privacy](https://developer.android.com/privacy)
- [Google Play Data Safety](https://support.google.com/googleplay/android-developer/answer/10787469)

Reference: `../mobile-security/README.md`

## 18. Third-Party Processors and Data Sharing

Applications often rely on external service providers.

Examples:

- Cloud hosting.
- Authentication providers.
- Email services.
- Analytics platforms.
- Payment processors.
- Customer support systems.
- AI model providers.
- Error monitoring platforms.

### Third-Party Assessment

Before integrating a provider, evaluate:

- What data is shared?
- Why is it shared?
- Where is it processed?
- Who can access it?
- How long is it retained?
- Is it used for additional purposes?
- Can it be deleted?
- Are subcontractors involved?
- What security controls exist?
- What happens after termination?

### Data Processing Agreements

Depending on applicable law and the relationship between parties, contractual arrangements may be required.

Examples include data processing agreements or equivalent contractual protections.

### Vendor Risk Management

- Review provider documentation.
- Evaluate security certifications where relevant.
- Understand breach notification procedures.
- Restrict integration permissions.
- Rotate API credentials.
- Review access periodically.
- Monitor integration behavior.
- Remove unused integrations.

### Important Principle

Sending personal information to a reputable service does not automatically make the transfer appropriate or lawful.

The organization remains responsible for evaluating its obligations.

## 19. Cross-Border Data Transfers

Applications may process information in multiple countries through cloud infrastructure, analytics providers and external processors.

### Questions to Consider

- Where is the information collected?
- Where is it stored?
- Where is it processed?
- Which providers receive it?
- Can support personnel access it from another country?
- Are international transfer restrictions applicable?
- Are additional safeguards required?

### Architecture Considerations

- Document processing locations.
- Review provider regions.
- Understand backup locations.
- Review subprocessors.
- Restrict unnecessary transfers.
- Apply appropriate contractual safeguards.
- Maintain transfer records where required.

Data localization and cross-border transfer requirements differ by jurisdiction and data category.

Do not assume that selecting a cloud region alone resolves every cross-border privacy obligation.

## 20. Privacy in AI-Powered Applications

AI applications can process large amounts of personal and confidential information.

Examples include:

- AI chat assistants.
- Personalized recommendation systems.
- Document analysis.
- Voice assistants.
- Image processing.
- AI-powered search.
- Automated decision support.
- Agent-based workflows.

### AI Privacy Risks

- Sending sensitive data to external model providers.
- Retaining prompts unnecessarily.
- Exposing private conversation history.
- Training or evaluating models using personal information without appropriate controls.
- Revealing data across users or tenants.
- Logging private prompts.
- Insecure vector database access.
- Excessive agent permissions.
- Unexpected data sharing through integrations.

### Privacy-Conscious AI Architecture

- Minimize prompt contents.
- Redact unnecessary personal information.
- Enforce tenant isolation.
- Apply authorization before retrieval.
- Restrict access to conversation history.
- Review model provider data policies.
- Define retention behavior.
- Protect vector databases.
- Secure tool and plugin permissions.
- Avoid exposing secrets in agent context.

### Retrieval-Augmented Generation

For applications using retrieval-augmented generation:

- Enforce access control before retrieving documents.
- Associate records with trusted ownership and tenant metadata.
- Prevent cross-user retrieval.
- Recheck authorization before returning content.
- Protect embeddings and source documents.
- Apply deletion policies to both documents and associated indexes.

Deleting a source document does not automatically guarantee deletion from every cache, embedding index or derived system.

### AI Training and User Data

Understand whether a provider uses submitted information for:

- Model training.
- Service improvement.
- Abuse monitoring.
- Diagnostics.
- Temporary processing.
- Long-term storage.

Policies differ by provider, product and configuration.

Do not promise that information is never retained or used for training unless the actual service terms and configuration support that statement.

### AI Agent Privacy

AI agents may have access to:

- User files.
- Emails.
- Databases.
- Internal documents.
- Application APIs.
- Connected services.

Apply:

- Least privilege.
- Explicit authorization.
- Scoped credentials.
- Audit logging.
- Human approval for sensitive actions.
- Data minimization.
- Clear retention policies.

## 21. Privacy and Logging

Logging systems can unintentionally become large repositories of personal information.

### Common Privacy Risks

- Recording email addresses unnecessarily.
- Logging full IP addresses without a justified purpose.
- Capturing complete request bodies.
- Storing authentication tokens.
- Recording private messages.
- Collecting excessive device information.
- Retaining diagnostic information indefinitely.

### Privacy-Aware Logging

- Define logging purposes.
- Exclude credentials.
- Minimize personal identifiers.
- Apply redaction.
- Restrict log access.
- Define retention periods.
- Protect log exports.
- Review third-party logging processors.

### Pseudonymization

Pseudonymization replaces direct identifiers with substitutes.

For example, an internal user identifier may be used instead of an email address in routine diagnostic logs.

However, pseudonymized data may still be personal data if re-identification remains possible.

Reference: `../logging-monitoring/README.md`

## 22. Data Breaches and Privacy Incidents

A data breach may involve unauthorized access, disclosure, alteration, loss or destruction of personal information.

The exact legal definition varies by jurisdiction.

### Examples

- Public database exposure.
- Misconfigured cloud storage.
- Stolen administrator credentials.
- Private files shared publicly.
- Unauthorized database exports.
- Compromised authentication tokens.
- Accidental email disclosure.
- Malware accessing personal information.

### Initial Response

1. Identify affected systems.
2. Determine whether exposure is ongoing.
3. Contain unauthorized access.
4. Preserve evidence.
5. Identify affected information.
6. Estimate the number of affected individuals.
7. Determine possible harm.
8. Involve security, legal and privacy stakeholders.
9. Assess applicable notification obligations.
10. Document actions and findings.

### Notification Considerations

Depending on applicable law, organizations may have obligations to notify:

- Relevant regulatory authorities.
- Affected individuals.
- Contractual partners.
- Customers.
- Other designated authorities.

Deadlines and notification thresholds vary.

Do not delay required reporting while attempting to establish unnecessary certainty, and do not make unsupported claims about the impact.

Reference: `../incident-response/README.md`

## 23. Privacy and Security in Development Environments

Development and testing environments can expose real user information.

### Common Risks

- Production database copies on developer laptops.
- Real user data in test environments.
- Personal information in screenshots.
- Credentials in debugging logs.
- Unprotected local database backups.
- Test accounts with production privileges.
- Sensitive data in AI coding agent context.

### Development Best Practices

- Use synthetic test data.
- Mask personal information when practical.
- Restrict production database access.
- Avoid unnecessary production data exports.
- Secure developer workstations.
- Protect local backups.
- Exclude secrets from source control.
- Review debugging output.
- Control AI tool access.
- Delete temporary data when no longer needed.

### Synthetic Data

Synthetic data is generated for testing and does not directly reproduce actual individuals' records.

It can reduce privacy risk, although teams should still check that generated datasets do not accidentally contain real information.

## 24. Privacy in CI/CD and Deployment Pipelines

Build and deployment systems may process secrets, application artifacts and personal information.

### Pipeline Privacy Risks

- Personal information in build logs.
- Secrets exposed in deployment output.
- Unrestricted access to artifacts.
- Public test reports containing private data.
- Production database exports.
- Third-party pipeline integrations.

### Recommended Controls

- Restrict pipeline permissions.
- Protect deployment credentials.
- Redact sensitive build output.
- Control artifact access.
- Avoid copying production data unnecessarily.
- Define artifact retention.
- Audit privileged pipeline actions.
- Review third-party actions and integrations.

Reference: `../infrastructure/README.md`

## 25. Privacy Requirements for Children's Data

Applications used by children may require additional safeguards.

### Considerations

- Applicable age requirements.
- Parental or guardian involvement where required.
- Age-appropriate privacy notices.
- Data minimization.
- Restricted profiling.
- Advertising and tracking limitations.
- Appropriate access controls.
- Safe communication features.
- Retention and deletion policies.

### Development Practices

- Avoid collecting unnecessary information.
- Make privacy settings appropriately protective.
- Restrict public exposure.
- Avoid unnecessary behavioral tracking.
- Review third-party SDKs.
- Apply applicable child protection requirements.

Legal requirements differ significantly across countries and types of service.

Obtain appropriate legal review before launching products that process children's personal information.

## 26. Privacy Compliance Frameworks and Regulations

Privacy obligations depend on where an organization operates, whom it serves and what information it processes.

The following resources provide useful starting points.

### India: Digital Personal Data Protection Framework

India's Digital Personal Data Protection Act, 2023 establishes a framework for processing digital personal data.

Developers and organizations should review the Act, applicable rules, commencement notifications and official guidance.

Important topics include:

- Personal data processing.
- Notice and consent where applicable.
- Obligations of Data Fiduciaries.
- Rights of Data Principals.
- Security safeguards.
- Personal data breach obligations.
- Children's data.
- Grievance mechanisms.
- Cross-border processing.

Official resources:

- [Ministry of Electronics and Information Technology](https://www.meity.gov.in/)
- [India Code](https://www.indiacode.nic.in/)
- [Data Protection Framework](https://www.meity.gov.in/data-protection-framework)

Do not assume every provision becomes applicable on the same date. Verify current commencement notifications and rules.

### GDPR: European Union

The General Data Protection Regulation establishes requirements for personal data processing within its scope.

Important concepts include:

- Lawfulness, fairness and transparency.
- Purpose limitation.
- Data minimization.
- Accuracy.
- Storage limitation.
- Integrity and confidentiality.
- Accountability.
- Data subject rights.
- Data protection by design and default.
- Personal data breach obligations.

Official resource:

https://eur-lex.europa.eu/eli/reg/2016/679/oj

### CCPA and CPRA: California

California privacy laws provide rights and obligations relating to personal information for covered businesses and other relevant entities.

Topics include:

- Notice.
- Access.
- Correction.
- Deletion.
- Opt-out rights.
- Sensitive personal information.
- Data sharing.
- Business obligations.

Official resource:

https://oag.ca.gov/privacy/ccpa

### Other Resources

- [OECD Privacy Guidelines](https://www.oecd.org/en/topics/privacy.html)
- [NIST Privacy Framework](https://www.nist.gov/privacy-framework)
- [ISO/IEC 27701](https://www.iso.org/standard/85819.html)

### Important Legal Disclaimer

This guide is technical and educational. It does not establish that an application or organization is legally compliant.

Privacy requirements depend on applicable legislation, current commencement dates, regulations, processing activities, organizational roles and legal interpretations.

Seek qualified legal and privacy advice when determining compliance obligations.

## 27. Privacy Impact Assessment

A Privacy Impact Assessment (PIA), or Data Protection Impact Assessment (DPIA) where applicable, helps evaluate privacy risks associated with a feature or system.

### When to Consider an Assessment

- Processing sensitive personal information.
- Introducing large-scale data collection.
- Implementing behavioral tracking.
- Processing precise location data.
- Introducing AI-based profiling.
- Combining multiple personal datasets.
- Deploying new surveillance-related functionality.
- Introducing high-risk automated processing.

Legal requirements for formal assessments vary.

### Assessment Template

```markdown
# Privacy Impact Assessment

## 1. Project Information

Project:
Feature:
Owner:
Date:
Review status:

## 2. Processing Purpose

What is the feature intended to do?

Why is personal information required?

## 3. Information Collected

Data categories:
Collection sources:
Sensitive information:
User groups:

## 4. Data Flow

Collection:
Processing:
Storage:
Recipients:
Transfers:
Deletion:

## 5. Access and Security

Authorized roles:
Authentication:
Authorization:
Encryption:
Logging:
Monitoring:

## 6. Retention

Retention period:
Deletion process:
Backup behavior:

## 7. Privacy Risks

Potential harms:
Unauthorized access:
Unnecessary collection:
Third-party exposure:
Re-identification:
Cross-tenant exposure:

## 8. Mitigations

Technical safeguards:
Organizational safeguards:
User controls:
Remaining risks:

## 9. Review

Reviewer:
Approval:
Follow-up actions:
Next review date:
```

## 28. Privacy Testing

Privacy controls should be tested throughout development.

### Data Collection Testing

- [ ] Only necessary information is collected.
- [ ] Optional fields are genuinely optional.
- [ ] Consent behavior is correct where applicable.
- [ ] Privacy notices match actual behavior.
- [ ] Unnecessary tracking is disabled.

### Access Control Testing

- [ ] Users cannot access other users' private records.
- [ ] Tenant isolation is enforced.
- [ ] Administrative access is restricted.
- [ ] APIs return only necessary information.
- [ ] Private files require authorization.

### Storage and Transmission Testing

- [ ] Sensitive network communication uses TLS.
- [ ] Storage permissions are restricted.
- [ ] Sensitive information is encrypted where appropriate.
- [ ] Backups are protected.
- [ ] Secrets are not stored in application code.

### Retention and Deletion Testing

- [ ] Retention policies are documented.
- [ ] Account deletion works.
- [ ] Associated files are handled.
- [ ] Relevant processors are considered.
- [ ] Backup expiration is documented.
- [ ] Deleted records are not unintentionally reintroduced.

### Logging and Analytics Testing

- [ ] Passwords are not logged.
- [ ] Tokens are not logged.
- [ ] Sensitive request bodies are excluded.
- [ ] Analytics collection is appropriate.
- [ ] Third-party scripts are reviewed.
- [ ] Diagnostic systems are configured safely.

### AI Privacy Testing

- [ ] Prompts do not expose unnecessary personal information.
- [ ] Retrieval respects authorization.
- [ ] Tenant isolation is tested.
- [ ] Conversation retention is understood.
- [ ] Model provider data policies are reviewed.
- [ ] Agent permissions are restricted.

## 29. Practical Privacy Review Checklist

### Data Collection

- [ ] Every collected data category has a documented purpose.
- [ ] Data collection is minimized.
- [ ] Sensitive information is identified.
- [ ] Privacy notices are available.
- [ ] Applicable legal grounds are evaluated.
- [ ] Optional processing is appropriately controlled.

### Application Architecture

- [ ] Privacy is considered during design.
- [ ] Private information is protected by default.
- [ ] Server-side authorization is enforced.
- [ ] Tenant isolation is implemented.
- [ ] API responses are minimized.
- [ ] Third-party integrations are reviewed.

### Data Storage

- [ ] Databases are access-controlled.
- [ ] Sensitive data is encrypted where appropriate.
- [ ] Object storage is private by default.
- [ ] Backups are protected.
- [ ] Retention periods are defined.
- [ ] Deletion processes are documented.

### User Rights

- [ ] Applicable privacy rights are identified.
- [ ] User requests can be handled.
- [ ] Identity verification is appropriate.
- [ ] Correction workflows exist where required.
- [ ] Deletion workflows are tested.
- [ ] Consent withdrawal works where applicable.

### Third Parties

- [ ] External providers are inventoried.
- [ ] Data sharing purposes are documented.
- [ ] Provider security is evaluated.
- [ ] Relevant contractual arrangements are reviewed.
- [ ] Cross-border transfers are assessed.
- [ ] Unused integrations are removed.

### Monitoring and Incident Response

- [ ] Sensitive information is excluded from logs.
- [ ] Log access is restricted.
- [ ] Data breach procedures are documented.
- [ ] Notification obligations are assessed.
- [ ] Evidence can be preserved.
- [ ] Privacy incidents are reviewed.

### AI and Analytics

- [ ] AI data processing is reviewed.
- [ ] Model provider retention is understood.
- [ ] User data is not unnecessarily exposed to agents.
- [ ] Analytics collection is minimized.
- [ ] Session replay is appropriately configured.
- [ ] Vector databases enforce access controls.

## 30. Common Privacy and Data Protection Mistakes

| Mistake | Potential consequence |
|---|---|
| Collecting unnecessary personal information | Increased exposure and privacy risk |
| Using data for unrelated purposes | Loss of transparency and potential legal violations |
| Public user profiles by default | Unwanted information disclosure |
| Returning excessive API fields | Personal data exposure |
| Logging authentication secrets | Credential compromise |
| Indefinite retention | Unnecessary long-term exposure |
| Weak tenant isolation | Cross-customer data exposure |
| Unreviewed analytics SDKs | Uncontrolled data sharing |
| Ignoring backup retention | Incomplete deletion expectations |
| Sending private prompts to AI services without review | Additional data exposure |
| Treating pseudonymization as anonymization | Incorrect privacy assumptions |
| Assuming encryption guarantees compliance | Unaddressed processing and governance risks |

## 31. Recommended Privacy and Data Protection Resources

### Security and Privacy Guidance

- [OWASP Privacy Risks](https://owasp.org/www-project-top-10-privacy-risks/)
- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- [NIST Privacy Framework](https://www.nist.gov/privacy-framework)
- [NIST Privacy Engineering](https://www.nist.gov/privacy-engineering)

### Legal and Regulatory Resources

- [India DPDP Framework](https://www.meity.gov.in/data-protection-framework)
- [India Code](https://www.indiacode.nic.in/)
- [GDPR Official Text](https://eur-lex.europa.eu/eli/reg/2016/679/oj)
- [California Privacy Rights](https://oag.ca.gov/privacy/ccpa)

### Platform Documentation

- [Apple Privacy](https://developer.apple.com/privacy/)
- [Android Privacy](https://developer.android.com/privacy)
- [Google Play Data Safety](https://support.google.com/googleplay/android-developer/answer/10787469)

### Related ShipSafe Domains

- [Authentication](../authentication/README.md)
- [Authorization](../authorization/README.md)
- [API Security](../api-security/README.md)
- [Database Security](../database-security/README.md)
- [Secrets Management](../secrets-management/README.md)
- [Cryptography](../cryptography/README.md)
- [Logging and Monitoring](../logging-monitoring/README.md)
- [Incident Response](../incident-response/README.md)

---

Privacy and data protection should be built into every stage of application development.

A secure application should collect only what it needs, explain how information is used, restrict access, protect stored data, control third-party sharing and provide appropriate retention and deletion mechanisms.

No checklist or technical control guarantees legal compliance or eliminates privacy risk. Effective privacy protection requires sound architecture, continuous testing, responsible data governance and attention to the people whose information an application processes.
