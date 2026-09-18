# VortexTech Cyber Security Internship — Week 3

## Basic Security Audit of a Sample Web Application

**Author:** Abdul Majid  
**Internship:** VortexTech Cyber Security Internship  
**Week:** 3 of 4  
**Level:** Intermediate  
**Status:** In Progress

## Objective

Perform a beginner-to-intermediate security assessment of a deliberately vulnerable
practice web application using legal, controlled, and authorized testing methods.

The assessment focuses on identifying, verifying, documenting, and recommending
remediation for selected web application security issues.

## Practice Target

**Application:** OWASP Juice Shop  
**Environment:** Locally hosted Docker container  
**URL:** `http://localhost:3000`

Testing is restricted to the local practice application.

## Scope

The assessment is limited to the locally hosted OWASP Juice Shop instance running
on the Kali Linux virtual machine.

### In Scope

* OWASP Juice Shop web application
* Locally accessible application functionality
* Authentication-related functionality
* Sensitive-data handling
* Cross-Site Scripting (XSS) testing
* Application behavior and relevant HTTP requests/responses

### Out of Scope

* Public websites
* Production systems
* Third-party services
* Systems that are not owned or explicitly authorized for testing
* Denial-of-service or destructive testing

## Tools

* OWASP Juice Shop
* Browser Developer Tools
* Kali Linux
* Docker

## Methodology

The assessment follows a controlled workflow:

1. Verify the local testing environment.
2. Perform basic application reconnaissance.
3. Identify potential security weaknesses.
4. Manually verify relevant findings.
5. Capture screenshots and supporting evidence.
6. Document confirmed findings with reproduction steps, impact, and remediation.
7. Compile the results into a final security audit report.

Only confirmed and reproducible findings are included as security findings.

## Confirmed Findings

### 1. Sensitive Data Exposure in Authentication Token

A password hash was observed inside the JWT returned after successful authentication.

Detailed documentation:

`findings/sensitive_data.md`

### 2. Cross-Site Scripting in Search Functionality

A controlled XSS proof-of-concept successfully executed JavaScript through the
product search functionality.

Detailed documentation:

`findings/xss.md`

## Authentication Assessment

Authentication testing did not confirm an authentication bypass.

Unauthenticated requests to protected functionality were tested and appropriate
authentication requirements were observed.

Detailed documentation:

`findings/authentication.md`

## Evidence

Screenshots and supporting evidence are stored in:

`evidence/screenshots/`

Current evidence includes:

* `authentication-login-response.png`
* `authentication-jwt-payload.png`
* `xss-search-alert.png`

## Project Structure

.
├── README.md
├── security_audit_report.md
├── findings/
│   ├── authentication.md
│   ├── sensitive_data.md
│   └── xss.md
├── evidence/
│   └── screenshots/
└── zap/

## Assessment Status

* [x] Local Juice Shop environment configured
* [x] Docker container verified
* [x] Project structure created
* [x] Assessment scope defined
* [x] Reconnaissance
* [x] Authentication testing
* [x] Sensitive-data testing
* [x] XSS testing
* [x] Evidence collection
* [x] Findings documentation
* [ ] Final security audit report
* [ ] Final project review and submission

## Security Testing Notes

Testing was performed only against the intentionally vulnerable OWASP Juice Shop
application running locally in the authorized Kali Linux lab environment.

No public or third-party systems were tested.
