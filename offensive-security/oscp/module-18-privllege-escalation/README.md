# Module 18 (Privllege Escalation)

## How to escalate your privleges?

*   Manual Enumeration

    * Enumeration Users
      * whoami - id - net user (win)
    * Enumeration Hostname
      * Hostname
      * Systeminfo
      * uname -a and /etc/issue, /etc/\*-releaseh
    * Enumeration Running Procceses and services
      * tasklist /svc => Windows
      * ps aux => linux
    * Enumeration Network&#x20;

    <figure><img src="../../../.gitbook/assets/image (351).png" alt=""><figcaption><p>open ports</p></figcaption></figure>



```bash
#linux
route
ifconfig - ip a
/sbin/route
netstat -ano
netstat -anp
ss -anp

#windows
ipconfig
netstat -ano
netstat -anp
```

* Enumeration Firewall Status and Reules
  * netsh advfirewall show currentprofile
  * netsh advfirewall firewall show rule name=all
* Enumeration scheduled Tasks
  * schtasks /query /fo LIST /v
  * ls -lah /etc/cron\*

```
/bin/sh -p
```

Enumeration installed applicatins and Patch levels

* wmic product get name, version. vendor
* wmic qfe get Caption, Description, HotFixID, installedOn
* dpkg -l

windows

```powershell
Get-ChildItem "C:\Program Files" -R ecurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}
```

Linux

```purebasic
find / -writable -type d 2>/dev/null
```

* Enumeration Readable/Writable Files and Directories
* Enumeration Unmounted Disks

```bash
#sindows
mountvol
#linux 
mount
```

* Enumeration Device Drivers and Kerenl Modules

```powershell
driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object ‘Display Name’, ‘Start Mode’, Path
Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}

```

Linux

```bash
lsmod
/sbin/modinfo <modualinfo>
```

