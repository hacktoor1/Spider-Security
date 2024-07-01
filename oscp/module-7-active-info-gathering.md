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

      <figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Dnsrecon brute force&#x20;

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* DNSEnum => dnsenum \<domain name>
  *

      <figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>



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

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* Port Scanning  Wth nmap&#x20;
  * Accountability for Our Traffic
  * TCP Connect Scanning
  *



> How check FW With out any Soc team catch U
>
> use sudo command  beacuse can change low level Traffic
>
> Ex : sudo nmap 192.168.1.4 -p 25 -sT

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

```bash
sudo nmap 192.168.1.4 --top-ports=100 -sV -sC 
```

### **Masscan**

```
suoo masscan -p80,53 192.168.1.4 --rate=1000 --interface WlanX --router-mac <macaddress>
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### SMB Enumeration

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



### NFS Enumeration

RCP Protocol

in first using nmap to scan rpc default  run in port 111

```bash
nmap 192.168.1.4 -p 111 
```

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

OK i will try use NSE  rpcinfo

```bash
nmap 192.168.1.4 -p 111 --script rpcinfo
```

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

### RPCINFO Tool

rpcinfo tool use to information about  rpc

```
rpcinfo 192.168.1.4
```

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

step 2  using showmount tool

```
sudo showmount -e 192.168.1.4
```

<figure><img src="../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

**all Files and dir can pwd in my machine**

**to get all info how ?**

&#x20;

```bash
mkdir /tmp/meta 
sudo service rpcbind start
sudo mount -t nfs 192.168.1.4:/ /tmp/meta/

```

* SMTP Enumeration



* Scanning for the SMTP Service&#x20;
  * VRFY Users manual & auto
  * Nmap&#x20;
  * Metasploit

in first using nmap to scan SMTP default  run in port 25

```bash
nc -nvv 192.168.1.4 25
```

### smtp-user-enum

```bash
smtp-user-enum  -M VRFY -u root -t 192.168.1.4 -w 15
```

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

Using NMAP

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

### SNMP Enumeration

* Simple Network Management Protocol&#x20;
* Management information Baise (**MIB**) Object identifier  (**OID**)

#### MIB <a href="#mib" id="mib"></a>

To ensure that SNMP access works across manufacturers and with different client-server combinations, the **Management Information Base (MIB)** was created. MIB is an **independent format for storing device information**. A MIB is a **text** file in which all queryable **SNMP objects** of a device are listed in a **standardized** tree hierarchy. It contains at **least one `Object Identifier` (`OID`)**, which, in addition to the necessary **unique address** and a **name**, also provides information about the type, access rights, and a description of the respective object MIB files are written in the `Abstract Syntax Notation One` (`ASN.1`) based ASCII text format. The **MIBs do not contain data**, but they explain **where to find which information** and what it looks like, which returns values for the specific OID, or which data type is used.

#### What is OIDs ? <a href="#oids" id="oids"></a>

**Object Identifiers (OIDs)** play a crucial role. These unique identifiers are designed to manage objects within a **Management Information Base (MIB)**.



#### **OID Example** <a href="#oid-example" id="oid-example"></a>



### Basic Information <a href="#basic-information" id="basic-information"></a>

**SNMP - Simple Network Management Protocol** is a protocol used to monitor different devices in the network (like routers, switches, printers, IoTs...).

Copy

```
PORT    STATE SERVICE REASON                 VERSION
161/udp open  snmp    udp-response ttl 244   ciscoSystems SNMPv3 server (public)
```

{% hint style="success" %}
SNMP also uses port **162/UDP** for **traps**. These are data **packets sent from the SNMP server to the client without being explicitly requested**.
{% endhint %}

#### SNMP Versions <a href="#snmp-versions" id="snmp-versions"></a>

There are 2 important versions of SNMP:

* <mark style="color:blue;">**SNMPv1**</mark>: Main one, it is still the most frequent, the **authentication is based on a string** (community string) that travels in **plain text** (all the information travels in plain text). **Version 2 and 2c** send the **traffic in plain text** also and uses a **community string as authentication**.
* <mark style="color:blue;">**SNMPv3**</mark>: Uses a better **authentication** form and the information travels **encrypted** using (a **dictionary attack** could be performed but would be much harder to find the correct creds than in SNMPv1 and v2).

**step 1**&#x20;

go in windows machine turn on SNMP service  Turn on windows Feature&#x20;

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

run `.\services.msc`

```bash
sudo nmap  -sU -p 161,162 --script=snmp-* 192.168.1.7
```

<figure><img src="../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

161/udp <mark style="color:red;">open | filtered</mark> && 162/udp trap <mark style="color:red;">open | filtered</mark>



ok i will use **snmpbulkwalk**&#x20;

```bash
snmpbulkwalk -c Public -v2c 192.168.1.7
```

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>
