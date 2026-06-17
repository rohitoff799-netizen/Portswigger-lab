# Lab: File path traversal, simple case

## Description
This lab contains a file path traversal vulnerability in the display of product images.

## Analysis
The application fetches images using a `filename` parameter.

```text
GET /image?filename=6.jpg
```

If the application doesn't sanitize the filename or check the directory, we can use the `../` (dot-dot-slash) sequence to navigate up the directory tree and access files outside the intended directory.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to: `../../../etc/passwd`.
3. Send the request and observe the contents of the password file in the response.

## Payload
```text
filename=../../../etc/passwd
```

## Takeaways
- Path traversal allows an attacker to read arbitrary files on the server that the application has access to.
- Use a whitelist of allowed filenames or store files in a database with unique IDs instead of using user-supplied names directly.
