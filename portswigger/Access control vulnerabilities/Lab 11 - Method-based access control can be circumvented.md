# Lab: Method-based access control can be circumvented

## Description
This lab contains an access control vulnerability where the application restricts access to sensitive functionality based on the HTTP method, but fails to account for alternative methods.

## Analysis
The application allows administrators to change user roles via a `POST` request to `/admin-roles`. A security filter blocks `POST` requests to this URL for non-admin users. However, if the back-end application also accepts `GET` or `HEAD` requests for the same functionality, we can bypass the filter.

## Exploitation
I need to elevate the user `wiener` to an administrator.

### Steps
1. Log in as `administrator:admin`.
2. Go to the admin panel and promote a user.
3. Intercept the `POST /admin-roles` request.
4. Log in as `wiener:peter`.
5. Try to replay the `POST` request. It should be blocked.
6. Change the request method to `GET` and move the parameters to the query string:
   `GET /admin-roles?username=wiener&action=upgrade`
7. Send the request. If the filter only blocks `POST`, the `GET` request will succeed.

## Payload
```text
GET /admin-roles?username=wiener&action=upgrade
```

## Takeaways
- Access control should be applied to the functionality itself, regardless of the HTTP method used to access it.
- Never rely on method-based filtering for security.
- Explicitly define which methods are allowed for each endpoint and ensure that all allowed methods are subject to the same authorization checks.
