# Lab: User ID controlled by request parameter

## Description
This lab contains an Insecure Direct Object Reference (IDOR) vulnerability.

## Analysis
The application identifies users by a `id` parameter in the URL.

```text
GET /my-account?id=wiener
```

If the application doesn't verify that the logged-in user matches the requested `id`, anyone can access any user's profile by simply changing the `id` in the URL.

## Exploitation
I need to find the API key for `carlos`.

### Steps
1. Log in as `wiener:peter`.
2. Observe the URL: `?id=wiener`.
3. Change the `id` to `carlos`: `?id=carlos`.
4. The page now shows Carlos's account details, including his API key.

## Payload
```text
URL: /my-account?id=carlos
```

## Takeaways
- IDOR occurs when an application provides direct access to objects based on user-supplied input without performing an authorization check.
- Every request to a sensitive resource must be authorized by verifying that the user has the right to access that specific object.
- Use indirect object references (like a session-based mapping) to prevent direct manipulation.
