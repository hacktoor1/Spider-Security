# Module 6 (Passive Info Gathering)

### Website Recon (1/2)

* About Us
* contact (Emails. Social)
* Support
* Careers
* Login

{% hint style="danger" %}
<mark style="color:red;">**DON'T REMOVE ANY NOTES!!!**</mark>
{% endhint %}

### Website Recon (2/2)

*   **Partners and thrid parties**

    * Mergers and acquisitions , Partenships, thrid parites
    * what type of technologies and systems they use internslly
    * [Agiliance](https://www.crunchbase.com/home) Site


*   **Job search sites**

    * Linkedin - Indeed - Monster - Careerbuilder - Glassdoor - Simplyhired - DICE - Aglilance



## User Information Gathering

*   Employee;s personal information such as:

    * pthone numbers , addresses , CV , opinions, responsibllities, project.


* &#x20;theHarvester
* pipl.com
* peoplefinders.com

### theHarvester

theHarvester is used to gather open source intelligence (OSINT) on a company or domain.

```bash
theHarvester -d <domain-name> -b all
```

***

### Serach engines (1/2)

* Google Hacking Data Base \[GHDB]
* The common operatros (ANDm OR, +, - , "")
* \[link:www.website.com]
* \[site:www.website.com]
* \[intext:www.website.com]
* \[cache:www.website.com]
* \[filetype:pdf] -filetype:html

### Search Operators

| Operator         | Description                                                                                                              | Syntax                         | Example                           |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------ | --------------------------------- |
| ()               | Group multiple terms or operators. Allows advanced expressions                                                           | (\<term> or \<operator>)       | inurl:(html \| php)               |
| \*               | Wildcard. Matches any word                                                                                               | \<text> \* \<text>             | How to \* a computer              |
| ""               | The given keyword has to match exactly. _case-insensitive_                                                               | "\<keywords>"                  | "google"                          |
| m..n / m...n     | Search for a range of numbers. _n_ should be greater than _m_                                                            | \<number>..\<number>           | 1..100                            |
| -                | Documents that match the operator are excluded. _NOT_-Operator                                                           | -\<operator>                   | -site:youtube.com                 |
| +                | Include documents that match the operator                                                                                | +\<operator>                   | +site:youtube.com                 |
| \|               | Logical _OR-Operator_. Only one operator needs to match in order for the overall expression to match                     | \<operator> \| \<operator>     | "google" \| "yahoo"               |
| \~               | Search for synonyms of the given word. Not supported by Google                                                           | \~\<word>                      | \~book                            |
| @                | Perform a search only on the given social media platform. Rather use **site**                                            | @\<socialmedia>                | @instagram                        |
| after            | Search for documents published / indexed after the given date                                                            | after:\<yy(-mm-dd)>            | after:2020-06-03                  |
| allintitle       | Same as **intitle** but allows multiple keywords seperated by a space                                                    | allintitle:\<keywords>         | allintitle:dog cat                |
| allinurl         | Same as **inurl** but allows multiple keywords seperated by a space                                                      | allinurl:\<keywords>           | allinurl:search com               |
| allintext        | Same as **intext** but allows multiple keywords seperated by a space                                                     | allintext:\<keywords>          | allintext:math science university |
| AROUND           | Search for documents in which the first word is up to _n_ words away from the second word and vice versa                 | \<word1> AROUND(\<n>) \<word2> | google AROUND(10) good            |
| author           | Search for articles written by the given author if applicable                                                            | author:\<name>                 | author:Max                        |
| before           | Search for documents published / indexed before the given date                                                           | before:\<yy(-mm-dd)>           | before:2020-06-03                 |
| cache            | Search on the cached version of the given website. Uses Google's cache to do so                                          | cache:\<domain>                | cache:google.com                  |
| contains         | Search for documents that link to the given fileype. Not supported by Google                                             | contains:\<filetype>           | contains:pdf                      |
| date             | Search for documents published within the past _n_ months. Not supported by Google                                       | date:\<number>                 | date:3                            |
| define           | Search for the definition of the given word                                                                              | define:\<word>                 | define:funny                      |
| ext              | Search for a specific filetype                                                                                           | ext:\<documenttype>            | ext:pdf                           |
| filetype         | Refer to **ext**                                                                                                         | filetype:\<documenttype>       | filetype:pdf                      |
| inanchor         | Search for the given keyword in a website's anchors                                                                      | inanchor:\<keyword>            | inanchor:security                 |
| index of         | Search for documents containing direct downloads                                                                         | index of:\<term>               | index of:mp4 videos               |
| info             | Search for information about a website                                                                                   | info:\<domain>                 | info:google.com                   |
| intext           | Keyword needs to be in the text of the document                                                                          | intext:\<keyword>              | intext:news                       |
| intitle          | Keyword needs to be in the title of the document                                                                         | intitle:\<keyword>             | intitle:money                     |
| inurl            | Keyword needs to be in the URL of the document                                                                           | inurl:\<keyword>               | inurl:sheet                       |
| link / links     | Search for documents whose links contain the given keyword. Useful for finding documents that link to a specific website | link:\<keyword>                | link:google                       |
| location         | Show documents based on the given location                                                                               | location:\<location>           | location:USA                      |
| numrange         | Refer to **m..n**                                                                                                        | numrange:\<number>-\<number>   | numrange:1-100                    |
| OR               | Refer to **\|**                                                                                                          | \<operator> OR \<operator>     | "google" OR "yahoo"               |
| phonebook        | Search for related phone numbers associated with the given name                                                          | phonebook:\<name>              | phonebook:"william smith"         |
| relate / related | Search for documents that are related to the given website                                                               | relate:\<domain>               | relate:google.com                 |
| safesearch       | Exclude adult content such as pornographic videos                                                                        | safesearch:\<keyword>          | safesearch:sex                    |
| source           | Search on a specific news site. Rather use **site**                                                                      | source:\<news>                 | source:theguardian                |
| site             | Search on the given site. Given argument might also be just a TLD such as **com, net**, etc                              | site:\<domain>                 | site:google.com                   |
| stock            | Search for information about a market stock                                                                              | stock:\<stock>                 | stock:dax                         |
| weather          | Search for information about the weather of the given location                                                           | weather:\<location>            | weather:Miami                     |

\


## [FOCA](https://github.com/ElevenPaths/FOCA?tab=readme-ov-file) (Fingerprinting Organizations with Collected Archives)



<figure><img src="../../.gitbook/assets/image (184).png" alt=""><figcaption><p>FOCA</p></figcaption></figure>

### ✔️ Requisites

<figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

* **Cached and archival sites**
  * archive.org
  * Wayback script

{% code title="Wayback script" overflow="wrap" %}
```bash
git clone https://github.com/tomnomnom/waybackurls

cd waybackurls

go build main.go

mv main waybackurls

sudo cp waybackurls /usr/local/bin #run waybackurls in any PATH
```
{% endcode %}

* **Bing - Yahoo - Ask - Aol - Pandastats.net - Dogpile.com**



## Whois Enumeration&#x20;

* The owner of a domain name
* IP address or range
* Technical contacts
* Expiration data of the domain



<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

**For Example**

{% embed url="https://whois.domaintools.com" %}

<figure><img src="../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

```
whois <domain name>    
```

### amass & Sublister

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

* Classless inter-Domain Routing (CIDR)
  * Ex: 163.144.128.0/24
* An Autonomous Systen Number (ASN)
  * Regional internet Registeries (RIRs)  AFRINIC, aRIN, lACNIC
    * EX: 54115



* find a List ASN numbers
  * amass intel -org \<company name here>

```bash
amass intel -org <company name >
```

{% embed url="https://bgp.he.net" %}

```bash
curl -s http://ip-api.com/<ip>  | jq -r .as
```

<figure><img src="../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

**Subdomian**

* amass enum --active  -d \<domian >
* amass enum --passive  -d  \<domain>
* amass intel -asn \<asn number here>
* amass intel -cidr <0.0.0.0/15>
* amass intel -whois -d  \<domian>

OR Using asnmap Fast&#x20;

[**asnmap**](https://github.com/projectdiscovery/asnmap)\


<figure><img src="../../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

* **ASN to CIDR** Lookup
* **ORG to CIDR** Lookup
* **DNS to CIDR** Lookup
* **IP to CIDR** Lookup
* **ASN/DNS/IP/ORG** input
* **JSON/CSV/TEXT** output
* STD **IN/OUT** support

<table><thead><tr><th>Input</th><th>ASN</th><th width="150">DNS</th><th>IP</th><th>ORG</th></tr></thead><tbody><tr><td>Example</td><td><mark style="color:red;"><code>AS16509</code></mark></td><td><code>example.com</code></td><td><a href="https://bgp.he.net/ip/18.238.80.2">18.238.80.2</a> </td><td><mark style="color:red;"><code>Grab</code></mark></td></tr></tbody></table>

### Open-Source Code (1/2)

* Manual
  *   Github

      * "Company" password
      * "Company" secret&#x20;
      * "Company" cerdentials
      * "Company" tocken
      * "Company" config
      * "Company" key
      * "Company" pass
      * "Company"  login
      * "Company" ftp
      * "Company" ssh\_auth\_password
      * "Company" pwd
      * "Company"  security\_credentials #LDAB (AD)
      * "Company"  connectionstring       #Data base
      * "Company"  JDBC                            #Data base
      * "Company"  send\_key,send\_keys


* **Scripts**:
  * Gitrob or Gitleaks

### Shoden & censys.io

* any device connected to internet
  * server - router - iot devices
* Using Dorks&#x20;
  * hostname: uber.com
