# Lab: File path traversal, validation of file extension with null byte bypass

## Description
This lab validates that the filename ends with a specific file extension (e.g., `.jpg`). We can bypass this using a null byte (`%00`).

## Analysis
Some file-handling functions (especially in older languages or libraries) treat the null byte as the end of a string. If the application validates the extension but then passes the string to a function that stops at the null byte, we can trick it.

Payload: `../../../etc/passwd%00.jpg`
- The application's extension check sees `.jpg` and passes the validation.
- The file-opening function sees `../../../etc/passwd` and stops at the null byte, ignoring the `.jpg`.

## Exploitation
I need to retrieve the contents of `/etc/passwd`.

### Steps
1. Intercept a request for an image.
2. Change the `filename` parameter to: `../../../etc/passwd%00.jpg`.
3. Send the request.

## Payload
```text
filename=../../../etc/passwd%00.jpg
```

## Takeaways
- The null byte bypass is a classic technique for bypassing extension-based filters.
- Modern languages and frameworks are generally not vulnerable to this, but it's still a valuable technique to know when dealing with legacy systems or specific low-level APIs.
