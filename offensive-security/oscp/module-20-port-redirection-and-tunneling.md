# Module 20 (Port Redirection and Tunneling)

The concept behind Port Forwarding&#x20;

### Port Redirection <a href="#id-7fa2" id="id-7fa2"></a>

* Port redirection is the process of redirecting traffic from one port on a local machine to another port on the same machine or on a remote machine.
* For example, you could use port redirection to redirect traffic from port 80 (the HTTP port) to port 8080 (a non-standard port).
* This would allow you to access a web application that is running on port 8080 from a remote machine, even if the firewall on the remote machine blocks traffic to port 8080.
* Port redirection can be implemented using the built-in features of a firewall or router.
* For example, many firewalls and routers allow you to create port-forwarding rules that redirect traffic from one port to another. You can also use a third-party software application to implement port redirection.
* For example, the PuTTY SSH client includes a port forwarding feature that allows you to redirect traffic from a local port to a remote port.

### Tunnelling <a href="#d7aa" id="d7aa"></a>

Tunnelling is the process of encapsulating data from one network protocol within another network protocol. For example, you could use tunnelling to encapsulate TCP traffic within UDP traffic. This would allow you to bypass a firewall that blocks TCP traffic.

Tunneling can be implemented using a variety of protocols, including:

* Secure Shell (SSH)
* Point-to-Point Tunneling Protocol (PPTP)
* Layer 2 Tunneling Protocol (L2TP)
* Virtual Private Network (VPN)

SSH tunnelling is a popular tunneling technique that is often used to securely connect to remote machines. SSH tunnelling can be used to access remote applications, connect to a remote network, or encrypt traffic between two machines.

PPTP and L2TP are tunneling protocols that are often used to create virtual private networks (VPNs). VPNs allow you to create a secure connection between your computer and a remote network, even if you are connected to the internet through an untrusted network.

<figure><img src="../../.gitbook/assets/image (371).png" alt=""><figcaption></figcaption></figure>

Attack Simulation

<figure><img src="../../.gitbook/assets/image (372).png" alt=""><figcaption></figcaption></figure>

* SSH Tunneling&#x20;
  * SSH Remote Port Forwarding
  * SSH Dynamic Port Forwarding
  * SSH Local Port Forwarding

### SSH Local Port Forwarding

```bash
ssh -N -L [bind_address:]port:host:hostport [username@address]
sudo ssh -L 127.0.0.1:80:192.168.1.19:80 victim@192.168.1.10
```

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

### SSH Remote Port Forwarding (reverse tunneling)

<pre class="language-bash"><code class="lang-bash">ssh -N -R [bind_address:]port:host:hostport [username@address]
<strong>sudo ssh -R  8080:127.0.0.1:80 attacker@192.168.1.8 
</strong></code></pre>

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

### SSH Dynamic Port Forwarding

```bash
ssh -D 8080 vps@112.125.1.54
```

<figure><img src="../../.gitbook/assets/image (373).png" alt=""><figcaption></figcaption></figure>

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
