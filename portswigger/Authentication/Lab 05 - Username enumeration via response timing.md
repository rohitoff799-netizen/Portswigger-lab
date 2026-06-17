# Lab: Username enumeration via response timing

## Description
This lab contains a username enumeration vulnerability that can be exploited by measuring the time it takes for the server to respond to login requests.

## Analysis
Some applications perform more work when a username is valid (e.g., fetching user data, hashing the provided password) than when it is invalid. This difference in processing time, even if it's only a few hundred milliseconds, can be measured and used to identify valid usernames.

## Exploitation
I need to identify a valid username based on response timing.

### Steps
1. Intercept the login request and send it to Intruder.
2. Brute-force the `username` field.
3. In the Intruder results table, look at the "Response received" column (you might need to enable it).
4. Look for a username that consistently results in a significantly longer response time.
5. To make the difference more pronounced, you can try sending a very long password, as hashing a long string takes more time.
6. Once the username is found, brute-force the password.

## Payload
```text
Username list: [ ... ]
```

## Takeaways
- Timing attacks are a powerful way to exfiltrate information when direct response differences are not available.
- To prevent timing attacks, ensure that all authentication attempts take a consistent amount of time, regardless of whether the username is valid or not. This can be achieved by always performing a "dummy" password hash even if the username is not found.
- Account for network jitter by taking multiple measurements.
