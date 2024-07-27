# Module 20 (Port Redirection and Tunneling)

The concept behind Port Forwarding&#x20;

<figure><img src="../.gitbook/assets/image (173).png" alt=""><figcaption></figcaption></figure>

Attack Simulation

<figure><img src="../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

* SSH Tunneling&#x20;
  * SSH Remote Port Forwarding
  * SSH Dynamic Port Forwarding
  * SSH Local Port Forwarding

### SSH Local Port Forwarding

```bash
sudo ssh -L 127.0.0.1:80:192.168.1.19:80 victim@192.168.1.10
```

### SSH Remote Port Forwarding

```bash
sudo ssh -R  8080:127.0.0.1:80 attacker@192.168.1.8 
```

### SSH Dynamic Port Forwarding

```bash
ssh -D 8080 vps@112.125.1.54
```

<figure><img src="../.gitbook/assets/image (175).png" alt=""><figcaption></figcaption></figure>

### **Bash** <a href="#bash" id="bash"></a>

```bash
# On the jump server connect the port 3333 to the 5985
mknod backpipe p;
nc -nvlp 1111 0>backpip  | nc -nv <ip> 22 1>backpip

# On InternalA accessible from Jump and can access InternalB
## Expose port 3333 and connect it to the winrm port of InternalB
exec 3<>/dev/tcp/internalB/5985
exec 4<>/dev/tcp/Jump/3333
cat <&3 >&4 &
cat <&4 >&3 &

# From the host, you can now access InternalB from the Jump server
evil-winrm -u username -i Jump
```

### Resources <a href="#bash" id="bash"></a>

{% embed url="https://book.hacktricks.xyz/generic-methodologies-and-resources/tunneling-and-port-forwarding" %}
