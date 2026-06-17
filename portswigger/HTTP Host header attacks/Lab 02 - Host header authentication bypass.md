# Lab: Host header authentication bypass

## Description
This lab contains an authentication bypass vulnerability. The application's internal admin interface can be accessed by providing a specific `Host` header.

## Analysis
The application's front-end server routes requests based on the `Host` header. It might be configured to allow access to `/admin` only if the `Host` is `localhost`.

## Exploitation
I need to access the admin panel and delete `carlos`.

### Steps
1. Navigate to `/admin`. You get a "403 Forbidden" or "Access denied" message.
2. Intercept the request.
3. Change the `Host` header to `localhost`:
   `Host: localhost`
4. Send the request.
5. If the application trusts the `Host` header for its internal routing and access control, you will now see the admin panel.
6. Delete carlos.

## Payload
```text
Host: localhost
```

## Takeaways
- Access control should never be based solely on the `Host` header.
- Ensure that your web server and application are configured to only respond to requests for authorized domains.
- Internal administrative interfaces should be protected by strong authentication and, ideally, not be accessible via the same front-end server as the public site.
