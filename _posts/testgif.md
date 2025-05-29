<!-- ---
title: "Test-GIF"
date: 30-Sep-2024
categories: [IBM, DataPower ]
tags: [DataPower, smtp, mpgw]
author: aditya-singh
--- -->

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

<!-- [![Buy-Me-Coffee](/assets/img/posts/buymecoffee/buy-me-coffee-giphy.gif)](https://buymeacoffee.com/aditya.singh){:target="_blank"}

[![Buy-Me-Coffee](https://media.giphy.com/media/7kZE0z52Sd9zSESzDA/giphy.gif)](https://buymeacoffee.com/aditya.singh){:target="_blank"} -->

<!-- <a href="https://buymeacoffee.com/aditya.singh"> <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" href="" ></a> -->

If this article helped you in someway and want to support me, you can ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"} 
