# Lab: File path traversal, validation of start of path

## Description
This lab validates that the filename starts with a specific expected directory path.

## Analysis
The application expects the `filename` to start with something like `/var/www/images/`.
Example: `filename=/var/www/images/6.jpg`

We can satisfy this requirement while still performing path traversal by including the expected path and then using `../` to move away from it.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to: `/var/www/images/../../../etc/passwd`.
3. Send the request.

## Payload
```text
filename=/var/www/images/../../../etc/passwd
```

## Takeaways
- Even if an application checks for a required prefix, it might still be vulnerable to path traversal if it doesn't resolve the path and validate the final location.
- Security checks should be applied to the *canonical* path.
