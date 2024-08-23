# HTTP Host header attacks

### What is the HTTP Host Header?

The HTTP `Host` header is a mandatory request header introduced in HTTP/1.1. It specifies the domain name of the server that the client wants to communicate with. This allows servers to differentiate between multiple domains or applications hosted on the same IP address.

#### Example

When a user navigates to `https://example.com/page`, the browser sends a request containing the `Host` header as follows:

```
GET /page HTTP/1.1
Host: example.com
```

### Purpose of the HTTP Host Header

The primary purpose of the `Host` header is to identify the intended server or back-end component that should process the request. This is especially crucial in scenarios involving multiple applications hosted on the same IP address. Without the `Host` header, servers would struggle to route incoming requests to the correct application, potentially leading to misrouting or failures.

#### Scenarios Where the Host Header is Critical

#### 1. **Virtual Hosting**

In virtual hosting, a single web server hosts multiple websites. Each site might belong to the same or different owners. The `Host` header allows the server to distinguish requests for different domains and route them to the appropriate back-end.

#### 2. **Routing Traffic via Intermediaries**

Websites often use intermediaries like load balancers or reverse proxies to distribute traffic. In these setups, the intermediary uses the `Host` header to direct requests to the correct back-end server. Content Delivery Networks (CDNs) also rely on the `Host` header to determine which origin server should handle the request.

#### How the HTTP Host Header Solves Routing

When a server receives a request, it uses the `Host` header to determine which back-end application should handle it. This mechanism is analogous to specifying an apartment number in a building’s address to ensure mail is delivered to the correct recipient.

### HTTP Host Header Attacks

HTTP Host header attacks exploit the insecure handling of the `Host` header by web applications. If a server trusts the `Host` header without proper validation, attackers can manipulate it to inject harmful payloads, leading to vulnerabilities such as web cache poisoning, business logic flaws, routing-based SSRF (Server-Side Request Forgery), and classic server-side vulnerabilities like SQL injection.

#### How Vulnerabilities Arise

Vulnerabilities typically arise from the mistaken belief that the `Host` header is not user-controllable. Attackers can modify the `Host` header using tools like Burp Suite, and if the server fails to validate or escape this input, it can lead to severe security issues.

### Exploiting HTTP Host Header Vulnerabilities

To test for vulnerabilities, you can use intercepting proxies like Burp Suite to modify the `Host` header and observe the server’s behavior.

#### Testing Steps

#### 1. **Supply an Arbitrary Host Header**

Start by sending a request with an unrecognized or arbitrary domain name in the `Host` header. Observe how the server responds and if it falls back to a default behavior.

```
GET /page HTTP/1.1
Host: malicious-domain.com
```

#### 2. **Check for Flawed Validation**

Inspect whether the server performs any validation on the `Host` header. Some servers might only validate the domain part, ignoring the port or other components.

```
GET /page HTTP/1.1
Host: example.com:bad-port
```

#### 3. **Send Ambiguous Requests**

You can send ambiguous requests by using duplicate `Host` headers or an absolute URL in the request line. This can reveal discrepancies in how different components interpret the `Host` header.

**Duplicate Host Headers:**

```
GET /page HTTP/1.1
Host: example.com
Host: bad-stuff-here
```

**Absolute URL:**

```
GET <https://example.com/page> HTTP/1.1
Host: bad-stuff-here
```

#### 4. **Inject Host Override Headers**

Use headers like `X-Forwarded-Host` to override the `Host` header value. This can bypass validation on the `Host` header itself.

```
GET /page HTTP/1.1
Host: example.com
X-Forwarded-Host: malicious-domain.com

```

#### Common Attack Vectors

1. **Password Reset Poisoning:** Manipulate the `Host` header to redirect password reset links to an attacker-controlled domain.
2. **Host Header Authentication Bypass:** Exploit improper handling of the `Host` header to bypass authentication mechanisms.
3. **Web Cache Poisoning:** Inject malicious content into a web cache using a forged `Host` header.
4. **Routing-Based SSRF:** Use the `Host` header to manipulate server routing and access internal systems.

### Prevention

#### Avoid Using the Host Header

Where possible, avoid using the `Host` header in server-side code. Use relative URLs instead of absolute URLs.

#### Validate the Host Header

If you must use the `Host` header, ensure it is validated against a whitelist of permitted domains. Reject or redirect requests for unrecognized hosts.

#### Disable Host Override Headers

Ensure that additional headers like `X-Forwarded-Host` are not supported unless absolutely necessary, and validate them as strictly as the `Host` header.

#### Whitelist Domains for Routing

Configure intermediaries like load balancers or reverse proxies to only forward requests to a whitelist of approved domains.
