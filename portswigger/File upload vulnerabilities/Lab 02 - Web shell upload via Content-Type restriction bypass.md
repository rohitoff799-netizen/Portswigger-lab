# Lab: Web shell upload via Content-Type restriction bypass

## Description
This lab attempts to restrict file uploads by checking the `Content-Type` header, which can be easily spoofed.

## Analysis
The application checks the `Content-Type` of the uploaded file. If we upload a `.php` file, the browser might set the `Content-Type` to `application/x-php`, which the application might reject. However, we can manually change this header to `image/jpeg` in our request.

## Exploitation
I need to upload a web shell and read `/home/carlos/secret`.

### Steps
1. Attempt to upload `shell.php`. The application rejects it.
2. Intercept the upload request in Burp Suite.
3. Change the `Content-Type` header for the file part to `image/jpeg`:
   `Content-Type: image/jpeg`
4. Send the request. The application should now accept the file.
5. Access the shell: `/avatars/shell.php`.

## Payload
```text
Content-Type: image/jpeg (for shell.php)
```

## Takeaways
- Never rely on client-supplied headers like `Content-Type` for security decisions.
- Perform server-side validation of the actual file content.
- Use a robust library to verify that the uploaded file is indeed what it claims to be.
