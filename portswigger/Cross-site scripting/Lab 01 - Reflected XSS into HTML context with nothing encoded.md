# Lab: Reflected XSS into HTML context with nothing encoded

## Description
This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

## Analysis
The application takes input from the `search` query parameter and reflects it directly into the HTML page without any sanitization or encoding. This allows an attacker to inject arbitrary HTML and JavaScript into the page.

### Vulnerable Code Path
1. User submits a search query: `?search=test`
2. Application processes the query.
3. Application renders the response: `<h1>0 results for 'test'</h1>`

By replacing `test` with a script tag, we can execute JavaScript in the context of the user's session.

## Exploitation
To solve the lab, I need to perform a cross-site scripting attack that calls the `alert()` function.

### Steps
1. Navigate to the lab homepage.
2. In the search box, enter the following payload: `<script>alert(1)</script>`
3. Click "Search".

## Payload
```html
<script>alert(1)</script>
```

## Takeaways
- Always encode user-supplied data before reflecting it in the HTML context.
- Use context-aware output encoding to prevent XSS.
