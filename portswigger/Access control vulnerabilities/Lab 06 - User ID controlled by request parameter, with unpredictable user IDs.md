# Lab: User ID controlled by request parameter, with unpredictable user IDs

## Description
This lab contains an IDOR vulnerability where the user IDs are UUIDs, making them hard to guess. However, the UUID for the target user is leaked on the site.

## Analysis
Even if identifiers are unpredictable (like UUIDs), an IDOR vulnerability still exists if authorization is not enforced. The attacker just needs to find the target's ID. In this lab, the IDs are revealed on the author profile pages of the blog posts.

## Exploitation
I need to find Carlos's UUID and use it to access his account.

### Steps
1. Navigate to a blog post written by `carlos`.
2. Click on the author's name to go to their profile page.
3. Observe the URL, which contains Carlos's UUID: `?id=8611985c-1974-4b9b-90e6-a8360f545464`.
4. Log in as `wiener:peter`.
5. Replace your own ID in the URL with Carlos's UUID.
6. The page now shows Carlos's account details.

## Payload
```text
URL: /my-account?id=8611985c-1974-4b9b-90e6-a8360f545464
```

## Takeaways
- Using unpredictable IDs is a good "defense-in-depth" measure, but it's not a substitute for proper authorization.
- Identifiers are often leaked in other parts of the application.
- Secure by default: always verify that the user has the required permission for the specific action on the specific object.
Base the authorization on the session, not the URL parameter.
