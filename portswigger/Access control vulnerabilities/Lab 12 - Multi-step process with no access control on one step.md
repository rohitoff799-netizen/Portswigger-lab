# Lab: Multi-step process with no access control on one step

## Description
This lab contains an access control vulnerability in a multi-step administrative process. The application fails to perform authorization checks on every step of the process.

## Analysis
The process of changing a user's role involves two steps:
1. Select the user and role.
2. Confirm the change.

The application might only check if the user is an admin in the first step. If the second step doesn't perform the same check, a non-admin user can skip the first step and directly submit the confirmation request.

## Exploitation
I need to elevate `wiener` to an administrator.

### Steps
1. Log in as `administrator:admin`.
2. Go to the admin panel and start the process of upgrading `wiener`.
3. Intercept the final confirmation request: `POST /admin-roles` with `action=upgrade&confirm=true&username=wiener`.
4. Log out and log in as `wiener:peter`.
5. Send the captured confirmation request.
6. Observe that Wiener's role has been updated.

## Payload
```text
POST /admin-roles
username=wiener&action=upgrade&confirm=true
```

## Takeaways
- Authorization checks must be performed for every request, even in multi-step processes.
- Never assume that because a user reached a certain step, they must have been authorized to do so.
- Each endpoint should be self-contained and responsible for its own security checks.
Base the authorization on the session, not the sequence of requests.
