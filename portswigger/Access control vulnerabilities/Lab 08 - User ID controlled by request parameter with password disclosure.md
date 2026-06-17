# Lab: User ID controlled by request parameter with password disclosure

## Description
This lab contains an IDOR vulnerability that leaks the user's password in the profile page's source code.

## Analysis
On the user profile page, the password is pre-filled in a hidden field or an input field for the "Change Password" function.

```html
<input type="password" name="password" value="current-password">
```

If we can access another user's profile via IDOR, we can see their password by inspecting the page source.

## Exploitation
I need to find the password for `administrator` and delete `carlos`.

### Steps
1. Log in as `wiener:peter`.
2. Change the `id` parameter to `administrator`: `/my-account?id=administrator`.
3. View the page source.
4. Find the password input field and copy the value.
5. Log out and log back in as `administrator`.
6. Go to the admin panel and delete carlos.

## Payload
```text
URL: /my-account?id=administrator
```

## Takeaways
- Never reveal sensitive information like passwords in the HTML source, even in hidden fields.
- Mask or omit sensitive data when rendering the user profile.
- Robust authorization is critical for preventing IDOR from leading to full account compromise.
