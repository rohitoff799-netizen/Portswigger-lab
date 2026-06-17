# Lab: JWT authentication bypass via jwk header injection

## Description
This lab contains a JWT vulnerability where the server trusts a public key provided in the `jwk` header of the token itself.

## Analysis
The `jwk` (JSON Web Key) header parameter allows a sender to include their public key in the JWT header. If the server uses this embedded key to verify the signature without checking if the key is trusted, an attacker can generate their own RSA key pair, sign the token with their private key, and include the public key in the `jwk` header.

## Exploitation
I need to sign a modified JWT with my own key and include it in the header.

### Steps
1. In Burp Suite, use the "JWT Editor" extension to generate a new RSA key.
2. Intercept a request with a JWT.
3. Modify the payload: `sub: administrator`.
4. Sign the JWT using your new RSA key.
5. In the signing dialog, select "Embed JWK in header".
6. Send the request.

## Payload
```json
Header: {
  "alg": "RS256",
  "jwk": { ... your public key ... }
}
```

## Takeaways
- Never use the `jwk` or `jku` headers to retrieve a verification key unless you have a way to verify the authenticity of the key itself.
- Servers should use a local, trusted set of public keys (or fetch them from a trusted, hardcoded URL) to verify JWT signatures.
- The token should not be able to dictate which key is used for its own verification.
