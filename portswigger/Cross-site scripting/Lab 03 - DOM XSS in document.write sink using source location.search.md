# Lab: DOM XSS in document.write sink using source location.search

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the `document.write` sink, which writes data to the page. The source of the data is `location.search`, which refers to the query string in the URL.

## Analysis
Looking at the source code of the search results page, I found the following script:

```javascript
function trackSearch(query) {
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```

The `trackSearch` function takes the `query` (from the `search` URL parameter) and uses `document.write` to create an `<img>` tag. Crucially, it doesn't sanitize the `query` before putting it into the string. We can break out of the `src` attribute and the `<img>` tag itself to inject our own HTML.

## Exploitation
I need to break out of the `<img>` tag and execute an `alert()`.

### Steps
1. Navigate to the lab.
2. Enter a random string in the search box (e.g., `test`) and click "Search".
3. Observe the URL: `?search=test`.
4. Modify the URL to inject a payload that closes the `<img>` tag and starts a script.
   - Initial string: `tracker.gif?searchTerms=test`
   - Breaking out: `tracker.gif?searchTerms="><script>alert(1)</script>`

## Payload
```text
"><script>alert(1)</script>
```

## Takeaways
- DOM XSS occurs when an application contains client-side JavaScript that processes data from an untrusted source in an unsafe way, usually by writing the data to a dangerous sink within the Document Object Model (DOM).
- Avoid using `document.write`. Safer alternatives like `textContent` should be used to handle user input.
