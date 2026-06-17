# Lab: 2FA simple bypass

## Description
This lab contains a simple bypass for Two-Factor Authentication (2FA).

## Analysis
The application requires a 2FA code after a successful password login. However, the application might consider the user "logged in" as soon as the password is correct, and the 2FA page is just a secondary UI step that isn't strictly enforced for subsequent requests.

If the application sets the session cookie *before* the 2FA code is entered, we might be able to access the `/my-account` page directly.

## Exploitation
I need to access Carlos's account.

### Steps
1. Log in as `wiener:peter`.
2. Notice the URL changes to `/login2`.
3. Instead of entering the code, try to navigate directly to `/my-account`. It works for your own account.
4. Now, log in as `carlos:montoya`.
5. When prompted for the 2FA code, manually change the URL to `/my-account`.
6. If the application is vulnerable, you will be taken to Carlos's account page.

## Payload
```text
URL: /my-account (after password login)
```

## Takeaways
- 2FA must be an integral part of the authentication process.
- Session cookies should not be granted (or should be restricted to a "pending 2FA" state) until the 2FA step is successfully completed.
- Every protected endpoint must verify that the user has completed all required authentication steps.
