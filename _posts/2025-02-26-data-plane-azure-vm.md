---
title: "Install Kong Data Plane Node (Kong Gateway) on Azure VM (Ubuntu) - Self-Managed Hybrid"
date: 26-Feb-2025
categories: [Kong, Konnect]
tags: [Kong Konnect, API Gateway, Azure, Virtual Machine, VM, Ubuntu, Linux]
author: aditya-singh
mermaid: true
image: /assets/img/posts/2025-02-26-data-plane-azure-vm/cover.gif
---

Hello Tech Enthusiasts 👋,

Continuing my journey of learning Kong Konnect, today I’ll walk you through installing a Kong Gateway node (Data Plane) on an Azure VM (Ubuntu).

If you're new to Kong, I recommend checking out my previous article before proceeding [Getting Started with Kong API Management](https://adityasingh-lab.github.io/posts/kong-konnect-getting-started/){:target="_blank"}.

## Prerequisites

- Azure account with appropriate permission to create VM
- Knowledge on basic cloud components
- Sign-up and access to Kong Konnect


## Created Azure VM (Ubuntu)

To try out, I've created Ubuntu VM in Azure.

- Login to Azure and navigate to Virtual Machine
- Create VM as below screenshot

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/azure-vm-setup.png)
_Azure Ubuntu VM Setup_

- Update network configuration as below (to access https/http port)

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/azure-port-rules.png)
_Azure VM Port Rules_

## Configure Dataplane node on Azure VM

- Login to Konnect portal and navigate to Gateway Manager
- Create New Gateway as 'On-Prem'
- Once created, select On-Prem and then navigate to Data Plane Nodes
- Click New Data Plane Node.
- Select Gateway Version as the latest , platform as Linux.
- Access the VM through putty using the remote username and password configured during VM setup.

> Use public key when setting up this for enterprise, I'm using password just for demo purpose
{: .prompt-info }

- Sudo to root access and follow the link [https://docs.konghq.com/gateway/3.9.x/install/linux/ubuntu/](https://docs.konghq.com/gateway/3.9.x/install/linux/ubuntu/){:target="_blank"}.

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/linux-installation-screenshot.png)
_Kong installation on Ubuntu_

- Navigate back to Konnect portal and click 'Generate certificate'. Follow the steps as in it

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/gateway-instance-konnect.png)
_Konnect Data Plane Node Instructions_

- After the execution of command kong restart, you should see 'Data Plane Node has been found' and then click done.

## Create Gateway Service to test

To create proxy service, navigate to  Gateway Manager > Serverless > Gateway Service, and then click '_**+ New gateway service**_' and follow the screenshot below. I'm using [https://httpbin.org/anything](https://httpbin.org/anything){:target="_blank"} as the backend proxy call.

### New Gateway Service

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/gateway-service-landing-page.png)
_Gateway Service Landing Page_

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/gateway-service-form.png)
_Gateway Service Form_

### Configure Routes

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/routes-landing-page.png)
_Routes Landing Page_

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/route-form.png)
_Fill the details_

## Testing

You can test the service locally (using curl on putty) or through browser to call the external URL. Following screenshot suffice both

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/test-curl-httpbin.png)
_Test locally in the within the Server_

![](/assets/img/posts/2025-02-26-data-plane-azure-vm/public-connectivity-test.png)
_Testing through browser for external URL_

Please do let me know your thoughts and any question in comments.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
