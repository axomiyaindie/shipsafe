
# Authentication Security

Authentication is the process of verifying the identity of a user, service or system attempting to access an application.

Poor authentication design can expose applications to account takeover, credential theft, unauthorized access and data breaches.

This guide covers authentication security principles, common vulnerabilities, secure implementation practices and verification procedures.

## 1. Authentication Fundamentals

Authentication establishes who or what is requesting access.

Common authentication factors include:

- Something the user knows: passwords and PINs.
- Something the user has: hardware security keys and authenticator devices.
- Something the user is: biometric characteristics.

Authentication must be distinguished from authorization. Authentication verifies identity; authorization determines what an authenticated identity is permitted to do.

## 2. Password Security

Passwords remain a common authentication mechanism and must be handled carefully.

### Security Requirements

- Never store passwords in plaintext.
- Never use reversible encryption as a substitute for password hashing.
- Use a dedicated password-hashing algorithm such as Argon2id.
- Consider scrypt, bcrypt or PBKDF2 where appropriate.
- Generate unique salts using established password-hashing libraries.
- Use parameters appropriate for the deployment environment.
- Support password changes and secure account recovery.
- Never log passwords or include them in error messages.

### Common Vulnerabilities

- Plaintext password storage.
- Fast, general-purpose hashes such as SHA-256 used directly for password storage.
- Weak password reset mechanisms.
- Password reuse.
- Credential stuffing.
- Inadequate protection against brute-force attacks.

### Prevention

Use established password-hashing libraries rather than implementing cryptographic algorithms yourself.

Review the OWASP Password Storage Cheat Sheet for algorithm selection and configuration guidance.

## 3. Multi-Factor Authentication

Multi-factor authentication (MFA) requires multiple independent authentication factors.

Common approaches:

- Passkeys and WebAuthn.
- Hardware security keys.
- Authenticator applications.
- Time-based one-time passwords.
- Recovery codes.

### Security Recommendations

- Prefer phishing-resistant authentication such as passkeys and hardware security keys.
- Protect MFA enrollment and removal.
- Require appropriate reauthentication before changing authentication factors.
- Provide secure recovery procedures.
- Avoid treating SMS-based authentication as phishing-resistant.
- Notify users about significant authentication changes.

## 4. OAuth 2.0 and OpenID Connect

OAuth 2.0 is an authorization framework. OpenID Connect (OIDC) adds an identity layer for authentication.

### Security Recommendations

- Use Authorization Code Flow with PKCE for public clients.
- Validate redirect URIs strictly.
- Validate OIDC ID tokens, including signature, issuer, audience and expiration.
- Validate state and nonce where applicable.
- Avoid implicit-flow implementations for modern applications.
- Store tokens securely.
- Apply appropriate access-token scopes and lifetimes.
- Never assume that possession of an access token alone establishes the user's identity.

Use the official OAuth and OpenID Connect specifications for implementation decisions.

## 5. JSON Web Token Security

JWT is a token format, not an authentication system by itself.

### Common Vulnerabilities

- Accepting unsigned or incorrectly signed tokens.
- Failing to validate issuer or audience.
- Accepting unexpected signing algorithms.
- Long-lived access tokens.
- Exposing tokens through URLs or logs.
- Incorrect key management.
- Trusting unverified token claims.

### Prevention

- Verify signatures using trusted keys.
- Explicitly allow expected signing algorithms.
- Validate issuer, audience, expiration and other required claims.
- Reject tokens with invalid or unexpected claims.
- Establish a token rotation and revocation strategy.
- Keep signing keys out of source code.

## 6. Session Management

Sessions maintain authenticated state between requests.

### Security Recommendations

- Use cryptographically secure session identifiers.
- Regenerate session identifiers after authentication and privilege changes.
- Invalidate sessions during logout.
- Configure appropriate session expiration.
- Protect browser cookies with Secure, HttpOnly and appropriate SameSite attributes.
- Avoid placing session identifiers in URLs.
- Consider session revocation after password changes or account compromise.
- Protect against session fixation and hijacking.

## 7. Password Reset and Account Recovery

Account recovery mechanisms must be protected as carefully as normal authentication.

### Security Recommendations

- Generate unpredictable, single-use recovery tokens.
- Use short expiration periods.
- Store recovery tokens securely.
- Avoid revealing whether an account exists.
- Invalidate recovery tokens after successful use.
- Notify users about password changes.
- Require appropriate additional verification for sensitive recovery actions.

## 8. Brute-Force and Credential-Stuffing Protection

Automated login attacks attempt to guess passwords or reuse credentials exposed in other breaches.

### Prevention

- Implement risk-based rate limiting.
- Monitor suspicious authentication patterns.
- Apply progressive delays where appropriate.
- Use MFA and passkeys.
- Detect credential-stuffing behavior.
- Avoid account lockout policies that enable denial-of-service attacks.
- Provide appropriate security notifications.

## 9. Common Authentication Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Weak passwords | Account compromise |
| Plaintext password storage | Credential exposure |
| Missing MFA | Increased account takeover risk |
| Session fixation | Session compromise |
| Weak recovery tokens | Account takeover |
| JWT validation failures | Token forgery or misuse |
| Credential stuffing | Unauthorized account access |
| Insecure OAuth implementation | Authentication or authorization compromise |

## 10. Secure Implementation and Verification

Before deploying authentication functionality, verify:

- Passwords use an appropriate dedicated password-hashing algorithm.
- Authentication endpoints use HTTPS.
- Login attempts are protected against automated abuse.
- MFA enrollment and recovery are protected.
- Session identifiers are securely generated.
- Session cookies have appropriate security attributes.
- Logout invalidates the relevant session.
- Password reset tokens are single-use and expire.
- JWT validation is correctly implemented wherever JWTs are used.
- OAuth and OIDC flows follow the applicable specifications.
- Sensitive authentication information is never written to logs.
- Authorization is independently enforced on protected resources.

Use automated tests and authorized security testing to verify these controls.

## 11. Remediation Priorities

When authentication weaknesses are identified:

1. Address exposed credentials immediately.
2. Replace insecure password storage with a dedicated password-hashing solution.
3. Correct authentication and token validation failures.
4. Secure account recovery mechanisms.
5. Fix session management weaknesses.
6. Introduce appropriate abuse prevention and monitoring.
7. Add regression tests to prevent recurrence.

Prioritize remediation according to exploitability, impact and exposure.

## 12. References and Further Reading

### OWASP

- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html)

### Standards

- [NIST Digital Identity Guidelines](https://pages.nist.gov/800-63-4/)
- [OAuth 2.0 Security Best Current Practice](https://www.rfc-editor.org/rfc/rfc9700)
- [OpenID Connect Specifications](https://openid.net/developers/specs/)

---

This guide is educational and does not replace application-specific security reviews, implementation testing or professional security assessments.
