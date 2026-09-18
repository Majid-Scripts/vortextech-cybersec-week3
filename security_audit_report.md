# VortexTech Cyber Security Internship — Week 3

# Security Audit Report

## 1. Executive Summary

This report documents a controlled security assessment of the OWASP Juice Shop
application running locally in a Docker container on Kali Linux.

The assessment was performed as part of the VortexTech Cyber Security
Internship Week 3 task.

The objective was to identify, manually verify, and document selected web
application security issues in an intentionally vulnerable practice environment.

The assessment confirmed two security findings:

1. Sensitive credential information was included in a JWT authentication token.
2. Cross-Site Scripting (XSS) was successfully demonstrated through the product
   search functionality.

Authentication testing also included checks for unauthenticated access to
selected endpoints. No authentication bypass was confirmed.

## 2. Assessment Information

- **Author:** Abdul Majid
- **Internship:** VortexTech Cyber Security Internship
- **Week:** 3 of 4
- **Target:** OWASP Juice Shop
- **Environment:** Kali Linux / Docker
- **Application URL:** `http://localhost:3000`
- **Assessment Type:** Controlled Web Application Security Assessment
- **Testing Scope:** Local authorized practice environment

## 3. Objective

The objective of this assessment was to practice basic-to-intermediate web
application security testing techniques by:

- Reviewing application behavior.
- Testing authentication-related functionality.
- Identifying sensitive-data exposure.
- Testing for Cross-Site Scripting.
- Collecting supporting evidence.
- Documenting confirmed findings.
- Providing remediation recommendations.

## 4. Scope

### In Scope

- OWASP Juice Shop web application.
- Locally accessible application functionality.
- Authentication-related functionality.
- Sensitive-data handling.
- Product search functionality.
- Relevant HTTP requests and responses.
- Browser-based testing.

### Out of Scope

- Public websites.
- Production systems.
- Third-party systems.
- Systems without authorization.
- Denial-of-service testing.
- Destructive testing.

## 5. Tools Used

- Kali Linux
- Docker
- OWASP Juice Shop
- Browser Developer Tools
- cURL

Only the tools and techniques actually used during the assessment should be
considered evidence for a finding.

## 6. Methodology

The assessment followed a controlled testing process:

1. Verified the local Juice Shop environment.
2. Performed basic application reconnaissance.
3. Reviewed accessible application endpoints.
4. Tested selected authentication functionality.
5. Tested handling of authentication responses.
6. Tested product search functionality for XSS.
7. Manually verified observed behavior.
8. Captured screenshots as supporting evidence.
9. Documented confirmed findings.
10. Prepared this final security audit report.

Testing was limited to the local intentionally vulnerable application.

## 7. Reconnaissance Results

Initial reconnaissance confirmed that the Juice Shop application was accessible
at:

`http://localhost:3000`

The application returned a successful HTTP response.

Application endpoints were reviewed to identify relevant functionality for
authorized security testing.

The `/api/Products` endpoint returned product information successfully.

The `/api/Users` endpoint required authentication and returned:

`HTTP/1.1 401 Unauthorized`

The `/rest/2fa/status` endpoint also required authentication and returned:

`HTTP/1.1 401 Unauthorized`

These observations were used to guide subsequent authentication and
application-security testing.

## 8. Authentication Assessment

Authentication testing was performed using a locally created dummy test account.

The following areas were tested:

### Unauthenticated User Identity

Request:

`GET /rest/user/whoami`

Result:

`HTTP/1.1 200 OK`

Response:

`{"user":{}}`

No authenticated user information was returned without an authentication token.

### Users API

Request:

`GET /api/Users`

Result:

`HTTP/1.1 401 Unauthorized`

No unauthenticated access to the users API was confirmed.

### 2FA Status

Request:

`GET /rest/2fa/status`

Result:

`HTTP/1.1 401 Unauthorized`

No unauthenticated disclosure of 2FA status was confirmed.

### Authentication Result

A valid login using the local dummy account successfully returned a JWT
authentication token.

The decoded JWT payload contained account information including a password field
containing a password hash.

This issue is documented as a separate sensitive-data finding.

**Detailed finding:** `findings/authentication.md`

## 9. Finding 1 — Sensitive Data Exposure in JWT

### Finding

Password Hash Exposed in JWT Authentication Token

### Severity

High

### OWASP Category

A02: Cryptographic Failures

### Status

Confirmed

### Affected Endpoint

`POST /rest/user/login`

### Description

During authentication testing, the login endpoint returned a JWT authentication
token after successful authentication.

The JWT payload was decoded locally and contained account information including a
password field containing a password hash.

The password was not observed in plaintext. However, exposing a password hash in a
client-accessible authentication token unnecessarily exposes sensitive
credential information.

### Impact

If an attacker obtains a valid authentication token through another compromise,
insecure storage, logging, or session exposure, the attacker may also obtain the
password hash contained within the token.

Depending on the hashing algorithm and password strength, an exposed hash may be
subject to offline password-cracking attempts.

### Evidence

- `evidence/screenshots/authentication-login-response.png`
- `evidence/screenshots/authentication-jwt-payload.png`

### Remediation

- Do not include password hashes in JWT tokens.
- Keep password hashes exclusively on the server side.
- Include only the minimum information required in authentication tokens.
- Review JWT generation and serialization logic.
- Avoid exposing sensitive credential material to clients.

**Detailed finding:** `findings/sensitive_data.md`

## 10. Finding 2 — Cross-Site Scripting

### Finding

Cross-Site Scripting (XSS) in Search Functionality

### Severity

High

### OWASP Category

A03: Injection

### Status

Confirmed

### Affected Component

Product search functionality

### Description

The product search functionality was tested using a controlled XSS
proof-of-concept payload.

The payload successfully caused JavaScript execution in the browser and
displayed an alert containing the test value:

`XSS-TEST`

This confirmed that attacker-controlled input could reach a browser execution
context without being adequately neutralized in the tested functionality.

### Impact

Successful XSS execution may allow attacker-controlled JavaScript to execute
within the context of the vulnerable application.

Depending on the affected context and available browser protections, this may
allow manipulation of displayed content or interaction with application
functionality in a victim's browser session.

The exact impact depends on the application's security controls and execution
context.

### Evidence

- `evidence/screenshots/xss-search-alert.png`

### Remediation

- Treat all user-controlled input as untrusted.
- Apply context-appropriate output encoding.
- Avoid unsafe DOM APIs that interpret strings as HTML or executable content.
- Use safe DOM manipulation methods.
- Implement an appropriate Content Security Policy as defense in depth.
- Review search-related rendering and input handling.

**Detailed finding:** `findings/xss.md`

## 11. Findings Summary

| ID | Finding | Severity | Status |
|---|---|---|---|
| F-01 | Password Hash Exposed in JWT Authentication Token | High | Confirmed |
| F-02 | Cross-Site Scripting in Search Functionality | High | Confirmed |

No authentication bypass was confirmed during the documented tests.

## 12. Evidence Summary

The assessment evidence is stored in:

`evidence/screenshots/`

Available evidence includes:

- `authentication-login-response.png`
- `authentication-jwt-payload.png`
- `xss-search-alert.png`

Sensitive token material was not intentionally included in the project
documentation.

## 13. Limitations

This assessment was performed against a deliberately vulnerable local practice
application.

The assessment was limited to selected authentication, sensitive-data, and XSS
tests.

It was not intended to represent a complete penetration test of every feature
or vulnerability class in the application.

Only findings that were manually reproduced and supported by collected evidence
were documented as confirmed findings.

## 14. Conclusion

The Week 3 assessment successfully demonstrated a controlled workflow for
web application security testing.

Two security findings were manually confirmed:

- Sensitive credential information exposed through a JWT authentication token.
- Cross-Site Scripting through the tested product search functionality.

Authentication controls were also tested, and no authentication bypass was
confirmed during the documented checks.

The findings have been documented separately with reproduction steps, impact,
evidence, and remediation recommendations.

## 15. Security Testing Declaration

All testing documented in this report was performed against the intentionally
vulnerable OWASP Juice Shop application running locally in the authorized
Kali Linux laboratory environment.

No public, production, or third-party systems were tested.
