# Open redirect



```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Open Redirect Lab</title>
</head>
<body>
    <h1>Welcome to the Open Redirect Lab</h1>
    <p><a href="redirect.php?url=success.php">Go to Success Page</a></p>
    <p><a href="redirect.php?url=http://malicious.com">Go to Malicious Page</a></p>
</body>
</html>
```

all seniors

```jsx
//Using #
<http://localhost/labs/op/redirect.php?url=http://evil.com#.google.com>  or using %23

//Using \\,\\\\
<http://localhost/labs/op/redirect.php?url=http://evil.com\\.google.com>
//Using @
<http://localhost/labs/op/redirect.php?url=http://evil.com@google.com>
//Using TLD
<http://localhost/labs/op/redirect.php?url=.test.com>
//withOut // [http:google.com]
<http://localhost/labs/op/redirect.php?url=http:google.com>

```

Mitigation

Allowlisted Redirects

```php
<?php
**$allowed_urls = [
    'success.php',**
];

if (isset($_GET['url']) && in_array($_GET['url'], $allowed_urls)) {
    header("Location: " . $_GET['url']);
    exit();
} else {
    echo "Invalid URL provided.";
}
```

Absolute URL Validation

```php
<?php
if (isset($_GET['url'])) {
    $url = $_GET['url'];
    $parsed_url = parse_url($url);

    if ($parsed_url['host'] === 'yourdomain.com') {
        header("Location: " . $url);
        exit();
    } else {
        echo "Invalid URL provided.";
    }
} else {
    echo "No URL provided.";
}

```
