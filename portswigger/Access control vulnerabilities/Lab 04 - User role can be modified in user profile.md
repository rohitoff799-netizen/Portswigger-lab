# Lab: User role can be modified in user profile

## Description
This lab contains a privilege escalation vulnerability in the user profile update function.

## Analysis
When a user updates their email, the application sends a JSON request to `/my-account/change-email`.

```json
{
    "username": "wiener",
    "email": "wiener@example.com"
}
```

If the application is vulnerable to mass assignment or fails to restrict which fields can be updated, an attacker can include an `roleid` or `isAdmin` field in the request to elevate their privileges.

## Exploitation
I need to find the correct field to elevate my role.

### Steps
1. Log in as `wiener:peter`.
2. Go to your account page and update your email.
3. Intercept the request.
4. Try adding a field to the JSON body: `"roleid": 2`. (You might need to guess the field name or find it elsewhere in the application).
5. Send the request. If the response shows `"roleid": 2`, you've successfully escalated your privileges.
6. Navigate to `/admin` and delete carlos.

## Payload
```json
{
    "username": "wiener",
    "email": "wiener@example.com",
    "roleid": 2
}
```

## Takeaways
- Mass assignment vulnerabilities occur when an application takes user input and uses it to update internal objects without explicitly defining which fields are allowed.
- Use a "white-list" approach for updating model properties to prevent unauthorized field modification.
