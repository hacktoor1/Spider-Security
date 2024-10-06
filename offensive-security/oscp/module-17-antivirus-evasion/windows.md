# Windows

## Automated Enumeration

* windows-privesc-check
* waston
* sherlock
* powerSpliot/privesc/powerUp
* windows-Exploit0Suggester
* JAWS
* WinPEAS&#x20;
* LinPEAS & linEnum

## Windows Privilege Escalation

| Code   | Technique                                                                                                          | Mitre                           |
| ------ | ------------------------------------------------------------------------------------------------------------------ | ------------------------------- |
| WPE-01 | [Stored Credentials](https://pentestlab.blog/2017/04/19/stored-credentials/)                                       | [NA](https://attack.mitre.org/) |
| WPE-02 | [Windows Kernel](https://pentestlab.blog/2017/04/24/windows-kernel-exploits/)                                      | [NA](https://attack.mitre.org/) |
| WPE-03 | [DLL Injection](https://pentestlab.blog/2017/04/04/dll-injection/)                                                 | [NA](https://attack.mitre.org/) |
| WPE-04 | [Weak Service Permissions](https://pentestlab.blog/2017/03/30/weak-service-permissions/)                           | [NA](https://attack.mitre.org/) |
| WPE-05 | [DLL Hijacking](https://pentestlab.blog/2017/03/27/dll-hijacking/)                                                 | [NA](https://attack.mitre.org/) |
| WPE-06 | [Hot Potato](https://pentestlab.blog/2017/04/13/hot-potato/)                                                       | [NA](https://attack.mitre.org/) |
| WPE-07 | [Group Policy Preferences](https://pentestlab.blog/2017/03/20/group-policy-preferences/)                           | [NA](https://attack.mitre.org/) |
| WPE-08 | [Unquoted Service Path](https://pentestlab.blog/2017/03/09/unquoted-service-path/)                                 | [NA](https://attack.mitre.org/) |
| WPE-09 | [Always Install Elevated](https://pentestlab.blog/2017/02/28/always-install-elevated/)                             | [NA](https://attack.mitre.org/) |
| WPE-10 | [Token Manipulation](https://pentestlab.blog/2017/04/03/token-manipulation/)                                       | [NA](https://attack.mitre.org/) |
| WPE-11 | [Secondary Logon Handle](https://pentestlab.blog/2017/04/07/secondary-logon-handle/)                               | [NA](https://attack.mitre.org/) |
| WPE-12 | [Insecure Registry Permissions](https://pentestlab.blog/2017/03/31/insecure-registry-permissions/)                 | [NA](https://attack.mitre.org/) |
| WPE-13 | [Intel SYSRET](https://pentestlab.blog/2017/06/14/intel-sysret/)                                                   | [NA](https://attack.mitre.org/) |
| WPE-14 | [Print Spooler](https://pentestlab.blog/2021/08/02/universal-privilege-escalation-and-persistence-printer/)        | [NA](https://attack.mitre.org/) |
| WPE-15 | [HiveNightmare](https://pentestlab.blog/2021/08/16/hivenightmare/)                                                 | [NA](https://attack.mitre.org/) |
| WPE-16 | [Resource Based Constrained Delegation](https://pentestlab.blog/2021/10/18/resource-based-constrained-delegation/) | [NA](https://attack.mitre.org/) |

### Discovery of Missing Patches

The discovery of missing patches can be identified easily either through manual methods or automatic. Manually this can be done easily be executing the following command which will enumerate all the installed patches.

```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn
	
wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB3136041" /C:"KB4018483"
```

### Metasploit

```bash
post/windows/gather/enum_patches
```

<figure><img src="../../../.gitbook/assets/image (105).png" alt=""><figcaption><p>Metasploit – Patches Enumeration</p></figcaption></figure>

### PowerShell

There is also a PowerShell script which target to identify patches that can lead to privilege escalation. This script is called [Sherlock](https://github.com/rasta-mouse/Sherlock) and it will check a system for the following:

* MS10-015 : User Mode to Ring (KiTrap0D)
* MS10-092 : Task Scheduler
* MS13-053 : NTUserMessageCall Win32k Kernel Pool Overflow
* MS13-081 : TrackPopupMenuEx Win32k NULL Page
* MS14-058 : TrackPopupMenu Win32k Null Pointer Dereference
* MS15-051 : ClientCopyImage Win32k
* MS15-078 : Font Driver Buffer Overflow
* MS16-016 : ‘mrxdav.sys’ WebDAV
* MS16-032 : Secondary Logon Handle
* CVE-2017-7199 : Nessus Agent 6.6.2 – 6.10.3 Priv Esc

### Privilege Escalation Table

The following table has been compiled to assist in the process of privilege escalation due to lack of sufficient patching.

| Operating System                                                             | Description                 | Security Bulletin                                                              | KB      | Exploit                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------------------------------------------------- | --------------------------- | ------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows Server 2016                                                          | Windows Kernel Mode Drivers | [MS16-135](https://technet.microsoft.com/en-us/library/security/ms16-135.aspx) | 3199135 | <p><a href="https://github.com/mwrlabs/CVE-2016-7255">Exploit</a></p><p><a href="https://github.com/FuzzySecurity/PSKernel-Primitives/blob/master/Sample-Exploits/MS16-135/MS16-135.ps1">Github</a></p>                                                                                                                                                                                 |
| Windows Server 2008 ,7,8,10 Windows Server 2012                              | Secondary Logon Handle      | [MS16-032](https://technet.microsoft.com/en-us/library/security/ms16-032.aspx) | 3143141 | <p> <a href="https://github.com/khr0x40sh/ms16-032">GitHub</a></p><p><a href="https://www.exploit-db.com/exploits/39719/">ExploitDB</a></p><p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms16_032_secondary_logon_handle_privesc">Metasploit</a></p>                                                                                                              |
| Windows Server 2008, Vista, 7                                                | WebDAV                      | [MS16-016](https://technet.microsoft.com/en-us/library/security/ms16-016.aspx) | 3136041 | [Github](https://github.com/koczkatamas/CVE-2016-0051)                                                                                                                                                                                                                                                                                                                                  |
| Windows Server 2003, Windows Server 2008, Windows 7, Windows 8, Windows 2012 | Windows Kernel Mode Drivers | [MS15-051](https://technet.microsoft.com/en-us/library/security/ms15-051.aspx) | 3057191 | <p><a href="https://github.com/hfiref0x/CVE-2015-1701/raw/master/Compiled/Taihou32.exe">GitHub</a></p><p><a href="https://github.com/offensive-security/exploit-database-bin-sploits/raw/master/sploits/37049-32.exe">ExploitDB</a></p><p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms15_051_client_copy_image">Metasploit</a></p>                               |
| Windows Server 2003, Windows Server 2008, Windows Server 2012, 7, 8          | Win32k.sys                  | [MS14-058](https://technet.microsoft.com/en-us/library/security/ms14-058.aspx) | 3000061 | <p><a href="https://github.com/sam-b/CVE-2014-4113">GitHub</a></p><p><a href="https://github.com/offensive-security/exploit-database-bin-sploits/raw/master/sploits/39666.zip">ExploitDB</a></p><p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms14_058_track_popup_menu">Metasploit</a></p>                                                                       |
| Windows Server 2003, Windows Server 2008, 7, 8, Windows Server 2012          | AFD Driver                  | [MS14-040](https://technet.microsoft.com/en-us/library/security/ms14-040.aspx) | 2975684 | <p><a href="http://bhafsec.com/files/windows/MS14-40-x32.py">Python</a></p><p><a href="http://bhafsec.com/files/windows/MS14-40-x32.exe">EXE</a></p><p><a href="https://www.exploit-db.com/exploits/39446/">ExploitDB</a></p><p><a href="https://github.com/JeremyFetiveau/Exploits/blob/master/MS14-040.cpp">Github</a></p>                                                            |
| Windows XP, Windows Server 2003                                              | Windows Kernel              | [MS14-002](https://technet.microsoft.com/en-us/library/security/ms14-002.aspx) | 2914368 | [Metasploit](https://www.rapid7.com/db/modules/exploit/windows/local/ms\_ndproxy)                                                                                                                                                                                                                                                                                                       |
| Windows Server 2003, Windows Server 2008, 7, 8, Windows Server 2012          | Kernel Mode Driver          | [MS13-005](https://technet.microsoft.com/en-us/library/security/ms13-005.aspx) | 2778930 | <p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms13_005_hwnd_broadcast">Metasploit</a></p><p><a href="https://www.exploit-db.com/exploits/24485/">ExploitDB</a></p><p><a href="https://github.com/0vercl0k/stuffz/blob/master/ms13-005-funz-poc.cpp">GitHub</a></p>                                                                                                |
| Windows Server 2008, 7                                                       | Task Scheduler              | [MS10-092](https://technet.microsoft.com/en-us/library/security/ms10-092.aspx) | 2305420 | <p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms10_092_schelevator">Metasploit</a></p><p><a href="https://www.exploit-db.com/exploits/15589/">ExploitDB</a></p>                                                                                                                                                                                                   |
| Windows Server 2003, Windows Server 2008, 7, XP                              |  KiTrap0D                   | [MS10-015](https://technet.microsoft.com/en-us/library/security/ms10-015.aspx) | 977165  | <p><a href="http://bhafsec.com/files/windows/KiTrap0d.zip">Exploit</a></p><p><a href="https://www.exploit-db.com/exploits/11199/">ExploitDB</a></p><p><a href="https://github.com/offensive-security/exploit-database-bin-sploits/raw/master/sploits/11199.zip">GitHub</a></p><p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms10_015_kitrap0d">Metasploit</a></p> |
| Windows Server 2003, XP                                                      | NDProxy                     | [MS14-002](https://technet.microsoft.com/en-us/library/security/ms14-002.aspx) | 2914368 | <p><a href="http://bhafsec.com/files/windows/MS14-002.exe">Exploit</a></p><p><a href="https://www.exploit-db.com/exploits/30014/">ExploitDB</a></p><p><a href="https://www.exploit-db.com/exploits/37732/">ExploitDB</a></p><p><a href="https://github.com/dev-zzo/exploits-nt-privesc/blob/master/MS14-002/MS14-002.c">Github</a></p>                                                  |
| Windows Server 2003, Windows Server 2008, 7, 8, Windows Server 2012          | Kernel Driver               | [MS15-061](https://technet.microsoft.com/en-us/library/security/ms15-061.aspx) | 3057839 | [Github](https://github.com/Rootkitsmm/MS15-061)                                                                                                                                                                                                                                                                                                                                        |
| Windows Server 2003, XP                                                      | AFD.sys                     | [MS11-080](https://technet.microsoft.com/en-us/library/security/ms11-080.aspx) | 2592799 | <p><a href="http://bhafsec.com/files/windows/ms110-080.exe">EXE</a></p><p><a href="https://www.rapid7.com/db/modules/exploit/windows/local/ms11_080_afdjoinleaf">Metasploit</a></p><p><a href="https://www.exploit-db.com/exploits/18176/">ExploitDB</a></p>                                                                                                                            |
| Windows Server 2003, XP                                                      | NDISTAPI                    | [MS11-062](https://technet.microsoft.com/en-us/library/security/ms11-062.aspx) | 2566454 | [ExploitDB](https://www.exploit-db.com/exploits/40627/)                                                                                                                                                                                                                                                                                                                                 |
| Windows Server 2003, Windows Server 2008, 7, 8, Windows Server 2012          | RPC                         | [MS15-076](https://technet.microsoft.com/en-us/library/security/ms15-076.aspx) | 3067505 | [Github](https://github.com/monoxgas/Trebuchet)                                                                                                                                                                                                                                                                                                                                         |
| Windows Server 2003, Windows Server 2008, 7, 8, Windows Server 2012          | Hot Potato                  | [MS16-075](https://technet.microsoft.com/en-us/library/security/ms16-075.aspx) | 3164038 | <p><a href="https://github.com/foxglovesec/RottenPotato">GitHub</a></p><p><a href="https://github.com/Kevin-Robertson/Tater">PowerShell</a></p><p><a href="https://github.com/foxglovesec/Potato">HotPotato</a></p>                                                                                                                                                                     |
| Windows Server 2003, Windows Server 2008, 7, XP                              | Kernel Driver               | [MS15-010](https://technet.microsoft.com/en-us/library/security/ms15-010.aspx) | 3036220 | <p><a href="https://github.com/offensive-security/exploit-database-bin-sploits/raw/master/sploits/39035.zip">GitHub</a></p><p><a href="https://www.exploit-db.com/exploits/37098/">ExploitDB</a></p>                                                                                                                                                                                    |
| Windows Server 2003, Windows Server 2008, 7, XP                              | AFD.sys                     | [MS11-046](https://technet.microsoft.com/en-us/library/security/ms11-046.aspx) | 2503665 | <p><a href="http://bhafsec.com/files/windows/ms11-046.exe">EXE</a></p><p><a href="https://www.exploit-db.com/exploits/40564/">ExploitDB</a></p>                                                                                                                                                                                                                                         |
