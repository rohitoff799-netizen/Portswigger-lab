# Lab: DOM XSS in innerHTML sink using source location.search

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the search functionality. It uses the `innerHTML` sink, which allows an attacker to inject HTML into an element. The source is `location.search`.

## Analysis
The search page uses JavaScript to display the search query on the page using `innerHTML`.

```javascript
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    document.getElementById('searchMessage').innerHTML = query;
}
```

The `innerHTML` sink is dangerous because it renders any HTML tags it receives. However, modern browsers often block `<script>` tags injected via `innerHTML`. To bypass this, we can use other elements like `<img>` with an `onerror` event handler.

## Exploitation
I need to use a payload that doesn't rely on `<script>` tags.

### Steps
1. Navigate to the lab.
2. In the search box, enter the payload: `<img src=x onerror=alert(1)>`
3. Click "Search".
4. The browser tries to load an image from the source `x`, fails, and triggers the `onerror` event, executing `alert(1)`.

## Payload
```html
<img src=x onerror=alert(1)>
```

## Takeaways
- `innerHTML` is a dangerous sink that should be avoided when handling user input.
- Even if `<script>` tags are blocked, other event handlers (like `onerror`, `onload`, `onmouseover`) can still be used for XSS.
- Prefer `textContent` or `innerText` to safely display user input without rendering it as HTML.
