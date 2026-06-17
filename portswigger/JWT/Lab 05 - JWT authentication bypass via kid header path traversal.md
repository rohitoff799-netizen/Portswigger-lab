# Lab: JWT authentication bypass via kid header path traversal

## Description
This lab contains a JWT vulnerability where the `kid` (Key ID) header is vulnerable to path traversal.

## Analysis
The `kid` header is used to identify which key should be used to verify the signature. Often, this is used as a filename on the server. If the application doesn't sanitize the `kid` parameter, an attacker can use path traversal (`../`) to point to a file with predictable content.

A classic trick is to point the `kid` to `/dev/null`. On many systems, reading `/dev/null` returns an empty string. If we sign our JWT with a null byte (or an empty string) as the key, and set the `kid` to `/dev/null`, the server will verify our signature using an empty string as the secret key.

## Exploitation
I need to use path traversal in the `kid` header to sign the JWT with a predictable value.

### Steps
1. Capture your JWT.
2. Modify the payload: `sub: administrator`.
3. In the header, set `kid: ../../../../../../../dev/null`.
4. Sign the JWT with a single null byte (or an empty string, depending on the library) as the secret key.
5. Send the modified JWT.

## Payload
```json
Header: { "alg": "HS256", "kid": "../../../../../../../dev/null" }
Secret: \x00
```

## Takeaways
- Sanitize the `kid` header to prevent path traversal.
- Use a whitelist of allowed `kid` values or a secure database lookup instead of direct file system access.
- Ensure that the verification logic fails if the specified key cannot be found or is empty.
