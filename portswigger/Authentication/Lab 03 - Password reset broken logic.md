# Lab: Password reset broken logic

## Description
This lab contains a vulnerability in the password reset functionality that allows an attacker to reset any user's password.

## Analysis
The password reset process often involves:
1. User requests a reset link.
2. Server sends a link with a token to the user's email.
3. User clicks the link and submits a new password.

If the final request to change the password includes the username but fails to verify that the token belongs to that specific user, an attacker can use their own token to change another user's password.

## Exploitation
I need to change Carlos's password.

### Steps
1. Request a password reset for your own account (`wiener`).
2. Open the reset link from your email.
3. Submit a new password.
4. Intercept the `POST` request to `/forgot-password`.
5. Observe the parameters: `username=wiener&token=...&new-password-1=...&new-password-2=...`.
6. Change the `username` to `carlos` but keep your own `token`.
7. Send the request. If the application only validates that the token is *valid* but doesn't check *who* it's for, Carlos's password will be changed.

## Payload
```text
POST /forgot-password
username=carlos&token=[your-token]&new-password-1=password&new-password-2=password
```

## Takeaways
- Every step of a sensitive process must be fully validated.
- Tokens must be cryptographically tied to the specific user and action for which they were issued.
- All parameters in a security-critical request must be verified for consistency and authorization.
