# Lab: OS command injection, simple case

## Description
This lab contains an OS command injection vulnerability in the product stock checker. The application executes a shell command containing user-supplied product and store IDs and returns the raw output from the command in its response.

## Analysis
The application likely uses a command-line utility to check stock.
Example command: `stockreport.sh <productId> <storeId>`

If the inputs aren't sanitized, we can use shell metacharacters to execute additional commands.
Common metacharacters: `&`, `&&`, `|`, `||`, `;`, newline (`%0a`).

## Exploitation
I need to execute the `whoami` command.

### Steps
1. Intercept the stock check request.
2. The parameters are `productId` and `storeId`.
3. Modify the `storeId` to include the `whoami` command using the `&` separator: `storeId=1 & whoami`.
4. Send the request and observe the output in the response.

## Payload
```text
productId=1&storeId=1 & whoami
```

## Takeaways
- OS command injection is a critical vulnerability that allows an attacker to execute arbitrary operating system commands on the server.
- Never concatenate user input into shell commands. Use built-in API functions that don't involve the shell, or rigorously sanitize all input.
