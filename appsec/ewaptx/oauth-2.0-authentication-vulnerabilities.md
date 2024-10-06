---
description: advanced Attack
---

# OAUTH 2.0 authentication vulnerabilities

**Outh2** is the main web standard for authorization between service. it is used to authorize 3rf party apps to access services or data form a provider with which you have an account.

&#x20;

Client App -> Web App wants  to access the user's data

Resource Owenr -> The user

OAuth service provider -> application that controls

**Important Parameters:**

* redirect\_uri
* code
* state

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

OAuth implementations can vary significantly based on the specific requirements and use cases. These variations are called OAuth "flows" or "grant types.”

What is OAuth grant types  “OAuth Flows”?

Before a client application can start an OAuth flow, the OAuth service must be configured to support a particular grant type&#x20;

### OAuth grant types “OAuth Flows”

There are several different grant types, each with varying levels of complexity and security considerations. "**authorization code**" and "**implicit**" grant types stand out as the most commonly used in OAuth protocols.

### OAuth Scope

In any OAuth grant type, the client application must specify the data it intends to access and the operations it seeks to perform. It accomplishes this through the "scope" parameter in the authorization request sent to the OAuth service.

The scopes available for client application access are specific to each OAuth service in basic OAuth. The scope name is a flexible text string whose format can vary significantly across providers. Some providers even use full URIs as scope names, resembling REST API endpoints. For instance, when seeking read access to a user's contact list, the scope name might take various forms depending on the OAuth service:

* <mark style="color:red;">**`scope=contacts`**</mark>
* <mark style="color:red;">**`scope=contacts.read`**</mark>
* <mark style="color:red;">**`scope=contact-list-r`**</mark>
* <mark style="color:red;">**`scope=https://oauth-authorization-server.com/auth/scopes/user/contacts.readonly`**</mark>

However, in OAuth scenarios involving authentication, standardized <mark style="color:red;">**`OpenID`**</mark> Connect scopes are often employed. For instance, the scope `"`<mark style="color:red;">**`openid profile`**</mark>`"` would grant the client application read access to a predefined set of basic user information, such as their email address and username.

### Identifying OAuth authentication <a href="#identifying-oauth-authentication" id="identifying-oauth-authentication"></a>

The most reliable way to identify OAuth authentication is to proxy your traffic through Burp and check the corresponding HTTP messages when you use this login option. Regardless of which OAuth grant type is being used, the first request of the flow will always be a request to the <mark style="color:red;">**`/authorization`**</mark> endpoint containing several query parameters that are used specifically for OAuth. In particular, keep an eye out for the <mark style="color:red;">**`client_id`**</mark>, <mark style="color:red;">**`redirect_uri`**</mark>, and <mark style="color:red;">**`response_type`**</mark> parameters. For example, an authorization request will usually look something like this:

{% code overflow="wrap" %}
```http
GET /authorization?client_id=12345&redirect_uri=https://client-app.com/callback&response_type=token&scope=openid%20profile&state=ae13d489bd00e3c24 HTTP/1.1
Host: oauth-authorization-server.com
```
{% endcode %}

Authorization Code Steps

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

1.  **Authorization request**

    The client application requests access to specific user data by sending a request to the OAuth service's `/authorization` endpoint. It's important to note that the endpoint mapping might differ among providers.

    However, you should always be able to identify the endpoint based on the parameters used in the request.

    ```http
    GET /authorization?client_id=12345&redirect_uri=https://client-app.com/callback&response_type=code&scope=openid%20profile&state=ae13d489bd00e3c24 HTTP/1.1
    Host: oauth-authorization-server.com
    ```

    The request to the OAuth service's endpoint includes several important parameters, typically provided in the query string:

    * <mark style="color:red;">**`client_id`**</mark>: A mandatory parameter containing the unique identifier of the client application, generated during registration with the OAuth service.
    * <mark style="color:red;">**`redirect_uri`**</mark>: The URI to which the user's browser should be redirected when sending the authorization code to the client application, also known as the "callback URI" or "callback endpoint." The validation of this parameter is crucial as many OAuth attacks exploit flaws in its validation.
    * <mark style="color:red;">**`response_type`**</mark>: Determines the type of response expected by the client application and thus the flow to be initiated. For the authorization code grant type, the value should be "code."
    * <mark style="color:red;">**`scope`**</mark>: Specifies the subset of the user's data the client application wants to access. These scopes may be custom or standardized, as defined by the OAuth provider or the OpenID Connect specification.
    * <mark style="color:red;">**`state`**</mark>: Stores a unique and unguessable value tied to the current session on the client application. The OAuth service returns this exact value in the response, along with the authorization code. This parameter acts as a CSRF token for the client application, ensuring that the request to its /callback endpoint originates from the same user who initiated the OAuth flow.
2.  **User login and consent**

    Upon receiving the initial request, the authorization server redirects the user to a login page, typically associated with the OAuth provider, such as their social media account. Here, the user is prompted to log in and presented with a list of data that the client application seeks to access, based on the scopes defined in the authorization request. The user can then choose to consent or decline this access.

    Importantly, once the user has approved a scope for a client application, subsequent logins are streamlined as long as the user maintains a valid session with the OAuth service. This means that while the initial login and consent process may require manual intervention, subsequent logins often involve a single click to grant access again.
3.  **Authorization code grant**

    Upon user consent, their browser is redirected to the specified `/callback` endpoint from the `redirect_uri` parameter in the authorization request. This GET request includes the authorization code as a query parameter. Optionally, it may also transmit the state parameter with the same value as in the initial authorization request.
4.  **Authorization code grant**

    Upon user consent, their browser is redirected to the specified `/callback` endpoint from the `redirect_uri` parameter in the authorization request. This GET request includes the authorization code as a query parameter. Optionally, it may also transmit the state parameter with the same value as in the initial authorization request.

    ```http
    GET /callback?code=a1b2c3d4e5f6g7h8&state=ae13d489bd00e3c24 HTTP/1.1
    Host: client-app.com
    ```
5.  **Access token request**

    After receiving the authorization code, the client application proceeds to exchange it for an access token. It accomplishes this by sending a server-to-server POST request to the OAuth service's `/token` endpoint. All communication beyond this point occurs within a secure back-channel, typically inaccessible to attackers.

    ```http
    POST /token HTTP/1.1
    Host: oauth-authorization-server.com
    …
    client_id=12345&client_secret=SECRET&redirect_uri=https://client-app.com/callback&grant_type=authorization_code&code=a1b2c3d4e5f6g7h8
    ```

    In addition to the client\_id and authorization code, the request includes the following parameters:

    * **`client_secret`**: The client application authenticates itself by providing the secret key assigned during registration with the OAuth service.
    * **`grant_type`**: Specifies the grant type the client application intends to use. In this case, it should be set to "authorization\_code."
6.  **Access token grant**

    Upon receiving the access token request, the OAuth service validates it. If all parameters align as expected, the server responds by issuing the client application an access token with the requested scope.

    ```json
    {
        "access_token": "z0y9x8w7v6u5",
        "token_type": "Bearer",
        "expires_in": 3600,
        "scope": "openid profile",
        …
    }
    ```
7.  **API call**

    With the access token in hand, the client application proceeds to retrieve the user's data from the resource server. This is achieved by making an API call to the OAuth service's `/userinfo` endpoint. The access token is included in the Authorization header with the value "`Bearer`" to authenticate the client application's permission to access the data.
8.  **Resource grant\\**

    The resource server should verify that the token is valid and that it belongs to the current client application. If so, it will respond by sending the requested resource i.e. the user's data based on the scope of the access token.

    ```json
    {
        "username":"n1ght",
        "email":"n1ght@n1ghtm4r3.com",
        …
    }
    ```

    The client application can finally use this data for its intended purpose. In the case of OAuth authentication, it will typically be used as an ID to grant the user an authenticated session, effectively logging them in.

### **Implicit grant type**

The implicit grant type in OAuth is simpler as it provides the access token directly to the client application after the user consents, without the intermediary step of obtaining an authorization code.

However, its simplicity comes with reduced security compared to other grant types. Communication occurs solely through browser redirects, lacking a secure back-channel like the authorization code flow. Consequently, the access token and user data are more vulnerable to potential attacks.

Despite its simplicity, the implicit grant type is more suitable for single-page applications and native desktop applications. These applications may find it challenging to securely store client secrets on the backend
