# Lab: Username enumeration via subtly different responses

## Description
This lab contains a username enumeration vulnerability where the differences in responses are very subtle, such as a missing period or a slightly different error message.

## Analysis
The application's error messages might look identical at first glance.
- Invalid username: `Invalid username or password.`
- Valid username, wrong password: `Invalid username or password` (missing the period).

By using Burp Intruder to compare the responses for a list of usernames, we can identify these subtle differences.

## Exploitation
I need to find a valid username and then brute-force its password.

### Steps
1. Intercept the login request and send it to Intruder.
2. Brute-force the `username` field.
3. In the "Grep - Match" tab of Intruder, add a rule to look for the exact error message.
4. Alternatively, use the "Grep - Extract" feature to extract the error message from every response.
5. Look for any response that has a different length or a different error message.
6. Once the username is found, brute-force the password for that user.

## Payload
```text
Username list: [ ... ]
Password list: [ ... ]
```

## Takeaways
- Even tiny differences in responses can be used for enumeration.
- Automated tools make it easy to find these differences that a human might miss.
- Ensure that the application uses a single, strictly identical error message for all failed login attempts.
