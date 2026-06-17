# Lab: Web shell upload via path traversal

## Description
This lab prevents the execution of uploaded files in the `/avatars/` directory. We need to use path traversal to upload the shell to a different directory where execution is allowed.

## Analysis
The application uploads files to `/avatars/`. This directory is configured to prevent the execution of scripts (e.g., using `AllowOverride None` in Apache or similar settings). However, the application doesn't sanitize the `filename` parameter in the `Content-Disposition` header.

By changing the filename to `../shell.php`, we can upload the file to the parent directory, which might have execution enabled.

## Exploitation
I need to upload a web shell to a writable and executable directory.

### Steps
1. Attempt to upload `shell.php`. It uploads to `/avatars/shell.php`, but when you access it, the raw code is displayed instead of being executed.
2. Intercept the upload request.
3. Change the `filename` in the `Content-Disposition` header:
   `filename="../shell.php"`
4. Send the request. The application might report that the file was uploaded to `/avatars/../shell.php`.
5. Access the shell at the new location: `/shell.php`.

## Payload
```text
Content-Disposition: form-data; name="avatar"; filename="../shell.php"
```

## Takeaways
- Sanitize all user-supplied data, including filenames.
- Use a whitelist of allowed characters for filenames and remove any path traversal sequences (`../`).
- Implementation of defense-in-depth: even if a file is uploaded, it shouldn't be executable in the upload directory. However, this is only effective if traversal is also prevented.
