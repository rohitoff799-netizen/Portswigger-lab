# Lab: Unprotected admin functionality

## Description
This lab contains an unprotected administrative interface. The application fails to restrict access to the `/admin` URL.

## Analysis
Applications often have administrative pages that are supposed to be restricted to authorized users. If the application doesn't implement proper access control checks, anyone can access these pages if they know the URL.

## Exploitation
I need to delete the user `carlos`.

### Steps
1. Navigate to the root of the lab.
2. Manually try to access common admin URLs like `/admin`.
3. In this lab, `/admin` is accessible.
4. Locate the "Delete" link for carlos and click it.

## Payload
```text
URL: /admin/delete?username=carlos
```

## Takeaways
- Simple, guessable URLs for sensitive functionality are a major security risk.
- Always implement robust authentication and authorization checks for all administrative and sensitive pages.
- Don't rely on "security by obscurity."
