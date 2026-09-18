# Sensitive Data Exposure in Authentication Token

## Finding Information

- **Finding:** Password Hash Exposed in JWT Authentication Token
- **Severity:** High
- **OWASP Category:** A02: Cryptographic Failures
- **Status:** Confirmed
- **Affected Endpoint:** `POST /rest/user/login`
- **Affected Component:** Authentication / JWT token generation
- **Target:** Local OWASP Juice Shop instance
- **Test Account:** Local dummy test account created specifically for this assessment

## Description

During authentication testing, a successful login request to the `/rest/user/login`
endpoint returned a JSON response containing a JWT authentication token.

After locally decoding the JWT payload, the token was found to contain the
authenticated user's account information, including a `password` field containing
a password hash.

The password was not exposed in plaintext. However, including a password hash in
a client-held authentication token unnecessarily exposes sensitive credential
information to the client.

## Steps to Reproduce

1. Start the locally hosted OWASP Juice Shop application.
2. Create or use a dummy test account for the authorized assessment.
3. Send a login request to:

   `POST /rest/user/login`

4. Provide valid test-account credentials.
5. Observe the successful `HTTP/1.1 200 OK` response.
6. Extract the JWT from the `authentication.token` field.
7. Decode the JWT payload locally.
8. Inspect the `data` object.
9. Observe that the payload contains a `password` field.

## Evidence

### Evidence 1 — Successful Authentication Response

The login request returned:

`HTTP/1.1 200 OK`

and an authentication object containing a JWT token.

Evidence file:

`evidence/screenshots/authentication-login-response.png`

### Evidence 2 — Decoded JWT Payload

The decoded JWT payload contains account information including:

- User ID
- Email address
- Role
- Account status
- Account timestamps
- `password` field

The actual password hash has been redacted from the evidence presentation.

Evidence file:

`evidence/screenshots/authentication-jwt-payload.png`

## Impact

A password hash included in a client-accessible authentication token increases the
amount of sensitive credential information exposed to the client.

If an attacker obtains a valid token through another vulnerability, insecure client
storage, logging, or session compromise, the attacker may also obtain the password
hash contained in that token.

Depending on the password hashing algorithm and password strength, an exposed hash
may potentially be subjected to offline password-cracking attempts.

## Remediation

1. Do not include password hashes in JWTs or other client-accessible authentication
   responses.
2. Generate authentication tokens using only the minimum information required by
   the application.
3. Keep password hashes exclusively on the server side.
4. Review JWT payload construction and remove unnecessary sensitive account fields.
5. Avoid placing other sensitive credential material in client-readable tokens.
6. Review application logs and client-side storage to ensure authentication tokens
   are not unnecessarily exposed.

## Verification

The finding was manually reproduced using a locally hosted OWASP Juice Shop
instance and a dummy test account.

The issue was confirmed by observing the password field in the decoded JWT payload
returned following successful authentication.

## Security Testing Notes

Testing was performed only against the intentionally vulnerable OWASP Juice Shop
application running locally in the authorized Kali Linux lab environment.

No public or third-party systems were tested.
