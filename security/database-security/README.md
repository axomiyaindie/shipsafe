
# Database Security

Databases store some of the most sensitive information handled by modern applications, including user accounts, personal information, financial records, business data, authentication credentials and application secrets.

Database security protects this information against unauthorized access, modification, disclosure, destruction and misuse.

A secure database requires more than a strong password. Queries, permissions, network exposure, encryption, backups, migrations and application-level access controls must all be considered.

This guide covers database security principles, common vulnerabilities, secure implementation practices and verification procedures.

## 1. Database Security Fundamentals

Database security involves protecting the database engine, stored information, connections, administrative interfaces and applications that interact with it.

Common database technologies include:

- PostgreSQL.
- MySQL and MariaDB.
- Microsoft SQL Server.
- MongoDB.
- SQLite.
- Redis.
- Firebase Firestore.
- Supabase-managed PostgreSQL.
- Other managed and self-hosted database services.

### Core Security Principles

- Apply least-privilege access.
- Authenticate database clients securely.
- Encrypt connections and sensitive stored information.
- Validate and safely handle application input.
- Restrict database network exposure.
- Protect credentials and connection strings.
- Maintain reliable backups.
- Monitor database activity.
- Keep database software and dependencies updated.
- Test access-control policies before production deployment.

A managed database service does not automatically guarantee that the application's database permissions and queries are secure.

## 2. SQL Injection

SQL injection occurs when untrusted input is incorporated into SQL statements in an unsafe manner.

An attacker may manipulate a query to access, modify or delete information beyond the application's intended permissions.

### Common Causes

- Building SQL queries through string concatenation.
- Inserting untrusted input directly into query statements.
- Unsafe dynamic query construction.
- Incorrect handling of search, filtering and sorting parameters.
- Insufficient input validation combined with unsafe query execution.

### Prevention

- Use parameterized queries or prepared statements.
- Use established database libraries.
- Avoid concatenating untrusted values into SQL statements.
- Allowlist dynamic column names and sort directions.
- Restrict database account permissions.
- Handle database errors without exposing internal details.
- Test query handling with authorized security tests.

Input validation is useful, but it does not replace parameterized queries.

## 3. NoSQL Injection

NoSQL injection occurs when untrusted input changes the intended behavior of a NoSQL database query.

It can affect applications using document databases and other NoSQL technologies.

### Common Risks

- Accepting unexpected query operators.
- Passing unvalidated objects directly into database queries.
- Unsafe dynamic query construction.
- Incorrect handling of authentication filters.
- Trusting client-controlled query structures.

### Prevention

- Validate request bodies against explicit schemas.
- Accept expected primitive data types.
- Reject unexpected query operators and fields.
- Construct database queries using trusted application logic.
- Avoid passing raw client objects directly into database filters.
- Apply least-privilege database permissions.
- Test authentication and authorization queries.

Do not assume NoSQL databases are immune to injection vulnerabilities.

## 4. Parameterized Queries and ORM Security

Object-Relational Mapping (ORM) tools help applications interact with relational databases.

Examples include:

- Prisma.
- Drizzle ORM.
- SQLAlchemy.
- Entity Framework.
- Hibernate.
- Sequelize.

Using an ORM can reduce common query-construction mistakes, but it does not eliminate database security risks.

### Security Recommendations

- Prefer parameterized query APIs.
- Avoid unsafe raw SQL methods.
- Review dynamically constructed queries.
- Validate input before database operations.
- Restrict fields that users can update.
- Apply authorization before modifying records.
- Review ORM-generated migrations and queries.
- Keep database libraries updated.

### Important Distinction

Safe:

- Parameterized values.
- Trusted query builders.
- Explicit field selection.
- Server-controlled query structure.

Potentially unsafe:

- Raw SQL constructed from user input.
- Unrestricted dynamic query objects.
- Client-controlled database operators.
- Unrestricted mass assignment.

An ORM is a development tool, not an authorization boundary.

## 5. Database Authentication and Access Control

Database access should be restricted to authorized users, services and administrators.

### Security Recommendations

- Use unique database identities for applications and services.
- Avoid using database administrator accounts for normal application operations.
- Apply least-privilege permissions.
- Separate read and write privileges where appropriate.
- Restrict schema modification permissions.
- Remove unused database accounts.
- Rotate credentials when necessary.
- Review administrative access regularly.
- Protect database management interfaces.

### Example Permission Separation

| Database identity | Intended access |
|---|---|
| Application reader | Approved read operations |
| Application writer | Required application read and write operations |
| Migration account | Controlled schema changes |
| Backup account | Required backup operations |
| Database administrator | Restricted administrative operations |

The exact permission model should reflect the database engine and application architecture.

## 6. Row-Level Security (RLS)

Row-Level Security restricts which individual records a database user or session can access.

It is particularly important in applications that store records belonging to different users or organizations.

### Example

An application stores customer records for multiple organizations.

A user from Organization A should only access records that the applicable database policy permits.

RLS can enforce restrictions directly within the database, providing an additional protection layer.

### Security Recommendations

- Enable RLS on tables that require row-level restrictions.
- Define explicit SELECT, INSERT, UPDATE and DELETE policies.
- Apply policies to every relevant table.
- Test access using different authenticated identities.
- Verify behavior when identity information is missing.
- Review policies for both existing and newly created records.
- Test administrative and background-job access separately.
- Avoid assuming that enabling RLS without policies provides the intended application behavior.

### Important Supabase Considerations

For applications using Supabase:

- Enable RLS on exposed tables.
- Write policies based on trusted authentication context.
- Review `auth.uid()` usage.
- Restrict service-role credentials to trusted server environments.
- Never expose service-role keys in browser or mobile application bundles.
- Test policies using real application access patterns.
- Review views, functions and storage policies separately.

A privileged service-role credential can bypass ordinary RLS protections. It must be handled as a highly sensitive secret.

## 7. Multi-Tenant Database Isolation

Multi-tenant applications serve multiple organizations or customers using shared application infrastructure.

Incorrect tenant isolation can expose one organization's records to another.

### Common Database Architectures

**Shared database and shared tables**

Multiple tenants share tables, with each record associated with a tenant identifier.

**Shared database with separate schemas**

Tenants use separate database schemas.

**Separate databases**

Each tenant has a separate database instance or database.

Each architecture has different operational, security and isolation trade-offs.

### Security Recommendations

- Associate tenant-owned records with a trusted tenant identity.
- Enforce tenant restrictions in database queries.
- Apply RLS where appropriate.
- Validate tenant membership on the server.
- Protect joins, subqueries and reporting operations.
- Review background jobs and scheduled tasks.
- Test cross-tenant access.
- Protect exports, search results and analytics.
- Ensure database administrators and support tools follow appropriate access controls.

A tenant ID supplied by a client must not be trusted without server-side verification.

## 8. Encryption at Rest

Encryption at rest protects stored information against certain forms of unauthorized access to storage media and backups.

### Security Recommendations

- Use database or storage-provider encryption capabilities.
- Encrypt sensitive backups.
- Protect encryption keys separately from encrypted data.
- Restrict key-management permissions.
- Establish key rotation procedures.
- Review the encryption scope of database snapshots and replicas.
- Consider application-level encryption for particularly sensitive fields where appropriate.

Encryption at rest does not prevent an authorized but compromised application account from reading data through normal database operations.

## 9. Encryption in Transit

Database connections should be protected against interception and unauthorized modification.

### Security Recommendations

- Use TLS for database connections.
- Require encrypted connections where supported.
- Validate server certificates.
- Avoid disabling certificate verification in production.
- Protect internal database connections.
- Review TLS configuration and supported versions.
- Avoid exposing database traffic over untrusted networks without encryption.

Do not treat a private network as a replacement for connection security.

## 10. Database Secrets and Connection Strings

Database connection strings often contain hostnames, usernames, passwords and other sensitive configuration.

### Security Recommendations

- Never commit production credentials to Git.
- Keep secrets outside source code.
- Use environment variables or an appropriate secret manager.
- Restrict access to deployment configuration.
- Separate development, staging and production credentials.
- Rotate exposed credentials immediately.
- Avoid printing connection strings in logs.
- Review CI/CD environment variables and build logs.
- Never embed privileged database credentials in frontend applications.

If a secret is accidentally committed, treat it as compromised and rotate it even after removing it from the repository.

## 11. Database Network Security

Database services should not be unnecessarily exposed to the public internet.

### Security Recommendations

- Restrict inbound connections.
- Use private networking where appropriate.
- Apply firewall rules and network access controls.
- Allow connections only from required application services.
- Restrict administrative access.
- Review public database endpoints.
- Avoid exposing database management ports unnecessarily.
- Monitor unexpected connection attempts.

Network restrictions should complement authentication and database authorization rather than replace them.

## 12. Secure Database Configuration

Default or insecure database configurations can expose sensitive services.

### Security Recommendations

- Change default credentials.
- Disable unused services and features.
- Remove unnecessary database accounts.
- Restrict administrative interfaces.
- Configure appropriate connection limits.
- Review database listening interfaces.
- Apply secure authentication methods.
- Keep database software updated.
- Restrict extensions and plugins to approved requirements.
- Review production configuration changes.

Use official hardening guidance for the specific database engine and hosting provider.

## 13. Database Backups and Disaster Recovery

Backups protect against accidental deletion, infrastructure failures, ransomware and other incidents.

However, backups can themselves become a source of sensitive information exposure.

### Security Recommendations

- Automate backups.
- Encrypt backup files and snapshots.
- Restrict backup access.
- Store backups separately from primary production systems.
- Define retention policies.
- Test restoration procedures regularly.
- Protect backup credentials.
- Monitor backup job failures.
- Document recovery objectives.
- Consider point-in-time recovery where supported.

### Recovery Planning

Understand these two concepts:

- Recovery Point Objective (RPO): the maximum acceptable amount of data loss measured in time.
- Recovery Time Objective (RTO): the target time required to restore service.

A backup that has never been tested through restoration should not be assumed to be reliable.

## 14. Database Migrations and Production Safety

Database migrations modify schemas, indexes, constraints and stored data.

Poorly managed migrations can introduce security weaknesses, data loss or service outages.

### Security Recommendations

- Review migrations before production deployment.
- Restrict schema modification permissions.
- Use version-controlled migration files.
- Test migrations against representative datasets.
- Review destructive operations carefully.
- Validate access-control policies after migrations.
- Avoid exposing sensitive production data in development environments.
- Plan rollback or recovery procedures.
- Monitor migration execution.
- Review new tables and views for appropriate access restrictions.

When a new table is introduced, review its permissions and RLS policies before exposing it to application clients.

## 15. Database Logging and Monitoring

Database monitoring helps identify suspicious activity, operational problems and possible security incidents.

### Security Events to Monitor

- Failed authentication attempts.
- Unexpected administrative operations.
- Permission and role changes.
- Schema modifications.
- Unusual query activity.
- Unexpected connection sources.
- Large or unusual data exports.
- Backup failures.
- Changes to security policies.

### Security Recommendations

- Enable appropriate database audit logging.
- Protect log storage.
- Restrict access to audit records.
- Avoid logging passwords or sensitive query parameters unnecessarily.
- Monitor unusual access patterns.
- Establish alerting for critical events.
- Define retention and investigation procedures.

Logging should follow privacy and data-protection requirements.

## 16. Database Security Testing

Database security must be tested as part of application development and production readiness.

### Essential Test Scenarios

- SQL injection attempts are safely handled.
- NoSQL query operators cannot be injected through untrusted input.
- Database accounts have only required privileges.
- Unauthorized users cannot read protected records.
- Unauthorized users cannot modify or delete protected records.
- RLS policies enforce the expected access boundaries.
- Cross-tenant access is prevented.
- Database connections use appropriate encryption.
- Production credentials are not exposed in source code.
- Backups are protected and can be restored.
- Migrations do not unintentionally remove security controls.
- Database errors do not reveal sensitive internal information.
- Administrative operations are appropriately restricted.

### Testing Approaches

- Automated application tests.
- Database permission reviews.
- RLS policy tests.
- Authorized SQL and NoSQL injection testing.
- Configuration audits.
- Backup restoration tests.
- Database vulnerability assessments.

Use isolated test environments and authorized test data when possible.

## 17. Common Database Security Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| SQL injection | Unauthorized data access or modification |
| NoSQL injection | Query manipulation and data exposure |
| Excessive database privileges | Expanded impact of application compromise |
| Missing RLS policies | Unauthorized record access |
| Weak tenant isolation | Cross-tenant data exposure |
| Exposed connection strings | Database compromise |
| Unencrypted connections | Data interception |
| Public database exposure | Unauthorized connection attempts |
| Insecure backups | Sensitive data disclosure |
| Unsafe migrations | Data loss or security misconfiguration |
| Excessive data retention | Increased exposure in a breach |

## 18. Remediation Priorities

When database security weaknesses are identified:

1. Restrict public or unauthorized database access.
2. Rotate exposed database credentials.
3. Fix SQL and NoSQL injection vulnerabilities.
4. Correct database permissions and access policies.
5. Enforce tenant isolation and RLS requirements.
6. Protect connections and stored backups.
7. Review database configuration and network exposure.
8. Secure migration and deployment processes.
9. Establish monitoring and backup restoration testing.
10. Add automated security regression tests.

Prioritize remediation according to exploitability, data sensitivity and potential business impact.

## 19. References and Further Reading

### OWASP

- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP Database Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html)
- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)

### Official Database Documentation

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [PostgreSQL Row Security Policies](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)
- [MySQL Security Documentation](https://dev.mysql.com/doc/refman/8.4/en/security.html)
- [MongoDB Security Checklist](https://www.mongodb.com/docs/manual/administration/security-checklist/)
- [Supabase Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security)

---

This guide is educational and does not replace application-specific security reviews, database configuration audits or professional security assessments.
