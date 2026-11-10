
# Mobile Application Security Checklist

A practical security assessment checklist for Android and iOS applications, including native, Flutter, React Native and other cross-platform applications.

**Project:**\
**Application name:**\
**Package name / Bundle ID:**\
**Reviewer:**\
**Review date:**\
**Application version:**\
**Platform:** Android / iOS / Both\
**Framework:** Native / Flutter / React Native / Other\
**Environment:** Development / Staging / Production\

---

## How to Use This Checklist

Review every applicable control against the actual application, its backend services and its deployment configuration.

Use these statuses:

- [ ] Not reviewed
- [x] Verified and implemented
- [ ] Not applicable (document the reason)

Do not mark an item complete based only on the presence of a feature. Verify its security behavior.

### Severity Guidance

| Severity | Meaning | Expected action |
|---|---|---|
| Critical | Severe compromise, sensitive data exposure or account takeover | Stop release until resolved |
| High | Significant weakness affecting security or privacy | Resolve before production |
| Medium | Meaningful weakness with limited or conditional impact | Resolve or formally accept risk |
| Low | Limited weakness or defense-in-depth improvement | Track and prioritize |
| Informational | Documentation or improvement opportunity | Review and document |

---

# 1. Application Architecture and Threat Modeling

- [ ] Application architecture is documented.
- [ ] Android and iOS components are identified.
- [ ] Backend services and API dependencies are documented.
- [ ] Authentication and authorization flows are documented.
- [ ] Sensitive data flows are identified.
- [ ] Trust boundaries are identified.
- [ ] Third-party SDKs and services are inventoried.
- [ ] Local storage locations are identified.
- [ ] Network communication paths are documented.
- [ ] Deep links and universal links are documented.
- [ ] Sensitive features have been threat-modeled.
- [ ] Platform-specific security requirements are considered.
- [ ] Debug and production configurations are separated.

# 2. Authentication and Account Security

- [ ] Authentication uses a trusted and maintained implementation.
- [ ] Passwords are never stored in plaintext.
- [ ] Passwords are securely hashed by the backend.
- [ ] Authentication tokens are protected.
- [ ] Login attempts are rate-limited by the backend.
- [ ] Account enumeration risks have been reviewed.
- [ ] Multi-factor authentication is supported where appropriate.
- [ ] Biometric authentication is implemented using platform-supported APIs where appropriate.
- [ ] Biometric authentication does not replace server-side authorization.
- [ ] Password reset flows are secure.
- [ ] Email and phone verification are implemented where required.
- [ ] Logout invalidates the relevant session.
- [ ] Account switching does not expose another user's cached data.
- [ ] Sensitive account changes require appropriate verification.
- [ ] Authentication state is not trusted solely because the client reports it.
- [ ] Session expiration and refresh behavior are tested.

# 3. Secure Local Storage

- [ ] Sensitive information is not stored unnecessarily on the device.
- [ ] Authentication tokens are stored using appropriate secure platform storage.
- [ ] Android Keystore is used where appropriate.
- [ ] iOS Keychain is used where appropriate.
- [ ] Encryption keys are not hardcoded.
- [ ] Sensitive data is not stored in plaintext preferences.
- [ ] Sensitive information is not stored in unprotected SQLite databases.
- [ ] Local databases are encrypted where required by the threat model.
- [ ] Cached API responses do not expose sensitive information.
- [ ] Temporary files do not contain unnecessary sensitive information.
- [ ] Sensitive files use appropriate application-private storage.
- [ ] Logout clears appropriate user-specific local data.
- [ ] Account deletion removes appropriate locally stored data.
- [ ] Backup behavior for sensitive data is reviewed.
- [ ] Clipboard exposure is considered for sensitive content.
- [ ] Screenshots and screen recording risks are considered.
- [ ] Sensitive information is not unnecessarily displayed in notifications.
- [ ] Shared device behavior is reviewed.

# 4. Cryptography and Key Management

- [ ] Standard, maintained cryptographic libraries are used.
- [ ] Custom cryptographic algorithms are avoided.
- [ ] Cryptographically secure random number generation is used.
- [ ] Encryption keys are generated securely.
- [ ] Encryption keys are stored separately from encrypted data where appropriate.
- [ ] Nonces and initialization vectors are generated and managed correctly.
- [ ] Authenticated encryption is used where appropriate.
- [ ] Passwords are not encrypted as a substitute for password hashing.
- [ ] Certificate validation is enabled.
- [ ] Cryptographic operations are performed in trusted platform APIs where appropriate.
- [ ] Key rotation and revocation procedures are documented.
- [ ] Deprecated cryptographic algorithms are avoided.
- [ ] Cryptographic errors fail safely.

# 5. Network and API Communication

- [ ] HTTPS is used for production API communication.
- [ ] Cleartext HTTP traffic is disabled where appropriate.
- [ ] TLS certificate validation is enabled.
- [ ] Invalid certificates are rejected.
- [ ] Hostname verification is enabled.
- [ ] Sensitive API requests do not transmit credentials in URLs.
- [ ] API tokens are not logged.
- [ ] API requests are authenticated.
- [ ] Backend authorization is enforced for every protected resource.
- [ ] API response data is minimized.
- [ ] Sensitive information is not unnecessarily cached.
- [ ] Network errors do not expose internal information.
- [ ] API rate limiting is enforced server-side.
- [ ] Network retry behavior does not cause unsafe duplicate operations.
- [ ] Offline synchronization is reviewed for data conflicts and authorization.
- [ ] WebSocket communication is authenticated and authorized where applicable.
- [ ] Network security configuration is reviewed.

# 6. Certificate Pinning

- [ ] The need for certificate or public-key pinning has been assessed.
- [ ] Pinning is implemented only where justified by the threat model.
- [ ] Backup pins are considered.
- [ ] Certificate rotation procedures are documented.
- [ ] Pinning failures are handled safely.
- [ ] Emergency certificate rotation procedures are tested.
- [ ] Pinning does not disable normal certificate validation.

**Important:** Certificate pinning is not a replacement for TLS validation. It can introduce availability risks when certificates or keys change. It is not suitable for every application.

# 7. Android Platform Security

- [ ] Application manifest has been reviewed.
- [ ] Minimum and target SDK versions are appropriate.
- [ ] Exported activities are reviewed.
- [ ] Exported services are reviewed.
- [ ] Exported broadcast receivers are reviewed.
- [ ] Content providers are protected.
- [ ] Intent handling is reviewed.
- [ ] PendingIntent mutability is configured appropriately.
- [ ] Android permissions follow least privilege.
- [ ] Unnecessary permissions are removed.
- [ ] Network Security Configuration is reviewed.
- [ ] Cleartext traffic is disabled where appropriate.
- [ ] Debuggable is disabled in production.
- [ ] Backup configuration is reviewed.
- [ ] Sensitive data is protected from inappropriate backup.
- [ ] WebView configuration is secure.
- [ ] JavaScript interfaces exposed to WebViews are reviewed.
- [ ] FileProvider configuration is restricted appropriately.
- [ ] App links are verified where applicable.
- [ ] Deep link handling is validated.
- [ ] Release builds are signed securely.
- [ ] Signing keys are protected.
- [ ] Application update integrity is considered.

# 8. iOS Platform Security

- [ ] Info.plist configuration has been reviewed.
- [ ] App Transport Security is appropriately configured.
- [ ] Keychain access groups are restricted.
- [ ] Keychain accessibility settings are appropriate.
- [ ] Sensitive files use appropriate Data Protection classes.
- [ ] Entitlements follow least privilege.
- [ ] Unnecessary permissions and capabilities are removed.
- [ ] Privacy usage descriptions are accurate.
- [ ] Universal links are configured securely.
- [ ] Custom URL schemes are reviewed.
- [ ] URL handlers validate incoming parameters.
- [ ] WebView configuration is secure.
- [ ] Background execution is reviewed.
- [ ] Sensitive application content is protected during app switching where appropriate.
- [ ] Pasteboard exposure is considered.
- [ ] App extensions do not unnecessarily expose sensitive data.
- [ ] Debug configuration is disabled in production.
- [ ] Distribution certificates and signing credentials are protected.
- [ ] App Store release configuration is reviewed.

# 9. Application Permissions and Privacy

- [ ] Permissions are requested only when needed.
- [ ] Permissions are requested at an appropriate time.
- [ ] Users understand why each permission is needed.
- [ ] Camera access is justified.
- [ ] Microphone access is justified.
- [ ] Location access is justified.
- [ ] Contacts access is justified.
- [ ] Photo and media access is justified.
- [ ] Bluetooth and nearby-device access is justified.
- [ ] Background location is justified where applicable.
- [ ] Permission denial does not cause unsafe behavior.
- [ ] Permission revocation is handled correctly.
- [ ] Sensitive permissions are not requested unnecessarily.
- [ ] Personal data collection is minimized.
- [ ] Privacy notices reflect actual application behavior.
- [ ] Third-party SDK data collection is reviewed.

# 10. Deep Links and Navigation

- [ ] Deep links are inventoried.
- [ ] Incoming URLs are validated.
- [ ] Sensitive operations require authentication.
- [ ] Deep links cannot bypass authorization.
- [ ] User-controlled parameters are validated.
- [ ] Open redirect risks are reviewed.
- [ ] Sensitive tokens are not unnecessarily placed in URLs.
- [ ] Android intent filters are reviewed.
- [ ] iOS universal link associations are verified.
- [ ] Custom URL scheme collisions are considered.
- [ ] Deep links do not expose private account information.
- [ ] Navigation state cannot be manipulated to bypass protected workflows.

# 11. WebViews and Embedded Browser Components

- [ ] WebView usage is inventoried.
- [ ] JavaScript is disabled when unnecessary.
- [ ] JavaScript bridges are reviewed.
- [ ] Untrusted content is not loaded into privileged WebViews.
- [ ] URL navigation is restricted where appropriate.
- [ ] File access is disabled unless required.
- [ ] Unnecessary local file access is disabled.
- [ ] Mixed content is restricted.
- [ ] SSL errors are not blindly ignored.
- [ ] WebView cookies are reviewed.
- [ ] Authentication information is protected.
- [ ] External links are handled safely.
- [ ] WebView debugging is disabled in production.

# 12. Push Notifications and Background Processing

- [ ] Push notification tokens are protected.
- [ ] Push notification payloads do not contain unnecessary sensitive information.
- [ ] Notification content is appropriate for lock-screen exposure.
- [ ] Notification actions require suitable authorization.
- [ ] Push tokens are associated with the correct account.
- [ ] Tokens are removed or invalidated when appropriate.
- [ ] Background jobs validate authorization.
- [ ] Background synchronization does not expose another user's data.
- [ ] Sensitive background operations are protected.
- [ ] Notification deep links are validated.
- [ ] Push notification provider credentials are kept server-side.

# 13. File Handling and Data Sharing

- [ ] Shared files are reviewed for sensitive information.
- [ ] File import functionality validates file types and sizes.
- [ ] Imported data is parsed safely.
- [ ] File paths are validated.
- [ ] Temporary files are protected.
- [ ] Exported files follow appropriate privacy rules.
- [ ] Shared files use appropriate platform permissions.
- [ ] File-sharing providers do not expose private files.
- [ ] External application handoffs are reviewed.
- [ ] Backup and restore behavior is tested.
- [ ] Sensitive file deletion behavior is understood.

# 14. Authentication Tokens and Session Lifecycle

- [ ] Access tokens have appropriate expiration.
- [ ] Refresh tokens are stored securely.
- [ ] Refresh token rotation is implemented where appropriate.
- [ ] Token revocation is supported where required.
- [ ] Expired tokens are rejected.
- [ ] Invalid tokens are rejected.
- [ ] Tokens are not embedded in application source code.
- [ ] Tokens are not included in crash reports.
- [ ] Tokens are not exposed through logs.
- [ ] Session state is cleared appropriately after logout.
- [ ] Account switching does not reuse another user's token.
- [ ] Token refresh race conditions are handled.
- [ ] Server-side authorization remains authoritative.

# 15. Reverse Engineering and Tampering

- [ ] Release builds do not contain unnecessary debugging symbols or debug interfaces.
- [ ] Application packages are reviewed for embedded secrets.
- [ ] Hardcoded credentials are absent.
- [ ] Sensitive business logic is not trusted solely because it is obfuscated.
- [ ] Application integrity requirements are assessed.
- [ ] Tampering risks are considered.
- [ ] Rooted and jailbroken device risks are assessed.
- [ ] Runtime instrumentation risks are considered.
- [ ] Sensitive operations are protected server-side.
- [ ] Debugging and development endpoints are disabled in release builds.
- [ ] Obfuscation is used where appropriate.
- [ ] Third-party protection SDKs are reviewed before integration.

**Important:** Obfuscation and root or jailbreak detection increase resistance but cannot guarantee that a mobile application cannot be reverse-engineered or modified.

# 16. Third-Party SDKs and Dependencies

- [ ] Third-party SDKs are inventoried.
- [ ] SDK publishers are reviewed.
- [ ] SDK permissions are reviewed.
- [ ] SDK data collection is understood.
- [ ] SDK network behavior is reviewed.
- [ ] Dependencies are maintained.
- [ ] Known vulnerable dependencies are identified.
- [ ] Unused dependencies are removed.
- [ ] Dependency update procedures are documented.
- [ ] SDK credentials are not exposed.
- [ ] Analytics and advertising SDKs follow privacy requirements.
- [ ] SDK license requirements are reviewed.
- [ ] Software composition analysis is performed where appropriate.

# 17. Flutter, React Native and Cross-Platform Security

- [ ] Framework versions are maintained.
- [ ] Native platform configuration is reviewed.
- [ ] Native plugins are inventoried.
- [ ] Plugin permissions are reviewed.
- [ ] Platform channels are reviewed.
- [ ] Sensitive values are not hardcoded in Dart or JavaScript bundles.
- [ ] Secure storage plugins use appropriate native platform storage.
- [ ] JavaScript bridge or WebView interactions are reviewed.
- [ ] Debug tools are disabled in production.
- [ ] Source maps and build artifacts are reviewed.
- [ ] Release builds use appropriate signing.
- [ ] Framework-specific security advisories are monitored.
- [ ] Native and framework-level dependencies are both reviewed.
- [ ] Backend authorization is not delegated to the client framework.

# 18. Logging, Crash Reports and Diagnostics

- [ ] Passwords are not logged.
- [ ] Access and refresh tokens are not logged.
- [ ] API secrets are not logged.
- [ ] Sensitive personal information is minimized in logs.
- [ ] Crash reports are reviewed for sensitive data.
- [ ] Analytics events do not unnecessarily contain personal data.
- [ ] Debug logging is disabled or restricted in production.
- [ ] Diagnostic data collection is documented.
- [ ] Crash reporting providers are reviewed.
- [ ] Log access is restricted.
- [ ] Data retention policies are defined.
- [ ] Sensitive error details are not shown to users.

# 19. Backend and Database Integration

- [ ] Backend endpoints authenticate protected operations.
- [ ] Backend authorization is enforced independently of the mobile client.
- [ ] User identity is verified server-side.
- [ ] Object ownership is checked.
- [ ] Multi-tenant isolation is enforced.
- [ ] Database permissions follow least privilege.
- [ ] API responses expose only necessary fields.
- [ ] Database credentials are never embedded in the application.
- [ ] Privileged service keys remain server-side.
- [ ] Database security policies are tested.
- [ ] Rate limiting is enforced server-side.
- [ ] Sensitive operations are logged appropriately.

# 20. Offline Functionality and Synchronization

- [ ] Offline data storage is reviewed.
- [ ] Sensitive offline data is protected.
- [ ] Cached data is isolated by user account.
- [ ] Logout clears appropriate cached data.
- [ ] Synchronization authenticates the current user.
- [ ] Server-side authorization is rechecked during synchronization.
- [ ] Conflict resolution cannot bypass business rules.
- [ ] Offline actions are validated before acceptance by the backend.
- [ ] Duplicate transaction risks are addressed.
- [ ] Sensitive data is not unnecessarily retained after account deletion.
- [ ] Offline access behavior is documented.

# 21. Build, Signing and Release Security

- [ ] Development and production build configurations are separated.
- [ ] Debug mode is disabled in release builds.
- [ ] Signing keys are stored securely.
- [ ] Signing credentials are not committed to Git.
- [ ] Build pipeline permissions follow least privilege.
- [ ] Release artifacts are protected.
- [ ] Dependency checks are included in CI/CD where appropriate.
- [ ] Secrets are supplied securely during builds.
- [ ] Build provenance and artifact integrity are considered.
- [ ] Release permissions are restricted.
- [ ] Application update mechanisms are trusted.
- [ ] Version and release history are maintained.
- [ ] Production configuration is reviewed before publication.

# 22. Security Testing

- [ ] Application package contents are inspected.
- [ ] Android manifest or iOS configuration is reviewed.
- [ ] Authentication flows are tested.
- [ ] Authorization boundaries are tested.
- [ ] Local storage is inspected.
- [ ] Sensitive data exposure is tested.
- [ ] Network traffic is reviewed in an authorized test environment.
- [ ] TLS validation is tested.
- [ ] API security is tested.
- [ ] Deep links are tested.
- [ ] File handling is tested.
- [ ] Permission behavior is tested.
- [ ] WebView security is tested where applicable.
- [ ] Backup behavior is tested.
- [ ] Root and jailbreak behavior is reviewed where relevant.
- [ ] Dependency vulnerabilities are assessed.
- [ ] Release builds are tested.
- [ ] Security findings are documented.
- [ ] Fixes are retested.

# 23. Production Readiness

- [ ] Production endpoints are verified.
- [ ] Production secrets are configured securely.
- [ ] No test credentials remain.
- [ ] No unnecessary debugging features remain.
- [ ] Permissions are minimized.
- [ ] Privacy documentation is available.
- [ ] Third-party SDK behavior is reviewed.
- [ ] Application signing is verified.
- [ ] API authentication and authorization are tested.
- [ ] Backend rate limiting is operational.
- [ ] Monitoring and crash reporting are reviewed.
- [ ] Incident response procedures are documented.
- [ ] Account deletion behavior is verified.
- [ ] Backup and recovery procedures are reviewed.
- [ ] Known security risks are documented.
- [ ] Critical and high-severity findings are resolved or formally handled before release.

---

# 24. Security Findings Register

| ID | Finding | Severity | Platform | Affected component | Status | Owner |
|---|---|---|---|---|---|---|
| MOB-001 | | | | | Open | |
| MOB-002 | | | | | Open | |
| MOB-003 | | | | | Open | |

### Finding Status

- Open
- In progress
- Fixed
- Retest required
- Accepted risk
- Not applicable

For each finding, document:

**Finding ID:**\
**Description:**\
**Affected platform:**\
**Affected component:**\
**Evidence:**\
**Reproduction steps:**\
**Impact:**\
**Severity:**\
**Recommended remediation:**\
**Assigned owner:**\
**Retest date:**\
**Retest result:**\

---

# 25. Final Security Review

**Total items reviewed:**\

**Verified controls:**\

**Items not applicable:**\

**Items not tested:**\

**Open findings:**\

**Critical findings:**\

**High findings:**\

**Medium findings:**\

**Low findings:**\

**Overall review result:** Pass / Conditional / Fail

**Reviewer notes:**

**Approved by:**

**Approval date:**

---

## Related ShipSafe Documentation

- [Authentication](../security/authentication/README.md)
- [Authorization](../security/authorization/README.md)
- [API Security](../security/api-security/README.md)
- [Mobile Security](../security/mobile-security/README.md)
- [Secrets Management](../security/secrets-management/README.md)
- [Cryptography](../security/cryptography/README.md)
- [Dependency Security](../security/dependencies/README.md)
- [Privacy and Data Protection](../security/privacy-data-protection/README.md)
- [Security Testing](../security/security-testing/README.md)
- [Incident Response](../security/incident-response/README.md)

## Important Disclaimer

This checklist is an educational and practical review aid. Completing it does not guarantee that an application is secure, free of vulnerabilities or legally compliant.

Mobile application security requires consideration of both the client and backend. A mobile client runs on a device outside the developer's direct control. Obfuscation, local checks and client-side restrictions cannot replace server-side authorization and security controls.

Automated tools cannot identify every vulnerability. Manual testing, architecture review, threat modeling and qualified security assessment may also be necessary.
