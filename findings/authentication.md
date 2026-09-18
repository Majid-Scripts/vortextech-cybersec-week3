# Authentication Security Assessment

## Assessment Information

- **Assessment Area:** Authentication
- **Status:** Tested
- **Target:** Local OWASP Juice Shop instance
- **Primary Endpoint:** `POST /rest/user/login`
- **Testing Environment:** Kali Linux / locally hosted Docker container
- **Test Account:** Local dummy account created specifically for this assessment

## Objective

The authentication functionality was tested to determine whether unauthenticated
access, authentication bypass, or improper handling of authentication responses
could be reproduced.

## Tests Performed

### 1. Unauthenticated User Identity Request

**Request:**

`GET /rest/user/whoami`

**Result:**

HTTP/1.1 200 OK

{"user":{}}

The endpoint did not return an authenticated user's account information when no
authentication token was supplied.

### 2. Unauthenticated Users API Access

**Request:**

`GET /api/Users`

**Result:**

HTTP/1.1 401 Unauthorized

The application reported that an Authorization header was required.
No unauthenticated access to the users API was confirmed.

### 3. Unauthenticated 2FA Status Request

**Request:**

`GET /rest/2fa/status`

**Result:**

HTTP/1.1 401 Unauthorized

No unauthenticated disclosure of 2FA status was confirmed.

### 4. Normal Authentication

A dummy local test account was successfully authenticated through:

`POST /rest/user/login`

The server returned:

HTTP/1.1 200 OK

and issued a JWT authentication token.

The JWT was decoded locally for security assessment purposes.

The decoded payload contained account information, including a `password` field
containing a password hash. This issue is documented separately in:

`findings/sensitive_data.md`

## Assessment Result

No authentication bypass was confirmed during the tests documented above.

However, the successful authentication response returned a JWT containing
sensitive account information, including a password hash.

This sensitive-data exposure is documented separately in:

`findings/sensitive_data.md`

## Evidence

Authentication evidence was collected during testing and stored under:

`evidence/screenshots/`

Relevant evidence includes:

- `authentication-login-response.png`
- `authentication-jwt-payload.png`

The complete authentication token was not included in project evidence to avoid
unnecessary exposure of credential material.

## Security Testing Notes

Testing was performed only against the intentionally vulnerable OWASP Juice Shop
application running locally in the authorized Kali Linux lab environment.

No public or third-party systems were tested.
