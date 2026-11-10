
# Cryptography

Cryptography is a fundamental part of application security.

It helps protect information against unauthorized disclosure, modification, impersonation and other security threats.

Modern applications use cryptography for password storage, authentication, encrypted communication, database protection, digital signatures, secure file storage, API communication and identity verification.

However, cryptography is easy to implement incorrectly. Weak algorithms, exposed keys, insecure random number generation, improper certificate validation and custom encryption implementations can undermine otherwise well-designed applications.

This guide explains practical cryptographic concepts, implementation principles, common mistakes and security testing approaches for web applications, mobile applications, APIs, databases, cloud infrastructure and AI-powered systems.

## 1. Understanding Cryptography

Cryptography is the discipline of protecting information using mathematical techniques and cryptographic algorithms.

It provides several important security properties.

| Security property | Purpose |
|---|---|
| Confidentiality | Prevent unauthorized parties from reading information |
| Integrity | Detect unauthorized modification |
| Authentication | Verify the identity of a party or the origin of information |
| Non-repudiation | Provide evidence of origin or approval under appropriate trust and legal conditions |

These properties are related but not interchangeable.

For example:

- Encryption protects confidentiality.
- Cryptographic hashes help detect changes.
- Message authentication codes help verify integrity and authenticity.
- Digital signatures provide integrity and public-key verification of origin.
- TLS combines multiple cryptographic mechanisms to secure communication.

No single algorithm solves every security requirement.

## 2. Core Cryptographic Concepts

Before selecting cryptographic algorithms, understand the basic building blocks.

### Plaintext

The original readable information before encryption.

Example:

```text
Hello, ShipSafe!
```

### Ciphertext

The transformed information produced by encryption.

It should not reveal the original information to an unauthorized observer.

### Encryption

A process that transforms plaintext into ciphertext using an algorithm and a key.

### Decryption

A process that converts ciphertext back into plaintext using the appropriate decryption key.

### Cryptographic Key

A value used by a cryptographic algorithm.

Keys must be generated, stored, accessed, rotated and retired securely.

### Initialization Vector and Nonce

An initialization vector (IV) or nonce is an additional input used by certain cryptographic algorithms.

Their requirements depend on the algorithm.

For example, AES-GCM requires nonce uniqueness for a given key. Reusing a nonce with the same key can seriously compromise security.

### Salt

A salt is a random value added to a password before password hashing.

It helps prevent identical passwords from producing identical stored hashes and reduces the effectiveness of precomputed hash attacks.

### Key Derivation

Key derivation transforms an existing secret or password into cryptographic key material.

Different key derivation functions serve different purposes.

## 3. Types of Cryptography

Cryptographic systems generally use several different techniques.

### Symmetric Cryptography

Symmetric encryption uses the same secret key for encryption and decryption.

<text></text>

```text
Plaintext
    |
    v
Encryption + Secret Key
    |
    v
Ciphertext
    |
    v
Decryption + Same Secret Key
    |
    v
Plaintext
```

Common algorithms:

- AES.
- ChaCha20.

Common authenticated encryption constructions:

- AES-GCM.
- ChaCha20-Poly1305.

Advantages:

- Efficient for large amounts of data.
- Suitable for file and database encryption.
- Relatively fast.

Challenges:

- The secret key must be protected.
- Secure key distribution is required.
- Nonce and IV requirements must be respected.

### Asymmetric Cryptography

Asymmetric cryptography uses a key pair:

- Public key.
- Private key.

The public key can generally be shared. The private key must remain protected.

Common uses:

- Digital signatures.
- Key establishment.
- Identity verification.
- Certificate-based authentication.

Common algorithms and schemes:

- RSA.
- Elliptic Curve Cryptography.
- Ed25519.
- ECDSA.
- ECDH.

Some asymmetric algorithms are designed for signatures, others for key establishment or encryption. They are not interchangeable.

### Hybrid Cryptography

Modern secure communication systems often combine symmetric and asymmetric cryptography.

For example, TLS uses authenticated key establishment to establish shared traffic keys, then uses efficient symmetric authenticated encryption for application data.

This approach combines efficient data encryption with secure key establishment.

## 4. Cryptographic Hashing

A cryptographic hash function transforms input data into a fixed-length digest.

A secure hash function is designed to make it computationally difficult to:

- Recover the original input.
- Find another input with the same digest.
- Find two different inputs producing the same digest.

### Common Hashing Algorithms

| Algorithm | Recommendation |
|---|---|
| SHA-256 | Suitable for many general-purpose hashing needs |
| SHA-384 | Suitable for applications requiring a longer digest |
| SHA-512 | Suitable for general-purpose cryptographic hashing |
| SHA-3 | Modern standardized hash family |
| MD5 | Avoid for security-sensitive uses |
| SHA-1 | Avoid for collision-sensitive security uses |

### Example: Hashing a File

A developer can calculate a SHA-256 digest to check whether a file matches an expected digest.

```text
File
 |
 v
SHA-256
 |
 v
Fixed-Length Digest
```

If the file changes, the digest will almost certainly change.

However, a digest alone does not prove that a file is authentic.

An attacker who can replace both the file and its expected digest can defeat a simple checksum comparison.

For authenticity, use a trusted signature or authenticated distribution mechanism.

### Hashing Is Not Encryption

Hashing is generally one-way.

Encryption is designed to be reversible with the appropriate key.

Do not use a plain hash when the application needs to recover the original information.

## 5. Password Hashing

Passwords require specialized protection.

Never store plaintext passwords.

Do not use ordinary fast hashes such as SHA-256 directly for password storage.

Fast hashes allow attackers to test large numbers of password guesses quickly if password databases are exposed.

### Recommended Password Hashing Algorithms

- Argon2id.
- scrypt.
- bcrypt.
- PBKDF2.

Use a maintained implementation and follow current security guidance for its parameters.

### Argon2id

Argon2id is a memory-hard password hashing algorithm.

Its resource requirements make large-scale password guessing more expensive.

It is a strong choice for new applications where suitable library support exists.

### bcrypt

bcrypt is an established password hashing algorithm.

Its work factor should be configured appropriately and reviewed as computing capabilities change.

### PBKDF2

PBKDF2 is a password-based key derivation function.

It remains useful in environments requiring compatibility with established standards or cryptographic modules.

Use suitable parameters and a unique random salt.

### Password Storage Workflow

```text
User Password
      |
      v
Password Hashing Function
      |
      +------ Random Salt
      |
      +------ Work Parameters
      |
      v
Stored Password Hash
```

Store the encoded hash and the parameters required for verification.

### Password Storage Best Practices

- Use a dedicated password hashing library.
- Generate a unique random salt for each password.
- Use a suitable work factor.
- Never store plaintext passwords.
- Never log passwords.
- Avoid custom password hashing schemes.
- Rehash passwords when parameters become outdated.
- Protect password reset tokens.
- Use rate limiting.
- Support MFA where appropriate.

### Password Pepper

A pepper is an additional secret used in a password hashing design.

Unlike a per-password salt, a pepper is shared across a defined scope and must be stored separately from the password database.

It can add protection against certain database-only compromises, but it introduces key management and rotation complexity.

Do not implement peppering casually without understanding the consequences of losing or rotating the pepper.

## 6. Key Derivation Functions

Key derivation functions generate cryptographic keys from input material.

### HKDF

HKDF is a key derivation function based on a cryptographic hash function.

It is useful for deriving keys from suitable high-entropy input keying material.

Common uses:

- Deriving multiple purpose-specific keys.
- Deriving session keys.
- Separating cryptographic contexts.

HKDF is not a password hashing algorithm.

### PBKDF2

PBKDF2 derives keys from passwords using repeated processing.

It is designed to make password guessing more expensive.

### Argon2id and scrypt

These algorithms are designed to impose significant memory costs, making large-scale password guessing more expensive.

### Important Distinction

| Function | Primary purpose |
|---|---|
| SHA-256 | General-purpose cryptographic hashing |
| HKDF | Deriving keys from suitable key material |
| PBKDF2 | Password-based key derivation |
| Argon2id | Password hashing and password-based derivation |
| scrypt | Memory-hard password hashing and derivation |

Do not substitute one primitive for another without understanding its security properties.

## 7. Symmetric Encryption

Symmetric encryption is commonly used to protect stored files, application data and communication traffic.

### AES

Advanced Encryption Standard (AES) is a widely used symmetric block cipher.

Common key sizes:

- AES-128.
- AES-192.
- AES-256.

AES supports several operating modes.

### AES-GCM

AES-GCM is an authenticated encryption mode.

It provides:

- Confidentiality.
- Integrity.
- Authentication of the encrypted data and associated authenticated data.

It is widely used in modern applications.

### ChaCha20-Poly1305

ChaCha20-Poly1305 is an authenticated encryption construction.

It can be useful on systems where software performance or hardware AES support differs.

### Prefer Authenticated Encryption

For most new application-level encryption, use an authenticated encryption with associated data (AEAD) construction.

Examples:

- AES-GCM.
- ChaCha20-Poly1305.

Authenticated encryption helps prevent undetected modification of ciphertext.

### Associated Authenticated Data

AEAD allows additional information to be authenticated without encrypting it.

For example, an application may authenticate:

- Record identifiers.
- Tenant identifiers.
- Data format versions.
- Context metadata.

This can help prevent ciphertext from being moved into an unintended context, provided the application verifies the associated data correctly.

### Encryption Best Practices

- Use established cryptographic libraries.
- Generate keys with a cryptographically secure random generator.
- Respect nonce requirements.
- Never reuse a GCM nonce with the same key.
- Authenticate before accepting decrypted data.
- Protect keys separately from ciphertext.
- Define key rotation procedures.
- Version encrypted data formats.
- Avoid custom encryption modes.

## 8. Initialization Vectors and Nonces

Some encryption algorithms require an IV or nonce.

These values are not necessarily secret.

However, they must satisfy the requirements of the selected algorithm.

### Important Rules

- Follow the algorithm's nonce requirements.
- Never reuse a nonce where uniqueness is required.
- Use an appropriate random or deterministic construction as specified by the algorithm.
- Store the nonce alongside ciphertext when necessary.
- Do not treat a nonce as an encryption key.
- Never assume every algorithm uses the same IV format.

### AES-GCM Example

For AES-GCM, a 96-bit nonce is commonly recommended.

Nonce uniqueness under a given key is essential.

A secure random generator may be appropriate for nonce generation when collision probability and usage limits are carefully managed. High-volume systems may require a controlled unique nonce construction.

### Common Mistake

Using a fixed IV or reusing the same GCM nonce with the same key can expose plaintext relationships and compromise authentication security.

## 9. Asymmetric Cryptography

Asymmetric cryptography uses public and private keys.

### Public Key

The public key can be distributed to other parties.

### Private Key

The private key must be protected against unauthorized access.

### Common Uses

- Digital signatures.
- Secure key establishment.
- Certificate-based identity.
- Secure communication.
- Code signing.

### RSA

RSA is a public-key cryptographic algorithm used in established signature and encryption schemes.

For new designs:

- Use modern key sizes and secure implementations.
- Prefer RSA-PSS for signatures when appropriate.
- Use RSA-OAEP for RSA encryption where applicable.
- Do not use textbook RSA.
- Do not design custom padding schemes.

RSA encryption should not generally be used to encrypt large application files directly. Use hybrid encryption instead.

### Elliptic Curve Cryptography

Elliptic Curve Cryptography provides public-key cryptographic operations using elliptic curve mathematics.

Examples:

- ECDSA for digital signatures.
- ECDH for key agreement.
- Ed25519 for digital signatures.
- X25519 for key agreement.

Different curves and schemes have different purposes.

### Important Principle

Do not assume that every elliptic curve algorithm supports the same operation.

Use the exact algorithm and key type supported by the selected protocol and library.

## 10. Digital Signatures

Digital signatures help verify the integrity and origin of information.

A signer creates a signature using a private key.

A verifier checks it using the corresponding public key.

### Signature Workflow

```text
Message
   |
   v
Signing Algorithm + Private Key
   |
   v
Digital Signature

Message + Signature
   |
   v
Verification Algorithm + Public Key
   |
   v
Valid / Invalid
```

### Common Signature Algorithms

- Ed25519.
- ECDSA.
- RSA-PSS.

### Common Uses

- Software release verification.
- Signed API requests.
- Document signing.
- Package integrity.
- Secure update mechanisms.
- Certificate infrastructure.

### Signature Best Practices

- Protect private signing keys.
- Verify signatures before trusting signed content.
- Define the exact data being signed.
- Use an unambiguous serialization format.
- Include relevant context and version information.
- Prevent replay where the protocol requires it.
- Manage key rotation and revocation.
- Avoid custom signature formats.

### Signature Verification Does Not Guarantee Everything

A valid signature proves that the signed data matches a signature created by the corresponding private key.

It does not automatically prove:

- That the signer is trustworthy.
- That the signer had legal authority.
- That the content is safe.
- That the signed data is current.
- That the key has not been compromised.

The application must establish trust in the signing key and validate the context.

## 11. Message Authentication Codes

A Message Authentication Code (MAC) verifies message integrity and authenticity using a shared secret key.

### HMAC

HMAC is a widely used MAC construction based on a cryptographic hash function.

Common examples:

- HMAC-SHA-256.
- HMAC-SHA-512.

### Common Uses

- API request signing.
- Webhook verification.
- Internal message authentication.
- Integrity verification between trusted services.

### HMAC Best Practices

- Use a strong random secret key.
- Never expose the secret to clients.
- Use a maintained cryptographic library.
- Compare authentication tags using an appropriate constant-time comparison function.
- Include the correct request context in the authenticated message.
- Validate timestamps and nonces when replay protection is required.
- Rotate shared secrets safely.

### HMAC vs. Digital Signatures

| Property | HMAC | Digital signature |
|---|---|---|
| Keys | Shared secret | Public/private key pair |
| Verification | Requires shared secret | Uses public key |
| Non-repudiation | Not provided | May provide supporting evidence |
| Typical use | Trusted service-to-service authentication | Public verification and signed content |

## 12. Secure Random Number Generation

Cryptographic systems depend on unpredictable values.

Applications commonly generate:

- Session identifiers.
- Password reset tokens.
- API secrets.
- Encryption keys.
- Nonces.
- Verification codes.

### Cryptographically Secure Randomness

Use a cryptographically secure pseudorandom number generator (CSPRNG).

Examples include:

- Operating system secure random APIs.
- Web Crypto API.
- Secure randomness functions in established programming language libraries.

### Never Use Ordinary Random Functions for Secrets

Do not use general-purpose random number generators intended for simulations or simple randomization to generate security-sensitive values.

For example, ordinary random functions in many languages are not designed to produce cryptographically unpredictable values.

### Token Generation

A secure token should:

- Contain sufficient entropy.
- Be generated using a CSPRNG.
- Be stored securely.
- Have an appropriate expiration.
- Be scoped to its purpose.
- Be single-use when appropriate.
- Be invalidated after successful use.

### Verification Codes

Short numeric verification codes have limited entropy.

Protect them with:

- Short expiration periods.
- Rate limiting.
- Attempt limits.
- Purpose binding.
- Secure delivery.
- Replay prevention.

A short code should never be treated as equivalent to a high-entropy cryptographic secret.

## 13. Transport Layer Security (TLS)

TLS protects communication between clients and servers.

It is fundamental to:

- HTTPS.
- API communication.
- Secure database connections.
- Service-to-service communication.
- Secure email transport.

### TLS Provides

- Encryption in transit.
- Integrity protection.
- Server authentication through certificates.
- Optional client authentication.

### Recommended Practices

- Use TLS 1.2 or newer where compatibility requires it.
- Prefer TLS 1.3 where supported.
- Disable obsolete SSL and TLS versions.
- Use trusted certificates.
- Validate certificate chains.
- Validate hostnames.
- Protect private keys.
- Renew certificates before expiration.
- Restrict insecure cipher suites.
- Monitor certificate failures.

### HTTPS Everywhere

Public web applications should use HTTPS.

Additional considerations:

- Redirect HTTP to HTTPS.
- Apply HSTS where appropriate.
- Protect cookies.
- Avoid mixed content.
- Secure internal service communication.
- Verify certificate configuration.

### Important Principle

Encryption in transit does not protect information after it reaches an endpoint.

Applications still require appropriate authorization, storage security, logging controls and endpoint protection.

## 14. Digital Certificates and Public Key Infrastructure

A digital certificate binds an identity to a public key through a trusted certificate authority or another established trust mechanism.

### Certificate Components

A certificate commonly includes:

- Subject information.
- Public key.
- Issuer.
- Validity period.
- Serial number.
- Signature algorithm.
- Certificate authority signature.

### Certificate Authorities

A Certificate Authority (CA) issues and signs certificates under a defined trust framework.

Browsers and operating systems maintain trust stores containing trusted certificate authorities.

### Certificate Validation

Applications and clients should validate:

- Certificate chain.
- Certificate validity period.
- Hostname.
- Trusted issuer.
- Appropriate key usage.
- Relevant revocation or status information where required.

Do not disable certificate validation to work around TLS errors.

### Certificate Management

- Protect private keys.
- Track certificate expiration.
- Automate renewal where appropriate.
- Restrict certificate issuance.
- Monitor unexpected certificates.
- Remove expired or compromised certificates.
- Document recovery procedures.

## 15. Secure Key Management

Cryptographic algorithms are only as secure as their key management.

An application using strong encryption can still be compromised if its keys are exposed.

### Key Lifecycle

A key lifecycle includes:

1. Generation.
2. Provisioning.
3. Distribution.
4. Storage.
5. Access.
6. Usage.
7. Rotation.
8. Revocation.
9. Archival where appropriate.
10. Destruction.

### Key Management Best Practices

- Generate keys using secure randomness.
- Restrict access by least privilege.
- Store keys separately from encrypted data where practical.
- Avoid hardcoding keys.
- Never commit private keys to source control.
- Audit key usage.
- Rotate keys according to policy and risk.
- Revoke compromised keys.
- Protect recovery and backup mechanisms.
- Document key ownership.

### Key Management Services

Examples include:

- AWS Key Management Service.
- Azure Key Vault.
- Google Cloud Key Management Service.
- HashiCorp Vault.
- Hardware Security Modules.

These services can help manage key storage, access and cryptographic operations.

They still require secure configuration, identity management and operational oversight.

### Envelope Encryption

Envelope encryption uses a data encryption key (DEK) to encrypt application data and a key encryption key (KEK) to protect the DEK.

```text
Application Data
       |
       v
Data Encryption Key
       |
       v
Encrypted Data

Data Encryption Key
       |
       v
Key Encryption Key
       |
       v
Encrypted Data Encryption Key
```

The application stores the encrypted data and protected DEK while controlling access to the KEK through a suitable key management system.

This approach can simplify key rotation and large-scale data encryption.

### Key Rotation

Key rotation may involve:

- Generating a replacement key.
- Updating encryption operations.
- Maintaining access to older keys for decryption.
- Re-encrypting data where appropriate.
- Revoking compromised keys.
- Verifying successful migration.

Simply replacing an encryption key can make existing encrypted data permanently inaccessible.

Plan rotation and recovery before deployment.

## 16. Application-Level Encryption

Some applications need encryption beyond the protection provided by the database or storage platform.

### When to Consider It

- Highly sensitive user records.
- Confidential documents.
- Financial information.
- Private communications.
- Sensitive identifiers.
- Information requiring restricted database-level access.

### Design Considerations

- What threat is the encryption intended to mitigate?
- Who should be able to decrypt the data?
- Where are keys stored?
- Which service performs encryption?
- Can database administrators access plaintext?
- How are keys rotated?
- How are backups handled?
- What happens when keys are lost?

### Important Trade-offs

Application-level encryption may affect:

- Searching.
- Sorting.
- Filtering.
- Indexing.
- Analytics.
- Data migration.
- Recovery.
- Performance.

Do not introduce encryption without understanding how the application needs to use the protected information.

### Searchable Encryption

Some specialized cryptographic techniques support limited searching over encrypted information.

These techniques involve additional complexity and may reveal information through access patterns or other leakage.

Do not assume that searchable encryption provides the same confidentiality as ordinary encryption.

## 17. Password Reset and Token Security

Password reset and account verification workflows depend on secure tokens.

### Common Token Types

- Password reset tokens.
- Email verification tokens.
- Account recovery tokens.
- Invitation tokens.
- API credentials.
- Session identifiers.

### Secure Token Design

- Generate tokens using a CSPRNG.
- Use sufficient entropy.
- Store only a secure representation where appropriate.
- Set short, purpose-appropriate expiration.
- Make single-use tokens invalid after successful use.
- Bind tokens to the intended account and action.
- Avoid exposing tokens in logs.
- Avoid third-party tracking on sensitive token pages.
- Rate-limit token requests.
- Invalidate tokens after relevant account changes.

### URL Token Risks

Tokens in URLs may appear in:

- Browser history.
- Reverse proxy logs.
- Analytics systems.
- Referrer headers.
- Screenshots.
- Shared links.

When URL-based tokens are necessary, reduce exposure with short lifetimes, single-use validation, careful referrer policy and exclusion from analytics and logs.

## 18. Cryptography in Web Applications

Web applications use cryptography across several layers.

### Common Use Cases

- HTTPS.
- Authentication.
- Password storage.
- Session protection.
- API request signing.
- File encryption.
- Signed downloads.
- Secure cookies.
- Digital signatures.

### Browser Cryptography

The Web Crypto API provides browser cryptographic functionality.

It supports operations such as:

- Key generation.
- Encryption and decryption.
- Hashing.
- Digital signatures.
- Key derivation.
- Random value generation.

Official documentation:

https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API

### Browser Security Considerations

- Use secure contexts.
- Protect against cross-site scripting.
- Do not expose server-side master keys.
- Restrict access to sensitive browser storage.
- Avoid custom cryptographic implementations.
- Understand that malicious code executing in the page may access available plaintext.

### Important Principle

Client-side encryption does not automatically protect information from a compromised browser or malicious JavaScript executing within the application.

## 19. Cryptography in Mobile Applications

Mobile applications use cryptography for local storage, secure communication and device authentication.

### Android

Relevant platform capabilities include:

- Android Keystore.
- Hardware-backed key storage where supported.
- Encrypted storage implementations.
- TLS.
- Biometric authentication integrations.

Official documentation:

https://developer.android.com/privacy-and-security/keystore

### iOS

Relevant platform capabilities include:

- Keychain Services.
- Secure Enclave where supported.
- Data Protection APIs.
- TLS.
- Local authentication integrations.

Official documentation:

https://developer.apple.com/documentation/security

### Mobile Best Practices

- Use platform-supported key storage.
- Avoid hardcoded encryption keys.
- Protect sensitive local data.
- Use secure network connections.
- Avoid storing unnecessary personal information.
- Protect backup behavior.
- Handle key invalidation.
- Consider device compromise.
- Test release builds.
- Review cryptographic SDKs.

### Important Limitation

A key stored securely on a device is not automatically protected against every form of device compromise.

Rooted devices, jailbroken devices, runtime instrumentation and malicious application behavior can affect security assumptions.

## 20. Cryptography in APIs and Backend Services

Backend services frequently use cryptography to authenticate requests and protect data.

### Common Applications

- JWT signatures.
- HMAC request signing.
- TLS.
- Password hashing.
- API credential protection.
- Signed webhooks.
- Encrypted database fields.
- Secure service-to-service communication.

### JWT Security

JSON Web Tokens can use digital signatures or message authentication codes.

Common algorithms include:

- RS256.
- ES256.
- EdDSA.
- HS256.

### JWT Best Practices

- Explicitly allow expected algorithms.
- Reject unexpected algorithms.
- Verify signatures.
- Validate issuer.
- Validate audience.
- Validate expiration.
- Validate not-before where relevant.
- Restrict token lifetime.
- Protect signing keys.
- Support key rotation.
- Avoid placing sensitive personal information in readable token payloads.

A signed JWT is not necessarily encrypted. Its payload can generally be decoded by anyone who has the token.

### HMAC API Authentication

When using HMAC-signed API requests:

- Use strong shared secrets.
- Define canonical request serialization.
- Include the method and relevant request components.
- Include a timestamp.
- Use a nonce or request identifier where appropriate.
- Prevent replay.
- Compare authentication tags safely.
- Rotate secrets.

### Webhook Verification

Verify webhook signatures before processing events.

Also consider:

- Timestamp validation.
- Replay protection.
- Event identifiers.
- Secret rotation.
- Duplicate event handling.
- Request size limits.

A valid signature does not necessarily mean the event should be processed more than once.

## 21. Cryptography in Databases and Cloud Systems

Cryptography is widely used in database and cloud infrastructure.

### Database Encryption

Common layers include:

- Disk encryption.
- Database-managed encryption.
- Backup encryption.
- Application-level encryption.
- Field-level encryption.

### Cloud Encryption

Cloud platforms provide services for:

- Key management.
- Secret storage.
- Managed encryption.
- Certificate management.
- Hardware security modules.
- Encrypted storage.

### Cloud Security Practices

- Restrict key management permissions.
- Separate key administrators from data access roles where practical.
- Audit key usage.
- Protect cloud credentials.
- Define rotation policies.
- Restrict cross-account access.
- Review backup encryption.
- Monitor configuration changes.

### Important Principle

Encryption at rest does not prevent an authorized application identity from accessing plaintext.

Combine encryption with strong identity management, authorization, auditing and operational controls.

## 22. Cryptographic Agility and Algorithm Migration

Cryptographic algorithms and security recommendations evolve.

Applications should be designed so cryptographic components can be upgraded without redesigning the entire system.

This capability is known as cryptographic agility.

### Recommended Practices

- Avoid scattering algorithm choices throughout application code.
- Centralize cryptographic operations.
- Store algorithm and format versions with encrypted records.
- Support controlled migration.
- Document key ownership.
- Monitor algorithm deprecation.
- Plan for certificate and key rotation.
- Maintain compatibility with existing encrypted data during transitions.

### Example Versioned Encryption Record

```json
{
  "version": 1,
  "algorithm": "AES-256-GCM",
  "key_id": "application-key-2026-01",
  "nonce": "base64-encoded-nonce",
  "ciphertext": "base64-encoded-ciphertext",
  "tag": "base64-encoded-authentication-tag"
}
```

This is an illustrative record format, not a complete encryption implementation.

Actual formats must define authenticated data, encoding, tag handling, key identification and validation requirements precisely.

## 23. Post-Quantum Cryptography

Quantum computing may eventually threaten some widely used public-key cryptographic algorithms.

In particular, sufficiently capable quantum computers could undermine the security assumptions behind traditional RSA and elliptic-curve public-key systems.

Symmetric cryptography and hash functions face different security considerations.

### NIST Post-Quantum Standards

NIST has published post-quantum cryptographic standards.

Important examples include:

- ML-KEM: Key encapsulation mechanism.
- ML-DSA: Digital signature algorithm.
- SLH-DSA: Stateless hash-based digital signature algorithm.

Official resources:

- https://csrc.nist.gov/projects/post-quantum-cryptography
- https://csrc.nist.gov/pubs/fips/203/final
- https://csrc.nist.gov/pubs/fips/204/final
- https://csrc.nist.gov/pubs/fips/205/final

### Developer Considerations

- Inventory cryptographic dependencies.
- Identify long-lived sensitive information.
- Monitor protocol and library support.
- Follow standards-based migration guidance.
- Avoid inventing hybrid cryptographic schemes.
- Plan for future certificate and key transitions.

### Harvest Now, Decrypt Later

An attacker may collect encrypted information today and attempt to decrypt it in the future using more capable technology.

This matters especially for information that must remain confidential for many years.

Post-quantum migration should be based on a realistic threat assessment and established standards, not speculative custom implementations.

## 24. Common Cryptographic Vulnerabilities

| Vulnerability | Risk |
|---|---|
| Weak encryption algorithm | Confidentiality compromise |
| Hardcoded encryption keys | Key exposure |
| Reused GCM nonce | Severe confidentiality and integrity compromise |
| Weak password hashing | Faster offline password cracking |
| Insufficient key entropy | Predictable cryptographic material |
| Insecure random generation | Predictable tokens or keys |
| Missing authentication | Undetected ciphertext modification |
| Disabled certificate validation | Man-in-the-middle attacks |
| Weak JWT validation | Authentication bypass |
| Exposed private signing keys | Forged signatures |
| Plaintext password storage | Direct credential exposure |
| Deprecated cryptographic protocols | Increased attack surface |
| Improper key rotation | Long-term exposure or data loss |
| Custom cryptographic implementation | Subtle implementation vulnerabilities |

## 25. Common Cryptographic Mistakes

### Mistake 1: Inventing Custom Encryption

Do not create your own encryption algorithms or protocols.

Use established, reviewed cryptographic libraries and standard constructions.

### Mistake 2: Using Base64 as Encryption

Base64 is an encoding scheme.

It does not provide confidentiality, integrity or authentication.

### Mistake 3: Using SHA-256 for Password Storage

SHA-256 is a general-purpose hash function, not a password hashing algorithm.

Use Argon2id, scrypt, bcrypt or PBKDF2 with suitable parameters.

### Mistake 4: Hardcoding Encryption Keys

Keys committed to source control can be exposed through repository access, history, build logs or leaked artifacts.

Use an appropriate secret or key management system.

### Mistake 5: Reusing Nonces

Some cryptographic constructions require unique nonces.

Understand and enforce the exact requirements of the selected algorithm.

### Mistake 6: Ignoring Certificate Errors

Never disable certificate validation simply to make a connection succeed.

Investigate the certificate chain, hostname, expiration and trust configuration.

### Mistake 7: Treating Signed Tokens as Encrypted

A digital signature protects integrity and authenticity.

It does not automatically hide token contents.

### Mistake 8: Storing Secrets in Frontend Code

Anything delivered to a browser or mobile client should be considered accessible to the user or an attacker who can inspect the application.

Keep privileged server-side keys on trusted backend infrastructure.

### Mistake 9: Losing Encryption Keys

Encrypted data may become permanently inaccessible if its required keys are lost.

Plan backup, recovery and key lifecycle procedures.

### Mistake 10: Assuming Encryption Solves Authorization

Encryption does not replace authorization.

An application must still decide who is allowed to access or decrypt each record.

## 26. Cryptographic Libraries and Developer Resources

Use established libraries maintained by trusted projects.

### General Cryptographic Libraries

- [OpenSSL](https://www.openssl.org/)
- [Google Tink](https://github.com/tink-crypto/tink)
- [libsodium](https://doc.libsodium.org/)
- [Bouncy Castle](https://www.bouncycastle.org/)
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

### Programming Language Resources

- [Python Cryptography](https://cryptography.io/)
- [Node.js Crypto](https://nodejs.org/api/crypto.html)
- [Go Cryptography](https://pkg.go.dev/crypto)
- [Java Security](https://docs.oracle.com/en/java/javase/21/security/java-security-overview1.html)
- [Rust Crypto](https://github.com/RustCrypto)

### Security Standards and Guidance

- [NIST Cryptographic Standards](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines)
- [NIST Key Management Guidelines](https://csrc.nist.gov/projects/key-management)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
- [RFC 8446: TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446)
- [RFC 9180: Hybrid Public Key Encryption](https://www.rfc-editor.org/rfc/rfc9180)

## 27. Cryptography Testing

Cryptographic implementations should be tested carefully.

### Algorithm and Configuration Testing

- [ ] Only approved algorithms are used.
- [ ] Deprecated algorithms are avoided.
- [ ] Key sizes are appropriate.
- [ ] Password hashing parameters are appropriate.
- [ ] TLS versions are configured securely.
- [ ] Certificate validation is enabled.

### Key Management Testing

- [ ] Keys are generated securely.
- [ ] Keys are not hardcoded.
- [ ] Private keys are access-controlled.
- [ ] Key rotation is documented.
- [ ] Compromised keys can be revoked.
- [ ] Recovery procedures are tested.

### Encryption Testing

- [ ] Encryption uses authenticated encryption where appropriate.
- [ ] Nonce requirements are satisfied.
- [ ] Authentication failures are rejected.
- [ ] Modified ciphertext is not accepted.
- [ ] Associated authenticated data is validated.
- [ ] Plaintext is not unnecessarily logged.

### Password Testing

- [ ] Passwords are never stored in plaintext.
- [ ] A suitable password hashing function is used.
- [ ] Unique salts are generated.
- [ ] Work parameters are configured appropriately.
- [ ] Password reset tokens are protected.
- [ ] Authentication attempts are rate-limited.

### Certificate and TLS Testing

- [ ] Certificates are valid.
- [ ] Hostnames are verified.
- [ ] Certificate chains are validated.
- [ ] Expiration is monitored.
- [ ] Obsolete protocols are disabled.
- [ ] Private keys are protected.

### Signature and Token Testing

- [ ] Signatures are verified.
- [ ] Unexpected algorithms are rejected.
- [ ] JWT claims are validated.
- [ ] Signing keys are protected.
- [ ] Replay protection is implemented where needed.
- [ ] Webhook signatures are checked before processing.

## 28. Cryptography Review Checklist for AI-Assisted Development

AI coding agents can generate encryption code, authentication flows, token handlers and cryptographic configuration.

Generated code must not be trusted merely because it compiles or passes basic tests.

### Before Accepting Generated Code

- [ ] Identify the security requirement.
- [ ] Verify the selected algorithm.
- [ ] Verify the cryptographic library.
- [ ] Review key generation.
- [ ] Review nonce and IV handling.
- [ ] Review password hashing.
- [ ] Review authentication tag verification.
- [ ] Review certificate validation.
- [ ] Review key storage.
- [ ] Review error handling.
- [ ] Review sensitive data logging.
- [ ] Review dependency maintenance.

### Questions to Ask an AI Coding Agent

1. Which cryptographic standard is this implementation based on?
2. Why was this algorithm selected?
3. Does the implementation use authenticated encryption?
4. How are encryption keys generated and stored?
5. How are nonce uniqueness requirements satisfied?
6. How are password hashes generated and verified?
7. How does the application handle key rotation?
8. Are certificate validation errors handled securely?
9. Can the implementation expose plaintext or secrets in logs?
10. Which independent tests or reviews validate this implementation?

### Important Rule

Never ask an AI coding agent to invent a cryptographic algorithm or protocol.

Ask it to use established libraries, documented standards and platform-supported security features.

Cryptographic code handling production secrets should receive careful human security review.

## 29. Practical Cryptography Review Checklist

### General

- [ ] Cryptographic requirements are documented.
- [ ] Established algorithms are used.
- [ ] Maintained libraries are selected.
- [ ] Custom cryptography is avoided.
- [ ] Deprecated algorithms are identified.

### Encryption

- [ ] Appropriate authenticated encryption is used.
- [ ] Keys are generated securely.
- [ ] Nonce requirements are respected.
- [ ] Authentication failures are handled safely.
- [ ] Sensitive plaintext is protected.

### Passwords

- [ ] Passwords are never stored in plaintext.
- [ ] Password hashing uses a dedicated algorithm.
- [ ] Salts are unique and securely generated.
- [ ] Password reset tokens are protected.
- [ ] Authentication attempts are restricted.

### Key Management

- [ ] Keys are not committed to Git.
- [ ] Keys are stored securely.
- [ ] Access is restricted.
- [ ] Rotation is planned.
- [ ] Compromised keys can be revoked.
- [ ] Recovery procedures exist.

### Communication

- [ ] HTTPS is enforced.
- [ ] TLS configuration is appropriate.
- [ ] Certificates are validated.
- [ ] Private keys are protected.
- [ ] Internal service communication is reviewed.

### Signatures and Authentication

- [ ] Signatures are verified.
- [ ] HMAC secrets are protected.
- [ ] JWT algorithms are explicitly controlled.
- [ ] Token claims are validated.
- [ ] Replay protection is considered.

### AI and Dependencies

- [ ] Generated cryptographic code is reviewed.
- [ ] Cryptographic dependencies are maintained.
- [ ] No privileged keys are exposed to clients.
- [ ] AI tools do not receive unnecessary secrets.
- [ ] Security testing is documented.

---

Cryptography is one of the most important foundations of application security, but its effectiveness depends on correct implementation and careful key management.

Use established standards, trusted libraries, secure defaults and regular testing. Never assume that strong algorithms alone make an application secure.

No cryptographic checklist guarantees security. Real-world protection requires a complete understanding of the application's threat model, architecture, implementation and operational environment.
