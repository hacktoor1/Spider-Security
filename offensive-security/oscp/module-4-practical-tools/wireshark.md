# Wireshark



Examine Layers captured by Wireshark

> Basically, when a user opens an application for sending or receiving Data then he directly interacts with the application layer for both operations either sending or receiving of data. For example, we act as a client when use Http protocol for uploading or Downloading a Game; FTP for downloading a File; SSH for accessing the shell of the remote system. While connecting with any application for sharing data between server and client we make use of Wireshark for capturing the flow of network traffic stream to examine the OSI model theory through captured traffic. From given below image you can observe that Wireshark has captured the traffic of four layers in direction of the source (sender) to destination (receiver) network



> Here it has successfully captured Layer 2 > Layer 3 > Layer 4 and then Layer 7 information.

<figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure>

Ethernet Header (Data Link)

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>

TCP Header (Transport Layer)

> **`TCP`** follows **`Three-Way-Handshakes`** as describe below
>
> &#x20;• A client sends a TCP packet to the server with the SYN flag&#x20;
>
> &#x20;• A server responds to the client request with the SYN and ACK flags set.
>
> &#x20;• Client completes the connection by sending a packet with the ACK flag set

Different Types of TCP flags

<figure><img src="../../../.gitbook/assets/image (254).png" alt=""><figcaption></figcaption></figure>

### Password Sniffing

> **Because clear text protocols do not encrypt communication, all data, including passwords, is visible to the naked eye. Anyone who is in a position to see the communication (for example, a man in the middle) can eventually see everything**.

**1. Capture HTTP Password**

* &#x20;Hypertext Transfer Protocol (HTTP). It usually works on port 80/TCP

<figure><img src="../../../.gitbook/assets/image (255).png" alt=""><figcaption></figcaption></figure>

> <mark style="color:red;">**Capture HTTPS Password need a RSA Key To Dencrypt Application Data over TLS or SSL**</mark>

<figure><img src="../../../.gitbook/assets/image (256).png" alt=""><figcaption></figcaption></figure>

