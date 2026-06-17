# Lab: File path traversal, traversal sequences stripped non-recursively

## Description
This lab strips traversal sequences like `../` from the user input, but it does so non-recursively.

## Analysis
The application likely uses a simple string replacement to remove `../`.
Example: `input.replace("../", "")`

If we use a nested sequence like `....//`, the application will remove the inner `../`, leaving behind a valid `../` sequence.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to: `....//....//....//etc/passwd`.
3. Send the request.

## Payload
```text
filename=....//....//....//etc/passwd
```

## Takeaways
- String stripping/sanitization should always be done recursively until no more matches are found, or better yet, avoided in favor of strict validation.
- Understanding how sanitization logic works allows for crafting effective bypasses.
