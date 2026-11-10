
# Cloud Security

Cloud security is the practice of protecting cloud-hosted applications, infrastructure, identities, workloads and data against unauthorized access, exploitation, misconfiguration and operational failures.

Modern applications frequently depend on cloud services for computing, databases, storage, authentication, networking, monitoring and deployment.

A single insecure cloud configuration can expose sensitive information, compromise application infrastructure or allow unauthorized access to critical resources.

This guide explains cloud security principles, practical implementation strategies, common misconfigurations, testing methods and security controls for developers and engineering teams.

## 1. Cloud Security Fundamentals

Cloud security combines technical controls, operational practices and shared responsibilities between cloud providers and customers.

### Core Security Objectives

- Protect cloud-hosted data.
- Restrict access to infrastructure.
- Secure application workloads.
- Prevent unauthorized resource exposure.
- Protect credentials and encryption keys.
- Monitor suspicious activity.
- Maintain availability and resilience.
- Detect and remediate misconfigurations.
- Support compliance and privacy requirements.

### Common Cloud Deployment Models

| Model | Description | Security responsibility |
|---|---|---|
| IaaS | Virtual machines, networks and storage | Customer manages operating systems, applications and configurations |
| PaaS | Managed application platforms | Provider manages more underlying infrastructure |
| SaaS | Fully managed software services | Customer manages users, data and service configurations |
| Serverless | Event-driven managed execution | Provider manages infrastructure; customer secures code, identity and configuration |

Security responsibilities vary by service and provider.

### Shared Responsibility Model

Cloud providers secure the underlying cloud infrastructure, while customers remain responsible for security controls within their configured services.

Depending on the service, customer responsibilities may include:

- Identity and access management.
- Application security.
- Data classification.
- Network configuration.
- Credential management.
- Storage permissions.
- Workload configuration.
- Monitoring and incident response.

Using a managed service does not eliminate application security responsibilities.

## 2. Major Cloud Providers

Cloud security principles apply across providers, although service names and configuration mechanisms differ.

### Amazon Web Services (AWS)

Important security services:

| Service | Purpose |
|---|---|
| IAM | Identity and access management |
| CloudTrail | API activity logging |
| GuardDuty | Threat detection |
| Security Hub | Centralized security findings |
| KMS | Key management |
| Secrets Manager | Secret storage |
| Systems Manager | Infrastructure management |
| AWS Config | Resource configuration tracking |
| Inspector | Vulnerability management |
| WAF | Web application firewall |

Official documentation:

https://docs.aws.amazon.com/security/

### Microsoft Azure

Important security services:

| Service | Purpose |
|---|---|
| Microsoft Entra ID | Identity management |
| Azure RBAC | Resource access control |
| Microsoft Defender for Cloud | Cloud security posture and threat protection |
| Azure Key Vault | Secrets and key management |
| Azure Monitor | Monitoring |
| Microsoft Sentinel | Security information and event management |
| Azure Policy | Configuration governance |
| Azure Firewall | Network security |

Official documentation:

https://learn.microsoft.com/azure/security/

### Google Cloud Platform (GCP)

Important security services:

| Service | Purpose |
|---|---|
| Cloud IAM | Identity and access management |
| Cloud Audit Logs | Activity auditing |
| Security Command Center | Security findings and posture |
| Secret Manager | Secret storage |
| Cloud KMS | Key management |
| Cloud Armor | Application and network protection |
| VPC Service Controls | Service perimeter controls |
| Organization Policy | Resource governance |
| Cloud Logging | Log management |

Official documentation:

https://cloud.google.com/security

## 3. Cloud Identity and Access Management

Identity and Access Management (IAM) determines who or what can access cloud resources and which operations they can perform.

Poor IAM configuration is one of the most common causes of cloud security incidents.

### Common IAM Risks

- Overly permissive roles.
- Shared administrative accounts.
- Publicly exposed credentials.
- Unused identities.
- Long-lived access keys.
- Unrestricted service accounts.
- Excessive application permissions.
- Missing MFA.
- Inadequate permission boundaries.

### Least Privilege

Grant each identity only the permissions required for its responsibilities.

For example:

A backend application that reads customer records should not automatically receive permission to:

- Delete production databases.
- Manage cloud IAM policies.
- Modify billing settings.
- Access unrelated storage buckets.
- Create administrative users.

### IAM Best Practices

- Use dedicated identities for applications and workloads.
- Avoid shared human accounts.
- Enable MFA for privileged users.
- Prefer temporary credentials.
- Review permissions regularly.
- Remove unused identities.
- Restrict administrative access.
- Separate production and development permissions.
- Audit privileged operations.
- Avoid wildcard permissions where possible.

### Human Identities vs. Workload Identities

Human identities represent developers, administrators and operators.

Workload identities represent applications, services, virtual machines and automated processes.

Use appropriate identity mechanisms for each category.

Do not distribute a developer's personal cloud credentials to application workloads.

### Example: AWS IAM Policy

The following illustrative policy grants read-only access to one specified S3 bucket.

Replace the placeholder with the intended bucket name.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::example-private-bucket",
        "arn:aws:s3:::example-private-bucket/*"
      ]
    }
  ]
}
```

Review the required resource-level permissions and test the policy before deployment.

### IAM Review Checklist

- [ ] Every identity has a defined purpose.
- [ ] Permissions follow least privilege.
- [ ] Privileged accounts require MFA.
- [ ] Unused credentials are removed.
- [ ] Workloads use dedicated identities.
- [ ] Administrative permissions are restricted.
- [ ] Access is periodically reviewed.
- [ ] Sensitive IAM changes are logged.

Reference: `../authorization/README.md`

## 4. Cloud Credentials and Secrets

Cloud credentials provide access to infrastructure and services.

Exposed credentials can allow attackers to access data, deploy malicious workloads, modify configurations or compromise production systems.

### Common Credential Types

- Cloud access keys.
- Service account credentials.
- API tokens.
- Database passwords.
- SSH private keys.
- OAuth client secrets.
- Deployment credentials.
- Encryption keys.
- Signing certificates.

### Common Exposure Locations

- Git repositories.
- `.env` files.
- Application source code.
- CI/CD variables.
- Container images.
- Build artifacts.
- Debug logs.
- Developer machines.
- Public storage.
- Configuration backups.

### Recommended Practices

- Use managed secret stores.
- Prefer workload identity and temporary credentials.
- Avoid hardcoding secrets.
- Restrict secret access.
- Rotate credentials when appropriate.
- Revoke exposed credentials immediately.
- Monitor credential usage.
- Avoid logging sensitive values.
- Separate production and development secrets.

### Cloud Secret Management Services

| Provider | Service |
|---|---|
| AWS | Secrets Manager and Systems Manager Parameter Store |
| Azure | Key Vault |
| Google Cloud | Secret Manager |

Environment variables can be useful for delivering configuration to processes, but they are not a complete secret-management solution.

Their security depends on process isolation, host permissions, deployment configuration and exposure through logs or diagnostics.

Reference: `../secrets-management/README.md`

## 5. Cloud Network Security

Cloud networking controls how resources communicate with one another and with external systems.

Incorrect network configurations can expose databases, administrative interfaces and internal services to the public internet.

### Common Network Components

- Virtual Private Clouds.
- Virtual networks.
- Subnets.
- Route tables.
- Firewalls.
- Security groups.
- Network ACLs.
- NAT gateways.
- Private endpoints.
- Load balancers.
- VPN connections.

### Network Segmentation

Separate resources according to their security requirements.

Example architecture:

```text
                  Internet
                     |
                Public DNS
                     |
                Load Balancer
                     |
              Public Subnet
                     |
              Application Layer
                     |
              Private Subnet
                     |
               Database Layer
                     |
              Restricted Access
```

A public-facing application should not require a publicly accessible database.

### Network Security Best Practices

- Keep databases on private networks.
- Restrict inbound ports.
- Restrict outbound traffic where practical.
- Use security groups and firewall rules carefully.
- Separate public and private subnets.
- Avoid unnecessary public IP addresses.
- Restrict administrative interfaces.
- Use private connectivity for internal services.
- Monitor unexpected network traffic.
- Review firewall rules periodically.

### Commonly Exposed Ports

| Port | Service | Security consideration |
|---|---|---|
| 22 | SSH | Restrict administrative access |
| 80 | HTTP | Redirect to HTTPS where appropriate |
| 443 | HTTPS | Maintain secure TLS configuration |
| 3306 | MySQL | Avoid public database exposure |
| 5432 | PostgreSQL | Restrict to trusted application networks |
| 6379 | Redis | Avoid public exposure |
| 27017 | MongoDB | Restrict network access |

A port number alone does not determine whether a service is secure. Review authentication, encryption and network restrictions as well.

### Network Review Checklist

- [ ] Public exposure is intentional.
- [ ] Databases are not unnecessarily public.
- [ ] Administrative ports are restricted.
- [ ] Firewall rules follow least privilege.
- [ ] Internal services use private networking.
- [ ] Network segmentation is implemented.
- [ ] Unused network interfaces are removed.
- [ ] Network activity is monitored.

Reference: `../infrastructure/README.md`

## 6. Cloud Storage Security

Cloud object storage is frequently used for images, documents, backups, application assets and user uploads.

Incorrect access policies can expose private information.

### Common Storage Risks

- Public buckets.
- Unrestricted object access.
- Exposed backup files.
- Weak access policies.
- Predictable object permissions.
- Missing encryption.
- Insecure upload handling.
- Long-lived public links.
- Inadequate audit logging.

### Storage Security Best Practices

- Keep private data private by default.
- Block public access unless explicitly required.
- Use narrowly scoped access policies.
- Apply object-level authorization.
- Use short-lived signed URLs where appropriate.
- Encrypt sensitive stored data.
- Validate uploaded file types and content.
- Restrict upload size.
- Scan uploaded files where appropriate.
- Protect backups.
- Enable relevant access logging.
- Apply retention and deletion policies.

### Signed URLs

A signed URL can grant temporary access to a specific object.

Security considerations:

- Set a short expiration time.
- Limit the allowed operation.
- Avoid unnecessarily broad object permissions.
- Do not expose signing credentials.
- Verify that the application checks user authorization before issuing a URL.

A signed URL is a bearer credential. Anyone who obtains it may be able to use it until it expires or becomes invalid.

### Storage Review Checklist

- [ ] Public access is disabled by default.
- [ ] Access policies are restrictive.
- [ ] Private objects require authorization.
- [ ] Signed URLs expire appropriately.
- [ ] Uploads are validated.
- [ ] Sensitive data is encrypted.
- [ ] Backups are protected.
- [ ] Access logs are reviewed.

## 7. Cloud Database Security

Managed cloud databases reduce infrastructure maintenance but still require secure configuration.

### Common Database Risks

- Public database endpoints.
- Weak credentials.
- Excessive database privileges.
- Missing encryption.
- Insecure backups.
- Poor tenant isolation.
- Unrestricted network access.
- Unpatched database versions.
- Missing audit logs.

### Database Security Controls

- Use private connectivity where possible.
- Require strong authentication.
- Restrict database permissions.
- Use encrypted connections.
- Protect backup storage.
- Monitor administrative operations.
- Maintain supported database versions.
- Apply security patches.
- Test recovery procedures.
- Separate database roles by responsibility.

### Application-Level Controls

Cloud database security does not replace application security.

Applications must still:

- Use parameterized queries.
- Enforce authorization.
- Validate untrusted input.
- Restrict returned fields.
- Protect credentials.
- Enforce tenant boundaries.

### Supabase Considerations

For applications using Supabase:

- Enable Row-Level Security on relevant exposed tables.
- Write policies for each required operation.
- Test policies using realistic user identities.
- Keep service-role credentials server-side.
- Review storage access policies.
- Restrict privileged database access.
- Protect database backups and connection strings.

The service-role key can bypass Row-Level Security. It must never be exposed in a browser or mobile application.

Reference: `../database-security/README.md`

## 8. Virtual Machine and Server Security

Cloud virtual machines are still operating systems that require regular security maintenance.

### Common Risks

- Unpatched operating systems.
- Weak SSH authentication.
- Public administrative ports.
- Excessive system privileges.
- Unnecessary running services.
- Insecure file permissions.
- Exposed application processes.
- Missing monitoring.
- Weak backup protection.

### Linux Server Hardening

Recommended practices:

- Apply security updates.
- Disable unnecessary services.
- Restrict SSH access.
- Prefer SSH keys over passwords where appropriate.
- Disable direct root login where practical.
- Use a firewall.
- Restrict user permissions.
- Protect application configuration.
- Monitor authentication attempts.
- Maintain reliable backups.

### Example: Check Listening Services

Linux:

```bash
ss -tulpn
```

This displays listening network sockets and associated process information where permissions allow.

### Example: Review Firewall Rules

For systems using UFW:

```bash
sudo ufw status verbose
```

Do not enable or change firewall policies remotely without confirming that the required management access will remain available.

### Server Security Checklist

- [ ] Operating system is supported.
- [ ] Security patches are applied.
- [ ] SSH access is restricted.
- [ ] Unnecessary services are disabled.
- [ ] Firewall rules are configured.
- [ ] Application processes use restricted accounts.
- [ ] Sensitive files have appropriate permissions.
- [ ] Logs are collected.
- [ ] Backups are protected.

Reference: `../infrastructure/README.md`

## 9. Container Security

Containers make application deployment portable but introduce security concerns involving images, privileges, runtime configuration and orchestration.

### Common Container Risks

- Vulnerable base images.
- Outdated packages.
- Running as root.
- Privileged containers.
- Exposed Docker sockets.
- Embedded secrets.
- Excessive Linux capabilities.
- Untrusted images.
- Insecure registries.
- Weak container isolation.

### Docker Security Best Practices

- Use trusted base images.
- Pin image versions or digests where appropriate.
- Scan images for vulnerabilities.
- Avoid running as root.
- Use minimal images.
- Remove unnecessary packages.
- Avoid embedding secrets.
- Restrict container capabilities.
- Apply resource limits.
- Keep Docker and host systems updated.
- Restrict access to the Docker daemon.

### Example: Non-Root Container

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

EXPOSE 3000

CMD ["node", "server.js"]
```

This is an illustrative example. Production images should also use appropriate build stages, dependency controls, file ownership, health checks and runtime restrictions.

### Docker Security Review

- [ ] Image sources are trusted.
- [ ] Image vulnerabilities are reviewed.
- [ ] Containers do not unnecessarily run as root.
- [ ] Privileged mode is avoided.
- [ ] Docker socket access is restricted.
- [ ] Secrets are not embedded in images.
- [ ] Container capabilities are restricted.
- [ ] Resource limits are configured.
- [ ] Image updates are managed.

### Kubernetes Security

For Kubernetes environments, review:

- RBAC permissions.
- Service account privileges.
- Network policies.
- Pod security controls.
- Secrets handling.
- Container image provenance.
- Admission policies.
- API server exposure.
- Cluster upgrades.
- Audit logging.

Reference:

https://kubernetes.io/docs/concepts/security/

## 10. Serverless Security

Serverless services execute application code through managed cloud platforms.

Examples include:

- AWS Lambda.
- Azure Functions.
- Google Cloud Run functions and Cloud Functions.

The provider manages much of the infrastructure, but application and identity security remain important.

### Common Serverless Risks

- Excessive execution permissions.
- Publicly exposed functions.
- Unauthenticated invocation.
- Insecure environment variables.
- Weak event validation.
- Dependency vulnerabilities.
- Excessive resource consumption.
- Insecure third-party integrations.
- Sensitive information in logs.

### Serverless Security Best Practices

- Give each function a dedicated identity.
- Restrict invocation permissions.
- Validate incoming events.
- Authenticate requests where required.
- Verify webhook signatures.
- Protect environment secrets.
- Restrict outbound access where practical.
- Set execution timeouts.
- Configure memory and concurrency limits.
- Monitor invocation patterns.
- Maintain dependencies.
- Avoid logging sensitive data.

### Event Validation

Never assume that an event is trustworthy merely because it arrives through a cloud event system.

Validate:

- Event source.
- Expected structure.
- Required fields.
- Authentication context.
- Signature or service identity where applicable.
- Replay protection where relevant.

## 11. Cloud Encryption and Key Management

Encryption helps protect sensitive information against unauthorized disclosure.

Cloud environments commonly support encryption at rest and encryption in transit.

### Encryption at Rest

Protects stored information.

Examples:

- Database storage.
- Object storage.
- Virtual machine disks.
- Backup archives.
- Managed storage services.

### Encryption in Transit

Protects information as it moves between systems.

Examples:

- HTTPS.
- TLS-protected database connections.
- Secure service-to-service communication.
- Encrypted administrative connections.

### Key Management

Cloud providers offer managed key management services.

| Provider | Service |
|---|---|
| AWS | Key Management Service |
| Azure | Key Vault |
| Google Cloud | Cloud Key Management Service |

### Key Management Best Practices

- Restrict key administration.
- Separate key usage from key management.
- Enable appropriate audit logging.
- Define key rotation policies.
- Protect key backups where applicable.
- Avoid embedding keys in application code.
- Use managed key services where appropriate.
- Review key access permissions.
- Plan for key revocation and recovery.

Encryption does not automatically provide access control. Applications must still enforce authorization.

Reference: `../cryptography/README.md`

## 12. Cloud Logging and Monitoring

Cloud monitoring provides visibility into resource activity, application behavior and security events.

Without reliable logging, suspicious activity may remain undetected.

### Important Events to Monitor

- Privileged account activity.
- IAM policy changes.
- New credential creation.
- Failed authentication attempts.
- Public storage configuration changes.
- Firewall modifications.
- Unexpected resource creation.
- Unusual API activity.
- Database administrative actions.
- Deployment changes.
- Resource deletion.

### Cloud Monitoring Services

| Provider | Services |
|---|---|
| AWS | CloudWatch and CloudTrail |
| Azure | Azure Monitor and Microsoft Sentinel |
| Google Cloud | Cloud Logging and Cloud Audit Logs |

### Logging Best Practices

- Enable relevant audit logs.
- Protect log integrity.
- Restrict log access.
- Use centralized collection.
- Establish retention policies.
- Monitor suspicious activity.
- Configure meaningful alerts.
- Avoid logging credentials.
- Protect logs from unauthorized deletion.
- Test alert delivery.

### Alerting Considerations

Prioritize events that may indicate:

- Account compromise.
- Privilege escalation.
- Credential abuse.
- Unauthorized data access.
- Public exposure.
- Unexpected infrastructure changes.

Reference: `../logging-monitoring/README.md`

## 13. Infrastructure as Code Security

Infrastructure as Code (IaC) allows infrastructure to be defined and managed through configuration files.

Examples:

- Terraform.
- OpenTofu.
- AWS CloudFormation.
- Azure Bicep.
- Kubernetes manifests.
- Docker Compose.

IaC improves consistency but can also reproduce insecure configurations at scale.

### Common IaC Risks

- Public storage.
- Overly permissive IAM policies.
- Unrestricted network access.
- Unencrypted storage.
- Weak resource configurations.
- Exposed credentials.
- Missing logging.
- Insecure container settings.

### IaC Security Tools

| Tool | Purpose |
|---|---|
| Checkov | Infrastructure configuration scanning |
| Trivy | Vulnerability and misconfiguration scanning |
| tfsec | Terraform security scanning |
| KICS | Infrastructure as Code analysis |
| Terrascan | Infrastructure configuration analysis |

Check the maintenance status and supported configuration formats of each tool before adopting it.

### Example: Checkov

Official documentation:

https://www.checkov.io/

Install:

```bash
python -m pip install checkov
```

Scan a directory:

```bash
checkov -d .
```

Review findings before deployment.

### IaC Security Best Practices

- Review infrastructure changes.
- Scan configurations before deployment.
- Store IaC in version control.
- Avoid hardcoded credentials.
- Restrict resource permissions.
- Use approved base configurations.
- Separate environments.
- Review generated plans.
- Protect state files.
- Restrict deployment identities.

## 14. Cloud Security Posture Management

Cloud Security Posture Management (CSPM) helps identify cloud misconfigurations and security risks across cloud environments.

### Typical CSPM Findings

- Public storage.
- Excessive permissions.
- Unencrypted resources.
- Missing audit logs.
- Exposed network services.
- Insecure configurations.
- Unused credentials.
- Noncompliant resource settings.

### CSPM Capabilities

- Continuous configuration assessment.
- Security policy evaluation.
- Misconfiguration detection.
- Asset visibility.
- Risk prioritization.
- Compliance monitoring.
- Remediation tracking.

### Common CSPM Options

- AWS Security Hub.
- Microsoft Defender for Cloud.
- Google Security Command Center.
- Wiz.
- Prisma Cloud.
- Orca Security.

Some services provide additional workload protection and threat detection capabilities.

### CSPM Implementation Principles

- Establish a complete asset inventory.
- Define security baselines.
- Identify critical resources.
- Prioritize exposed and high-impact findings.
- Assign remediation owners.
- Track exceptions.
- Verify remediation.
- Review recurring findings.

A CSPM tool provides visibility, not a guarantee that every cloud vulnerability will be detected.

## 15. Cloud Compliance and Governance

Organizations may need to satisfy legal, regulatory, contractual or internal security requirements.

Common frameworks and standards include:

- ISO/IEC 27001.
- SOC 2.
- PCI DSS.
- GDPR.
- HIPAA, where applicable.
- CIS Benchmarks.
- NIST Cybersecurity Framework.

### Governance Controls

- Define cloud security policies.
- Maintain asset inventories.
- Classify sensitive data.
- Establish access review procedures.
- Define approved cloud services.
- Document security exceptions.
- Maintain audit evidence.
- Review third-party providers.
- Establish incident response responsibilities.

Compliance does not automatically mean an application is secure.

Security controls should be selected according to the application's actual risks and obligations.

## 16. Cloud Backup and Disaster Recovery

Cloud infrastructure can fail because of accidental deletion, malicious activity, software defects, provider outages and operational mistakes.

Backups and recovery procedures reduce the impact of such events.

### Important Concepts

**Recovery Point Objective (RPO)**

The maximum acceptable amount of data loss measured in time.

**Recovery Time Objective (RTO)**

The target time required to restore a service after disruption.

### Backup Best Practices

- Back up critical application data.
- Protect backup permissions.
- Encrypt sensitive backups.
- Separate backup administration.
- Maintain appropriate retention.
- Protect against accidental deletion.
- Consider immutable backups.
- Test restoration regularly.
- Document recovery procedures.
- Monitor backup failures.

### Disaster Recovery Planning

A recovery plan should identify:

- Critical services.
- Recovery dependencies.
- Responsible personnel.
- Backup locations.
- Restoration procedures.
- Required credentials.
- Communication channels.
- Recovery priorities.
- Validation requirements.

A backup that has never been restored successfully should not be treated as a proven recovery capability.

## 17. Cloud Security Testing

Cloud security testing evaluates whether cloud resources and configurations satisfy security requirements.

### Assessment Areas

**Identity**

- Excessive permissions.
- Missing MFA.
- Unused identities.
- Long-lived credentials.

**Networking**

- Public database exposure.
- Unrestricted firewall rules.
- Exposed administrative interfaces.
- Insecure service communication.

**Storage**

- Public buckets.
- Weak access policies.
- Unprotected backups.
- Missing encryption.

**Compute**

- Vulnerable operating systems.
- Insecure containers.
- Excessive privileges.
- Missing security updates.

**Monitoring**

- Missing audit logs.
- Unprotected log storage.
- Missing critical alerts.

**Infrastructure as Code**

- Insecure templates.
- Hardcoded credentials.
- Public resource configurations.

### Cloud Security Testing Tools

- Prowler.
- ScoutSuite.
- Cloudsplaining.
- Steampipe.
- Trivy.
- Checkov.
- Provider-native security services.

Official resources:

- https://prowler.com/
- https://github.com/nccgroup/ScoutSuite
- https://github.com/salesforce/cloudsplaining
- https://trivy.dev/

### Example: Prowler

Prowler performs security assessments for supported cloud environments.

Official documentation:

https://docs.prowler.com/

Before running an assessment:

1. Obtain authorization.
2. Identify the account and environment.
3. Use an appropriately scoped read-only identity where supported.
4. Review required permissions.
5. Understand the checks being executed.
6. Protect assessment output.
7. Validate findings.

Never run cloud assessment tools against accounts you do not own or have permission to assess.

## 18. Cloud Security for AI-Assisted Development

AI coding agents can generate infrastructure configurations, deployment scripts, container definitions and cloud integration code.

These outputs must be reviewed before use.

### Common Risks

- Overly permissive IAM policies.
- Public database configurations.
- Exposed cloud credentials.
- Insecure storage policies.
- Unrestricted firewall rules.
- Unsafe deployment scripts.
- Incorrect secret handling.
- Privileged containers.
- Missing audit logging.

### AI-Assisted Cloud Review

Before deploying generated infrastructure:

1. Review all IAM policies.
2. Check network exposure.
3. Verify storage permissions.
4. Inspect secret handling.
5. Scan infrastructure configurations.
6. Review container privileges.
7. Verify encryption settings.
8. Check logging and monitoring.
9. Review deployment permissions.
10. Test in an isolated environment.

### Suggested AI Agent Instruction

```text
Review this cloud infrastructure configuration for
security weaknesses.

Examine:

1. IAM permissions and workload identities.
2. Publicly exposed resources.
3. Network security rules.
4. Storage access policies.
5. Encryption configuration.
6. Secret handling.
7. Container and workload privileges.
8. Logging and monitoring.
9. Backup and recovery controls.
10. Infrastructure deployment permissions.

For each finding:

- Identify the affected resource or file.
- Explain the security risk.
- Provide supporting evidence.
- Classify severity with justification.
- Recommend a specific fix.
- Identify any assumptions or limitations.

Do not claim that a configuration is secure without
reviewing its relevant dependencies and context.

Do not access external infrastructure or modify
production resources.
```

AI-generated infrastructure should be treated as untrusted until reviewed and tested.

## 19. Cloud Incident Response

Cloud incidents may involve compromised identities, exposed storage, unauthorized deployments, data access or malicious infrastructure changes.

### Common Cloud Incident Scenarios

- Leaked cloud access keys.
- Compromised administrator accounts.
- Unauthorized IAM modifications.
- Public exposure of private storage.
- Suspicious compute resources.
- Unexpected data transfer.
- Ransomware affecting cloud workloads.
- Compromised deployment pipelines.

### Initial Response Priorities

1. Confirm the suspected incident.
2. Identify affected accounts and resources.
3. Preserve relevant evidence.
4. Restrict compromised access.
5. Revoke or rotate exposed credentials.
6. Contain affected workloads.
7. Investigate activity logs.
8. Restore trusted configurations.
9. Verify recovery.
10. Document lessons learned.

Do not destroy affected resources or logs before considering evidence preservation and incident response requirements.

### Provider Resources

- AWS Security Incident Response: https://docs.aws.amazon.com/security-incident-response/
- Microsoft Security Response Center: https://msrc.microsoft.com/
- Google Cloud Security: https://cloud.google.com/security

Reference: `../incident-response/README.md`

## 20. Practical Cloud Security Checklist

### Identity and Access

- [ ] Human identities are individually assigned.
- [ ] Workloads use dedicated identities.
- [ ] Least privilege is enforced.
- [ ] MFA protects privileged access.
- [ ] Long-lived credentials are minimized.
- [ ] Unused identities are removed.
- [ ] Privileged changes are logged.

### Network

- [ ] Public exposure is intentional.
- [ ] Databases use restricted network access.
- [ ] Administrative ports are restricted.
- [ ] Firewall policies are reviewed.
- [ ] Internal services use appropriate private connectivity.
- [ ] Unnecessary public IP addresses are removed.

### Storage and Data

- [ ] Storage is private by default.
- [ ] Access policies are reviewed.
- [ ] Sensitive data is encrypted.
- [ ] Signed URLs expire appropriately.
- [ ] Backups are protected.
- [ ] Retention policies are defined.
- [ ] Data deletion requirements are addressed.

### Compute and Containers

- [ ] Operating systems are supported.
- [ ] Security updates are applied.
- [ ] Containers use trusted images.
- [ ] Unnecessary privileges are removed.
- [ ] Runtime secrets are protected.
- [ ] Resource limits are configured.
- [ ] Workloads are monitored.

### Application and Deployment

- [ ] Application authentication is secure.
- [ ] Authorization is enforced server-side.
- [ ] Cloud credentials are not embedded in clients.
- [ ] Dependencies are scanned.
- [ ] Infrastructure as Code is reviewed.
- [ ] CI/CD permissions are restricted.
- [ ] Deployment changes are auditable.

### Monitoring and Recovery

- [ ] Audit logging is enabled.
- [ ] Critical alerts are configured.
- [ ] Log access is restricted.
- [ ] Backups are maintained.
- [ ] Recovery procedures are documented.
- [ ] Restoration is tested.
- [ ] Incident response responsibilities are defined.

## 21. Common Cloud Security Mistakes

| Mistake | Potential consequence |
|---|---|
| Using administrator permissions for applications | Excessive access after compromise |
| Public database exposure | Unauthorized database access |
| Public storage buckets | Sensitive data disclosure |
| Hardcoded cloud credentials | Credential compromise |
| Missing audit logs | Reduced incident visibility |
| Unrestricted firewall rules | Unnecessary attack surface |
| Unpatched virtual machines | Exploitable vulnerabilities |
| Privileged containers | Increased host compromise risk |
| Unprotected backups | Data theft or destruction |
| Untested recovery procedures | Failed disaster recovery |
| Ignoring cloud findings | Persistent misconfigurations |
| Trusting generated infrastructure blindly | Automated deployment of insecure resources |

## 22. Recommended Cloud Security Resources

### Cloud Provider Documentation

- [AWS Security Documentation](https://docs.aws.amazon.com/security/)
- [Microsoft Azure Security](https://learn.microsoft.com/azure/security/)
- [Google Cloud Security](https://cloud.google.com/security)

### Security Standards and Guidance

- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST SP 800-53](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- [OWASP Cloud-Native Application Security](https://owasp.org/www-project-cloud-native-application-security-top-10/)

### Assessment Tools

- [Prowler](https://prowler.com/)
- [ScoutSuite](https://github.com/nccgroup/ScoutSuite)
- [Checkov](https://www.checkov.io/)
- [Trivy](https://trivy.dev/)
- [Steampipe](https://steampipe.io/)

### Infrastructure and Container Security

- [Docker Security](https://docs.docker.com/engine/security/)
- [Kubernetes Security](https://kubernetes.io/docs/concepts/security/)
- [Terraform Security](https://developer.hashicorp.com/terraform/tutorials/security)

---

Cloud security is an ongoing responsibility. Secure configurations, restricted identities, protected data, continuous monitoring and tested recovery procedures work together to reduce cloud-related risks.

No cloud provider, security scanner or compliance certification guarantees that an application or infrastructure is completely secure.

All cloud assessments and security testing must be performed only against accounts and resources that you own or are explicitly authorized to assess.
