# Lab: User ID controlled by request parameter with data leakage in redirect

## Description
This lab contains an IDOR vulnerability where the sensitive data is leaked in the body of a redirect response.

## Analysis
The application has a redirect when an unauthorized user tries to access a profile.

```text
HTTP/1.1 302 Found
Location: /login
...
[Sensitive User Data here in the body]
```

Many browsers will automatically follow the redirect and not show the body of the 302 response. However, if the server includes the user's data in the body before the redirect happens, it can be captured using a proxy like Burp Suite.

## Exploitation
I need to capture the API key for `carlos`.

### Steps
1. Navigate to `/my-account?id=carlos`.
2. The browser redirects you to `/login`.
3. Check the Burp Suite history for the request to `/my-account?id=carlos`.
4. Inspect the response body. Even though the status is `302`, the body contains Carlos's account page with his API key.

## Payload
```text
URL: /my-account?id=carlos
```

## Takeaways
- Never include sensitive data in the body of a redirect response.
- Access control should be checked before any data is retrieved or rendered.
- Don't assume that a redirect prevents the user from seeing the original response content.
