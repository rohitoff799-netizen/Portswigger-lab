# Lab: Server-side template injection with information disclosure via user-supplied objects

## Description
This lab contains an SSTI vulnerability where we can access internal objects to leak sensitive information, such as the secret key.

## Analysis
The application uses the Django template engine (Python).
Testing with `{{7*7}}` results in `49`.
Django templates don't allow arbitrary code execution by default, but we can explore the available objects. The `settings` object is often available in Django templates and contains sensitive configuration data.

## Exploitation
I need to leak the `SECRET_KEY`.

### Steps
1. Navigate to your profile.
2. Change your name to `{{settings.SECRET_KEY}}`.
3. Save and observe the reflected secret key on the profile page.

## Payload
```django
{{settings.SECRET_KEY}}
```

## Takeaways
- Even if RCE is not possible, SSTI can be used for significant information disclosure by accessing global or context-specific objects.
- Always check what objects and functions are exposed to the template context.
