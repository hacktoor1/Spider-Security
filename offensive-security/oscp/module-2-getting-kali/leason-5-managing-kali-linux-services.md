# Leason 5 - Managing Kali Linux Services

## HTTP Service

<figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption><p>Http service</p></figcaption></figure>

and listens by default on port 80. To start the **HTTP** service in Kali, we can use systemctl as we did when starting the SSH service, replacing the service name with “apache2”:

```sh
kali@kali:~$ sudo systemctl start apache2
```

As with the SSH service, we can verify that the **HTTP** service is running and listening on TCP port **`80`** with the ss and grep commands.

```shell
kali@kali:~$ sudo ss -antlp | grep apache
LISTEN 0 128 :::80 :::*
users:(("apache2",pid=1481,fd=4),("apache2",pid=1480,fd=4),("apache2",pid=1479,fd=4),(
"apache2",pid=1478,fd=4),("apache2",pid=1477,fd=4),("apache2",pid=1476,fd=4),("apache2
",pid=1475,fd=4))
```

`systemctl` is a command-line utility in Unix-like operating systems, particularly in Linux distributions that use the systemd system and service manager. It is used to examine and control the systemd system and service manager. Here are some common uses and options for `systemctl`:

#### Basic Commands

*   **Start a Service**

    ```sh
    sudo systemctl start service_name**

    ```

    Starts the specified service immediately.
*   **Stop a Service**

    ```
    sudo systemctl stop service_name**

    ```

    Stops the specified service immediately.
*   **Restart a Service**

    ```
    sudo systemctl restart service_name**

    ```

    Stops and then starts the specified service.
*   **Reload a Service**

    ```
    sudo systemctl reload service_name**

    ```

    Reloads the configuration of the specified service without restarting it.
*   **Enable a Service**

    ```
    sudo systemctl enable service_name**

    ```

    Configures the service to start automatically at boot.
*   **Disable a Service**

    ```
    sudo systemctl disable service_name**

    ```

    Prevents the service from starting automatically at boot.
*   **Check the Status of a Service**

    ```
    sudo systemctl status service_name**

    ```

    Displays the current status of the specified service, including whether it is running, its PID, and recent log entries.
*   **List All Services**

    ```
    systemctl list-units --type=service**

    ```

    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0cf0a799-143b-40ad-bd72-1de5eb986a7f/8911e49b-b597-4cf5-9925-9b6bbc699ae9/Untitled.png)

#### Advanced Commands

*   **View All Units (Services, Mounts, Sockets, etc.)**

    ```
    systemctl list-units**

    ```
*   **View All Active Units**

    ```
    systemctl list-units --all**

    ```
*   **View Unit Files**

    ```
    systemctl list-unit-files**

    ```

    Lists unit files and their enabled/disabled state.
*   **Mask a Service**

    ```
    sudo systemctl mask service_name**

    ```

    Links the specified service to `/dev/null`, preventing it from being started manually or automatically.
*   **Unmask a Service**

    ```
    sudo systemctl unmask service_name**

    ```

    Removes the link created by `mask`, allowing the service to be started.
*   **Show Service Logs**

    ```
    #ournalctl - Print log entries from the systemd journal

    sudo journalctl -u service_name**

    ```

    Displays the logs for the specified service.

#### Examples

1.  **Start the Apache HTTP Server**

    ```
    sudo systemctl start apache2**

    ```
2.  **Enable MySQL to Start at Boot**

    ```
    sudo systemctl enable mysql**

    ```
3.  **Check the Status of the SSH Service**

    ```
    sudo systemctl status ssh**

    ```
4.  **List All Running Services**

    ```sh
    systemctl list-units --type=service --state=running**

    ```
5.  **Restart the Networking Service**

    ```sh
    sudo systemctl restart networking**

    ```

Using `systemctl`, you can manage and **monitor services**, **sockets**, **devices**, and other systemd units efficiently, giving you powerful control over the system's initialization and service management.

*   SSH Service

    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0cf0a799-143b-40ad-bd72-1de5eb986a7f/5f61acf8-651e-4800-955e-a378caad4034/Untitled.png)

    The Secure SHell (SSH)43 service is most commonly used to remotely access a computer, using a secure, encrypted protocol. The SSH service is TCP-based and listens by default on port 22. To start the SSH service in Kali, we run systemctl with the start option followed by the service name (ssh in this example)

    ```markdown
    kali@kali:~$ sudo systemctl start ssh
    ```

    When the command completes successfully, it does not return any output but we can verify that the SSH service is running and listening on TCP port 22 by using the ss command and piping the output into grep to search the output for “sshd”

    ```sh
    kali@kali:~$ sudo ss -antlp | grep sshd
    LISTEN 0 128 *:22 *:* users:(("sshd",pid=1343,fd=3))
    LISTEN 0 128 :::22 :::* users:(("sshd",pid=1343,fd=4))
    ```

    he command `sudo ss -anple` is used in Unix-like operating systems to display detailed information about socket connections. Here's a breakdown of what each part of the command does:

    * `sudo`: This command runs the following command with superuser (root) privileges. It's required because accessing detailed information about network connections often requires elevated permissions.
    * `ss`: The `ss` command is used to dump socket statistics. It is a modern replacement for the older `netstat` command.
    * `a`: This option tells `ss` to show all sockets, including listening (waiting for incoming connections) and non-listening (connected) sockets.
    * `n`: This option ensures that the addresses are not resolved into hostnames. It shows numerical addresses instead, which can be quicker and avoids potential issues with DNS resolution.
    * `p`: This option includes the process information for each socket, showing which process is using each socket.
    * `l`: This option restricts the output to listening sockets only.
    * `e`: This option displays extended socket information, which may include additional details like memory usage or TCP socket flags.

    If we want to have the SSH service start automatically at boot time (as many users prefer), we simply enable it using the systemctl command. However, be sure to change the default password first!

***

**SSH Service**

<figure><img src="../../../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

The Secure SHell (SSH)43 service is most commonly used to remotely access a computer, using a secure, encrypted protocol. The SSH service is TCP-based and listens by default on port 22. To start the SSH service in Kali, we run systemctl with the start option followed by the service name (ssh in this example)

```sh
kali@kali:~$ sudo systemctl start ssh
```

When the command completes successfully, it does not return any output but we can verify that the SSH service is running and listening on TCP port 22 by using the ss command and piping the output into grep to search the output for “sshd”

```markdown
kali@kali:~$ sudo ss -antlp | grep sshd
LISTEN 0 128 *:22 *:* users:(("sshd",pid=1343,fd=3))
LISTEN 0 128 :::22 :::* users:(("sshd",pid=1343,fd=4))
```

he command `sudo ss -anple` is used in Unix-like operating systems to display detailed information about socket connections. Here's a breakdown of what each part of the command does:

* `sudo`: This command runs the following command with superuser (root) privileges. It's required because accessing detailed information about network connections often requires elevated permissions.
* `ss`: The `ss` command is used to dump socket statistics. It is a modern replacement for the older `netstat` command.
* `a`: This option tells `ss` to show all sockets, including listening (waiting for incoming connections) and non-listening (connected) sockets.
* `n`: This option ensures that the addresses are not resolved into hostnames. It shows numerical addresses instead, which can be quicker and avoids potential issues with DNS resolution.
* `p`: This option includes the process information for each socket, showing which process is using each socket.
* `l`: This option restricts the output to listening sockets only.
* `e`: This option displays extended socket information, which may include additional details like memory usage or TCP socket flags.

If we want to have the SSH service start automatically at boot time (as many users prefer), we simply enable it using the systemctl command. However, be sure to change the default password first!
