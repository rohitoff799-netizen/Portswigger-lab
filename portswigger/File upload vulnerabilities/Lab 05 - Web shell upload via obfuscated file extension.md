# Lab: Web shell upload via obfuscated file extension

## Description
This lab uses a more sophisticated extension check that can be bypassed using null byte obfuscation.

## Analysis
The application might use a validation logic that is inconsistent with the final file-saving function.
- Validation: Sees `shell.php%00.jpg` and thinks it's a `.jpg` file.
- Saving/Execution: Sees `shell.php` (due to the null byte) and saves it as a `.php` file.

Other obfuscation techniques include:
- `shell.php.jpg` (if the application only checks the last extension)
- `shell.pHp` (if the check is case-sensitive)
- `shell.php.` or `shell.php ` (trailing dots or spaces)

## Exploitation
I need to upload a web shell using a null byte to bypass the extension check.

### Steps
1. Create `shell.php`.
2. Intercept the upload request.
3. Change the `filename` to: `shell.php%00.jpg`.
4. Send the request.
5. Access the shell: `/avatars/shell.php`. (The server saves it as `shell.php` because the null byte terminates the string in the file system API).

## Payload
```text
filename="shell.php%00.jpg"
```

## Takeaways
- Inconsistent string processing between different layers of an application (e.g., validation vs. file system) is a common source of vulnerabilities.
- Strip null bytes and other control characters from user-supplied filenames.
- Use a strict whitelist and ensure the validation and final usage are consistent.
