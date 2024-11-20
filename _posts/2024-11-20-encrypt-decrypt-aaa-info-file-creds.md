---
title: "Encrypt & Decrypt AAA-Info file credentials"
date: 20-Nov-2024
categories: [IBM, DataPower ]
tags: [datapower, aaa, aaa-info]
author: aditya-singh
mermaid: true
image: /assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/aaa-auth-image.png
---

Hello Tech Enthusiasts 👋,

AAA is something DataPower is offerring from a day of appliance available in marked. So it's not a new topic of discussion. However, as a authentication mechanism, which is basic-auth, aaa-info file is widely used. I usually don't support as the creds are visible whomesover has access to domain/environment. 

There are other mechanism which can be helpful, but in this article I'm purely focussing on easy solution. The easiest option is using encrypt/decrypt mechanism which to be used for credentials. Here you go with the design and sequence diagram to understand the process.

**Admin Process**

![](/assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/admin-steps.gif){: width="600" height="400" }
_Adminstrator Steps_

1. Admin post plain credentials credential by using encrypt/decrypt service in DataPower.
2. DataPower returns encrypted text to admin.
3. Admin shared the plain username and encrypted password with client via secure line.
4. Admin to update the aaa-info.xml file with plain username and password.

Below postman screenshot depicts the behavior of service

![](/assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/generate-encrypted-string-dark.png){: .dark }
![](/assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/generate-encrypted-string-light.png){: .light }
_Encrypt Password String_

![](/assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/AAA-Authentication-light.png){: .light}
![](/assets/img/posts/2024-11-20-datapower-aaa-info-alt-auth/AAA-Authentication-dark.png){: .dark}
_Validate Encrypted Password_

**Sequence Diagram**

> Enable light theme for better visibilty
{: .prompt-tip }

```mermaid
sequenceDiagram
participant cl as Client
participant dp as DataPower
autonumber
cl ->> dp: Sends request with Authorization header
activate cl
activate dp
dp ->> dp: extract authorization header <br/>ZnJlZDpuYzY5a0I1c0RwNTlibDl3WURRL3pvQ2p2REsvQStHY0V1WjlKVFZkdVo4PTw=
dp ->> dp: extract encrypted password : nc69kB5sDp59bl9wYDQ/zoCjvDK/A+GcEuZ9JTVduZ8=
dp ->> dp: decrypt password using the encrypted algo and shared-secret-key : smith
dp ->> dp: construct new authorization header with plain username and decrypted password : Basic ZnJlZDpzbWl0aA== 
dp ->> dp: calls AAA policy to authenticate using AAA-Info.xml file
dp ->> cl: complete transaction for further processing
deactivate dp
deactivate cl
```

Please do comment down your views.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
