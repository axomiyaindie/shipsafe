
# Infrastructure Security

Infrastructure security focuses on protecting the servers, operating systems, networks, containers, cloud resources and deployment environments that support an application.

Even a securely developed application can become vulnerable when deployed on an improperly configured server or exposed infrastructure.

Infrastructure security helps prevent unauthorized access, privilege escalation, service compromise, data exposure and disruption of production systems.

This guide covers infrastructure hardening, server security, network protection, container security, deployment configuration and infrastructure monitoring.

## 1. Understanding Infrastructure Security

Application infrastructure includes the systems and services required to build, deploy, operate and maintain an application.

### Common Infrastructure Components

- Linux and Windows servers.
- Virtual machines.
- Cloud computing instances.
- Containers and container orchestration platforms.
- Databases and storage systems.
- Firewalls and network gateways.
- Reverse proxies and load balancers.
- DNS and domain configuration.
- CI/CD infrastructure.
- Monitoring and logging systems.
- Backup and recovery systems.

### Security Objectives

- Prevent unauthorized infrastructure access.
- Minimize exposed services and network ports.
- Restrict administrative privileges.
- Protect application and database servers.
- Secure communication between infrastructure components.
- Maintain secure deployment configurations.
- Detect suspicious infrastructure activity.
- Protect backups and recovery systems.

## 2. Infrastructure Attack Surface

An attack surface consists of the entry points through which an attacker may interact with an application or its supporting infrastructure.

### Common Attack Surfaces

- Publicly exposed server ports.
- Remote administration services.
- SSH endpoints.
- Database ports.
- Web servers and reverse proxies.
- Container management interfaces.
- Cloud management APIs.
- Exposed storage buckets.
- Misconfigured firewalls.
- Default credentials.
- Unpatched operating systems.
- Unprotected deployment systems.

### Security Recommendations

- Inventory infrastructure assets.
- Identify publicly accessible services.
- Remove unnecessary services.
- Restrict management interfaces.
- Review network exposure.
- Monitor externally reachable systems.
- Reassess the attack surface after infrastructure changes.

Only expose services that are required for legitimate application functionality.

## 3. Linux Server Hardening

Linux servers commonly host web applications, APIs, databases and background services.

A secure server configuration reduces the opportunities for unauthorized access and exploitation.

### Security Recommendations

- Use supported operating system releases.
- Install security updates regularly.
- Remove unnecessary packages.
- Disable unused services.
- Restrict administrative access.
- Use individual user accounts.
- Apply least-privilege permissions.
- Configure host firewalls.
- Monitor authentication activity.
- Protect system logs.
- Review scheduled tasks.
- Secure application directories.

### Update Management

For Debian and Ubuntu-based systems:

```bash
sudo apt update
sudo apt upgrade
```

Review available updates and test important production changes according to your deployment policy.

For security-sensitive environments, establish a process for applying security updates promptly.

### Service Review

Inspect running services:

```bash
systemctl --type=service --state=running
```

Review listening network sockets:

```bash
sudo ss -tulpen
```

Investigate services that are not required.

Do not disable a service without understanding its role and dependencies.

## 4. SSH Security

Secure Shell (SSH) provides remote access to Linux and other systems.

Weak SSH configuration can allow unauthorized access to production infrastructure.

### Security Recommendations

- Use SSH keys instead of password-only authentication where practical.
- Protect private keys with appropriate permissions.
- Use passphrases for private keys.
- Disable direct root login where appropriate.
- Restrict access to authorized users.
- Prefer modern supported cryptographic algorithms.
- Keep SSH software updated.
- Monitor authentication attempts.
- Use network restrictions where practical.
- Consider multi-factor authentication for administrative access.

### Example SSH Configuration

Review the server's SSH daemon configuration, commonly located at:

`/etc/ssh/sshd_config`

Example settings:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no
```

These are example hardening settings, not universal defaults.

Before applying changes:

1. Confirm that key-based access works.
2. Keep an existing administrative session open.
3. Validate the SSH configuration.
4. Restart or reload the service appropriately.
5. Test a new connection before closing the original session.

On supported systems:

```bash
sudo sshd -t
```

Never disable password authentication until a working alternative administrative access method has been verified.

## 5. Firewall and Network Security

Firewalls control which network connections are allowed to reach a server or service.

A secure firewall configuration reduces unnecessary network exposure.

### Common Network Controls

- Host-based firewalls.
- Cloud security groups.
- Network access control lists.
- Private network segmentation.
- Application-level access restrictions.
- Web application firewalls.

### Security Recommendations

- Deny unnecessary inbound traffic.
- Allow only required services.
- Restrict administrative access by trusted network where practical.
- Avoid exposing database ports publicly.
- Separate application and database networks.
- Review outbound network permissions where appropriate.
- Monitor firewall rule changes.
- Document required network connections.

### UFW Example

On Ubuntu systems using UFW:

```bash
sudo ufw status verbose
```

Allow SSH:

```bash
sudo ufw allow OpenSSH
```

Allow web traffic:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Enable the firewall:

```bash
sudo ufw enable
```

These commands are examples. Confirm that required remote administration access is allowed before enabling firewall restrictions.

## 6. Ports and Service Exposure

Every listening service can introduce potential security risks.

### Common Ports

| Port | Common service | Security consideration |
|---|---|---|
| 22 | SSH | Restrict administrative access |
| 80 | HTTP | Redirect to HTTPS where appropriate |
| 443 | HTTPS | Configure TLS correctly |
| 3306 | MySQL | Avoid public database exposure |
| 5432 | PostgreSQL | Restrict access to trusted systems |
| 6379 | Redis | Avoid unrestricted public exposure |
| 27017 | MongoDB | Restrict access and require authentication |

The table is illustrative. Actual service ports and configurations may differ.

### Security Recommendations

- Identify all listening ports.
- Remove unnecessary listeners.
- Restrict database ports to trusted networks.
- Require authentication for administrative services.
- Avoid exposing internal dashboards.
- Monitor unexpected services.
- Review cloud and host firewall rules together.

A service should not be considered safe simply because it listens on a non-standard port.

## 7. Reverse Proxy Security

Reverse proxies receive client requests and forward them to application servers.

Common reverse proxies include:

- Nginx.
- Apache HTTP Server.
- Caddy.
- Traefik.

### Security Responsibilities

- Terminate HTTPS connections.
- Route requests to appropriate services.
- Apply request size limits.
- Configure security headers.
- Restrict access to internal services.
- Forward trusted client information correctly.
- Apply appropriate rate limiting.
- Avoid exposing backend administration endpoints.

### Security Recommendations

- Keep reverse proxy software updated.
- Use valid TLS certificates.
- Redirect HTTP to HTTPS where appropriate.
- Restrict access to internal upstream services.
- Configure appropriate request and connection timeouts.
- Review forwarded headers.
- Prevent clients from spoofing trusted proxy headers.
- Disable unnecessary modules and features.

Do not blindly trust `X-Forwarded-For` or similar headers from arbitrary clients. Configure trusted proxy boundaries correctly.

## 8. TLS and HTTPS Configuration

Transport Layer Security (TLS) protects communication between clients and servers.

### Security Recommendations

- Use valid certificates.
- Keep certificates renewed.
- Use supported TLS versions.
- Disable obsolete protocol versions.
- Configure appropriate cipher suites through maintained server defaults.
- Protect private keys.
- Redirect HTTP to HTTPS where appropriate.
- Review certificate expiration.
- Avoid disabling certificate validation.
- Test public-facing TLS configuration.

### Certificate Management

Certificates should have:

- Validity periods.
- Appropriate subject names.
- Trusted certificate chains.
- Securely stored private keys.
- Renewal and replacement procedures.

Automated certificate management can reduce expiration-related outages, but renewal processes should be monitored.

## 9. Server Authentication and Access Control

Infrastructure access should be granted only to authorized users and services.

### Security Recommendations

- Use individual administrator accounts.
- Avoid shared root credentials.
- Apply least privilege.
- Require strong authentication.
- Use MFA where supported.
- Review sudo permissions.
- Remove unused accounts.
- Disable inactive credentials.
- Audit privileged actions.
- Review access after personnel changes.

### Linux Permission Review

Linux permissions control access to files and directories.

Inspect permissions:

```bash
ls -la
```

Inspect ownership:

```bash
stat FILE_OR_DIRECTORY
```

Use appropriate ownership and permissions for application files, configuration and logs.

Avoid applying broad permissions such as:

```bash
chmod -R 777 /var/www
```

This grants excessive access and can create serious security weaknesses.

The correct permissions depend on the application's runtime user and deployment architecture.

## 10. Privilege Escalation Prevention

Privilege escalation occurs when a user or process obtains permissions beyond those intended.

### Common Causes

- Excessive sudo privileges.
- Writable privileged scripts.
- Insecure file permissions.
- Vulnerable system packages.
- Misconfigured scheduled tasks.
- Exposed container sockets.
- Unrestricted service accounts.
- Insecure application execution contexts.

### Security Recommendations

- Run applications using dedicated service accounts.
- Avoid running application processes as root.
- Restrict sudo access.
- Protect executable files and deployment scripts.
- Review privileged scheduled tasks.
- Remove unnecessary capabilities.
- Patch operating system vulnerabilities.
- Monitor unexpected privilege changes.

## 11. Docker and Container Security

Containers package application code and dependencies into isolated execution environments.

Containerization does not automatically make an application secure.

### Common Container Risks

- Running containers as root.
- Using vulnerable base images.
- Exposing unnecessary ports.
- Mounting sensitive host directories.
- Embedding credentials in image layers.
- Using privileged containers.
- Exposing the Docker socket.
- Using untrusted images.
- Running outdated container runtimes.

### Security Recommendations

- Use trusted and maintained base images.
- Prefer non-root execution.
- Minimize installed packages.
- Scan container images.
- Restrict container capabilities.
- Avoid privileged mode unless essential.
- Apply appropriate resource limits.
- Use read-only filesystems where practical.
- Restrict host filesystem mounts.
- Protect container runtime interfaces.
- Keep Docker and related components updated.

### Example Dockerfile

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

COPY --chown=node:node . .

USER node

EXPOSE 3000

CMD ["node", "server.js"]
```

This is an illustrative example. Select a supported base image version and verify compatibility with the application.

For production:

- Pin base images appropriately.
- Review image provenance.
- Avoid copying secrets into image layers.
- Scan the final image.
- Run with only required permissions.

## 12. Container Orchestration Security

Container orchestration systems such as Kubernetes introduce additional security responsibilities.

### Security Recommendations

- Restrict cluster administration.
- Apply role-based access control.
- Protect cluster credentials.
- Use network policies where supported.
- Restrict privileged workloads.
- Use dedicated service accounts.
- Protect secrets.
- Scan container images.
- Monitor cluster events.
- Keep control-plane components updated.
- Review exposed management endpoints.

Avoid granting broad cluster administrator permissions to application workloads.

## 13. Database Infrastructure Security

Database infrastructure stores sensitive application information and requires strong network and access controls.

### Security Recommendations

- Keep databases on private networks where possible.
- Restrict inbound database connections.
- Require authentication.
- Use encrypted connections.
- Apply least-privilege database accounts.
- Separate administrative and application credentials.
- Enable appropriate database auditing.
- Patch database software.
- Protect backups.
- Monitor unusual access patterns.

A database should not be publicly reachable simply because the application needs to connect to it.

## 14. Cloud Infrastructure Security

Cloud platforms provide flexible infrastructure but introduce configuration and identity risks.

Common providers include:

- AWS.
- Microsoft Azure.
- Google Cloud.

### Common Cloud Security Risks

- Publicly accessible storage.
- Excessive IAM permissions.
- Exposed management interfaces.
- Unrestricted security groups.
- Long-lived access keys.
- Insecure network configuration.
- Missing audit logs.
- Unprotected snapshots and backups.

### Security Recommendations

- Use least-privilege IAM policies.
- Prefer managed identities and temporary credentials.
- Restrict public resource access.
- Enable appropriate audit logging.
- Review security group rules.
- Encrypt sensitive storage.
- Protect cloud management accounts with MFA.
- Monitor unexpected resource creation.
- Review infrastructure changes.
- Secure cloud backup configurations.

Cloud provider defaults and service capabilities vary. Verify configuration against the actual provider documentation.

## 15. DNS and Domain Security

Domain configuration is part of application infrastructure security.

### Security Recommendations

- Use trusted DNS providers.
- Protect registrar accounts with MFA.
- Restrict DNS administration access.
- Review DNS records regularly.
- Remove obsolete records.
- Protect domain transfer settings.
- Enable DNSSEC where appropriate and supported.
- Configure SPF, DKIM and DMARC for email domains.
- Monitor unexpected DNS changes.
- Protect certificate issuance and validation workflows.

Misconfigured DNS can expose services, redirect traffic or weaken trust in application communications.

## 16. File Storage and Object Storage Security

Applications frequently store user uploads, backups, documents and generated files.

### Common Risks

- Public storage buckets.
- Predictable or unrestricted file access.
- Insecure file permissions.
- Exposed backup archives.
- Missing encryption.
- Unrestricted upload destinations.
- Inadequate retention policies.

### Security Recommendations

- Keep private files private by default.
- Apply access policies to individual resources.
- Use short-lived signed URLs where appropriate.
- Validate uploaded file types and contents.
- Restrict administrative storage access.
- Encrypt sensitive data where required.
- Review bucket and container policies.
- Monitor public access changes.
- Protect backups separately.

A difficult-to-guess URL is not a substitute for access control.

## 17. Infrastructure as Code Security

Infrastructure as Code (IaC) allows teams to define infrastructure using version-controlled configuration.

Examples include:

- Terraform.
- OpenTofu.
- AWS CloudFormation.
- Azure Bicep.
- Kubernetes manifests.
- Ansible.

### Common Risks

- Publicly exposed resources.
- Excessive IAM permissions.
- Hardcoded credentials.
- Unencrypted storage.
- Unrestricted network rules.
- Insecure container configuration.
- Unsafe default settings.

### Security Recommendations

- Review infrastructure changes before deployment.
- Scan IaC configuration.
- Avoid hardcoded credentials.
- Restrict deployment permissions.
- Protect state files.
- Review resource exposure.
- Separate environments.
- Track configuration changes.
- Apply policy checks where practical.

### Useful Tools

- [Checkov](https://www.checkov.io/)
- [Trivy](https://trivy.dev/)
- [Terrascan](https://runterrascan.io/)
- [KICS](https://kics.io/)

## 18. Production Environment Separation

Development, staging and production environments should have clearly separated security boundaries.

### Security Recommendations

- Separate production credentials.
- Avoid copying real production data into development.
- Restrict production access.
- Use separate databases where practical.
- Protect deployment approvals.
- Isolate internal services.
- Apply environment-specific configuration.
- Review cross-environment access.
- Monitor production changes.

A staging environment should not automatically inherit unrestricted access to production resources.

## 19. Infrastructure Monitoring and Auditing

Infrastructure monitoring helps identify unusual activity, configuration changes and operational failures.

### Monitor

- Authentication attempts.
- SSH access.
- Privileged commands.
- Listening ports.
- Firewall changes.
- CPU and memory usage.
- Disk usage.
- Network traffic.
- Container activity.
- Cloud identity events.
- Configuration changes.
- Service availability.

### Security Recommendations

- Centralize important logs.
- Protect logs from unauthorized modification.
- Monitor authentication anomalies.
- Configure alerts for important events.
- Retain logs according to policy.
- Review alerts regularly.
- Synchronize system clocks.
- Protect monitoring credentials.

Monitoring is most useful when alerts have defined ownership and response procedures.

## 20. Backup and Disaster Recovery

Backups help restore applications and infrastructure after accidental deletion, ransomware, corruption or system failure.

### Security Recommendations

- Maintain regular backups.
- Encrypt sensitive backups.
- Restrict backup access.
- Separate backups from production credentials.
- Protect backup deletion permissions.
- Test restoration procedures.
- Define recovery time objectives (RTO).
- Define recovery point objectives (RPO).
- Monitor backup completion.
- Document disaster recovery procedures.

A backup that has never been tested should not be assumed recoverable.

## 21. Infrastructure Security Testing

Infrastructure should be reviewed periodically and after significant configuration changes.

### Common Testing Approaches

- Configuration reviews.
- Network exposure assessments.
- Vulnerability scanning.
- Operating system patch reviews.
- Firewall rule reviews.
- Container image scanning.
- Cloud configuration assessments.
- Infrastructure as Code scanning.
- Access control audits.
- Authorized penetration testing.

### Useful Tools

| Tool | Purpose |
|---|---|
| Nmap | Network discovery and port scanning |
| Lynis | Linux and Unix security auditing |
| OpenSCAP | Security compliance assessment |
| Trivy | Vulnerability and configuration scanning |
| Checkov | Infrastructure as Code scanning |
| Docker Scout | Container image analysis |
| Greenbone/OpenVAS | Vulnerability management |

Use these tools only against systems you own or have explicit permission to assess.

### Example: Inspect Listening Ports

```bash
sudo ss -tulpen
```

### Example: Run a Lynis Audit

After installing Lynis from its official source:

```bash
sudo lynis audit system
```

Review the results manually. Audit suggestions are not automatically confirmed vulnerabilities.

## 22. Infrastructure Security Checklist

### Server Hardening

- [ ] Supported operating system versions are used.
- [ ] Security updates are applied.
- [ ] Unnecessary services are disabled.
- [ ] Administrative access is restricted.
- [ ] Applications run with appropriate service accounts.
- [ ] File permissions are reviewed.
- [ ] System logs are protected.

### Network Security

- [ ] Only required ports are exposed.
- [ ] Firewall rules are reviewed.
- [ ] Database services are not publicly exposed unnecessarily.
- [ ] Internal services use appropriate network restrictions.
- [ ] Administrative interfaces are protected.
- [ ] TLS is configured correctly.

### SSH and Access Control

- [ ] Root login is restricted appropriately.
- [ ] SSH authentication is securely configured.
- [ ] Private keys are protected.
- [ ] Individual administrator accounts are used.
- [ ] Sudo permissions follow least privilege.
- [ ] Unused accounts are removed.

### Container and Cloud Security

- [ ] Containers use appropriate permissions.
- [ ] Container images are scanned.
- [ ] Secrets are not embedded in images.
- [ ] Cloud IAM permissions are restricted.
- [ ] Public storage access is reviewed.
- [ ] Cloud audit logging is enabled where appropriate.

### Production Operations

- [ ] Development and production environments are separated.
- [ ] Infrastructure changes are reviewed.
- [ ] Backups are protected and tested.
- [ ] Monitoring and alerting are configured.
- [ ] Incident response procedures are documented.
- [ ] Recovery procedures are tested.

## 23. Common Infrastructure Security Vulnerabilities

| Vulnerability | Potential impact |
|---|---|
| Unpatched operating system | Exploitation of known vulnerabilities |
| Weak SSH configuration | Unauthorized server access |
| Open database ports | Data exposure or database compromise |
| Excessive permissions | Privilege escalation |
| Misconfigured firewall | Unnecessary network exposure |
| Insecure containers | Runtime or host compromise |
| Public storage | Unauthorized data access |
| Weak cloud IAM | Infrastructure compromise |
| Exposed secrets | Unauthorized system access |
| Missing backups | Permanent data loss |
| Insecure TLS | Communication interception |
| Missing monitoring | Delayed threat detection |

## 24. Remediation Priorities

When infrastructure weaknesses are discovered:

1. Restrict unauthorized public exposure.
2. Address critical operating system and service vulnerabilities.
3. Secure administrative access.
4. Restrict excessive privileges.
5. Correct firewall and network configuration.
6. Secure databases and storage.
7. Review container and cloud configurations.
8. Rotate exposed infrastructure credentials.
9. Verify backup and recovery procedures.
10. Improve monitoring and security auditing.

Prioritize weaknesses according to exposure, exploitability, business impact and sensitivity of affected resources.

## 25. References and Further Reading

### Security Standards and Guidance

- [OWASP Infrastructure Security Guidance](https://owasp.org/)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST SP 800-123: Guide to General Server Security](https://csrc.nist.gov/pubs/sp/800/123/final)

### Linux and Server Security

- [Ubuntu Server Documentation](https://documentation.ubuntu.com/server/)
- [Red Hat Security Documentation](https://docs.redhat.com/)
- [OpenSSH Manual](https://www.openssh.com/manual.html)
- [Lynis](https://cisofy.com/lynis/)

### Container Security

- [Docker Security](https://docs.docker.com/engine/security/)
- [Kubernetes Security](https://kubernetes.io/docs/concepts/security/)
- [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker)

### Cloud Security

- [AWS Security Documentation](https://docs.aws.amazon.com/security/)
- [Microsoft Azure Security](https://learn.microsoft.com/en-us/azure/security/)
- [Google Cloud Security](https://cloud.google.com/security)

### Security Testing Tools

- [Nmap](https://nmap.org/)
- [OpenSCAP](https://www.open-scap.org/)
- [Trivy](https://trivy.dev/)
- [Checkov](https://www.checkov.io/)
- [Greenbone](https://greenbone.net/)

---

This guide is educational and does not replace infrastructure-specific security assessments, professional penetration testing or operational security procedures.
