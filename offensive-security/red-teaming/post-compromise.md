# Post Compromise

### Internal Networks

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### A Demilitarized Zone (DMZ)

{% hint style="info" %}
A DMZ Network is an edge network that protects and adds an extra security layer to a corporation's internal local-area network from untrusted traffic. A common design for DMZ is a subnetwork that sits between the public internet and internal networks.\


Designing a network within the company depends on its requirements and need. For example, suppose a company provides public services such as a website, DNS, FTP, Proxy, VPN, etc. In that case, they may design a DMZ network to isolate and enable access control on the public network traffic, untrusted traffic
{% endhint %}

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### Network Enumeration

```bash
netstat -na
```

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

### What is the Active Directory (AD) environment?

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

More details here :

{% embed url="https://h3ckt0r.gitbook.io/0xsec/offensive-security/oscp/module-21-active-directory-attacks" %}
AD&#x20;
{% endembed %}

The following is a list of Active Directory components that we need to be familiar with:

* Domain Controllers
* Organizational Units
* AD objects
* AD Domains
* Forest
* AD Service Accounts: Built-in local users, Domain users, Managed service accounts
* Domain Administrators

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

AD domains are a collection of Microsoft components within an AD network.

### AD Forest&#x20;

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

## Host Security Solution&#x20;

It is a set of software applications used to monitor and detect abnormal and malicious activities within the host, including:

1. Antivirus software
2. Microsoft Windows Defender
3. Host-based Firewall
4. Security Event Logging and Monitoring&#x20;
5. Host-based Intrusion Detection System (HIDS)/ Host-based Intrusion Prevention System (HIPS)
6. Endpoint Detection and Response (EDR)

### Antivirus Software (AV)

There are various detection techniques that the antivirus uses, including

* Signature-based detection
* Heuristic-based detection

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

### Heuristic VS Signature

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

* Behavior-based detection

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

### Microsoft Windows Defender

We can use the following PowerShell command to check the service state of Windows Defender:

```powershell
Get-Service WinDefend
```

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

```powershell
 Get-MpComputerStatus | select RealTimeProtectionEnabled
```

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Host-based Firewall:

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption><p>FW</p></figcaption></figure>

```powershell
Get-NetFirewallProfile | Format-Table Name, Enabled
```

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

```powershell
Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False
Get-NetFirewallProfile | Format-Table Name, Enabled
```

We can also learn and check the current Firewall rules, whether allowing or denying by the firewall.

```powershell
Get-NetFirewallRule | select DisplayName, Enabled, Description
```

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 80
(New-Object System.Net.Sockets.TcpClient("127.0.0.1", "80")).Connected
```

threat details

```powershell
Get-MpThreat
```

Get Firewall Rules connection

```
 Get-NetFirewallRule | findstr "THM-Connection"
```

### Security Event Logging and Monitoring

Event logs in Windows&#x20;

```
Get-eventlog -list
```

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Eventlogs</p></figcaption></figure>

