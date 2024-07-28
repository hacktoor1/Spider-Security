# File and Command Monitoring

* **tail**

The most common use of tail75 is to monitor log file entries as they are being written. For example, we may want to monitor the Apache logs to see if a web server is being contacted by a given client we are attempting to attack via a client-side exploit. This example will do just that:

```bash
kali@kali:~$ sudo tail -f /var/log/apache2/access.log
127.0.0.1 - - [02/Feb/2018:12:18:14 -0500] "GET / HTTP/1.1" 200 3380 "-" "Mozilla/5.0
(X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0"
127.0.0.1 - - [02/Feb/2018:12:18:14 -0500] "GET /icons/openlogo-75.png HTTP/1.1" 200
6040 "http://127.0.0.1/" "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101
Firefox/52.0"
127.0.0.1 - - [02/Feb/2018:12:18:15 -0500] "GET /favicon.ico HTTP/1.1" 4
```

The -`f` option (follow) is very useful as it continuously updates the output as the target file grows. Another convenient switch is -`nX`, which outputs the last “X” number of lines, instead of the default value of 10.

* **watch**&#x20;

```sh
watch [options] command
```

```bash
watch -n Time/s -d command
watch -n 0,5 -d free

```

#### Common Options

* `-n seconds`: Specify the interval in seconds between executions of the command. The default is 2 seconds.
* `-d`: Highlight the differences between successive updates.
* `-t`: Turn off the header showing the interval, command, and current time.
* `-x`: Treat arguments as a single command to be executed.



* Wget

```bash
wget link
```

* **curl**
* **axel**

```
axel -n 
```

