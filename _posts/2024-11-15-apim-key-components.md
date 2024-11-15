---
title: "API Management Key Components"
date: 15-Nov-2024
categories: [Miscellaneous, API Management ]
tags: [apim, api-management]
author: aditya-singh
# image: /assets/img/posts/2024-11-15-apim-key-components/key-component.png
---

![API Management Key Components](/assets/img/posts/2024-11-15-apim-key-components/key-component-light.png){: width="450" height="300" .light }
![API Management Key Components](/assets/img/posts/2024-11-15-apim-key-components/key-component-dark.png){: width="450" height="300" .dark }
_API Management Key Components_

Hello Tech Enthusiasts 👋,

With the nearly end of year 2024, thought it would be good time to initiate discussion about API Management principles. 

## **Overview**

To start with, let's first understand what exactly API is. API stands for Application Prgramming interface which basically is a set of rules and protocols that allow different software applications to communicate with each other. It defines how requests and responses should be formatted, enabling developers to interact with a service, system, or application without needing to understand its internal workings. 

Example: Food ordering application

1) **Customer App** :  The mobile app uses APIs to communicae with various services. E.g. fetch restaurant details (Restaurant API) ,  process payments (Payment Gateway API (e.g., PayPal or Stripe) ).
   
2) **Restaurant System** : Once an order is placed, the app communicates with the restaurant’s Order Management System API to send the order details.
   
3) **Delivery Partner App** : The delivery tracking system uses Mapping APIs (e.g., Google Maps API) to calculate routes and show real-time tracking to the customer.
   
4) **Notifications** : Uses Push Notification APIs (e.g., Firebase) to inform the customer and delivery partner about order status updates.

In order to build a good API Management platform, there are numerous API Management applications like IBM API Connect, Kong, Mulesoft, Google APIGEE, and so on. I've seen organisations having their own API Management application to some extent, but these custom build platform have their own pros (limited) and cons(many). The API Management platform in market are build to ease the overall development process with low-code/no-code paradigm.

## **Key Components**
So let's first understand, what are the components that your API Management platform should have. There are four major components

### **API Portal**

The API Portal is main portal which helps the provider organisation to define the APIs for consumers. On a high level, the purpose of API Portal are :

- **Centralised Platform of API Management**: If the platform build on-prem, the first job is to install and connect other various component (API Gateway, Developer Portal, Analytics). This is due to behaviour of API Management as management portal usually be the taking care of all communication. If the solution is on cloud (e.g. Azure API-Management), the resource installation usually takes care of all the component installation and their connection as well. 
- **Define APIs**: This is management portal where developers (provider organisation) develops the APIs and in some cases Products (group of APIs) and deploy them. These APIs need to be developed through API standards.
- **API Lifecycle Management**: The management portal also takes care of API liefcycle which is deployment of APIs, management of application subscriptions (this is usually part of developer -prtal, but in some-case, management portal also can do the same), retire/deprecate/archive of APIs. Whenever a new version of API need to be deployed, it's important this process to make sure the existing API version to be retired. 

### **API Gateway**

- API Gateway is the main application/server where all APIs are deployed. All consumers and provider connects only through API Gateway. It acts as an entry point for all requests. It main job is to handle request routing, authentication, rate limiting, caching, and monitoring.
- Usually the API-Gateway connects to management portal through management IPs while the client application connects.

### **Analytics**

- Tracks API usage, performance, errors, and latency through dashboards and reports.
- Helps identify trends and issues for optimization.


### **Developer Portal**

- Provides documentation, usage guidelines, and testing tools for developers to understand and consume APIs effectively.
- Often includes an API sandbox for experimentation.

Please do comment yout thoughts and experience in API Management.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
