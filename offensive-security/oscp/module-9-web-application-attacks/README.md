# Module 9 (Web Application Attacks)

### Web Application Enumeration

* Programming language and Frameworks
  * PHP - ASP.net - JSP - PYTHON - JAVA  and More
  * Wappalyzer - inspecting URLs - whatweb - <mark style="color:red;">**Error**</mark>&#x20;
* Web Server Software
  * Apache - Nginx - IIS
  * Wappalyzer  -   whatweb - <mark style="color:red;">**Error**</mark> &#x20;
* Database software
  * MySQL - MariaDB - MongoDB - Oracle - SQL Server
  * Wappalyzer  -   <mark style="color:red;">**Error**</mark> &#x20;
* Server OS
  * Linux - Windows
  * Wappalyzer - NSE (Nmap Script Engine)



### Web Application Assessment Tools

* Fuzz Directories
  * Drip - **gobuster** - **dirsearsh** - fuff - **wfuzz**



### Wfuzz

Fuzzing Dir

```bash
wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --hc 404,302,301 http://192.168.2.5/bWAPP/FUZZ
```

<figure><img src="../../../.gitbook/assets/image (289).png" alt=""><figcaption></figcaption></figure>



Fuzzing Files

**`.bak, .php, .zip . xml , .json ...etc`**

```
wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --hc 404,302,301 http://192.168.2.5/bWAPP/FUZZ.php
```

<figure><img src="../../../.gitbook/assets/image (290).png" alt=""><figcaption></figcaption></figure>

Dirbuaster

<figure><img src="../../../.gitbook/assets/image (291).png" alt=""><figcaption></figcaption></figure>

## Fuzzing Parameters In URLs

```bash
wfuzz -z range,0-10 --hl 97 http://testphp.vulnweb.com/listproducts.php?cat=FUZZ
```

## &#x20;**Fuzzing Cookies**

```bash
wfuzz -z file,wordlist/general/common.txt -b cookie=value1 -b cookie2=value2 http://testphp.vulnweb.com/FUZZ
```

## **Fuzzing POST Requests**

```bash
 wfuzz -z file,wordlist/others/common_pass.txt -d "uname=FUZZ&pass=FUZZ"  --hc 302 http://testphp.vulnweb.com/userinfo.php
```



*   **Test Vulnerabilities**

    * Nikto - Nessus - acunetix - netsparker - **burp** and zap proxy



## OWASP Top Ten

<figure><img src="../../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

1. **A01:2021-Broken Access Control**
2. **A02:2021-Cryptographic Failures**&#x20;
3. **A03:2021-Injection**
4. **A04:2021-Insecure Design**&#x20;
5. **A05:2021-Security Misconfiguration**
6. **A06:2021-Vulnerable and Outdated Components**
7. **A07:2021-Identification and Authentication Failures**&#x20;
8. **A08:2021-Software and Data Integrity Failures**&#x20;
9. **A09:2021-Security Logging and Monitoring Failures**&#x20;
10. **A10:2021-Server-Side Request Forgery**
