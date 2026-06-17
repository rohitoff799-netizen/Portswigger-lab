# Lab: JWT authentication bypass via algorithm confusion

## Description
This lab contains a JWT vulnerability where an attacker can trick the server into using a public key as a symmetric HMAC key.

## Analysis
This attack occurs when a server is configured to accept both `RS256` (asymmetric) and `HS256` (symmetric) tokens.
1. The server normally uses a public key (e.g., `public.pem`) to verify `RS256` tokens.
2. If an attacker changes the `alg` to `HS256`, the server might use the *content* of `public.pem` as the symmetric secret key for HMAC verification.

Since the public key is usually public or easily accessible (e.g., at `/.well-known/jwks.json`), the attacker can use it to sign their own `HS256` tokens.

## Exploitation
I need to find the server's public key and use it to sign an `HS256` token.

### Steps
1. Find the server's public key (often available at a path like `/jwks.json` or in a header).
2. Clean the key to ensure it's in the correct format (e.g., standard PEM).
3. Modify your JWT: `alg: HS256`, `sub: administrator`.
4. Sign the JWT using the `HS256` algorithm, providing the server's public key as the secret.
5. Send the token.

## Payload
```json
Header: { "alg": "HS256" }
Secret: [Content of the Public Key]
```

## Takeaways
- Never allow a single endpoint to accept both symmetric and asymmetric algorithms.
- Explicitly set the expected algorithm for each key.
- Avoid using libraries that automatically choose the verification method based on the `alg` header.
- Ensure that the verification key is used correctly according to its intended type.
