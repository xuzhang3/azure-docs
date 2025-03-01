---
title: Tutorial - Stream logs to an Azure event hub | Microsoft Docs
description: Learn how to set up Azure Diagnostics to push Azure Active Directory logs to an event hub
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''

ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 09/02/2021
ms.author: markvi
ms.reviewer: besiler

# Customer intent: As an IT administrator, I want to learn how to route Azure AD logs to an event hub so I can integrate it with my third party SIEM system.
ms.collection: M365-identity-device-management
---
# Tutorial: Stream Azure Active Directory logs to an Azure event hub

In this tutorial, you learn how to set up Azure Monitor diagnostics settings to stream Azure Active Directory (Azure AD) logs to an Azure event hub. Use this mechanism to integrate your logs with third-party Security Information and Event Management (SIEM) tools, such as Splunk and QRadar.

## Prerequisites 

To use this feature, you need:

* An Azure subscription. If you don't have an Azure subscription, you can [sign up for a free trial](https://azure.microsoft.com/free/).
* An Azure AD tenant.
* A user who's a *global administrator* or *security administrator* for the Azure AD tenant.
* An Event Hubs namespace and an event hub in your Azure subscription. Learn how to [create an event hub](../../event-hubs/event-hubs-create.md).

## Stream logs to an event hub

1. Sign in to the [Azure portal](https://portal.azure.com). 

1. Select **Azure Active Directory** > **Audit logs**. 

1. Select **Export Data Settings**.  
    
1. In the **Diagnostics settings** pane, do either of the following:
    * To change existing settings, select **Edit setting**.
    * To add new settings, select **Add diagnostics setting**.  
      You can have up to three settings.

1. Select the **Stream to an event hub** check box, and then select **Event Hub/Configure**.

   [ ![Export settings](./media/tutorial-azure-monitor-stream-logs-to-event-hub/diagnostic-setting-stream-to-event-hub.png) ](./media/tutorial-azure-monitor-stream-logs-to-event-hub/diagnostic-setting-stream-to-event-hub.png)
   
   1. Select the Azure subscription and Event Hubs namespace that you want to route the logs to.  
    The subscription and Event Hubs namespace must both be associated with the Azure AD tenant that the logs stream from. You can also specify an event hub within the Event Hubs namespace to which logs should be sent. If no event hub is specified, an event hub is created in the namespace with the default name **insights-logs-audit**.

   1. Select any combination of the following items:
       - To send audit logs to the event hub, select the **AuditLogs** check box. 
       - To send interactive user sign-in logs to the event hub, select the **SignInLogs** check box.
       - To send non-interactive user sign-in logs to the event hub, select the **NonInteractiveUserSignInLogs** check box. 
       - To send service principal sign-in logs to the event hub, select the **ServicePrincipalSignInLogs** check box.
       - To send managed identity sign-in logs to the event hub, select the **ManagedIdentitySignInLogs** check box.
       - To send provisioning logs to the event hub, select the **ProvisioningLogs** check box.
       - To send sign-ins sent to Azure AD by an AD FS Connect Health agent, select the **ADFSSignInLogs** check box.
       - To send risky user information, select the **RiskyUsers** check box.
       - To send user risk events information, select the **UserRiskEvents** check box. 

       > [!NOTE]
       > Some sign-in categories contain large amounts of log data depending on your tenant’s configuration. In general, the non-interactive user sign-ins and service principal sign-ins can be 5 to 10 times larger than the interactive user sign-ins.

   1. Select **Save** to save the setting.

1. After about 15 minutes, verify that events are displayed in your event hub. To do so, go to the event hub from the portal and verify that the **incoming messages** count is greater than zero. 

    [ ![Audit logs](./media/tutorial-azure-monitor-stream-logs-to-event-hub/azure-monitor-event-hub-instance.png)](./media/tutorial-azure-monitor-stream-logs-to-event-hub/azure-monitor-event-hub-instance.png)

## Access data from your event hub

After data is displayed in the event hub, you can access and read the data in two ways:

* **Configure a supported SIEM tool**. To read data from the event hub, most tools require the event hub connection string and certain permissions to your Azure subscription. Third-party tools with Azure Monitor integration include, but aren't limited to:
    
    * **ArcSight**: For more information about integrating Azure AD logs with ArcSight, see [Integrate Azure Active Directory logs with ArcSight using Azure Monitor](howto-integrate-activity-logs-with-arcsight.md).
    
    * **Splunk**: For more information about integrating Azure AD logs with Splunk, see [Integrate Azure AD logs with Splunk by using Azure Monitor](./howto-integrate-activity-logs-with-splunk.md).
    
    * **IBM QRadar**: The DSM and Azure Event Hub Protocol are available for download at [IBM support](https://www.ibm.com/support). For more information about integration with Azure, go to the [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) site.
    
    * **Sumo Logic**: To set up Sumo Logic to consume data from an event hub, see [Install the Azure AD app and view the dashboards](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards). 

* **Set up custom tooling**. If your current SIEM isn't supported in Azure Monitor diagnostics yet, you can set up custom tooling by using the Event Hubs API. To learn more, see the [Getting started receiving messages from an event hub](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md).

## Next steps

* [Create diagnostic settings to send platform logs and metrics to different destinations](../../azure-monitor/essentials/diagnostic-settings.md)
* [Integrate Azure Active Directory logs with ArcSight using Azure Monitor](howto-integrate-activity-logs-with-arcsight.md)
* [Integrate Azure AD logs with Splunk by using Azure Monitor](./howto-integrate-activity-logs-with-splunk.md)
* [Integrate Azure AD logs with SumoLogic by using Azure Monitor](howto-integrate-activity-logs-with-sumologic.md)
* [Integrate Azure AD logs with Elastic using an event hub](https://github.com/Microsoft/azure-docs/blob/master/articles/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Interpret audit logs schema in Azure Monitor](./overview-reports.md)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
