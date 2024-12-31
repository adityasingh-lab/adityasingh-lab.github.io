---
title: "Implementing Advance Scope Check (before Token Generation) for Client Credential Grant in IBM API Connect v10 "
date: 31-Dec-2024
categories: [IBM, IBM API Connect]
tags: [IBM API Connect, DataPower, OAUTH, Zero-Trust]
author: aditya-singh
mermaid: true
image: /assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/header.png
---

Hello Tech Enthusiasts 👋,

This article talks about implementing 'Advance Scope Check' in IBM API Connect v10. If you're new to my technical blog, I'd recommend to checkout my last article [link](https://adityasingh-lab.github.io/posts/apim-oauth-client-cred-simple/){:target="_blank"}, where I've covered basics on OAuth and implementing Client Credential (Application) grant type in IBM API Connect.

## Overview

For any organization, Security is on top priority. To begin with, let’s understand the importance of scopes in the OAuth framework. 

In the authentication and authorization flow of OAuth, authentication is managed through credential verification, while authorization determines access to resources. For example, User-1 might have full access (read/write/update/delete) to a resource or application, whereas User-2 might only have read access to the same resource. This variation in access levels for a particular resource or application is referred to as the **_scope_** of access.

## Scope Standard

There is no standardized naming convention for scopes. However, as a best practice I recommend using the format <_application/resource_>:<_access-level_>, while super-user or applications can have _application_ scope omitting the access level, to indicate unrestricted access.

Example:

- Accessing bank account (Account API) : account:read , account:write, account:update, account:delete
- Accessing credit information (Credit) : credit:read, credit:write

Alternatively, you can use underscores (_) or hyphens (-) instead of colons (:) such as account-read , or account_read.

## Why Scope Check is important ?

I’d like to reference the **Zero Trust** concept here, which operates on the principle of  **NEVER TRUST, ALWAYS VERIFY**.

By default, when application request token, the Native OAuth Provider checks if the application is subscribed to the API which has OAuth security enforcement. The API could have multiple paths/operations typically aligned with CRUD (Create/Read/Update/Delete) model as per API standards and best Practices. Since these are distinct operations, it is often necessary to enforce access restrictions for specific applications or users.

n a native OAuth provider, it is challenging to determine whether an application subscribed to an API plan is authorized for a specific operation. One alternative is to create multiple plans for each operation, but this approach introduces significant complexity—both operationally and conceptually. A better solution is to implement this with the correct approach, namely **Advance Scope Check**.

## How to implement Simple Advance Scope Check ?

High Level Steps of implementation:

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/advance-scope-check-flow-chart-light.svg){: .light }
![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/advance-scope-check-flow-chart-dark.svg){: .dark }
_Implementation Steps_

### Key Things to be taken care during implementation

To implement Advance Scope Check, we'd need additional service that would :

- The advance scope check service should map the input application & it's supplied scope through a property file and returns the allowed scope in response http header `x-selected-scope`.
- In case unable to identify, the service should reject the transaction and return non-200 response.
- The Native OAuth Provider looks for `x-selected-scope` and override allowed scopes.


### Step 1: _Create Scope Check Service_

This service can be implemented and hosted at any application / server. I'm creating this service on DataPower itself (in separate non-apim domain). The service type I've implement an XML-Firewall service with loopback backend type.

- Create XML Firewall with loopback type.
- Have the local-ip as 127.0.0.1 so that service isn't accessible from outside.
- Add policy with following implementation:
  - Match action to match all incoming request.
  - Convert Query param to convert the input GET request to XML
  - Add transformation XSLT action which should function in following steps:
    - Extract input `app_name` and `token_scope`.
    - Match it with attribute `app_name` and `token_scope`from the XML property file.
    - Set `x-selected-scope` header with mapped value.
    - If no value match, log error message `No Application Scope Match`
  - Add result action

```xml
<!-- Sample XML Property file -->
<AdvanceScopeCheck>
    <Scopes app_name="application-1" token_scope="scope1">
    scope1:read scope1:write scope1:update scope1:delete
    </Scopes>
    <Scopes app_name="application-2" token_scope="scope1">
    scope1:read
    </Scopes>
</AdvanceScopeCheck>
```
{: file='advance-scope-check-property.xml'}

> The `application-1` and `application-2` are the client app that we'd be creating later in [Add Subscription](https://adityasingh-lab.github.io/posts/apim-client-cred-advance-scope-check/#step-4-add-subscription).
{: .prompt-info }

### Step 2: _Add Scope Service URL and OAuth Scope in Native OAuth Provider_

- Go to Resources > OAuth Provider
- Select the Native OAuth Provider
- Select Scope section from design tab and check **Advanced scope check before Token Generation**
- Add Endpoint URL as `http:127.0.0.1:<port>/` and the click save.

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/native-oauth-scopes.png)
_Sample Scopes in Native OAuth Provider API_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/native-oauth-advance-scope-check.png)
_Advance Scope Check Sample in Native OAuth Provider_

> (_Optional_) 
> The default error generates non intuitive response, so additionally, In the Assemble part, I updated the gateway script '_oauth-auto-generated-advanced-scope-application_' for non-200 response as :
> ```javascript
>              // Error non 200 response
>              context.set('oauth.custom_error.message', 'Invalid Scope');
>              context.set('oauth.custom_error.description', 'Please provide the valid scope.');
>```
>{: file='oauth-auto-generated-advanced-scope-application.js'}
{: .prompt-tip }

### Step 3: _Create/Update OAuth Secure API_

In my API, I created 4 operations for single path and modified the security:scope specifically. Here is the sample yaml file

```yaml
paths:
  /test-oauth-secure:
    get:
      responses:
        '200':
          description: success
          schema:
            type: string
      security:
        - oauth-client-cred:
            - scope1:read
    post:
      responses:
        '200':
          description: success
          schema:
            type: string
      security:
        - oauth-client-cred:
            - scope1:write
    delete:
      responses:
        '200':
          description: success
          schema:
            type: string
      security:
        - oauth-client-cred:
            - scope1:delete
    put:
      responses:
        '200':
          description: success
          schema:
            type: string
      security:
        - oauth-client-cred:
            - scope1:update
securityDefinitions:
  oauth-client-cred:
    type: oauth2
    x-ibm-oauth-provider: oauth-client-credential-grant
    flow: application
    tokenUrl: https://$(catalog.url)/oauth-client-credential-grant/oauth2/token
    scopes:
      scope1: Access to all paths of application
      scope1:read: Read Access
      scope1:write: Write Access
      scope1:delete: Delete Access
      scope1:update: Update Access
security:
  - oauth-client-cred: []
```

### Step 4: _Add subscription_

As specified above advance-scope-check-property.xml, create 2 applications and subscribe both to the OAuth-Secure-Client-Cred-API

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-subscription-apim.png)
_Sample Application Subscription_

### Step 5: _Testing_

Below sequence design 👇 depicts the advance-scope flow which we're going to implement as part of this article: 

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/sequence-diagram.gif)
_Advance Scope Check Sequence Diagram_

> 
> - I'm using postman scripts to get token and use the token in my next operation. 
> - The following screenshot covers all the explanation. Add your comments in the posts if you'd like to additional details or reach out to me on my [linkedIn](https://www.linkedin.com/in/aditya-singh-apimgmt1987/){:target="_blank"}.
{: .prompt-info }

#### 5.1: _Success / Happy-Path_

##### 5.1.1:  _Application-1 Testing_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-1-extract-token.png)
_Application-1 : Get OAuth Token_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-1-validate-token-get-method.png)
_Application-1 : Validate Read operation_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-1-validate-token-post-method.png)
_Application-1: Validate Write(Post) Operation_

##### 5.1.2: _Application-2 Testing_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-2-extract-token.png)
_Application-2 : Get OAuth Token_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/application-1-validate-token-get-method.png)
_Application-2 : Validate Read operation_

#### 5.2: _Failure / Unhappy-path_

##### 5.2.1: _No Scope as part of OAuth token request_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/no-application-scope-error.png)
_Error thrown when no scope in token request_

##### 5.2.2: _Scope not part of listed scopes_

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/random-scope-error.png)
_Error when Invalid Scope part of token request_

##### 5.2.3 _Application-2 calling POST(write) Operation_

The following screenshot depicts the behavior where application-2 trying to access POST (write) operation. The call get fails because Application-2 token scope doesn't have access to `scope:write`.

![](/assets/img/posts/2024-12-31-apim-client-cred-advance-scope-check/api-path-scope-check-error.png)
_Application-2: Calling Write Operation (Error)_

Please do let me know your thoughts and any question in comments.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
