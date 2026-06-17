# Lab: File path traversal, traversal sequences blocked with absolute path bypass

## Description
This lab blocks traversal sequences like `../` but fails to prevent the use of absolute paths.

## Analysis
The application might be checking for the presence of `../` in the `filename` parameter and rejecting the request if found. However, if the application directly passes the filename to a file-opening function, we can simply provide an absolute path like `/etc/passwd`.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to: `/etc/passwd`.
3. Send the request.

## Payload
```text
filename=/etc/passwd
```

## Takeaways
- Filters that only look for specific sequences are often easy to bypass.
- Security should be implemented by validating that the final resolved path is within the expected directory.
Base the validation on the canonical path of the file.
