# Lab: Referer-based access control

## Description
This lab contains an access control vulnerability where the application relies on the `Referer` header to determine if a request is authorized.

## Analysis
The application allows administrators to change user roles. It checks if the `Referer` header matches the administrative page's URL.

```text
POST /admin-roles
Referer: https://vulnerable-site.com/admin
```

Since the `Referer` header is controlled by the client, it can be easily spoofed. An attacker can make a request from a non-admin account and include a fake `Referer` header to bypass the check.

## Exploitation
I need to elevate `wiener` to an administrator.

### Steps
1. Log in as `administrator:admin`.
2. Go to the admin panel and upgrade a user.
3. Observe the `Referer` header in the request.
4. Log in as `wiener:peter`.
5. Replay the upgrade request for `wiener`, but manually add the `Referer: https://vulnerable-site.com/admin` header.
6. The request should succeed.

## Payload
```text
POST /admin-roles
Referer: https://vulnerable-site.com/admin
username=wiener&action=upgrade
```

## Takeaways
- Never rely on the `Referer` header for security-critical decisions. It is easily manipulated by the client.
- Authorization should always be based on the authenticated session data stored on the server.
- The `Referer` header can be used for logging or analytics, but never for access control.
