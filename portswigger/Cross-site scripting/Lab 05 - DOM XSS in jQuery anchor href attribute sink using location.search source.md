# Lab: DOM XSS in jQuery anchor href attribute sink using location.search source

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the "Submit feedback" page. It uses the jQuery library to modify the `href` attribute of an anchor element.

## Analysis
On the feedback page, there is a "Back" link. The script uses the `returnPath` parameter from the URL to set the `href` of this link.

```javascript
$(function() {
    $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
});
```

The `attr()` function in jQuery is used to set the `href` attribute. Since the value is taken directly from the `returnPath` parameter, an attacker can use the `javascript:` pseudo-protocol to execute JavaScript when the link is clicked.

## Exploitation
I need to set the `returnPath` to a `javascript:` payload.

### Steps
1. Navigate to the "Submit feedback" page.
2. Observe the URL, it should have a `returnPath` parameter (e.g., `?returnPath=/`).
3. Change the `returnPath` value to `javascript:alert(document.cookie)`.
4. Click the "Back" link at the bottom of the page.

## Payload
```text
javascript:alert(document.cookie)
```

## Takeaways
- The `javascript:` pseudo-protocol allows code execution within attributes that expect URLs (like `href` or `src`).
- Always validate that URLs provided by users use safe protocols (like `http:` or `https:`) and do not contain malicious scripts.
