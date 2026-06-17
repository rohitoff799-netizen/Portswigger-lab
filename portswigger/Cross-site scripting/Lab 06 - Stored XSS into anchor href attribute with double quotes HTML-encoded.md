# Lab: Stored XSS into anchor href attribute with double quotes HTML-encoded

## Description
This lab contains a stored cross-site scripting vulnerability in the comment functionality. It's unique because the input is reflected within an `href` attribute of an `<a>` tag.

## Analysis
When a user submits a comment, they can also provide a website URL. This URL is then used in the author's name link in the comments section.

```html
<a href="http://user-website.com">User Name</a>
```

The application HTML-encodes double quotes, which prevents us from breaking out of the `href` attribute (e.g., using `"><script>...`). however, since the input is *inside* the `href` attribute, we can use the `javascript:` pseudo-protocol.

## Exploitation
I need to submit a comment with a malicious URL.

### Steps
1. Go to any blog post.
2. Fill out the comment form.
3. In the "Website" field, enter: `javascript:alert(1)`
4. Submit the comment.
5. Click on the name of the commenter to trigger the alert.

## Payload
```text
javascript:alert(1)
```

## Takeaways
- Even if an application encodes special characters like `<` and `>`, vulnerabilities can still exist if the input is reflected in a context where those characters aren't needed for exploitation (like a URL attribute).
- Use a whitelist of allowed URL schemes (e.g., `http`, `https`) to prevent the use of `javascript:`.
