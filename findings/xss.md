# Cross-Site Scripting (XSS) in Search Functionality

## Finding Information

- **Finding:** Cross-Site Scripting (XSS) in Search Functionality
- **Severity:** High
- **OWASP Category:** A03: Injection
- **Status:** Confirmed
- **Affected Component:** Product search functionality
- **Target:** Local OWASP Juice Shop instance
- **Testing Environment:** Kali Linux / locally hosted Docker container

## Description

During security testing, the Juice Shop search functionality was tested with a
controlled XSS proof-of-concept payload.

The payload caused JavaScript to execute in the browser and display an alert
containing the text `XSS-TEST`.

This demonstrates that attacker-controlled input supplied through the search
functionality can reach a browser execution context without being adequately
neutralized.

## Steps to Reproduce

1. Start the locally hosted OWASP Juice Shop application.
2. Open the application at:

   `http://localhost:3000`

3. Locate the product search functionality.
4. Enter the following controlled proof-of-concept payload:

   `<iframe src="javascript:alert('XSS-TEST')">`

5. Submit the search input.
6. Observe that a browser alert appears containing:

   `XSS-TEST`

## Evidence

### Evidence 1 — XSS JavaScript Execution

The browser displayed an alert containing `XSS-TEST` after the controlled payload
was submitted through the search functionality.

Evidence file:

`evidence/screenshots/xss-search-alert.png`

## Impact

Successful XSS execution indicates that attacker-controlled content can execute
JavaScript in the context of the vulnerable web application.

Depending on the affected context and browser/session protections, XSS may allow
an attacker to perform actions in a victim's authenticated browser session,
modify displayed content, or interact with application functionality as the
victim.

The exact impact depends on the application's security controls and the context
in which the injected content is executed.

## Remediation

1. Treat all user-controlled input as untrusted.
2. Apply context-appropriate output encoding before inserting data into HTML.
3. Avoid unsafe DOM APIs that interpret strings as HTML or executable content.
4. Use safe DOM manipulation methods where possible.
5. Implement an appropriate Content Security Policy (CSP) as an additional
   defense-in-depth control.
6. Review search-related client-side rendering and sanitization logic.

## Verification

The issue was manually reproduced using the locally hosted OWASP Juice Shop
application.

A controlled proof-of-concept payload successfully triggered a browser alert,
confirming JavaScript execution through the tested search functionality.

## Security Testing Notes

Testing was performed only against the intentionally vulnerable OWASP Juice Shop
application running locally in the authorized Kali Linux lab environment.

No public or third-party systems were tested.
