---
description: Reflected XSS Stored  XSS DOM XSS self XSS
---

# Cross Site Scripting (XSS)

### Mechanism

Cross-site scripting (XSS) is a vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users

### What Is Vulnerability?

Cross-Site Scripting (XSS) is a type of security vulnerability typically found in web applications. It allows attackers to inject malicious scripts into content from otherwise trusted websites. These scripts can then be executed in the context of the user's browser, leading to various potential attacks including data theft, session hijacking, and defacement of websites.

## Types

#### Types of XSS Attacks

1. **Stored XSS**
2. **Blind XSS**
3. **Reflected  XSS**
4. **DOM XSS**
5. **Self XSS**

#### How Does It Happen?

### Reflected XSS <a href="#types" id="types"></a>

> **The malicious script is reflected off a web server, such as in an error message or a search result, and executed immediately as part of the response.**

{% tabs %}
{% tab title="PHP" %}
```php
php
<?php
if (isset($_GET['qq'])){
    $qq = $_GET['qq'];
    echo'Result Found '.$qq;
}
?>
<cneter>
    <form action="" method="get">
        <label aria-hidden="true">Search Anything</label>
        <input type="text" name="qq" id="qq">
        <input type="submit" value="Search">
    </form>
</cneter>
```
{% endtab %}

{% tab title="Python" %}
```python
from flask import Flask, request, escape

app = Flask(__name__)

@app.route("/", methods=["GET"])
def search():
    qq = request.args.get("qq")
    result = f"Result Found {escape(qq)}" if qq else ""
    form_html = '''
    <center>
        <form action="" method="get">
            <label aria-hidden="true">Search Anything</label>
            <input type="text" name="qq" id="qq">
            <input type="submit" value="Search">
        </form>
        <div>{}</div>
    </center>
    '''.format(result)
    return form_html

if __name__ == "__main__":
    app.run(debug=True)

```
{% endtab %}

{% tab title="javaScript" %}
```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    const qq = req.query.qq;
    const result = qq ? `Result Found ${qq}` : '';
    const formHtml = `
    <center>
        <form action="" method="get">
            <label aria-hidden="true">Search Anything</label>
            <input type="text" name="qq" id="qq">
            <input type="submit" value="Search">
        </form>
        <div>${result}</div>
    </center>
    `;
    res.send(formHtml);
});

app.listen(port, () => {
    console.log(`App listening at http://localhost:${port}`);
});

```
{% endtab %}
{% endtabs %}

<figure><img src="../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

I will try use some tags to test rxss <mark style="color:red;">**`'><h1>Hacked</h1>`**</mark>

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

Ok this search Vulnerable HTML injection &&  RXSS

Simple JS payload

```javascript
<script>alert('OSCP+EWAPTXv2')</script>
```

<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

### Stored XSS

```php

<?php
if(isset($_POST["c"])) {
    $c = $_POST['c'];
    echo 'your commit is '.$c.' Thanks';
}
?>
<cneter>
    <form action="" method="post">
        <label aria-hidden="true">Write Commint</label>
        <input type="text" name="c" id="c">
         <input type="submit" value="Submit">
    </form>
</cneter>
```





### Mitiegstion Code

Use htmlentities() Function => PHP

**use escape() Function = PYTHON**

use escapeHtml() Function = JS

{% tabs %}
{% tab title="PHP" %}
```php
from flask import Flask, request, escape

app = Flask(__name__)

@app.route("/", methods=["GET"])
def search():
    # Get the user input from the query parameters
    qq = request.args.get("qq")
    
    # Escape the user input to prevent XSS
    result = f"Result Found {escape(qq)}" if qq else ""
    
    # Create the HTML form with the result safely included
    form_html = '''
    <center>
        <form action="" method="get">
            <label aria-hidden="true">Search Anything</label>
            <input type="text" name="qq" id="qq">
            <input type="submit" value="Search">
        </form>
        <div>{}</div>
    </center>
    '''.format(result)
    
    return form_html

if __name__ == "__main__":
    app.run(debug=True)

```
{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const express = require('express');
const app = express();
const port = 3000;

// Function to escape HTML entities
function escapeHtml(unsafe) {
    return unsafe
         .replace(/&/g, "&amp;")
         .replace(/</g, "&lt;")
         .replace(/>/g, "&gt;")
         .replace(/"/g, "&quot;")
         .replace(/'/g, "&#039;");
}

app.get('/', (req, res) => {
    const qq = req.query.qq;
    const result = qq ? `Result Found ${escapeHtml(qq)}` : '';
    const formHtml = `
    <center>
        <form action="" method="get">
            <label aria-hidden="true">Search Anything</label>
            <input type="text" name="qq" id="qq">
            <input type="submit" value="Search">
        </form>
        <div>${result}</div>
    </center>
    `;
    res.send(formHtml);
});

app.listen(port, () => {
    console.log(`App listening at http://localhost:${port}`);
});

```
{% endtab %}
{% endtabs %}

```bash
$string = $_GET['search'] ;


$regex = "/{|}|src|confirm|prompt|write|<|>|alert|print/" ;

?>
<script>
    
    window.test = {
        site: "Night",
    page: {
        name : "<?php echo  preg_replace( $regex, '' ,$string) ?>" ;    
    }
}    

</script>

<center>
<form method="GET" >    
<label aria-hidden="true">Search For Anything</label>
    <input type="text" placeholder="Enter Something" name="search" >
    <button type="submit">Send</button>
</form>    

</center>
```



Additional Security Measures

1.  **Content Security Policy (CSP)**

    ```php
    header("Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';");
     
    ```



## How To PenTest?

## How to Bypass Protection



## Escalating the Attack

