# Module 10 ( Intro Buffer OverFlow)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
