---
title: "Install Kong Data Plane Node (Kong Gateway) on Azure Kubernetes Service - Self-Managed Hybrid"
date: 27-Feb-2025
categories: [Kong, Konnect]
tags: [Kong Konnect, API Gateway, Kubernetes, AKS, Azure]
author: aditya-singh
mermaid: true
image: assets/img/posts/2025-02-27-data-pane-node-aks/cover.gif
---

Hello Tech Enthusiasts 👋,

Continuing my journey of learning Kong Konnect, today I’ll walk you through installing a Kong Gateway node (Data Plane) on Azure Kubernetes Service.

If you're new to Kong, I recommend checking out my previous article before proceeding [Getting Started with Kong API Management](https://adityasingh-lab.github.io/posts/kong-konnect-getting-started/){:target="_blank"}.

## Prerequisites

- Azure account with appropriate permission to create AKS.
- Knowledge on basic cloud components.
- Sign-up and access to Kong Konnect.
- Install [Azure CLI](https://learn.microsoft.com/en-gb/cli/azure/install-azure-cli?view=azure-cli-latest){:target="_blank"}.
- Install [kubectl](https://learn.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az-aks-install-cli){:target="_blank"}.
- Install [Helm 3](https://helm.sh/docs/intro/install){:target="_blank"}.

## Instantiate Azure Kubernetes Service (AKS)

- Navigate to Azure Portal and search for Kubernetes Service
- Select create new and select Kubernetes Cluster

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-selection.png)
_AKS Selection ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-basic-form.png)
_AKS basic form ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-create-form.png)
_Review and Create AKS ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-deployment-complete.png)
_AKS Deployment Completed ☝️_

## Connect AKS

- Ensure to install Azure CLI and Kubectl as mentioned earlier in prerequisites.
- Click connect and follow the screen to ensure connectivity is successful.

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-connect-steps.png)
_AKS Connect Steps ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/az-login.png)
_AZ CLI Login ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/az-cluster-cred.png)
_Download AKS Cluster Credential ☝️_

### Verify the AKS Connectivity

Verify the workload by executing the command `kubectl get deployments --all-namespaces=true`

![](assets/img/posts/2025-02-27-data-pane-node-aks/az-aks-login-verify.png)
_AKS remote connectivity verification ☝️_

## Setup Gateway Node

- Login to Kong Konnect portal and create a new Gateway Manager as 'Self-Managed Hybrid'.

![](/assets/img/posts/2025-02-27-data-pane-node-aks/konnect-new-gateway-manager.png)
_Kong Konnect New Gateway Manager ☝️_

- On the next screen, select Platform as Kubernetes.
- You should see deployment steps to be followed.
- Follow the steps 1 and 2.

### Step 1 & 2 : Complete pre-reqs and setup helm

![](/assets/img/posts/2025-02-27-data-pane-node-aks/konnect-k8s-dpnode-step-1-and-2.png)
_Kong Konnect Data Plane K8s node steps 1 and 2 ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-create-kong-ns.png)
_AKS Create Kong Namespace ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/help-repo-update.png)
_Helm Repo Update ☝️_

### Step 3 : Generate cert and key pair

![](/assets/img/posts/2025-02-27-data-pane-node-aks/konnect-k8s-dpnode-step-3.png)
_Kong Konnect Data Plane K8s node steps 3 ☝️_

- Store the certificate locally in a folder with the name as tls.crt and tls.key and execute the command (modify the path param for cert and key)

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-create-secret.png)
_AKS Create Secret ☝️_


### Step 4 : Configuration Parameter

- Copy the content and store it in value.yaml. Execute the final command as mentioned

![](/assets/img/posts/2025-02-27-data-pane-node-aks/konnect-k8s-dpnode-step-4.png)
_Kong Konnect Data Plane k8s node steps 4 ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-step-4-cli-execution.png)
_AKS Kong Konnect Step 4 execution - as cli ☝️_

- In Azure AKS UI, after few minutes, you should see successful workload and data-plane found prompt in Kong Konnect

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-step-4-success-data-plane.png)
_'Success Data Plane Found' Prompt ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/aks-step-4-success-kong-workloads.png)
_Workload in AKS ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/success-connection-konnect-aks.png)
_Verify Success Connection between Kong and AKS ☝️_

## Create Gateway Service

- Create new gateway service as below screenshots

![](/assets/img/posts/2025-02-27-data-pane-node-aks/new-gw-service.png)
_New Gateway Service Form ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/new-route.png)
_New Gateway Route ☝️_

## Testing

- Get external IP address, by executing `kubectl get svc --namespace kong` or from Azure UI , navigate AKS >  Services and ingresses (in Kubernetes Resources) > my-kong-kong-proxy.

![](/assets/img/posts/2025-02-27-data-pane-node-aks/get-external-ip-address.png)
_Get External IP address☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/test-gateway-service.png)
_Postman - Test Gateway Service ☝️_

![](/assets/img/posts/2025-02-27-data-pane-node-aks/api-usage-analytics-screenshot.png)
_API Usage from Kong Analytics ☝️_

Please do let me know your thoughts and any question in comments.

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){:target="_blank"}
