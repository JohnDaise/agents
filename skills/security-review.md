---
name: security-review
description: Security-focused code review practices and vulnerability detection. USE FOR identifying security vulnerabilities; reviewing code for security best practices; checking for credential leaks, injection attacks, and common security flaws. DO NOT USE FOR general code quality reviews (use code-review-principles skill) or penetration testing.
---

# Security Review Practices

Apply these security-focused checks when reviewing code:

## 1. Credential Management
- **No hardcoded secrets**: API keys, passwords, tokens, connection strings
- **Environment variables**: Sensitive data must use environment variables or secret management
- **Configuration files**: Check for credentials in config files committed to version control
- **Code comments**: Look for accidentally leaked credentials in comments

## 2. Input Validation & Sanitization
- **User input validation**: All user-supplied data must be validated
- **Type checking**: Verify data types match expectations
- **Length limits**: Enforce maximum input lengths
- **Whitelist validation**: Prefer whitelists over blacklists when possible
- **Encoding**: Properly encode data based on context (HTML, URL, SQL)

## 3. Injection Prevention
- **SQL Injection**: Use parameterized queries/prepared statements, never string concatenation
- **Command Injection**: Avoid shell execution with user input; use proper APIs
- **LDAP Injection**: Escape special characters in LDAP queries
- **XML Injection**: Use safe XML parsers and validate input
- **Code Injection**: Never use `eval()` or execute user-supplied code

## 4. Cross-Site Scripting (XSS) Protection
- **Output encoding**: Encode all user data before rendering in HTML
- **Context-aware encoding**: Use appropriate encoding for HTML, JavaScript, URL contexts
- **Content Security Policy**: Implement CSP headers
- **Avoid innerHTML**: Use safer DOM manipulation methods

## 5. Authentication & Authorization
- **Authentication checks**: Verify user identity for protected resources
- **Authorization checks**: Verify user permissions for operations
- **Session management**: Secure session handling with proper timeouts
- **Password security**: Use strong hashing (bcrypt, scrypt, Argon2)
- **Multi-factor authentication**: Consider MFA for sensitive operations

## 6. Data Protection
- **Encryption in transit**: Use HTTPS/TLS for network communication
- **Encryption at rest**: Encrypt sensitive data in storage
- **Secure storage**: Use appropriate storage mechanisms for sensitive data
- **Data exposure**: Avoid logging sensitive information
- **PII handling**: Follow data protection regulations (GDPR, CCPA)

## 7. Error Handling & Logging
- **No sensitive data in errors**: Don't expose system details in error messages
- **Proper logging**: Log security events without leaking credentials
- **Stack traces**: Don't expose full stack traces to users in production
- **Rate limiting**: Implement rate limiting to prevent abuse

## 8. Dependency Security
- **Known vulnerabilities**: Check for vulnerabilities in dependencies
- **Version pinning**: Use specific versions, not ranges
- **Supply chain**: Verify package integrity and sources
- **Regular updates**: Keep dependencies current with security patches

## Security Issue Format

When reporting security issues:

```
🔴 Security Vulnerability: [Type]
File: [path]:[line]
Severity: Critical/High/Medium/Low

Current Code:
[Show vulnerable code]

Risk:
[Explain the security risk and potential impact]

Remediation:
[Provide secure code example]

References:
[OWASP, CWE, or other security standard references]
```

## Common Vulnerability References

- OWASP Top 10: https://owasp.org/www-project-top-ten/
- CWE/SANS Top 25: https://cwe.mitre.org/top25/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
