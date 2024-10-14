---
title: "Trigger email in DataPower"
date: 30-Sep-2024
categories: [IBM, DataPower ]
tags: [DataPower, smtp, mpgw]
author: aditya-singh
---

Hello Tech Enthusiasts 👋,

In this article, we'll talk about how we can trigger email in DataPower. To trigger, we can either use 1) log-target, 2) url-open xslt. I'll guide how we can use both the methods.
Before beginning, regardless of the method chosen, we will need the following details:

- SMTP Server host/IP,
- SMTP Server port,
- SMTP Server authentication credentials if used,
- SMTP Server TLS enabled or disabled,
- Recipient email address.
- From email address. This is important as many SMTP server would reject is incoming email domain isn't recognized.

To enable the test, I'm using free public smtp server : https://mailtrap.io

## Through log-target
The log-target method is fairly straight forward.

1). For test purpose, I'm creating a new log-category 'testsmtp'.
2). Next create log-target object and configure the details as below:

![smtp-log-target-main-tab](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/smtp-log-target-main-tab.jpg)

3). Add log-catagory to event Subscription tab

![](assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/smtp-log-target-event-subscription-tab.jpg)

> If SMTP server needs TLS and Username/Password authentication, we would need to update default user-agent.
{: .prompt-tip }

4). Create a new password-alias object (hardcoding password is deprecated) and create new Basic Auth Policy in default user-agent as below:

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/user-agent-auth-tab.jpg)

5). Add SMTP Client policy in default user-agent tab and ensure to check STARTTLS and Authentication options:

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/user-agent-smtp-client-auth-tab.jpg)

To test, go to troubleshooting and 'Generate Log Event' as below:

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/generate-log-event-troubleshooting.jpg)

Message Received in email as :

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/email-confirmation-logtarget.jpg)

In XSLT, we can trigger log category using dp:type="smtptest" in <xsl:message>. Refer [xsl:message](https://www.ibm.com/docs/en/datapower-gateway/10.5.x?topic=extensions-xslmessage).

## url-open xslt

In this solution, we're calling SMTP server within a service (here I'm using mpgw, but applicable for any of the service). To call, we'll use the most common DataPower extension element : [dp:url-open](https://www.ibm.com/docs/en/datapower-gateway/10.5.x?topic=elements-dpurl-open). 
The smtp endpoint url needs to be `dpsmtp//`. Ensure don't use smtp as that's now deprecated at the time of this article.

For demo purpose, I'm using loopback service to demonstrate url-open.

1). - Create a mpgw and add http/https front-side handler, processing policy and single rule with loopback variable (`var://service/mpgw/skip-backside = 1`)

2). Upload following xslt in the file-management and refer it using processing action (Transform using XSLT)

```xml

<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:dp="http://www.datapower.com/extensions" xmlns:dpconfig="http://www.datapower.com/param/config" xmlns:date="http://exslt.org/dates-and-times" extension-element-prefixes="dp" exclude-result-prefixes="dp dpconfig date" version="1.0">

<xsl:template match="/*">
<xsl:variable name="log-message">
  <MessageBody>
    <Date>
      <xsl:value-of select="date:date()" />
    </Date>
    <Time>
      <xsl:value-of select="date:time()" />
    </Time>
    <Domain>
      <xsl:value-of select="dp:variable('var://service/domain-name')" />
    </Domain>
    <TransactionID>
      <xsl:value-of select="dp:variable('var://service/transaction-id')" />
    </TransactionID>
  </MessageBody>
</xsl:variable>
  <dp:url-open target="dpsmtp://live.smtp.mailtrap.io:587/?To=xxxx@abc.com&amp;From=demoemail@demomailtrap.com&amp;Subject=Demo-Email" response="responsecode">This is sample message.
  <xsl:copy-of select="$log-message"/></dp:url-open>
</xsl:template>
</xsl:stylesheet>

```

Trigger the service and we can see this from DataPower probe as well

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/dp-probe-screenshot-troubleshooting.jpg)

![](/assets/img/posts/2024-09-30-dp-trigger-smtp-email-service/email-confirmation-urlopen.jpg)
