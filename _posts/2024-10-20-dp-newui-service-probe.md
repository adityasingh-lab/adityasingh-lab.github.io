---
title: "DataPower: Enabling troubleshooting probe in New UI"
date: 20-Oct-2024
categories: [IBM, DataPower ]
tags: [DataPower, debug, probe]
author: aditya-singh
image: /assets/img/posts/2024-10-20-dp-newui-service-probe/p0-default-probe-page.png
---

Hello Tech Enthusiasts 👋,

As we all know by now, our beloved WebGUI in DataPower will no longer be available beyond version 10.5.4 (something I’ll definitely miss). Previously, IBM introduced the Blueprint Console as a replacement, but it never quite lived up to expectations. Now, we have a new UI that we must adopt going forward.

I was a bit skeptical at first, especially since I was never comfortable using the earlier Blueprint Console — thankfully, it’s gone. But with the new UI, I must say, the moment I started using it, I was impressed. It's damn fast compared to the existing UI! One reason for the shift is also that the existing UI consumes a lot of memory, which is why it’s not recommended to enable and use it in Kubernetes or OpenShift environments. I believe things are finally heading in the right direction.

Anyway, when it comes to enabling the probe from the new UI, the approach is slightly different.

## Difference in existing WebGUI probe vs new UI probe

| Item                          | WebGUI                                                                                                                                                   | New UI                                                                                                                                                 |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Enabling Probe**            | We can enable directly from Service or from<br/>Troubleshooting Probe. No pre-setting <br/>needs to be enabled.                                          | We first need to enable gateway-peering ,<br/>following with probe setting. Then we can<br/>enable probe which also needed to add<br/>capture setting. |
| **Disabling Multiple Probes** | We cannot disable multiple probes at once.<br/>We would either need to disable one after<br/>the other manually or need automation<br/>through SSH/SOMA. | We can disable by simple disabling the<br/>probe setting (we'll talk about below).                                                                     |
| **Flush Probe Record**        | We can flush the probe to clear our all the<br/>recorded transactions.                                                                                   | In new UI, the one way is to delete and<br/>create a new probe capture.                                                                                |
| **Modify Probe Setting**      | We can edit the probe setting to capture only<br/>particular transactions.                                                                               | We need to create new capture probe<br/>every time and delete the existing (if<br/>required)  we need to modify.                                       |
| **Performance**               | Slow, compare to new UI                                                                                                                                  | Quite fast as compare to traditional<br/>WebGUI probe                                                                                                  |

## Steps for enabling probe in New UI

1.) As default, the probes are in disable state. This is because it's dependant on probe setting. If you logon to DataPower Homepage > Monitoring & Troubleshooting > Troubleshooting > Probe, you'd notice all probes are greyed out 👇:

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p0-default-probe-page.png)
_Default Probe Page_

2.) You can either directly click on Modify setting or search 'Probe' in search bar and select '_Probe Setting_'

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p1-search-probe-setting.png)
_Search Probe Setting_

3.) Click on edit '_default-gateway-peering_' 👇:

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p2-probe-setting-edit-gateway-peering.png)
_Default Probe Setting Page_

> The gateway peering instance is useful to synchronize the captured transaction data. This is much useful in case of IBM API Connect.
{: .prompt-tip }

4.) Enable gateway-peering 👇. Ensure to modify the configuration like ports / local-address. If there is multiple DataPower domain, ensure to change the port# to a unique value.

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p3-enable-gateway-peering.png)
_Gateway Peering Page_

5.) Enable Probe Setting and Apply/Save the config 👇:

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p4-save-probe-setting.png)
_Enable Probe Setting_

6.) Follow DataPower Homepage > Monitoring & Troubleshooting > Troubleshooting > Probe. Now you should see all 'Open Probe' icon now available. Select the probe that needs to capture : 👇

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p5-open-probe-troubleshooting.png)
_Troubleshooting Probe Page_

7.) After selection, we see following screen 👇:

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p6-add-create-probe.png)
_Service Probe Screen_

8.) Create probe capture. Enter Name e.g: 'capture_all' , capture count e.g. 50 and leave Interval as 0: 👇

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p7-create-probe-capture.png)
_Create Probe Capture_

9.) Hit the service, that should create the capture count. 👇

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p8-capture-probe-count.png)
_Capture Probe Count_

10.) Select the probe above and that should take to the list of transaction ID. Similar to WebGUI probe

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p9-probe-transactions.png)
_Transaction list_

11.) Select the particular transaction which would take you to transaction debug (similar to WebGUI transaction debug)

![](/assets/img/posts/2024-10-20-dp-newui-service-probe/p10-probe-transaction-id.png)
_Debug Transaction_

— Keep Learning 😊

— Aditya Singh

>_If this article helped you in someway and want to support me, you can_ ... [![](/assets/img/posts/buymecoffee/default-yellow-ezgif.com-webp-to-jpg-converter.jpg)](https://buymeacoffee.com/aditya.singh){: width="700" height="400" }{:target="_blank"}
