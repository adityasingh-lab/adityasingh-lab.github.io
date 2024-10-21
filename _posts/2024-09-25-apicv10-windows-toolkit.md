---
title: "Installing IBM API Connect Designer Toolkit on Windows"
date: 25-Sep-2024
categories: [IBM, IBM API Connect ]
tags: [IBM API Connect, toolkit]
author: aditya-singh
image: /assets/img/posts/2024-09-24-apicv10-windows-toolkit/designer-toolkit-homepage.jpg
---

Hello Tech Enthusiasts 👋,

Discussing toolkits, IBM API Connect (hereafter referred as APIC) provides an offline toolkit designed for development purposes, which includes an offline UI, also known as the Designer toolkit, and command-line enabled commands to automate deployment processes (CI/CD).

We can either download it from [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/selectFixes?parent=ibm%7EWebSphere&product=ibm/WebSphere/IBM+API+Connect&release=10.0.5.7&platform=All&function=textSearch&text=toolkit){:target="_blank"} or best case, use APIC Cloud/Manager UI homepage. Here, I'm using API Manager UI to install.

I will also provide guidance on automating deployment in upcoming articles. Stay tuned.

## Installation

- On APIC Manager UI homepage and click 'Download toolkit'. I prefer to set up the credentials in the first step as well, as this allows the designer UI to run immediately after installation.

![apim-homepage](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/apim-homepage.jpg)
_APIM Manager UI Homepage_

![toolkit-installation-options](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/toolkit-installation-options.jpg)
_Toolkit Installation Options_

- Download API Designer credentials (designer_credentials.json).

- Update environment variable to designer_credentials.json. Run following command from windows CMD (need admin to run). You can do the same from windows gui.

`setx APIC_DESIGNER_CREDENTIALS <folderpath>\designer_credentials.json /m`

![env-var-set-command](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/set-env-variable-cmd.jpg)
_Windows Environment Set Variable CMD_

- In case permission issue, we can use temporary set instead of setx variable.

- Next, on the step-1 Download toolkit, select the windows operating system. It will download toolkit-loopback-designer-windows.zip file.

- Extract the content and you can see two files as **api_designer-win.exe** and **apic.exe** {command line toolkit}. 

- Double click the api_designer-win.exe and follow the prompt (Next / Install).

> Follow the below 👇 steps as per screenshots and you should be easily able to install the toolkit 🤞 😊

![windows-installation-prompt](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/windows-installation-prompt.jpg)
_Windows Installation Prompt (click Run anyway)_

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/windows-installation-complete-prompt.jpg)
_Windows Installation Complete Prompt_

- After installation, it would request to Connect to cloud

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/toolkit-connect-to-cloud-prompt.jpg)
_Designer Toolkit Connect to Cloud Prompt_

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/enter-apim-url-image.jpg)
_Enter API-Manager UI URL_

- Next it would ask you to 'Open a Directory'. If this is your first time, create a project directory in your local windows folder, then browse and select it.

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/toolkit-open-directory-prompt.jpg)
_Designer Toolkit Open a director prompt_

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/designer-toolkit-login-prompt.jpg)
_Designer Toolkit Login Prompt_

![](/assets/img/posts/2024-09-24-apicv10-windows-toolkit/designer-toolkit-homepage.jpg)
_Designer Toolkit Homepage UI_

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
