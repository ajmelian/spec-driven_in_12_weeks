# Security Requirements: [Module Name]

## Metadata

| Field | Value |
|---|---|
| Module ID | MOD-XXX |
| Version | 0.1 |
| Status | Draft / Reviewed / Approved |

## 1. Security Objective

Describe the security purpose of this module and the main risks to avoid.

## 2. Assets Protected

| Asset | Sensitivity | Protection required |
|---|---|---|
| | Public / Internal / Sensitive / Secret | |

## 3. Trust Boundaries

Describe where untrusted input enters the system and where privilege boundaries exist.

## 4. Authentication Requirements

- [ ] Authentication required
- [ ] Anonymous access allowed only for specific routes
- [ ] Step-up authentication required for sensitive operations

## 5. Authorization Requirements

| Action | Required role/permission | Enforcement point |
|---|---|---|
| | | Controller / Filter / Service |

## 6. Input Validation Requirements

| Input | Validation rule | Rejection behavior |
|---|---|---|
| | | |

## 7. Output Encoding Requirements

Describe where output escaping is required.

## 8. Audit Events

| Event | Severity | Data to log | Data not to log |
|---|---|---|---|
| | Info / Warning / Critical | | |

## 9. OWASP Top 10 Web Mapping

| Risk | Applies? | Required control |
|---|---|---|
| A01 Broken Access Control | Yes / No | |
| A02 Cryptographic Failures | Yes / No | |
| A03 Injection | Yes / No | |
| A04 Insecure Design | Yes / No | |
| A05 Security Misconfiguration | Yes / No | |
| A06 Vulnerable and Outdated Components | Yes / No | |
| A07 Identification and Authentication Failures | Yes / No | |
| A08 Software and Data Integrity Failures | Yes / No | |
| A09 Security Logging and Monitoring Failures | Yes / No | |
| A10 Server-Side Request Forgery | Yes / No | |

## 10. Abuse Cases

| ID | Abuse case | Expected control | Test required |
|---|---|---|---|
| ABUSE-001 | | | Yes / No |

## 11. Residual Risks

| Risk | Reason accepted | Mitigation / Monitoring |
|---|---|---|
| | | |
