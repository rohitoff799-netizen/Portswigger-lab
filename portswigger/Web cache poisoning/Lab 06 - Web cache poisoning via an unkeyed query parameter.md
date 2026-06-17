# Lab: Web cache poisoning via an unkeyed query parameter

## Description
This lab contains a web cache poisoning vulnerability where a specific query parameter is unkeyed.

## Analysis
Unlike the previous lab where the whole query string was unkeyed, here only certain parameters are excluded from the cache key. We can use Param Miner to identify these unkeyed parameters.

In this lab, the parameter `utm_content` is unkeyed but reflected in the response.

## Exploitation
I need to poison the cache using the `utm_content` parameter.

### Steps
1. Use Param Miner to find unkeyed parameters. It identifies `utm_content`.
2. Verify that `utm_content` is reflected in the response.
3. Send a request with a malicious value: `/?utm_content='/><script>alert(1)</script>`.
4. Check if the payload is reflected in the cached response for the main page.
5. Poison the cache until `X-Cache: hit` is received.
6. Now, any user visiting the page will execute the script.

## Payload
```text
URL: /?utm_content='/><script>alert(1)</script>
```

## Takeaways
- Even if the query string is generally keyed, individual parameters can be unkeyed, often for analytics or tracking purposes (like UTM parameters).
- These parameters are frequently overlooked and can provide a simple path to cache poisoning.
- Always include all parameters that affect the response in the cache key.
