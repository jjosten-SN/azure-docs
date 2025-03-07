---
title: Microsoft 365 Defender integration with Microsoft Sentinel | Microsoft Docs
description: Learn how using Microsoft 365 Defender together with Microsoft Sentinel lets you use Microsoft Sentinel as your universal incidents queue while seamlessly applying Microsoft 365 Defender's strengths to help investigate Microsoft 365 security incidents. Also, learn how to ingest Defender components' advanced hunting data into Microsoft Sentinel.
author: yelevin
ms.topic: conceptual
ms.date: 03/23/2022
ms.author: yelevin
ms.custom: ignite-fall-2021
---

# Microsoft 365 Defender integration with Microsoft Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

> [!IMPORTANT]
> The Microsoft 365 Defender connector is currently in **PREVIEW**. See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

> [!IMPORTANT]
>
> **Microsoft Defender for Cloud Apps** was formerly known as **Microsoft Cloud App Security** or **MCAS**.
>
> You may see the old names still in use for a period of time.

## Incident integration

Microsoft Sentinel's [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection) incident integration allows you to stream all Microsoft 365 Defender incidents into Microsoft Sentinel and keep them synchronized between both portals. Incidents from Microsoft 365 Defender (formerly known as Microsoft Threat Protection or MTP) include all associated alerts, entities, and relevant information, providing you with enough context to perform triage and preliminary investigation in Microsoft Sentinel. Once in Sentinel, incidents will remain bi-directionally synced with Microsoft 365 Defender, allowing you to take advantage of the benefits of both portals in your incident investigation.

This integration gives Microsoft 365 security incidents the visibility to be managed from within Microsoft Sentinel, as part of the primary incident queue across the entire organization, so you can see – and correlate – Microsoft 365 incidents together with those from all of your other cloud and on-premises systems. At the same time, it allows you to take advantage of the unique strengths and capabilities of Microsoft 365 Defender for in-depth investigations and a Microsoft 365-specific experience across the Microsoft 365 ecosystem. Microsoft 365 Defender enriches and groups alerts from multiple Microsoft 365 products, both reducing the size of the SOC’s incident queue and shortening the time to resolve. The component services that are part of the Microsoft 365 Defender stack are:

- **Microsoft Defender for Endpoint** (formerly Microsoft Defender ATP)
- **Microsoft Defender for Identity** (formerly Azure ATP)
- **Microsoft Defender for Office 365** (formerly Office 365 ATP)
- **Microsoft Defender for Cloud Apps** (formerly Microsoft Cloud App Security)

Other services whose alerts are collected by Microsoft 365 Defender include:

- **Microsoft Purview Data Loss Prevention (DLP)** ([Learn more](/microsoft-365/security/defender/investigate-dlp))

In addition to collecting alerts from these components and other services, Microsoft 365 Defender generates alerts of its own. It creates incidents from all of these alerts and sends them to Microsoft Sentinel.

### Common use cases and scenarios

- One-click connect of Microsoft 365 Defender incidents, including all alerts and entities from Microsoft 365 Defender components, into Microsoft Sentinel.

- Bi-directional sync between Sentinel and Microsoft 365 Defender incidents on status, owner, and closing reason.

- Application of Microsoft 365 Defender alert grouping and enrichment capabilities in Microsoft Sentinel, thus reducing time to resolve.

- In-context deep link between a Microsoft Sentinel incident and its parallel Microsoft 365 Defender incident, to facilitate investigations across both portals.

### Connecting to Microsoft 365 Defender

Once you have enabled the Microsoft 365 Defender data connector to [collect incidents and alerts](connect-microsoft-365-defender.md), Microsoft 365 Defender incidents will appear in the Microsoft Sentinel incidents queue, with **Microsoft 365 Defender** in the **Product name** field, shortly after they are generated in Microsoft 365 Defender.
- It can take up to 10 minutes from the time an incident is generated in Microsoft 365 Defender to the time it appears in Microsoft Sentinel.

- Incidents will be ingested and synchronized at no extra cost.

Once the Microsoft 365 Defender integration is connected, all the component alert connectors (Defender for Endpoint, Defender for Identity, Defender for Office 365, Defender for Cloud Apps) will be automatically connected in the background if they weren't already. If any component licenses were purchased after Microsoft 365 Defender was connected, the alerts and incidents from the new product will still flow to Microsoft Sentinel with no additional configuration or charge.

### Microsoft 365 Defender incidents and Microsoft incident creation rules

- Incidents generated by Microsoft 365 Defender, based on alerts coming from Microsoft 365 security products, are created using custom Microsoft 365 Defender logic.

- Microsoft incident-creation rules in Microsoft Sentinel also create incidents from the same alerts, using (a different) custom Microsoft Sentinel logic.

- Using both mechanisms together is completely supported, and can be used to facilitate the transition to the new Microsoft 365 Defender incident creation logic. Doing so will, however, create **duplicate incidents** for the same alerts.

- To avoid creating duplicate incidents for the same alerts, we recommend that customers turn off all **Microsoft incident creation rules** for Microsoft 365 products (Defender for Endpoint, Defender for Identity, and Defender for Office 365, and Defender for Cloud Apps) when connecting Microsoft 365 Defender. This can be done by disabling incident creation in the connector page. Keep in mind that if you do this, any filters that were applied by the incident creation rules will not be applied to Microsoft 365 Defender incident integration.

    > [!NOTE]
    > All Microsoft Defender for Cloud Apps alert types are now being onboarded to Microsoft 365 Defender.

### Working with Microsoft 365 Defender incidents in Microsoft Sentinel and bi-directional sync

Microsoft 365 Defender incidents will appear in the Microsoft Sentinel incidents queue with the product name **Microsoft 365 Defender**, and with similar details and functionality to any other Sentinel incidents. Each incident contains a link back to the parallel incident in the Microsoft 365 Defender portal.

As the incident evolves in Microsoft 365 Defender, and more alerts or entities are added to it, the Microsoft Sentinel incident will update accordingly.

Changes made to the status, closing reason, or assignment of a Microsoft 365 incident, in either Microsoft 365 Defender or Microsoft Sentinel, will likewise update accordingly in the other's incidents queue. The synchronization will take place in both portals immediately after the change to the incident is applied, with no delay. A refresh might be required to see the latest changes.

In Microsoft 365 Defender, all alerts from one incident can be transferred to another, resulting in the incidents being merged. When this merge happens, the Microsoft Sentinel incidents will reflect the changes. One incident will contain all the alerts from both original incidents, and the other incident will be automatically closed, with a tag of "redirected" added.

> [!NOTE]
> Incidents in Microsoft Sentinel can contain a maximum of 150 alerts. Microsoft 365 Defender incidents can have more than this. If a Microsoft 365 Defender incident with more than 150 alerts is synchronized to Microsoft Sentinel, the Sentinel incident will show as having “150+” alerts and will provide a link to the parallel incident in Microsoft 365 Defender where you will see the full set of alerts.

## Advanced hunting event collection

The Microsoft 365 Defender connector also lets you stream **advanced hunting** events - a type of raw event data - from Microsoft 365 Defender and its component services into Microsoft Sentinel. You can now *(as of April 2022)* collect [advanced hunting](/microsoft-365/security/defender/advanced-hunting-overview) events from *all* Microsoft 365 Defender components, and stream them straight into purpose-built tables in your Microsoft Sentinel workspace. These tables are built on the same schema that is used in the Microsoft 365 Defender portal, giving you complete access to the full set of advanced hunting events, and allowing you to do the following:

- Easily copy your existing Microsoft Defender for Endpoint/Office 365/Identity/Cloud Apps advanced hunting queries into Microsoft Sentinel.

- Use the raw event logs to provide further insights for your alerts, hunting, and investigation, and correlate these events with events from other data sources in Microsoft Sentinel.

- Store the logs with increased retention, beyond Microsoft 365 Defender’s or its components' default retention of 30 days. You can do so by configuring the retention of your workspace or by configuring per-table retention in Log Analytics.

## Next steps

In this document, you learned how to benefit from using Microsoft 365 Defender together with Microsoft Sentinel, using the Microsoft 365 Defender connector.

- Get instructions for [enabling the Microsoft 365 Defender connector](connect-microsoft-365-defender.md).
- Create [custom alerts](detect-threats-custom.md) and [investigate incidents](investigate-cases.md).
