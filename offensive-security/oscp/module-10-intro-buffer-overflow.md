# Module 10 ( Intro Buffer OverFlow)

<figure><img src="../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

## Architecture Fundamentals (Numbers)



| Number System | Base | Used digits                     |
| ------------- | ---- | ------------------------------- |
| Binary        | 2    | 0,1                             |
| Octal         | 8    | 0,1,2,3,4,5,6,7                 |
| Decimal       | 10   | 0,1,2,3,4,5,6,7,8,9             |
| Hexadecimal   | 16   | 0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F |

EX

| Binary |    | Hexadecimal |
| ------ | -- | ----------- |
| 0000   | 0  | 0x0         |
| 0001   | 1  | 0x1         |
| 0010   | 2  | 0x2         |
| 0011   | 3  | 0x3         |
| 0100   | 4  | 0x4         |
| 0101   | 5  | 0x5         |
| 0110   | 6  | 0x6         |
| 0111   | 7  | 0x7         |
| 11111  | 15 | 0xf         |

```
bit    => 0 OR 1
byte   => 000001010 => 0x0A               ===> 0 to 7
H-Word => 000000000000000 => 0x0000       ===> 0 to 15
Word   => 0000000000000000000000000000000 ===> 0 to 31

```



## BUFFER OVERFLOW <a href="#a79b" id="a79b"></a>

{% hint style="warning" %}
**A buffer overflow occurs when the size of data exceeds the storage capacity of the memory buffer**
{% endhint %}

### WHAT ARE BUFFERS? <a href="#id-6ef2" id="id-6ef2"></a>

Buffers are memory storage regions that temporarily hold data while it is being transferred from one location to another

### CAUSE  <a href="#id-87d5" id="id-87d5"></a>

Buffer overflow is triggered by user input

In the case of buffer overflow vulnerabilities, the developer must check the input length before using any functions that might cause an overflow to happen

These attacks are caused by vulnerable functions in C

The following five common unsafe functions that can lead to a buffer overflow vulnerability:

```c
printf, sprintf, strcat, strcpy, and gets.
```

### MEMORY LAYOUT <a href="#a79b" id="a79b"></a>

\
[\
](https://aidenpearce369.medium.com/?source=post\_page-----670765bf405a--------------------------------)

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

The buffer space grows towards the Base Pointer (BP) and Instruction Pointer (IP) from lower memory to higher memory

\


<figure><img src="../../.gitbook/assets/image (336).png" alt=""><figcaption></figcaption></figure>

Below Base Pointer (BP) there will be Instruction Pointer (IP)/Return Address

The stack components of the program are always stored above the Base Pointer (BP)



### Intro to BOF

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

### Buffer Oveflow&#x20;

Ex  Code&#x20;

```c
#include <stdio.h>
int main(int argc,char  *argv[]){
    Buffer[8];
    strcpy(Buffer, argv[1]);
    return 0;
}
```

* Step1: open Immuntiy Debbuger&#x20;

<figure><img src="../../.gitbook/assets/image (335).png" alt=""><figcaption></figcaption></figure>

* Step2 run the app
* Step3&#x20;

Main Code&#x20;

```asm6502
00401500  /$ 55             PUSH EBP
00401501  |. 89E5           MOV EBP,ESP
00401503  |. 83E4 F0        AND ESP,FFFFFFF0
00401506  |. 83EC 20        SUB ESP,20

00401509  |. E8 72090000    CALL Buffer.00401E80
0040150E  |. 8B45 0C        MOV EAX,DWORD PTR SS:[EBP+C]             
00401511  |. 83C0 04        ADD EAX,4                               
00401514  |. 8B00           MOV EAX,DWORD PTR DS:[EAX]              
00401516  |. 894424 04      MOV DWORD PTR SS:[ESP+4],EAX            
0040151A  |. 8D4424 18      LEA EAX,DWORD PTR SS:[ESP+18]            
0040151E  |. 890424         MOV DWORD PTR SS:[ESP],EAX
               
00401521  |. E8 D2100000    CALL <JMP.&msvcrt.strcpy>   #strcpy           
00401526  |. B8 00000000    MOV EAX,0
0040152B  |. C9             LEAVE
0040152C  \. C3             RETN
```

### Register

```asm6502
Instruaction pointer  EIP = 
Stack Pointer         ESP = 
Base  Pointer         EBP =
```











Security Implementations

* What is the vulnerable Functions?
  * gets
  * scanf
  * sprintf
  * Strcpy
* what is the security implementations ?
  * ASLR , Dep Canary...
* How to bypass it ?
  * SHE, ret to libc , etc...
