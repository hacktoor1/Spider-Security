# XML injection

### XML Tag injection

{% hint style="success" %}
in this scenario, the attacker can alter to alter the XML document structure by injecting both XML data and XML tags
{% endhint %}

```xml
<?xml version="1.0"?>
<users>
    <user>
        <username>admin</username>
        <password>secrte</password>
        <group>Administrator</group>
    </user>
     <user>
        <username>hacktor</username>
        <password>secrtehacktor</password>
        <group>users</group>
    </user>
<users>
```

if Can Injection some XML Metacharacters within the document, then, if the application fails to contextually validate data&#x20;

Metacharacters: <mark style="color:red;">**' " < > &**</mark>

### XML Injection - Single/Double Quotes

> Sigle and Double quote are used to define an attribute vale in the tag:
>
> ```xml
> <group id="id">admin</group>                <group id='id'>admin</group>
> ```
>
> an id , Like the following , will make the XML incorrect
>
> ```xml
> <group id="12"">admin</group> <group id='12''>admin</group>
> ```

### XML Injection - XSS with CDATA

```bash
#Normal payload
<script>alert('h3ckt00r')</script>

#Using CDATA
<![CDATA[<]]>script<![CDATA[>]]>
alert('xss')
<![CDATA[<]]>/script<![CDATA[>]]
```





#### Mitigations for XML Injection:

1. **Proper Input Sanitization**:
   * Always sanitize and validate user input before embedding it in XML.
   * Escape special characters like `<`, `>`, `&`, `'`, and `"` in user-provided data.
2. **Use XML Libraries**:
   * Use secure XML libraries that automatically handle character encoding and prevent injection. Many libraries have built-in mechanisms to safely escape special characters.
   * Avoid the manual construction of XML strings.
3.  **Disable External Entity Processing**:

    * Disable **XXE** (External Entity Processing) if it’s not required, as it can lead to other severe vulnerabilities.

    In PHP, for example:

    ```php
    libxml_disable_entity_loader(true);
    ```
4. **XML Schema Validation**:
   * Use XML Schema Definitions (XSD) or Document Type Definitions (DTD) to enforce the structure of XML documents. This can help ensure that malicious data or unexpected elements are not accepted.
5. **Use CDATA for User Input**:
   * If you must insert user input into XML elements, consider wrapping it in a `<![CDATA[]]>` section. This will prevent special characters from being interpreted as markup.
