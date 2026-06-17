# Lab: Insecure direct object references

## Description
This lab contains an IDOR vulnerability where sensitive information is stored in static files with predictable names.

## Analysis
The application allows users to download their chat transcripts. The transcript files are named sequentially (e.g., `1.txt`, `2.txt`).

```text
GET /download-transcript/1.txt
```

By guessing the names of other transcript files, we can access the chat history of other users.

## Exploitation
I need to find the password for `carlos`.

### Steps
1. Start a chat and download the transcript.
2. Observe the filename (e.g., `2.txt`).
3. Change the filename in the URL to `1.txt`.
4. The file `1.txt` contains a chat where a user mentions their password.

## Payload
```text
URL: /download-transcript/1.txt
```

## Takeaways
- Static files should also be protected by access control.
- Avoid using predictable names for sensitive files. Use UUIDs or other long, random strings.
- Ensure that the application verifies the user's permission to download a specific file.
