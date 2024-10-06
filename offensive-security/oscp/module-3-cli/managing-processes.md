# Managing Processes



### How To  Works Processes in Linux ?

<figure><img src="../../../.gitbook/assets/image (198).png" alt=""><figcaption><p>Procecces Worls</p></figcaption></figure>

### Stages of a Process in Linux <a href="#id00" id="id00"></a>

In Linux, a process goes through several stages during its lifetime. Understanding these stages and checking how they run in the background is important for process management and troubleshooting. The states of a process in Linux are as follows:

1. **Created:** A process is created when a program is executed. At this stage, the process is in a "created" state, and its data structures are initialized.
2. **Ready:** The process enters the "ready" state when it is waiting to be assigned to a processor by the Linux scheduler. At this stage, the process is waiting for its turn to execute.
3. **Running:** The process enters the "running" state when it is assigned to a processor and is actively executing its instructions.
4. **Waiting:** The process enters the "waiting" state when it is waiting for some event to occur, such as input/output completion, a signal, or a timer. At this stage, the process is not actively executing its instructions.
5. **Terminated:** The process enters the "terminated" state when it has completed its execution or has been terminated by a signal. At this stage, the process data structures are removed, and its resources are freed.
6. **Zombie:** A process enters the "zombie" state when it has completed its execution but its parent process has not yet read its exit status. At this stage, the process details still have an entry in the process table, but it does not execute any instructions. The zombie process is removed from the process table when its parent process reads its exit status.

<figure><img src="../../../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

### pstree <a href="#types-of-linux-processes" id="types-of-linux-processes"></a>

```
pstree -p -s PID
```

### Types of Linux Processes <a href="#types-of-linux-processes" id="types-of-linux-processes"></a>

Processes are classified into 2 types in Linux Distributions:

1. Foreground Processes (FG)
2. Background Processes (Non-Interactive Processes):

### Commands Used to Manage Processes in Linux <a href="#id40" id="id40"></a>

There are several [commands](https://unstop.com/blog/linux-commands) used in process management in Linux. Here are some of the most commonly used commands:



1. **ps:** This command is used to display information about running processes. The "ps" command can be used to list all processes or filter the list based on various criteria, such as the user who started the process, the process ID (PID), and the process status.   &#x20;

```bash
➜  ~ ps 
    PID TTY          TIME CMD
 209975 pts/1    00:00:00 zsh
 212977 pts/1    00:00:00 ps

```

2. **top:** This command is used to display a real-time view of system processes. The "top" command provides information about the processes running on the system, including their resource usages, such as CPU and memory.
3. **kill:** This command is used to [terminate a process](https://unstop.com/blog/kill-process-linux). The "kill" command can be used with the process ID (PID) of the process or with a signal number to request a specific action.
4. **nice:** This command is used to adjust the priority of a process. Higher-priority processes get more CPU time than lower-priority processes. The "nice" command can be used to increase or decrease the priority of a process, which affects its CPU usage.
5. **renice:** This command is used to change or adjust the priority of a running process, which affects its CPU usage.
6. **pkill:** This command is used to send a signal to a process to request it to terminate. The "pkill" command can be used with a current process name or a regular expression to match multiple processes.
7. **top:** This command is used to display a real-time view of system processes. The "top" command provides information about the processes running on the system, including their resource usages, such as CPU and memory.
8. **jobs:** This command is used to display a list of background jobs running in the current shell session.
9. **fg:** This command is used to move a background process to the foreground. The "fg" command can be used with the job ID of the background process.
10. **bg:** This command is used to move a suspended process to the background. The "bg" command can be used with the job ID of the suspended process.

The built-in jobs utility lists the jobs that are running in the current terminal session, while `fg` returns a job to the foreground. These commands are shown in action below:

<figure><img src="../../../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

```bash
ping -c 400 ::1 > ping_results.txt
^Z
zsh: suspended  ping -c 400 ::1 > ping_results.txt
```

```bash
jobs 
[1]    running    sleep 90000
[2]  - suspended  ping -c 400 ::1
[3]  + suspended  ping -c 400 ::1 > ping_results.txt

```

FG

<pre class="language-bash"><code class="lang-bash"><strong>fg %1
</strong></code></pre>

In Unix-like operating systems, backgrounding a process refers to running a process in the background, allowing the user to continue using the terminal for other tasks while the background process continues to run. This can be particularly useful for long-running tasks. Here’s a quick guide on how to manage background processes using the `bg` command and other related commands.

#### _Starting a Process in the Background_

When you start a process, you can run it in the background by appending an ampersand (`&`) to the command. For example:

```sh
long_running_command &
```

#### Stopping and Backgrounding a Running Process

If you have already started a process in the foreground and want to move it to the background, you can do so using the following steps:

1. **Suspend the process**: Press `Ctrl+Z` to stop (pause) the running process and put it into the background in a suspended state.
2.  **Background the process**: Use the `bg` command to resume the process in the background.

    ```sh
    bg
    ```

#### Listing Background Jobs

To see a list of all background jobs, use the `jobs` command:

```sh
jobs
```

This will show a list of jobs along with their job IDs, states, and the commands that started them.

#### Bringing a Background Job to the Foreground

If you need to bring a background job back to the foreground, use the `fg` **command** followed by the job ID. For example:

```sh
fg %1
```

This command will bring job number 1 to the foreground.

#### Sending a Running Process to the Background

If you want to send a currently running process to the background without suspending it first, you can start the process in the background using the ampersand (`&`) as shown above. However, for processes that are already running in the foreground, you must first suspend them using `Ctrl+Z`, and then background them using `bg`.

#### Example Workflow

Here is an example workflow that demonstrates these commands:

1.  Start a process in the foreground:

    ```sh
    sleep 100
    ```
2.  Suspend the process:

    ```sh
    [Ctrl+Z]
    ```

    Output will be similar to:

    ```
    [1]+  Stopped                 sleep 100
    ```
3.  Move the suspended process to the background:

    ```sh
    bg
    ```

    Output will be:

    ```
    [1]+ sleep 100 &
    ```
4.  Check the background jobs:

    ```sh
    jobs
    ```

    Output will be similar to:

    ```
    [1]+  Running                 sleep 100 &
    ```
5.  Bring the background process to the foreground:

    ```sh
    fg %1
    ```

    This brings job number 1 back to the foreground.

#### Summary

* **Start a process in the background**: `command &`
* **Suspend a running process**: `Ctrl+Z`
* **Resume a suspended process in the background**: `bg`
* **List background jobs**: `jobs`
* **Bring a background job to the foreground**: `fg %job_id`

Understanding how to manage background processes can significantly improve your productivity when working with the command line in Unix-like operating systems.

