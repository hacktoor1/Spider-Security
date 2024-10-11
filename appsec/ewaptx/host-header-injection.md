# Host Header Injection

### Host Header injection

The HTTP Host header is a mandatory request header introduced in HTTP/1.1.

Example

```http
GET /page HTTP/1.1
Host: example.com
```

### Testing Steps:

* Suppl an Arbitrary Host Header

Send arequest&#x20;

```http
GET /page HTTP/1.1
Host: malicious-domain.com
```

* Check for flawed Validation

```http
GET /page HTTP/1.1
Host: example.com:bad-port
```

* Send Ambiguous Requests

#### Duplicate <mark style="color:red;">**Host**</mark>

```http
GET /page HTTP/1.1
Host: example.com:bad-port
Host: t.example.com:bad-port
```

#### Absolute URL:

```http
GET https://example.com/page HTTP/1.1
Host: bad-stuff-here
```

* Inject Host Override Headers

Use header like <mark style="color:red;">**`X-Forwarded-Host`**</mark>

```http
GET /page HTTP/1.1
Host: example.com
X-Forwarded-Host: malicious-domain.com
```

### Common Attack Vectors

1. **Password Reset Poisoning:** Manipulate the `Host` header to redirect password reset links to an attacker-controlled domain.
2. **Host Header Authentication Bypass:** Exploit improper header handling to bypass authentication mechanisms.
3. **Web Cache Poisoning:** Inject malicious content into a web cache using a forged `Host` header.
4. **Routing-Based SSRF:** Use the `Host` header to manipulate server routing and access internal systems.

### Mitigation and Prevention

* **Avoid Using the Host Header**
* **Validate the Host Header**
* **Disable Host Override Headers**
* **Whitelist Domains for Routing**
