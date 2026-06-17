# Lab: Web cache poisoning via an unkeyed query string

## Description
This lab contains a web cache poisoning vulnerability where the entire query string is unkeyed.

## Analysis
The application has a cache that ignores the query string when deciding whether to serve a cached response. However, the query string is still used by the application to generate the response.

If we send a request to `/` with a malicious query string, the response will be cached and served to any user who visits `/`, even without the query string.

## Exploitation
I need to poison the cache for the homepage.

### Steps
1. Navigate to the homepage and observe that adding a query parameter (e.g., `/?test=1`) doesn't change the `X-Cache` behavior (it still says `hit` for the root `/`).
2. This indicates the query string is unkeyed.
3. Find a parameter that is reflected in the response. In this lab, it's the `X-Forwarded-Host` header again, but we can also use query parameters if they are reflected.
4. Let's say the application reflects a query parameter in a script tag.
5. Send a request with a malicious query parameter: `/?test='/><script>alert(1)</script>`
6. Check if the payload is reflected.
7. Poison the cache until `X-Cache: hit` is received for the root URL `/`.
8. Now, anyone visiting `/` will trigger the alert.

## Payload
```text
URL: /?evil='/><script>alert(1)</script>
```

## Takeaways
- Unkeyed query strings are extremely dangerous as they allow poisoning the most common entry points of an application.
- The query string should always be part of the cache key unless there's a very specific and well-secured reason not to.
- Use a "cache buster" query parameter during testing to avoid accidentally affecting other users.
