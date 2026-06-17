# Lab: Basic password reset poisoning

## Description
This lab contains a password reset poisoning vulnerability. The application uses the `Host` header to generate the password reset link sent to the user's email.

## Analysis
When a user requests a password reset, the application sends an email containing a link like `https://[Host]/forgot-password?token=...`. If the application trusts the `Host` header from the request, an attacker can provide a malicious host.

The victim will receive an email with a link pointing to the attacker's server. When they click the link, their reset token is sent to the attacker.

## Exploitation
I need to capture Carlos's password reset token.

### Steps
1. Navigate to the "Forgot password" page.
2. Enter the username `carlos`.
3. Intercept the request.
4. Change the `Host` header to your exploit server:
   `Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
5. Send the request.
6. Check the access logs on your exploit server. You should see a request from Carlos's browser (or the simulated victim) containing the token:
   `GET /forgot-password?token=...`
7. Use the captured token to reset Carlos's password.

## Payload
```text
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

## Takeaways
- Never trust the `Host` header for generating absolute URLs.
- Use a fixed, trusted domain name for links in emails.
- If you must use a dynamic host, validate it against a whitelist of allowed domains.
