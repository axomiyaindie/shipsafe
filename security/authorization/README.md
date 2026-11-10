
# Authorization Security

Authorization determines what an authenticated user, service or system is permitted to access and perform.

Authentication verifies identity. Authorization evaluates permissions.

A system can have perfectly implemented authentication and still be vulnerable if users can access another user's information, perform unauthorized actions or escalate their privileges.

This guide covers authorization models, common access-control vulnerabilities, secure implementation practices and security verification.

## 1. Authorization Fundamentals

Authorization should answer three questions:

1. Who is making the request?
2. What resource or operation are they trying to access?
3. Is that identity permitted to perform that operation on that specific resource?

Authorization decisions should be enforced by trusted server-side components.

Never rely exclusively on frontend controls, hidden buttons, client-side route guards or values supplied by the client.

## 2. Authentication vs. Authorization

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permissions |
| Establishes who the user is | Establishes what the user can do |
| Uses passwords, passkeys or MFA | Uses roles, policies and permissions |
| Usually occurs during sign-in | Must be evaluated when protected resources are accessed |

Both controls are necessary for application security.

## 3. Role-Based Access Control (RBAC)

RBAC assigns permissions to roles, and roles to users.

Example roles:

- Administrator
- Manager
- Editor
- Member
- Viewer

### Example Permission Model

| Role | View records | Create records | Edit records | Delete records |
|---|---|---|---|---|
| Administrator | Yes | Yes | Yes | Yes |
| Manager | Yes | Yes | Yes | No |
| Editor | Yes | Yes | Yes | No |
| Viewer | Yes | No | No | No |

### Security Recommendations

- Follow the principle of least privilege.
- Define permissions centrally.
- Avoid assigning administrator privileges by default.
- Validate role changes on the server.
- Restrict access to role-management functionality.
- Review permissions whenever responsibilities change.
- Avoid relying on role names alone when resource ownership also matters.

RBAC is useful for applications with clearly defined organizational roles.

## 4. Attribute-Based Access Control (ABAC)

ABAC evaluates policies using attributes associated with the user, resource, action and environment.

Example:

A user may edit a document only when:

- The user belongs to the document's organization.
- The user has editing permission.
- The document is not locked.
- The requested operation is permitted by organizational policy.

ABAC is useful when permissions depend on context rather than only predefined roles.

### Security Recommendations

- Define policies explicitly.
- Validate attributes using trusted server-side data.
- Avoid trusting client-supplied ownership or organization identifiers.
- Test policy combinations and exceptional conditions.
- Keep policy evaluation consistent across application services.

## 5. Broken Access Control

Broken Access Control occurs when an application fails to correctly enforce authorization restrictions.

Examples:

- A regular user accesses an administrator endpoint.
- A user edits another user's profile.
- A customer retrieves another customer's invoice.
- An unauthorized user changes account settings.
- A user accesses restricted files by changing a URL.

### Prevention

- Enforce authorization on every protected server-side operation.
- Deny access by default.
- Verify ownership and permissions for each resource.
- Apply consistent authorization policies across APIs and application services.
- Test direct requests rather than relying only on the user interface.

## 6. IDOR and BOLA Vulnerabilities

### Insecure Direct Object Reference (IDOR)

IDOR occurs when an application exposes an object reference and fails to verify whether the requesting user is authorized to access that object.

Example:

A user accesses:

`GET /api/invoices/1001`

They change the identifier:

`GET /api/invoices/1002`

If invoice 1002 belongs to another customer and the server returns it without authorization checks, the application has an IDOR vulnerability.

Using UUIDs instead of sequential identifiers does not fix missing authorization checks.

### Broken Object Level Authorization (BOLA)

BOLA is an API security vulnerability in which an API fails to enforce authorization at the object level.

It can expose records, documents, orders, profiles and other resources belonging to different users or organizations.

### Prevention

- Verify resource ownership or an explicit sharing permission.
- Apply authorization checks to every object lookup.
- Scope database queries to the authenticated user's permitted resources.
- Never assume that an unpredictable identifier provides authorization.
- Test requests using multiple user accounts.

## 7. Privilege Escalation

Privilege escalation occurs when a user gains permissions beyond those intended.

### Vertical Privilege Escalation

A user obtains higher-level permissions.

Example: A normal member gains administrator access.

### Horizontal Privilege Escalation

A user accesses resources belonging to another user with equivalent privileges.

Example: One customer accesses another customer's account information.

### Prevention

- Validate role assignments on the server.
- Restrict administrative operations.
- Prevent users from modifying protected role or permission fields.
- Verify resource ownership.
- Audit sensitive permission changes.
- Test both vertical and horizontal access boundaries.

## 8. Multi-Tenant Authorization

Multi-tenant applications serve multiple organizations or customers within a shared application infrastructure.

Incorrect tenant isolation can expose one organization's information to another.

### Example

An application stores:

- Organization A: Customer records, invoices and projects.
- Organization B: Customer records, invoices and projects.

A user belonging to Organization A must not be able to retrieve Organization B's data without an explicitly authorized cross-tenant permission.

### Security Recommendations

- Derive tenant identity from a trusted authenticated context.
- Verify tenant membership and resource ownership server-side.
- Scope database queries to the authorized tenant.
- Validate tenant boundaries in background jobs and administrative operations.
- Protect file storage and object access across tenants.
- Apply tenant-aware authorization to APIs, exports, search and reporting.
- Test cross-tenant access using separate accounts.

Do not trust a tenant ID supplied by the browser or mobile application without independently verifying it.

## 9. API and Resource-Level Authorization

Every API operation should enforce the permissions required for that operation.

Consider these endpoints:

| Endpoint | Required authorization |
|---|---|
| `GET /api/users/123` | Permission to view user 123 |
| `PATCH /api/users/123` | Permission to modify user 123 |
| `DELETE /api/users/123` | Permission to delete user 123 |
| `GET /api/admin/reports` | Administrative reporting permission |
| `POST /api/organizations` | Permission to create an organization |

### Security Recommendations

- Enforce authorization independently of HTTP method.
- Protect list, detail, search and export endpoints.
- Validate permissions for nested resources.
- Prevent mass assignment of protected fields.
- Check authorization before performing sensitive operations.
- Return appropriate error responses without exposing protected information.

## 10. Server-Side Authorization Enforcement

Frontend restrictions improve user experience but are not security boundaries.

An attacker can bypass the interface and send requests directly to the backend.

### Required Practices

- Authenticate requests using a trusted identity mechanism.
- Load sensitive permissions from trusted server-side sources.
- Verify resource ownership or explicit access rights.
- Apply authorization checks before executing protected operations.
- Reject unauthorized operations consistently.
- Avoid trusting user IDs, role names or tenant IDs supplied by clients.
- Ensure background jobs and internal services enforce equivalent access policies.

## 11. Authorization Testing

Test authorization using separate accounts with different roles and ownership boundaries.

### Essential Test Scenarios

- Anonymous users cannot access protected resources.
- Regular users cannot access administrator functionality.
- Users cannot read another user's private records.
- Users cannot modify another user's resources.
- Users cannot delete resources they do not own.
- Users cannot change their own role through ordinary profile updates.
- Users cannot access another tenant's data.
- Restricted operations remain protected through direct API requests.
- Nested resources enforce the correct ownership checks.
- Permission changes take effect as intended.

### Testing Approaches

- Automated authorization unit tests.
- API integration tests.
- Negative permission tests.
- Manual access-control testing.
- Authorized penetration testing.

## 12. Common Authorization Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Broken Access Control | Unauthorized functionality or data access |
| IDOR | Access to another user's resources |
| BOLA | Unauthorized API object access |
| Vertical privilege escalation | Unauthorized administrative privileges |
| Horizontal privilege escalation | Access to another user's resources |
| Missing tenant isolation | Cross-organization data exposure |
| Mass assignment | Unauthorized modification of protected fields |
| Client-side-only authorization | Bypass of access restrictions |

## 13. Remediation Priorities

When authorization weaknesses are identified:

1. Restrict access to exposed sensitive resources.
2. Fix missing server-side authorization checks.
3. Enforce ownership and tenant boundaries.
4. Correct role and permission assignment logic.
5. Review nested resources, exports and background operations.
6. Add automated authorization regression tests.
7. Audit sensitive permission changes.

Prioritize remediation according to exposure, exploitability and potential business impact.

## 14. References and Further Reading

### OWASP

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Insecure Direct Object Reference Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)
- [OWASP API Security Top 10](https://owasp.org/API-Security/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

### Additional Resources

- [NIST Role-Based Access Control](https://csrc.nist.gov/projects/role-based-access-control)
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

---

This guide is educational and does not replace application-specific security reviews, implementation testing or professional security assessments.
