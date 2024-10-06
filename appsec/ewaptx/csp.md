# CSP

> **Content Security Policy** ([CSP](https://developer.mozilla.org/en-US/docs/Glossary/CSP)) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting ([XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site\_scripting)) and data injection attacks. These attacks are used for everything from data theft, to site defacement, to malware distribution.

### Why is the CSP necessary?

1. **Mitigating XSS Attacks**
2. **Preventing Data Injection Attacks**
3. **Protecting Against Clickjacking**

### How does CSP work?

1. **Policy Declaration:**

**HTTP Header:** Web servers can include the CSP header in their HTTP responses. For example:

{% code overflow="wrap" %}
```markup
Content-Security-Policy: default-src 'self'; script-src 'self' <https://apis.example.com>; style-src 'self' <https://cdn.example.com>;
```
{% endcode %}

**Meta Tag**: Alternatively, developers can include a meta tag in the HTML document to declare the CSP policy.

{% code overflow="wrap" %}
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' https://apis.example.com; style-src 'self' https://cdn.example.com;">
```
{% endcode %}

2. **Policy Directives:**
   * **default-src:** Specifies the default source for content that is not explicitly specified by other directives.
   * **script-src:** Specifies valid sources for JavaScript.
   * **style-src:** Specifies valid sources for stylesheets.
   * **img-src:** Specifies valid sources for images.
   * **font-src:** Specifies valid sources for fonts.
   * **connect-src:** Specifies valid sources for network requests (e.g., AJAX, WebSocket).
   * **frame-src:** Specifies valid sources for frames and iframes.
   * **object-src:** Specifies valid sources for plugins, like Flash.
   * And more.
3. **Source Expressions:**
   * Source expressions define the allowed sources for each content type. They can be specific domains (<mark style="color:blue;">**`'self'`**</mark><mark style="color:blue;">**,**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**`'example.com'`**</mark><mark style="color:blue;">**)**</mark>, URLs (<mark style="color:red;">**`https://example.com`**</mark>), or other keywords.

Examples

**Configure CSP for WordPress**

Option1: Modify Server Headers

* **Apache** (using <mark style="color:red;">**`.htaccess`**</mark>)

```html
<IfModule mod_headers.c>
    Header set Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:; object-src 'none';"
</IfModule>
```

**Nginx (**<mark style="color:red;">**`nginx.conf`**</mark>**)**

```
add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:; object-src 'none';";

```

**Option 2: Use a WordPress Plugin**

**CSP by WP2Static**
