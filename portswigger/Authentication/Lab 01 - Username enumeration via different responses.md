# Lab: Username enumeration via different responses

## Description
This lab contains a username enumeration vulnerability. The application returns different responses when an invalid username is entered compared to when a valid username is entered with an incorrect password.

## Analysis
When we try to log in:
- If the username doesn't exist, the response might be: `Invalid username`.
- If the username exists but the password is wrong, the response might be: `Invalid password`.

This difference allows an attacker to identify valid usernames by brute-forcing the username field and looking for the "Invalid password" message.

## Exploitation
I need to find a valid username and then brute-force its password.

### Steps
1. Intercept the login request.
2. Send it to Burp Intruder.
3. Brute-force the `username` field using a list of common usernames.
4. Look for a response that is different from the others (e.g., different message or different response length).
5. Once the username is found (e.g., `admin`), brute-force the `password` field for that specific user.
6. Log in with the discovered credentials.

## Payload
```text
Username list: [ ... ]
Password list: [ ... ]
```

## Takeaways
- Use generic error messages like "Invalid username or password" to prevent username enumeration.
- Account for all possible response differences, including status codes, message content, and response headers.
- Implement rate limiting or account lockout to slow down brute-force attacks.
Base the error message on a consistent response template.
