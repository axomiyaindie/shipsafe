
# Mobile Application Security

Mobile applications operate in environments that developers do not fully control. Users can inspect application packages, modify device settings, intercept network traffic and interact with application components outside the intended user interface.

Mobile application security focuses on protecting user information, authentication credentials, application functionality, network communication and interactions with the operating system.

This guide covers Android and iOS security, common vulnerabilities, secure implementation practices and mobile application security testing.

## 1. Mobile Security Fundamentals

Mobile applications commonly use:

- Android with Kotlin or Java.
- iOS with Swift or Objective-C.
- Flutter.
- React Native.
- .NET MAUI.
- Other cross-platform frameworks.

A typical mobile application communicates with backend APIs, stores information locally, integrates with operating system services and may use third-party SDKs.

### Core Security Principles

- Treat the mobile device as an untrusted environment.
- Enforce authentication and authorization on trusted backend services.
- Protect sensitive information stored locally.
- Use secure network communication.
- Avoid embedding privileged secrets in application packages.
- Request only necessary operating system permissions.
- Protect cryptographic keys using platform security facilities.
- Validate deep links and external application inputs.
- Keep application dependencies updated.
- Test release builds, not only development builds.

A mobile application cannot guarantee that its own code or local data will remain inaccessible to the device owner.

## 2. Android Security

Android provides security mechanisms through application sandboxing, permissions, application signing, platform APIs and hardware-backed security features.

### Security Recommendations

- Use current supported Android SDK versions.
- Set appropriate minimum and target SDK versions.
- Request only necessary permissions.
- Protect exported activities, services, receivers and providers.
- Apply appropriate component-level access restrictions.
- Use Android Keystore for cryptographic key protection.
- Configure network security policies.
- Review application backup behavior.
- Protect sensitive application data.
- Disable unnecessary debugging capabilities in release builds.

### Android Manifest Review

Review `AndroidManifest.xml` for:

- Unnecessary permissions.
- Exported components.
- Intent filters.
- Backup configuration.
- Debuggable settings.
- Cleartext traffic configuration.
- Network security configuration.
- Application component exposure.

An exported component should be accessible to other applications only when that exposure is intentional and appropriately protected.

## 3. iOS Security

iOS provides application sandboxing, code signing, Keychain services, entitlements and platform security mechanisms.

### Security Recommendations

- Use supported iOS versions.
- Apply appropriate application sandbox restrictions.
- Store sensitive credentials in Keychain.
- Review application entitlements.
- Configure App Transport Security appropriately.
- Request permissions only when required.
- Protect sensitive information in application backups.
- Review universal links and custom URL schemes.
- Avoid exposing sensitive information through application logs.
- Review third-party SDK permissions and behavior.

### iOS Configuration Review

Review:

- `Info.plist`.
- Entitlements.
- URL schemes.
- Associated domains.
- App Transport Security exceptions.
- Keychain access groups.
- Background execution capabilities.
- Privacy permission descriptions.
- Release build configuration.

Avoid weakening platform security protections without a documented and justified requirement.

## 4. Secure Local Data Storage

Mobile applications may store authentication tokens, user preferences, documents, cached API responses and sensitive business information.

### Common Risks

- Plaintext credentials in local files.
- Sensitive information in preferences.
- Tokens stored in unprotected browser or application storage.
- Database files accessible through insecure backups.
- Sensitive information left in application caches.
- Private information exposed through logs.

### Security Recommendations

- Store credentials using Android Keystore-backed or iOS Keychain-based mechanisms.
- Encrypt sensitive local data when required by the threat model.
- Protect encryption keys separately from encrypted data.
- Avoid storing information that the application does not need.
- Apply appropriate backup exclusions.
- Clear sensitive temporary files when no longer required.
- Review database and file permissions.
- Protect cached authentication information.
- Consider device lock and biometric requirements for sensitive operations.

Encryption does not provide complete protection if the application or device is already compromised.

## 5. Mobile Authentication and Session Management

Mobile authentication commonly involves backend APIs, access tokens, refresh tokens, biometric authentication and identity providers.

### Security Recommendations

- Use established authentication protocols.
- Prefer Authorization Code Flow with PKCE for appropriate OAuth mobile clients.
- Validate tokens on the backend.
- Store tokens using platform-protected storage.
- Apply appropriate token expiration and rotation.
- Protect account recovery workflows.
- Invalidate sessions after logout where applicable.
- Support remote session revocation.
- Require additional verification for sensitive operations.
- Avoid treating local biometric checks as backend authorization.

### Biometric Authentication

Biometric authentication can improve the user experience and protect local application actions.

However:

- Use platform-provided biometric APIs.
- Avoid implementing biometric matching yourself.
- Protect sensitive cryptographic operations with platform authentication controls where appropriate.
- Define fallback behavior carefully.
- Do not treat a local biometric success result as proof of server-side authorization.

## 6. Secure API Communication

Mobile applications frequently communicate with backend services over the internet.

### Security Recommendations

- Use HTTPS for production API communication.
- Validate TLS certificates.
- Reject invalid certificate chains.
- Avoid disabling certificate validation.
- Do not send credentials through URL parameters.
- Protect authentication tokens.
- Handle network errors without exposing sensitive information.
- Avoid transmitting unnecessary personal data.
- Apply backend authorization to every protected operation.

A mobile application's API endpoint is not private simply because it is embedded in the application package.

## 7. Certificate Pinning and Its Limitations

Certificate or public-key pinning restricts which server certificates or public keys a mobile application accepts.

It can provide an additional defense against certain interception scenarios.

### Security Considerations

- Prefer platform-supported networking and trust mechanisms.
- Understand certificate renewal and key rotation requirements.
- Define backup pins when appropriate.
- Plan recovery procedures for pinning failures.
- Test certificate rotation before production deployment.
- Avoid treating pinning as a substitute for correct TLS configuration.

Pinning increases operational complexity and is not a complete defense against a compromised device or application.

A security review should assess whether pinning provides meaningful protection for the application's threat model.

## 8. Mobile Permissions and Privacy

Mobile operating systems allow applications to request access to sensitive device capabilities and user information.

Examples include:

- Camera.
- Microphone.
- Location.
- Contacts.
- Photos and media.
- Bluetooth.
- Notifications.
- Files and documents.

### Security Recommendations

- Request permissions only when functionality requires them.
- Explain permission usage clearly.
- Prefer limited or temporary access where available.
- Handle permission denial safely.
- Avoid collecting unnecessary personal information.
- Review background location and microphone access.
- Protect information obtained through device permissions.
- Reassess permissions when application features change.

Permission access should follow the principle of least privilege.

## 9. Hardcoded Secrets and API Keys

Application packages can be inspected and reverse-engineered.

Credentials embedded in mobile binaries, resources or configuration files should therefore be considered potentially public.

### Common Exposure Locations

- Source code.
- Build configuration.
- Application resources.
- Environment configuration included in builds.
- Native libraries.
- JavaScript bundles.
- Debug logs.
- Third-party SDK configuration.

### Security Recommendations

- Never embed privileged backend credentials in mobile applications.
- Keep sensitive secrets on trusted backend systems.
- Use restricted public client keys only when the provider explicitly supports that security model.
- Apply quotas and restrictions to public identifiers where supported.
- Rotate exposed credentials.
- Review release packages for accidental secrets.
- Avoid committing production credentials to Git.

Obfuscation does not turn a hardcoded secret into a secure secret.

## 10. Reverse Engineering and Application Tampering

Attackers can inspect mobile application packages, analyze code, modify application behavior and redistribute altered builds.

### Potential Risks

- Exposed configuration.
- Extracted credentials.
- Modified client-side permission checks.
- Altered application workflows.
- Manipulated API requests.
- Unauthorized application redistribution.

### Security Recommendations

- Keep sensitive business logic on the backend.
- Enforce authorization independently of the client.
- Remove debug information from production builds where appropriate.
- Use code shrinking and obfuscation as defense-in-depth measures.
- Sign release packages securely.
- Protect signing keys.
- Monitor application integrity where the threat model justifies it.

Obfuscation increases reverse-engineering effort but does not guarantee confidentiality or application integrity.

## 11. Root and Jailbreak Detection

Rooted Android devices and jailbroken iOS devices may have weakened platform security boundaries.

Applications sometimes use device integrity checks to identify potentially compromised environments.

### Security Recommendations

- Assess device integrity where appropriate.
- Use platform-supported integrity mechanisms.
- Treat detection as a risk signal rather than absolute proof.
- Avoid relying on client-side checks as the only security control.
- Enforce sensitive operations on the backend.
- Define proportionate responses to detected risks.
- Avoid unnecessarily blocking legitimate users.

Root and jailbreak detection can be bypassed. It should not replace server-side authorization or secure data handling.

## 12. Deep Links and Universal Links

Deep links allow mobile applications to open specific screens or resources from external links.

Improper handling can expose sensitive functionality or create unsafe navigation behavior.

### Security Recommendations

- Validate incoming URLs.
- Restrict accepted schemes and destinations.
- Validate parameters before processing them.
- Require authentication for protected resources.
- Enforce authorization after opening a deep link.
- Avoid trusting user identifiers supplied in link parameters.
- Configure Android App Links and iOS Universal Links appropriately.
- Protect sensitive actions against unwanted invocation.

A valid deep link does not imply that the requesting user is authorized to access its destination.

## 13. WebView Security

WebViews allow mobile applications to display web content inside the application.

Incorrect configuration can introduce risks involving JavaScript execution, local file access and communication between web content and native code.

### Security Recommendations

- Load only trusted content where possible.
- Avoid exposing native bridges to untrusted web content.
- Restrict file access.
- Disable unnecessary JavaScript functionality.
- Validate navigation destinations.
- Avoid loading arbitrary external URLs.
- Review mixed-content behavior.
- Keep WebView implementations and dependencies updated.

Native application interfaces exposed through JavaScript bridges must be treated as security-sensitive APIs.

## 14. Cryptography and Key Management

Mobile applications sometimes require encryption for locally stored information, secure communication or sensitive operations.

### Security Recommendations

- Use established platform cryptographic APIs.
- Avoid implementing cryptographic algorithms yourself.
- Use secure random number generators.
- Protect encryption keys using Android Keystore or iOS Keychain and available hardware-backed facilities.
- Define key rotation and recovery strategies.
- Avoid hardcoded encryption keys.
- Use appropriate algorithms and authenticated encryption modes.
- Review key access permissions.

Do not assume that encrypting a database is sufficient if its encryption key is stored alongside it without adequate protection.

## 15. Mobile File Handling and Backups

Mobile applications may create temporary files, export reports, download documents and store user-generated content.

### Security Recommendations

- Store private files in application-private locations.
- Apply appropriate file permissions.
- Exclude sensitive files from backups when required.
- Validate downloaded file content.
- Use secure temporary file handling.
- Avoid exposing private files through unrestricted content providers.
- Restrict file-sharing destinations.
- Protect exported documents.
- Remove sensitive temporary data when appropriate.

Review platform-specific backup behavior and file-sharing configuration before release.

## 16. Notifications and Clipboard Security

Notifications and clipboard functionality can unintentionally expose sensitive information.

### Security Recommendations

- Avoid displaying passwords, tokens or sensitive personal data in notifications.
- Use privacy-conscious notification previews.
- Require appropriate authentication before opening sensitive notification destinations.
- Avoid copying credentials to the clipboard unnecessarily.
- Clear sensitive clipboard content where platform behavior permits.
- Validate data received through clipboard or notification interactions.

Consider how sensitive information appears on lock screens and shared devices.

## 17. Third-Party SDK and Dependency Security

Mobile applications frequently integrate analytics, crash reporting, advertising, payment, authentication and other third-party SDKs.

### Security Recommendations

- Use trusted and maintained SDKs.
- Review SDK permissions and data collection.
- Keep dependencies updated.
- Remove unused SDKs.
- Monitor security advisories.
- Review SDK network communication.
- Avoid exposing privileged credentials through SDK configuration.
- Apply appropriate privacy disclosures.
- Assess software supply-chain risks.

A third-party SDK can introduce security and privacy risks even when the application's own code is secure.

## 18. Build, Signing and Release Security

Mobile release processes are part of the application's security boundary.

### Security Recommendations

- Protect Android signing keys and iOS signing credentials.
- Restrict access to release infrastructure.
- Separate development and production configurations.
- Avoid shipping debug builds.
- Protect CI/CD secrets.
- Review build artifacts before distribution.
- Use reproducible or verifiable build practices where practical.
- Maintain an application release and update process.
- Monitor signing certificate and provisioning changes.

Compromised signing credentials can undermine the trustworthiness of distributed application updates.

## 19. Mobile Application Security Testing

Mobile applications should be assessed on both real devices and suitable test environments.

### Essential Test Scenarios

- Sensitive data is not stored insecurely.
- Authentication tokens use appropriate platform storage.
- Network communication validates TLS certificates.
- Privileged credentials are absent from release packages.
- Android exported components are appropriately restricted.
- iOS entitlements and URL schemes are reviewed.
- Deep links enforce authentication and authorization.
- WebViews do not expose unsafe native interfaces.
- File access and backup behavior are appropriate.
- Debug functionality is disabled in release builds.
- Root and jailbreak detection is not the only security boundary.
- Third-party SDK permissions and data collection are reviewed.
- Application signing credentials are protected.
- Backend authorization cannot be bypassed by modifying the mobile client.

### Testing Approaches

- Static Application Security Testing (SAST).
- Mobile application package inspection.
- Dynamic Application Security Testing (DAST).
- API security testing.
- Device-based manual testing.
- Dependency and SDK analysis.
- Authorized penetration testing.

Use test accounts, controlled devices and non-production environments wherever practical.

## 20. Common Mobile Security Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Insecure local storage | Exposure of sensitive information |
| Hardcoded secrets | Credential compromise |
| Weak authentication | Account takeover |
| Insecure network communication | Data interception |
| Improper certificate validation | Connection interception |
| Excessive permissions | Unnecessary access to device information |
| Insecure deep links | Unauthorized application actions |
| Unsafe WebView configuration | Script execution or data exposure |
| Application tampering | Modified application behavior |
| Weak cryptographic key management | Exposure of protected data |
| Insecure file handling | Unauthorized file access |
| Vulnerable third-party SDKs | Application compromise or data exposure |

## 21. Remediation Priorities

When mobile security weaknesses are identified:

1. Address exposed credentials and sensitive user information.
2. Fix authentication and authorization weaknesses.
3. Secure local storage and cryptographic key handling.
4. Correct insecure network communication.
5. Restrict unsafe application components and deep links.
6. Review WebViews, file handling and backups.
7. Remove unnecessary permissions and exposed secrets.
8. Protect build, signing and release infrastructure.
9. Update vulnerable dependencies and third-party SDKs.
10. Add automated security regression tests.

Prioritize remediation according to exploitability, data sensitivity and potential impact.

## 22. References and Further Reading

### OWASP Mobile Security

- [OWASP Mobile Application Security](https://mas.owasp.org/)
- [OWASP Mobile Application Security Verification Standard (MASVS)](https://mas.owasp.org/MASVS/)
- [OWASP Mobile Application Security Testing Guide (MASTG)](https://mas.owasp.org/MASTG/)
- [OWASP Mobile Application Security Checklist](https://mas.owasp.org/checklists/)

### Official Platform Documentation

- [Android Security Best Practices](https://developer.android.com/privacy-and-security/security-best-practices)
- [Android Keystore System](https://developer.android.com/privacy-and-security/keystore)
- [Android Network Security Configuration](https://developer.android.com/privacy-and-security/security-config)
- [Apple Platform Security](https://support.apple.com/guide/security/welcome/web)
- [Apple Keychain Services](https://developer.apple.com/documentation/security/keychain_services)

### Framework Documentation

- [Flutter Security](https://docs.flutter.dev/)
- [React Native Security](https://reactnative.dev/docs/security)

---

This guide is educational and does not replace application-specific security reviews, mobile testing or professional security assessments.
