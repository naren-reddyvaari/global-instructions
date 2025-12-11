# Security Guidelines

## 1. General Principles
- Adopt a **Zero Trust** approach (never trust, always verify).
- Validate all inputs—never trust user-supplied data.
- Apply the **Principle of Least Privilege (PoLP)** for users, services, and APIs.
- Keep dependencies up to date to avoid known vulnerabilities.

## 2. Authentication & Authorization
- Use industry standards: OAuth2, OIDC, JWT.
- Do not implement custom auth unless absolutely required.
- Protect endpoints using role-based access control (RBAC).
- Expire sessions and tokens appropriately.

## 3. Data Protection
- Encrypt sensitive data at rest (AES-256) and in transit (TLS 1.2+).
- Mask sensitive fields in logs and error messages.
- Do not store passwords; always store salted and hashed values.
- Avoid hardcoding secrets, credentials, or API keys.

## 4. Secrets Management
- Use secrets managers such as Vault, AWS Secrets Manager, GCP Secret Manager.
- Access secrets via environment variables or mounted volumes.
- Rotate secrets periodically.
- Do not commit secrets to Git.

## 5. API Security
- Validate input using whitelists.
- Use rate limiting and throttling.
- Implement CSRF protection for stateful web applications.
- Use strong API keys and tokens.

## 6. Logging & Monitoring
- Never log sensitive information (passwords, tokens, PAN, Aadhaar, etc.).
- Enable structured logging.
- Use centralized logging solutions (ELK, Cloud Logging, Splunk).
- Monitor login attempts, access patterns, and suspicious activities.

## 7. Error Handling
- Do not reveal internal details in error messages.
- Return generic error responses for security-related failures.
- Log technical details only on server side.

## 8. Secure Coding Practices
- Validate and sanitize all user inputs.
- Avoid constructing SQL queries dynamically; use prepared statements.
- Use parameterized queries to prevent SQL Injection.
- Sanitize output to prevent XSS.
- Use secure random functions for token generation.

## 9. Dependency & Build Security
- Continuously scan dependencies using Snyk, OWASP Dependency Check, or GitHub Security Alerts.
- Avoid untrusted third-party libraries.
- Verify checksums for downloaded artifacts.

## 10. Cloud & Infrastructure Security
- Use private networks for internal communication.
- Enable VPC Service Controls in cloud environments.
- Ensure firewall rules follow least privilege.
- Enable automatic security updates.
- Enforce secure IAM roles.

## 11. Container Security
- Scan Docker images for vulnerabilities.
- Run containers as non-root users.
- Use minimal base images (e.g., alpine, distroless).
- Restrict container resource limits (CPU, memory).

## 12. Security Testing
- Perform regular penetration testing.
- Include static code analysis (SAST) in CI/CD.
- Include dependency scanning (SCA) in CI/CD.
- Apply dynamic testing (DAST) for running apps.

---
Following these guidelines significantly reduces risks and ensures a secure, production-ready application.