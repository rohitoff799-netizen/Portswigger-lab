# Lab: JWT authentication bypass via weak signing key

## Description
This lab uses a weak, easily guessable signing key for its HS256 JWTs.

## Analysis
The `HS256` (HMAC with SHA-256) algorithm uses a single symmetric secret key to both sign and verify the token. If this key is weak (e.g., `secret`, `123456`, or a common dictionary word), an attacker can brute-force it offline using a tool like `hashcat` or `jwt_tool`.

Once the secret key is discovered, the attacker can create their own valid JWTs with any claims they want.

## Exploitation
I need to crack the JWT secret and create an admin token.

### Steps
1. Capture your JWT.
2. Use `hashcat` with a wordlist to crack the signature:
   `hashcat -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt`
3. Once the secret is found (e.g., `secret123`), use it to sign a modified JWT where `sub` is `administrator`.
4. Send the new token to the server.

## Payload
```text
Secret: [discovered-secret]
Modified Payload: { "sub": "administrator" }
```

## Takeaways
- Always use strong, long, and random secret keys for symmetric signing algorithms.
- Rotate your signing keys regularly.
- Consider using asymmetric algorithms (like RS256) where the private key used for signing is never shared with the components that only need to verify the token.
