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

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

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

Attcker => `nc -nvv -w  192.168.1.1 1234-1236`

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

```bash
sudo  iptables -nvL

sudo  iptables -z

#rule
#source
sudo iptables -I INPUT 1 -s 192.168.1.8 -j ACCEPT
#clint 
sudo iptables -I OUTPUT 1 -d 192.168.1.8 -j ACCEPT  
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* Port Scanning  Wth nmap&#x20;
  * Accountability for Our Traffic
  * TCP Connect Scanning
  *



> How check FW With out any Soc team catch U
>
> use sudo command  beacuse can change low level Traffic
>
> Ex : sudo nmap 192.168.1.4 -p 25 -sT

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

> `-sT` => Connect Scanning
>
> `-sS` => SYN Scanning
>
> `-sA` => ACK Scanning
>
> `-sF`  => Fen Scanning

UDP Scanning

```bash
sudo nmap 192.168.1.4 -sU -p 
```



### Nmap SEN

```
nmap -sn 192.168.1.1/24 -v 
```

### **OS fingerprinting**

```bash
nmap 192.168.1.4 -O
```



**Banner Grabbing/Service Enumeration**

```
nmap nmap 192.168.1.4 -sV
```

`-sV` => Service Scanning

### **Nmap Scripting Engine (NSE)**

```bash
sudo nmap --script=/usr/share/nmap/scripts/dns-zone-transfer.nse ns2.megacorpone.com  -p 53
```

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```bash
sudo nmap 192.168.1.4 --top-ports=100 -sV -sC 
```

### **Masscan**

```
suoo masscan -p80,53 192.168.1.4 --rate=1000 --interface WlanX --router-mac <macaddress>
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### SMB Enumeration == eternal blue

### nbtscan&#x20;

make scanning to show **NetBOIS**&#x20;

```bash
sudo nbtscan -r 192.168.1.1/24
```

How can show file use -v => verbose

```bash
sudo nbtscan -r 192.168.1.1/24 -v
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### smbclinet

{% hint style="danger" %}
With Passweord Protected ON
{% endhint %}

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
With Passweord Protected OFF
{% endhint %}

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### SMPMAP

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### enum4linux

```bash
sudo enum4linux 192.168.1.4
```



Nmap NSE Secipting SMB

```bash
sudo namp  192.168.1.6 --sV -P T:139,445 U:137 --script="smb-enum-*"
```

```bash
sudo namp  192.168.1.6 --sV -P T:139,445 U:137 --script="smb-vuln-*" 
```

```bash
sudo namp  192.168.1.6 --sV -P T:139,445 U:137 --script="dns-nsec-enum.nse" 
```
