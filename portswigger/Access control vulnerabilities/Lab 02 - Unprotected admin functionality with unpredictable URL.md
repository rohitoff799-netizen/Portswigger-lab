# Lab: Unprotected admin functionality with unpredictable URL

## Description
The application has an administrative interface at an unpredictable URL, but the URL is leaked in the front-end code.

## Analysis
Even if an administrative URL is not guessable (e.g., `/admin-m3f9a2`), it can still be discovered if it's referenced in the application's source code, JavaScript files, or `robots.txt`.

## Exploitation
I need to find the admin URL and delete `carlos`.

### Steps
1. Navigate to the lab homepage.
2. View the page source (Ctrl+U).
3. Search for "admin".
4. Find a script or element that contains the administrative URL. In this lab, it's a JavaScript variable:
   `var adminPath = '/admin-afj28d';`
5. Navigate to that URL and delete carlos.

## Payload
```text
URL: /admin-afj28d/delete?username=carlos
```

## Takeaways
- Obscurity is not a substitute for security.
- Sensitive URLs should never be leaked to the client-side, even if they are "hard to find."
- Proper authorization checks must be performed on the server-side for every request to a sensitive resource.
