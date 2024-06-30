# Module 7 ( Active Info Gathering)

### DNS Enumeration

* What is DNS
* interacring with a DNS Tracffic
  * A :&#x20;
    * Maps a hostname ot an ip , "for worf" lookup/zone&#x20;

```bash
host -t A  grab.com 
```

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

* PTR :&#x20;
  * Maps an ip to a hostname , "reverse" lookup/zone

```bash
host -t PTR  52.84.66.32
```

* CNAME :&#x20;
  * Maps an alias hostname to an A record hostname

```
host -t CNAME  52.84.66.32
```

* MX :&#x20;
  * contain the names of the servers resposible for handling email for the domain&#x20;

```bash
host -t MX grab.com 
```

* Brute force Nslookup
  * host -t A \<hostname>
  *   can see admin.megacorpone.com but I will try to brute force hacktor.megacorpone.com&#x20;

      <figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

Make Script to brute force sub using bash Script :&#x20;

```bash
 for sub in $(cat /path/to/wordlist/dns.txt);do host -t A $sub.megacorpone.com | grep -v "not found" | grep "mega";done

```

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

And can Make Brute Force all (A,AAAA,PTR,MX)

*   DNS Zone Transfers

    * Full dump of the zone files.
    * host -i \<domain name> \<dns server address>


* Automate Tools
  * DNSRecon => `dnsrecon -d megacorpone.com -t axfr`
  *

      <figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Dnsrecon brute force&#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* DNSEnum => dnsenum \<domain name>
  *

      <figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>



* Other tools
  * fierce - DNSdumpster - Dnsmap - Metagoofil - foca - maltego - Dmitry - Recon-ng



### Port Scanning

* TCP / UDP Scanning
  * **TCP**
  * **UDP**
* Three way Handshake

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption><p>Three way Handshake</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

Ex :&#x20;

clint => nc nvlp 1234

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

Attcker => nc -nvv -w  192.168.1.1 1234-1236

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

