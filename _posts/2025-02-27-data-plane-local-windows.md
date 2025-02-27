---
title: "Install Kong Data Plane Node (Kong Gateway) on Windows (docker) - Self-Managed Hybrid"
date: 27-Feb-2025
categories: [Kong, Konnect]
tags: [Kong Konnect, API Gateway]
author: aditya-singh
mermaid: true
image: /assets/img/posts/2025-02-27-data-plane-local-windows/cover.gif
---


Hello Tech Enthusiasts 👋,

Continuing my journey of learning Kong Konnect, today I’ll walk you through installing a Kong Gateway node (Data Plane) on Windows.

If you're new to Kong, I recommend checking out my previous article before proceeding [Getting Started with Kong API Management](https://adityasingh-lab.github.io/posts/kong-konnect-getting-started/){:target="_blank"}.

## Prerequisites

- Sign-up and access to Kong Konnect
- Docker Desktop
- Basic knowledge on docker container and pods

## Create Data Plane Node

- Login to Kong Konnect and navigate to Gateway Manager
- If this is first time, then New Gateway as 'On-Prem'
- Once created, select On-Prem and then navigate to Data Plane Nodes
- Click New Data Plane Node.
- Select Gateway Version as the latest , platform as Windows.

Follow the following screenshot :

![](/assets/img/posts/2025-02-27-data-plane-local-windows/data-plane-node-windows.png)
_Create Data Plane Node Windows_

![](/assets/img/posts/2025-02-27-data-plane-local-windows/dc-desktop-vs-konnect-gw-dp-node.png)
_Docker Desktop and Konnect GW Node Comparison_


## Create Gateway Service and Test

![](/assets/img/posts/2025-02-27-data-plane-local-windows/gateway-service-landing-page.png)
_Gateway Service Landing Page_

![](/assets/img/posts/2025-02-27-data-plane-local-windows/gateway-service-form.png)
_Gateway Service Form_

![](/assets/img/posts/2025-02-27-data-plane-local-windows/routes-landing-page.png)
_Routes Landing Page_

![](assets/img/posts/2025-02-27-data-plane-local-windows/route-form.png)
_Routes Form_

![](/assets/img/posts/2025-02-27-data-plane-local-windows/test-gateway-service.png)
_Postman Testing_

![](/assets/img/posts/2025-02-27-data-plane-local-windows/analytics-screenshot.png)
_Analytics Screen Testing_


Please do let me know your thoughts and any question in comments.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
