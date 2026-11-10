
# Web Application Security

Web application security focuses on protecting websites, web applications, browsers, servers and the data exchanged between them.

Modern web applications combine frontend frameworks, backend services, APIs, authentication systems, databases, third-party integrations and browser technologies.

A vulnerability in any of these layers can expose user information, compromise accounts or allow unauthorized operations.

This guide covers web security fundamentals, common vulnerabilities, secure implementation practices and verification procedures.

## 1. Web Security Fundamentals

Web security protects applications against unauthorized access, data exposure, malicious input, session compromise and service disruption.

Common web application technologies include:

- HTML, CSS and JavaScript.
- React, Next.js, Vue and Angular.
- Node.js, Express and other backend frameworks.
- Server-rendered and static web applications.
- Progressive Web Applications (PWAs).
- Reverse proxies, web servers and content delivery networks.

### Core Security Principles

- Use HTTPS throughout production environments.
- Validate untrusted input on the server.
- Encode output according to its context.
- Enforce authentication and authorization server-side.
- Protect sessions and cookies.
- Apply appropriate browser security policies.
- Minimize exposed data and functionality.
- Keep dependencies and frameworks updated.
- Monitor suspicious application activity.
- Test security controls before production deployment.

HTTPS protects data in transit, but does not prevent application vulnerabilities such as XSS, broken access control or SQL injection.

## 2. Cross-Site Scripting (XSS)

Cross-Site Scripting occurs when an application allows untrusted content to execute as script in another user's browser.

XSS can expose sensitive information, manipulate application behavior or perform actions within a victim's authenticated browser context.

### Common Types

**Stored XSS**

Malicious content is stored by the application and later rendered to other users.

**Reflected XSS**

Untrusted request data is included in a response without appropriate handling.

**DOM-Based XSS**

Client-side JavaScript processes untrusted data in an unsafe way, resulting in script execution.

### Common Risk Areas

- User-generated comments.
- Search results.
- Profile information.
- Rich-text editors.
- URL parameters.
- Dynamic HTML rendering.
- Third-party content integrations.

### Prevention

- Use framework-provided output escaping.
- Avoid unsafe HTML rendering APIs.
- Sanitize HTML when rich-text content is required.
- Use context-aware output encoding.
- Avoid inserting untrusted values into executable JavaScript.
- Validate data at appropriate trust boundaries.
- Implement a restrictive Content Security Policy.
- Keep frontend dependencies updated.

Input validation alone does not prevent every XSS vulnerability. Output handling must match the context in which data is rendered.

## 3. Cross-Site Request Forgery (CSRF)

CSRF occurs when an attacker causes a user's browser to submit an unwanted request to an application where the user is already authenticated.

It is particularly relevant to applications that authenticate users through automatically attached browser cookies.

### Potential Impact

- Unauthorized profile changes.
- Account setting modifications.
- Unwanted financial or administrative operations.
- Unauthorized resource creation or deletion.

### Prevention

- Use framework-supported CSRF protection.
- Apply unpredictable CSRF tokens where appropriate.
- Configure cookies with suitable SameSite attributes.
- Validate request origins for sensitive operations where appropriate.
- Require appropriate request methods for state-changing operations.
- Avoid state-changing GET requests.
- Reauthenticate users for particularly sensitive actions.

SameSite cookies provide useful protection but should not be treated as a universal replacement for a complete CSRF defense.

Applications using bearer tokens explicitly supplied through an Authorization header have different CSRF considerations. They still require protection against token theft and XSS.

## 4. Content Security Policy (CSP)

Content Security Policy is a browser security mechanism that restricts which resources a page can load or execute.

It can reduce the impact of certain injection vulnerabilities, especially XSS.

### Important Directives

| Directive | Purpose |
|---|---|
| `default-src` | Default resource loading policy |
| `script-src` | Controls permitted scripts |
| `style-src` | Controls permitted stylesheets |
| `img-src` | Controls permitted images |
| `font-src` | Controls permitted fonts |
| `connect-src` | Controls connections such as fetch and WebSocket |
| `object-src` | Controls plugin content |
| `frame-ancestors` | Controls which sites may embed the page |
| `base-uri` | Restricts permitted base URLs |
| `form-action` | Restricts form submission destinations |

### Security Recommendations

- Start with a policy appropriate to the application's resource requirements.
- Prefer nonces or hashes for permitted inline scripts where needed.
- Avoid unrestricted `unsafe-inline` and `unsafe-eval`.
- Restrict resource sources to trusted origins.
- Use report-only mode to evaluate policies before enforcement.
- Monitor CSP violation reports.
- Review policies whenever third-party integrations change.

A CSP must be tested against the actual application. An overly permissive policy provides limited protection, while an incorrectly restrictive policy can break application functionality.

## 5. Clickjacking and Frame Protection

Clickjacking occurs when an attacker tricks a user into interacting with an application through an embedded or disguised interface.

### Prevention

Use appropriate browser frame restrictions:

- `Content-Security-Policy: frame-ancestors`
- `X-Frame-Options` for compatible legacy browser scenarios.

### Security Recommendations

- Prevent unauthorized websites from embedding sensitive application pages.
- Allow trusted embedding origins only when required.
- Review embedded authentication and payment workflows.
- Test frame restrictions across important application routes.

`frame-ancestors` is the modern CSP mechanism for controlling who may embed a page.

## 6. Secure Cookies and Browser Storage

Browser storage and cookies can contain authentication credentials, session identifiers and user-specific information.

### Secure Cookie Attributes

| Attribute | Security purpose |
|---|---|
| `Secure` | Sends the cookie over secure connections |
| `HttpOnly` | Prevents ordinary JavaScript access to the cookie |
| `SameSite` | Restricts certain cross-site cookie behavior |
| `Path` | Limits the cookie's URL path scope |
| `Domain` | Controls the cookie's domain scope |

### Security Recommendations

- Use Secure cookies in production.
- Use HttpOnly for session cookies that do not require JavaScript access.
- Choose an appropriate SameSite policy.
- Use narrow cookie scope where practical.
- Set suitable expiration and lifetime policies.
- Regenerate session identifiers after authentication.
- Invalidate sessions when users log out.
- Avoid placing sensitive credentials in localStorage.
- Avoid exposing session identifiers through URLs.

Browser storage is accessible to JavaScript running in the application's origin. An XSS vulnerability can therefore expose sensitive data stored there.

## 7. HTTP Security Headers

HTTP response headers provide browser security controls and help reduce exposure to common web attacks.

### Important Headers

| Header | Purpose |
|---|---|
| `Strict-Transport-Security` | Enforces HTTPS for applicable future requests |
| `Content-Security-Policy` | Restricts resource loading and execution |
| `X-Content-Type-Options` | Prevents MIME-type sniffing when set to `nosniff` |
| `Referrer-Policy` | Controls referrer information |
| `Permissions-Policy` | Restricts selected browser features |
| `X-Frame-Options` | Provides legacy frame protection |

### Security Recommendations

- Configure headers at the application or trusted edge layer.
- Review headers across all relevant routes.
- Avoid conflicting policies between reverse proxies and application servers.
- Test the actual response headers delivered to browsers.
- Remove unnecessary technology-disclosure headers where practical.

HSTS should only be enabled after HTTPS is correctly configured. Subdomain and preload directives require additional planning.

## 8. Same-Origin Policy and CORS

The Same-Origin Policy restricts how browser scripts interact with resources from different origins.

Cross-Origin Resource Sharing (CORS) allows servers to specify which browser origins may access certain responses.

### Security Recommendations

- Allow only required origins.
- Avoid reflecting arbitrary Origin headers.
- Do not combine wildcard origins with credentialed requests.
- Restrict allowed methods and request headers.
- Configure credential handling carefully.
- Validate preflight behavior.
- Review development and production origin differences.

CORS is not authentication or authorization. Non-browser clients can make requests without following browser CORS enforcement.

## 9. Input Validation and Output Encoding

Web applications receive untrusted input from forms, URLs, headers, cookies, uploaded files and external services.

### Security Recommendations

- Validate input on the server.
- Define explicit schemas and allowed values.
- Enforce type, length and range restrictions.
- Reject unexpected fields where appropriate.
- Use parameterized database queries.
- Encode output for its intended context.
- Sanitize HTML when HTML input is genuinely required.
- Avoid evaluating untrusted strings as code.

Validation, sanitization and output encoding solve different problems. They should not be treated as interchangeable controls.

## 10. Open Redirects

An open redirect occurs when an application redirects users to destinations controlled by untrusted input.

Attackers may use open redirects in phishing campaigns or to make malicious destinations appear trustworthy.

### Common Risk Areas

- Login redirect parameters.
- Return URL parameters.
- Post-authentication navigation.
- External link handling.
- Redirect functionality in API responses.

### Prevention

- Prefer predefined internal destinations.
- Validate redirect targets against strict allowlists.
- Avoid accepting arbitrary external URLs.
- Normalize and validate URLs carefully.
- Reject unexpected schemes and malformed destinations.

## 11. Path Traversal and Directory Listing

Path traversal occurs when untrusted input manipulates file paths to access resources outside the intended directory.

Directory listing exposes files and directory structures that should not be publicly accessible.

### Prevention

- Avoid building file paths directly from untrusted input.
- Use server-generated file identifiers.
- Resolve and validate canonical paths.
- Restrict file access to approved storage locations.
- Disable unnecessary directory listing.
- Apply appropriate filesystem permissions.
- Avoid exposing source files, configuration files and backups.
- Test encoded and normalized path variations.

## 12. File Upload Security

File uploads can introduce malware, executable content, storage abuse and unauthorized file access.

### Security Recommendations

- Restrict upload size.
- Allow only required file types.
- Validate actual file content rather than trusting extensions.
- Generate server-side filenames.
- Store uploads outside executable application directories.
- Prevent path traversal.
- Apply malware scanning where appropriate.
- Restrict access to private uploads.
- Avoid serving untrusted files from the application's trusted origin without appropriate controls.
- Use safe download response headers.

File type checks based only on client-supplied Content-Type headers are insufficient.

## 13. Server-Side Request Forgery (SSRF)

SSRF occurs when an application can be manipulated into making unintended server-side network requests.

### Common Risk Areas

- URL preview services.
- Remote image processing.
- Document import functionality.
- Webhook configuration.
- External API integrations.
- Server-side file retrieval.

### Prevention

- Avoid arbitrary server-side URL fetching where possible.
- Use destination allowlists.
- Block access to internal and reserved network ranges.
- Validate DNS resolution and destination addresses.
- Handle redirects safely.
- Apply outbound network restrictions.
- Account for DNS rebinding.
- Restrict access to cloud metadata services.

Application-level validation should be combined with network-level controls.

## 14. Authentication and Session Security

Web applications must protect user identities and authenticated sessions throughout their lifecycle.

### Security Recommendations

- Use secure authentication mechanisms.
- Support MFA or passkeys where appropriate.
- Protect password recovery workflows.
- Regenerate sessions after authentication and privilege changes.
- Apply appropriate session expiration.
- Invalidate sessions during logout.
- Protect sensitive account changes with reauthentication.
- Notify users about important security events.
- Enforce authorization on every protected server-side operation.

Frontend route guards improve navigation and user experience but are not security boundaries.

## 15. Browser and Frontend Security

Frontend code executes in an environment that users can inspect, modify and control.

### Security Recommendations

- Never embed privileged API keys or database credentials in frontend bundles.
- Treat browser input and stored state as untrusted.
- Avoid unsafe DOM manipulation.
- Review third-party scripts.
- Restrict unnecessary browser permissions.
- Avoid exposing internal application information.
- Use secure dependency management.
- Protect authentication tokens.
- Keep sensitive authorization decisions on the server.

Anything shipped to a user's browser should be considered accessible to that user.

## 16. Third-Party Scripts and Integrations

Third-party scripts can access application page content and may introduce additional security and privacy risks.

### Security Recommendations

- Load scripts only from trusted sources.
- Minimize third-party script usage.
- Review requested permissions and data access.
- Use Subresource Integrity (SRI) where applicable to externally hosted static resources.
- Apply CSP restrictions.
- Monitor changes to third-party dependencies.
- Review analytics, advertising and chat integrations.
- Avoid giving unnecessary access to sensitive application pages.

A compromised third-party script can affect every page on which it executes.

## 17. Web Security Testing

Web application security should be evaluated throughout development and before production deployment.

### Essential Test Scenarios

- HTTPS is enforced.
- Security headers match the application's requirements.
- XSS payloads are handled safely.
- CSRF protections work on relevant state-changing operations.
- Unauthorized embedding is restricted.
- Cookies have appropriate security attributes.
- CORS policies allow only intended origins.
- Redirect destinations are validated.
- File uploads enforce security restrictions.
- Path traversal attempts are rejected.
- SSRF protections prevent unintended network access.
- Authentication and authorization controls are enforced server-side.
- Sensitive data is not exposed through browser storage or frontend bundles.
- Third-party scripts and dependencies are reviewed.
- Production errors do not reveal sensitive implementation details.

### Testing Approaches

- Automated security tests.
- Browser-based manual testing.
- Static application security testing.
- Dynamic application security testing.
- Dependency scanning.
- Authorized penetration testing.
- Security header and TLS configuration reviews.

Test using authorized accounts and non-production environments wherever practical.

## 18. Common Web Application Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Cross-Site Scripting | Script execution in users' browsers |
| Cross-Site Request Forgery | Unwanted authenticated actions |
| Clickjacking | Deceptive user interactions |
| Open Redirect | Phishing and unsafe navigation |
| Path Traversal | Unauthorized file access |
| SSRF | Unauthorized server-side network requests |
| Insecure Cookies | Session exposure or compromise |
| Weak CSP | Reduced browser-side injection protection |
| Broken Access Control | Unauthorized functionality or data access |
| File Upload Vulnerabilities | Malware, unauthorized access or code execution |
| Security Misconfiguration | Unintended exposure of application resources |

## 19. Remediation Priorities

When web security weaknesses are identified:

1. Address active exploitation and exposed sensitive information.
2. Fix broken authentication and authorization controls.
3. Correct XSS and injection vulnerabilities.
4. Protect sessions and sensitive browser data.
5. Implement appropriate CSRF and framing protections.
6. Correct CORS and security header configuration.
7. Secure file uploads, redirects and file access.
8. Address SSRF and third-party integration risks.
9. Review frontend dependencies and exposed configuration.
10. Add automated regression tests.

Prioritize remediation according to exploitability, exposure and potential impact.

## 20. References and Further Reading

### OWASP

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP Cross-Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
- [OWASP Clickjacking Defense Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html)
- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
- [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

### Web Standards

- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
- [MDN HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers)
- [W3C Content Security Policy](https://www.w3.org/TR/CSP3/)

---

This guide is educational and does not replace application-specific security reviews, implementation testing or professional security assessments.
