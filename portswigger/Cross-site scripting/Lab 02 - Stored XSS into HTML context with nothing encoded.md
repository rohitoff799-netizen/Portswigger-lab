# Lab: Stored XSS into HTML context with nothing encoded

## Description
This lab contains a stored cross-site scripting vulnerability in the comment functionality.

## Analysis
The application allows users to submit comments on blog posts. The comment content is stored in the database and then rendered on the page whenever a user views the post. Since the application does not sanitize the comment content, a malicious script can be stored and executed by anyone who views the comments.

### Vulnerable Code Path
1. User submits a comment via a POST request to `/post/comment`.
2. The application stores the comment content in the database.
3. When any user visits the blog post page, the application retrieves all comments and renders them: `<p>User Comment Content</p>`

## Exploitation
The goal is to store a script that calls `alert()` when someone views the post.

### Steps
1. Navigate to any blog post.
2. Fill out the comment form with any name/email.
3. In the "Comment" field, enter the payload: `<script>alert(1)</script>`
4. Submit the comment.
5. Reload the page or navigate back to the post to trigger the alert.

## Payload
```html
<script>alert(1)</script>
```

## Takeaways
- Stored XSS is generally more dangerous than reflected XSS because it affects all users who view the compromised content and doesn't require the victim to click a malicious link.
- Input validation and output encoding are essential defenses.
