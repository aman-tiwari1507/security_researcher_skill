# Web Application Security Review Checklist

Use this during security reviews of web applications, APIs, or code. Work through relevant sections based on scope. Don't just tick boxes — use this as a prompt to think deeply about each area.

---

## Authentication & Session Management
- [ ] Passwords hashed with bcrypt/scrypt/Argon2 (not MD5, SHA1, unsalted SHA256)
- [ ] No credential stuffing exposure (rate limiting, lockout, CAPTCHA)
- [ ] MFA implemented? Is it bypassable (SIM swap, OTP reuse, backup codes stored securely)?
- [ ] Password reset flow: does it use time-limited, single-use tokens? Is username enumeration possible?
- [ ] Session tokens: sufficient entropy (≥128 bits), not in URL, regenerated after login
- [ ] Session invalidation on logout (server-side), on password change, on privilege change
- [ ] JWT: algorithm validated server-side, `alg: none` rejected, weak secrets ruled out, `kid` header not injectable, expiry (`exp`) enforced
- [ ] OAuth 2.0: state parameter used (CSRF protection), redirect_uri validated strictly, PKCE for public clients
- [ ] Concurrent session handling — is it intentional?
- [ ] "Remember me" functionality — how are persistent tokens stored and validated?

---

## Authorization & Access Control
- [ ] Every endpoint enforces authorization — not just UI hiding
- [ ] Vertical privilege escalation tested: can regular user reach admin functions?
- [ ] Horizontal privilege escalation (IDOR): can user A access user B's resources by changing IDs?
- [ ] BOLA/BFLA tested in API: object-level and function-level authorization
- [ ] Mass assignment: are all object properties whitelisted, not blacklisted?
- [ ] Insecure direct object reference: are references to files, records, internal IDs exposed?
- [ ] Role-based access control (RBAC): are roles enforced consistently, including edge cases?
- [ ] JWT/token claims are validated, not blindly trusted

---

## Injection
- [ ] SQL injection: parameterized queries / prepared statements used everywhere, ORM usage reviewed
- [ ] NoSQL injection: MongoDB `$where`, operator injection checked
- [ ] OS command injection: no user input reaches shell commands; `subprocess` without `shell=True`
- [ ] LDAP injection: input sanitized before directory queries
- [ ] Server-Side Template Injection (SSTI): user input never passed to template engines unsanitized
- [ ] XPath injection: parameterized queries for XML data
- [ ] GraphQL: input validation, query depth/complexity limits, introspection disabled in prod

---

## Cross-Site Scripting (XSS)
- [ ] Output encoding applied in HTML context, JS context, attribute context, URL context — separately
- [ ] Content-Security-Policy header: `unsafe-inline` and `unsafe-eval` avoided; nonces/hashes used
- [ ] DOM-based XSS: `innerHTML`, `document.write`, `eval`, `location.href` reviewed
- [ ] Mutation XSS: `innerHTML` reassignment patterns reviewed
- [ ] Sanitization libraries used (DOMPurify) where HTML must be allowed
- [ ] `X-XSS-Protection: 0` set to disable legacy broken browser behavior

---

## CSRF
- [ ] Anti-CSRF tokens on all state-changing requests, validated server-side
- [ ] `SameSite=Strict` or `SameSite=Lax` on session cookies
- [ ] Sensitive endpoints verify `Origin` or `Referer` header as secondary control
- [ ] Custom request headers (e.g., `X-Requested-With`) used for APIs as lightweight CSRF defense

---

## SSRF
- [ ] Application fetches URLs? Filter applied: scheme allowlist (https only), no internal IPs, no cloud metadata ranges (169.254.169.254, fd00::/8)
- [ ] DNS rebinding mitigations in place?
- [ ] Redirects followed? Each hop re-validated?
- [ ] IMDSv2 enforced on AWS (token-based metadata access)
- [ ] Webhook, URL preview, PDF generator, image fetcher, import-from-URL — all SSRF vectors reviewed

---

## XXE (XML External Entities)
- [ ] XML parsing libraries configured with external entities disabled
- [ ] DTD processing disabled
- [ ] Document uploads (DOCX, XLSX, SVG) — XXE possible in embedded XML?
- [ ] SOAP endpoints reviewed

---

## File Upload
- [ ] MIME type validated server-side (not just `Content-Type` header)
- [ ] File extension whitelist enforced
- [ ] Files stored outside webroot, served via separate domain or pre-signed URLs
- [ ] Filename sanitized (path traversal: `../../../etc/passwd`, null bytes)
- [ ] Polyglot files considered (valid image + valid PHP)
- [ ] AV scanning for malware (if receiving untrusted files)
- [ ] Upload size limits enforced

---

## Insecure Deserialization
- [ ] Application accepts serialized objects from users?
- [ ] Java: ObjectInputStream, XStream, Jackson with default typing — gadget chains possible?
- [ ] PHP: `unserialize()` on user input?
- [ ] Python: `pickle.loads()`, `yaml.load()` (use `safe_load`)?
- [ ] .NET: `BinaryFormatter`, `NetDataContractSerializer`?
- [ ] Integrity check (HMAC signature) on serialized data before deserialization?

---

## Security Headers
- [ ] `Strict-Transport-Security` (HSTS): `max-age` ≥ 1 year, `includeSubDomains`, submitted to preload list?
- [ ] `Content-Security-Policy`: comprehensive, no `unsafe-inline`/`unsafe-eval`
- [ ] `X-Frame-Options: DENY` or CSP `frame-ancestors 'none'`
- [ ] `X-Content-Type-Options: nosniff`
- [ ] `Referrer-Policy: strict-origin-when-cross-origin` or stricter
- [ ] `Permissions-Policy`: restrict camera, mic, geolocation, etc.
- [ ] `Cache-Control: no-store` on sensitive pages
- [ ] `Server` and `X-Powered-By` headers removed or obfuscated

---

## API Security
- [ ] Rate limiting and throttling on all endpoints
- [ ] Authentication required on all non-public endpoints
- [ ] Input validation: schema enforcement (types, lengths, formats)
- [ ] Versioning: old/deprecated API versions decommissioned or locked down
- [ ] Error messages: no stack traces, no internal paths, no schema details in prod
- [ ] Pagination limits enforced (no unbounded data dump)
- [ ] Sensitive data (PII, tokens, secrets) not logged in access logs
- [ ] Webhook signatures validated (HMAC on payload)

---

## Cryptography
- [ ] TLS 1.2+ enforced, TLS 1.0/1.1 disabled
- [ ] Weak cipher suites disabled (RC4, 3DES, NULL, EXPORT)
- [ ] Certificate: valid, not self-signed in prod, not expired, correct hostname
- [ ] Sensitive data encrypted at rest (not just hashed)
- [ ] Encryption keys stored securely (KMS, Vault — not hardcoded)
- [ ] No ECB mode used for symmetric encryption
- [ ] Random number generation uses CSPRNG, not `Math.random()`

---

## Business Logic
- [ ] Price/quantity/discount fields manipulable? Negative values, zero, overflow?
- [ ] Race conditions possible on critical operations (payment, coupon redemption, account limits)?
- [ ] Workflow steps skippable? (step 1 → step 3 directly)
- [ ] Email/phone verification bypassable?
- [ ] Invitation/referral codes: unlimited reuse, enumerable, self-referral?
- [ ] TOCTOU (time-of-check to time-of-use) vulnerabilities?

---

## Information Disclosure
- [ ] Debug mode / developer tools disabled in production
- [ ] Stack traces not returned to user
- [ ] Error messages are generic (no DB type, query, internal path)
- [ ] Directory listing disabled
- [ ] `.git`, `.env`, `backup.zip`, `phpinfo.php`, `web.config` accessible?
- [ ] Robots.txt doesn't reveal sensitive paths
- [ ] Source maps not exposed in production JS
- [ ] API documentation (Swagger/OpenAPI) not publicly accessible in prod
- [ ] HTTP response headers don't leak software versions

---

## Dependency & Supply Chain
- [ ] SCA (Software Composition Analysis) run — known vulnerable dependencies?
- [ ] Dependencies locked (lockfile committed, hashes verified)
- [ ] No typosquatted packages installed
- [ ] CI/CD pipeline: are secrets scoped correctly, are GitHub Actions pinned to commit SHA?
- [ ] Container images: base image source verified, trivy/grype scan run

---

## Logging & Monitoring
- [ ] Authentication events logged (success, failure, lockout)
- [ ] Authorization failures logged
- [ ] Sensitive operations logged (admin actions, data export, privilege change)
- [ ] Logs protected from tampering (separate storage, append-only)
- [ ] No sensitive data in logs (passwords, tokens, PII)
- [ ] Alerting on anomalous patterns (brute force, mass data access)
