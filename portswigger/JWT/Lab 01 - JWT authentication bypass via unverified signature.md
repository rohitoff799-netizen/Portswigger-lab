# Lab: JWT authentication bypass via unverified signature

## Description
This lab contains a JWT-based authentication mechanism where the server fails to verify the signature of the token.

## Analysis
A JWT consists of three parts: Header, Payload, and Signature. The signature is meant to ensure that the payload hasn't been tampered with. If the server only decodes the payload and trusts the data without verifying the signature against a secret key, an attacker can modify the payload (e.g., change the `sub` or `username` to `admin`) and the server will accept it.

## Exploitation
I need to modify my JWT to become `administrator`.

### Steps
1. Log in and capture your JWT.
2. Decode the payload part of the JWT (the second part).
3. Change the `sub` or `username` claim to `administrator`.
4. Re-encode the payload.
5. Send the modified JWT back to the server. Since the signature is not verified, the server will accept the modified payload.

## Payload
```json
{
  "sub": "administrator",
  "iat": 1516239022,
  "exp": 1516242622
}
```

## Takeaways
- Always verify the JWT signature before trusting any claims in the payload.
- Use a robust library for JWT handling and ensure it's configured to enforce signature verification.
- A JWT with an unverified signature is just a Base64-encoded JSON object that can be easily manipulated.
