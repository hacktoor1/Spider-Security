# Leason 4 - Finding Your Way Around Kali

* **Which**

The which command39 searches through the directories that are defined in the $PATH environment variable for a given file name

{% code overflow="wrap" lineNumbers="true" fullWidth="false" %}
```sh
kali@kali:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
kali@kali:~$ which sbd
/usr/bin/sbd
```
{% endcode %}

*   **find**

    The find command41 is the most complex and flexible search tool among the three. Mastering its syntax can sometimes be tricky, but its capabilities go beyond a normal file search. The most basic usage of the find command is shown in Listing 14, where we perform a recursive search starting from the root file system directory and look for any file that starts with the letters “sbd”

{% code lineNumbers="true" %}
```sh
kali@kali:~$ sudo find / -name sbd*
/usr/bin/sbd
/usr/share/doc/sbd
/usr/share/windows-resources/sbd
/usr/share/windows-resources/sbd/sbd.exe
/usr/share/windows-resources/sbd/sbdbg.exe
/var/cache/apt/archives/sbd_1.37-1kali3_amd64.deb
/var/lib/dpkg/info/sbd.md5sums
/var/lib/dpkg/info/sbd.list
```
{% endcode %}



*   **locate**

    The locate command40 is the quickest way to find the locations of files and directories in Kali. In order to provide a much shorter search time, locate searches a built-in database named locate.db rather than the entire hard disk itself. This database is automatically updated on a regular basis by the cron scheduler. To manually update the locate.db database, you can use the updatedb command.

{% code lineNumbers="true" %}
```sh
kali@kali:~$ sudo updatedb
kali@kali:~$ locate sbd.exe
/usr/share/windows-resources/sbd/sbd.exe
```
{% endcode %}
