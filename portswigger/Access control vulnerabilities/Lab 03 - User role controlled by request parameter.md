# Lab: User role controlled by request parameter

## Description
This lab allows users to gain administrative access by modifying a request parameter.

## Analysis
The application determines the user's role based on a parameter in the request, such as a cookie or a hidden form field. If the application trusts this client-supplied value without server-side verification, an attacker can elevate their privileges.

In this lab, when you log in, the application sets an `Admin` cookie.

## Exploitation
I need to become an administrator and delete `carlos`.

### Steps
1. Log in with the provided credentials (`wiener:peter`).
2. Observe the cookies in the response. There is an `Admin=false` cookie.
3. Use a proxy or browser extension to change the cookie to `Admin=true`.
4. Refresh the page. You should now see an "Admin panel" link.
5. Go to the admin panel and delete carlos.

## Payload
```text
Cookie: Admin=true
```

## Takeaways
- Never trust the client to specify their own role or privileges.
- All authorization decisions must be made on the server based on the user's authenticated session.
- Client-side parameters like cookies or hidden fields should only be used as hints, not as the source of truth for security decisions.
