# Lab: JWT authentication bypass via flawed signature verification

## Description
This lab contains a JWT vulnerability where the server accepts JWTs with the `none` algorithm.

## Analysis
The JWT header contains an `alg` parameter that specifies the algorithm used for the signature (e.g., `HS256`, `RS256`). A common vulnerability is that servers accept the `none` algorithm, which means no signature is required.

If we change the `alg` to `none` and remove the signature part of the token, some libraries will consider the token valid regardless of its contents.

## Exploitation
I need to modify my JWT to become `administrator` using the `none` algorithm.

### Steps
1. Capture your JWT.
2. Decode the header and change `alg` to `none`.
3. Decode the payload and change `sub` to `administrator`.
4. Re-encode both parts.
5. Reconstruct the JWT as `header.payload.` (note the trailing dot, but no signature).
6. Send the token to the server.

## Payload
```json
Header: { "alg": "none", "typ": "JWT" }
Payload: { "sub": "administrator" }
```

## Takeaways
- Never allow the `none` algorithm in your JWT verification logic.
- Explicitly define the expected algorithm(s) and reject any tokens that use a different one.
- Keep your JWT libraries up to date, as many older versions were vulnerable to this attack by default.
