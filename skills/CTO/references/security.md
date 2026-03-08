# Security Review — CTO Reference

## When to Load
Load this reference when:
- The user asks about security specifically (`/CTO security`)
- The codebase handles authentication, authorization, or sensitive data
- You notice auth flows, API keys, tokens, encryption, or user data handling
- The project has network-facing endpoints or processes user input

## Security Evaluation Framework

### Authentication & Authorization
- Auth flows follow current best practices (OAuth 2.0, PKCE, etc.)
- Session management is secure (proper expiry, rotation, invalidation)
- Role-based access control is properly enforced
- No hardcoded credentials, keys, or secrets in source

### Input Validation & Injection
- All user input is validated at system boundaries
- SQL injection prevention (parameterized queries, ORM usage)
- XSS prevention (output encoding, CSP headers)
- Command injection prevention (no shell exec with user input)
- Path traversal prevention

### Data Protection
- Sensitive data encrypted at rest and in transit
- PII handling follows minimal-collection principles
- Secrets management (env vars, vault, keychain — not in code)
- Proper error messages that don't leak internal state

### API Security
- Rate limiting on public endpoints
- CORS configured correctly (not wildcard in production)
- API authentication is enforced consistently
- Request size limits to prevent DoS

### Dependency Security
- Dependencies are up to date (no known CVEs)
- Minimal dependency surface (fewer deps = fewer attack vectors)
- Lock files committed and used

### Platform-Specific (Apple/iOS/macOS)
- Keychain used for sensitive storage (not UserDefaults)
- App Transport Security not disabled without reason
- Proper entitlements and sandboxing
- No sensitive data in logs or crash reports

## STRIDE Threat Modeling

When reviewing security holistically, use STRIDE to systematically identify threats:

```
# Threat Model: [Application Name]

System Overview:
- Architecture: [Monolith/Microservices/Serverless]
- Data Classification: [PII, financial, health, public]
- Trust Boundaries: [User -> API -> Service -> Database]

STRIDE Analysis:
| Threat              | Component      | Risk  | Mitigation                         |
|---------------------|----------------|-------|------------------------------------|
| Spoofing            | Auth endpoint  | High  | MFA + token binding                |
| Tampering           | API requests   | High  | HMAC signatures + input validation |
| Repudiation         | User actions   | Med   | Immutable audit logging            |
| Info Disclosure     | Error messages | Med   | Generic error responses            |
| Denial of Service   | Public API     | High  | Rate limiting + WAF                |
| Elevation of Priv   | Admin panel    | Crit  | RBAC + session isolation           |

Attack Surface:
- External: Public APIs, OAuth flows, file uploads
- Internal: Service-to-service communication, message queues
- Data: Database queries, cache layers, log storage
```

## CI/CD Security Pipeline

Recommend this pipeline for projects with security requirements:

```yaml
# GitHub Actions security scanning
name: Security Scan
on:
  pull_request:
    branches: [main]

jobs:
  sast:
    name: Static Analysis
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Semgrep SAST
        uses: semgrep/semgrep-action@v1
        with:
          config: >-
            p/owasp-top-ten
            p/cwe-top-25

  dependency-scan:
    name: Dependency Audit
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          severity: 'CRITICAL,HIGH'
          exit-code: '1'

  secrets-scan:
    name: Secrets Detection
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Run Gitleaks
        uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Security Headers (Nginx)

```nginx
server {
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
    add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self';" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Permissions-Policy "camera=(), microphone=(), geolocation=(), payment=()" always;
    server_tokens off;
}
```

## Security-First Principles
- Never recommend disabling security controls
- Always assume user input is malicious
- Prefer well-tested libraries over custom crypto
- No hardcoded credentials, no secrets in logs
- Default to deny; whitelist over blacklist

## Review Output Format
When delivering a security-focused review, add a **Security Assessment** section:

```markdown
### Security Assessment
- **Risk Level:** [Low / Medium / High / Critical]
- **Attack Surface:** [Summary of exposed interfaces]
- **Findings:**
  1. **[Finding]** — [Risk and impact]
     - Severity: [Low / Medium / High / Critical]
     - Fix: [Specific remediation]
```
