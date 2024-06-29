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

* DNS Zone Transfers
  * Full dump of the zone files.
  * host -i \<domain name> \<dns server address>
