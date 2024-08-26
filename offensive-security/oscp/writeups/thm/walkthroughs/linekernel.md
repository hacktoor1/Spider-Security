# LineKernel

### Recon

python version&#x20;

```
python -V
```

<figure><img src="../../../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

```
cat /etc/issuse
```

<figure><img src="../../../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

CVE&#x20;

<figure><img src="../../../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

###

### initial Access

### privEsc

### sudo&#x20;

<figure><img src="../../../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

password  hash





<figure><img src="../../../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

unshasow

<figure><img src="../../../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

```bash
base64 "$LFILE" | base64 --decode
```

<figure><img src="../../../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

capabilities

<figure><img src="../../../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

crontab

```
unshadow pass.txt shadow.txt > passshadow.txt
john passshadow.txt
```

<figure><img src="../../../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>



Privilege Escalation: PATH

```
export PATH=/tmp:$PATH
```

<figure><img src="../../../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Privilege Escalation: NFS

<figure><img src="../../../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Exploit Resources





### Tools Resources

