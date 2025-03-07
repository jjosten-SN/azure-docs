---
title: Smart detection rule settings - Azure Application Insights
description: Automate management and configuration of Azure Application Insights smart detection rules with Azure Resource Manager Templates
ms.topic: conceptual
ms.date: 02/14/2021
ms.reviewer: yagil
---
# Manage Application Insights smart detection rules using Azure Resource Manager templates

>[!NOTE]
>You can migrate your Application Insight resources to alerts-bases smart detection (preview). The migration creates alert rules for the different smart detection modules. Once created, you can manage and configure these rules just like any other Azure Monitor alert rules. You can also configure action groups for these rules, thus enabling multiple methods of taking actions or triggering notification on new detections.
>
> See [Smart Detection Alerts migration](../alerts/alerts-smart-detections-migration.md) for more details on the migration process and the behavior of smart detection after the migration.
> 

Smart detection rules in Application Insights can be managed and configured using [Azure Resource Manager templates](../../azure-resource-manager/templates/syntax.md).
This method can be used when deploying new Application Insights resources with Azure Resource Manager automation, or for modifying the settings of existing resources.

## Smart detection rule configuration

You can configure the following settings for a smart detection rule:
- If the rule is enabled (the default is **true**.)
- If emails should be sent to users associated to the subscription’s [Monitoring Reader](../../role-based-access-control/built-in-roles.md#monitoring-reader) and [Monitoring Contributor](../../role-based-access-control/built-in-roles.md#monitoring-contributor) roles when a detection is found (the default is **true**.)
- Any additional email recipients who should get a notification when a detection is found.
    -  Email configuration is not available for Smart Detection rules marked as _preview_.

To allow configuring the rule settings via Azure Resource Manager, the smart detection rule configuration is now available as an inner resource within the Application Insights resource, named **ProactiveDetectionConfigs**.
For maximal flexibility, each smart detection rule can be configured with unique notification settings.

## Examples

Below are a few examples showing how to configure the settings of smart detection rules using Azure Resource Manager templates.
All samples refer to an Application Insights resource named _“myApplication”_, and to the "long dependency duration smart detection rule", which is internally named _“longdependencyduration”_.
Make sure to replace the Application Insights resource name, and to specify the relevant smart detection rule internal name. Check the table below for a list of the corresponding internal Azure Resource Manager names for each smart detection rule.

### Disable a smart detection rule

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "Application_Type": "web"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### Disable sending email notifications for a smart detection rule

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "Application_Type": "web"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### Add additional email recipients for a smart detection rule

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "Application_Type": "web"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ["alice@contoso.com", "bob@contoso.com"],
            "enabled": true
          }
        }
      ]
    }

```


## Smart detection rule names

Below is a table of smart detection rule names as they appear in the portal, along with their internal names, that should be used in the Azure Resource Manager template.

> [!NOTE]
> Smart detection rules marked as _preview_ don’t support email notifications. Therefore, you can only set the _enabled_ property for these rules. 

| Azure portal rule name | Internal name
|:---|:---|
| Slow page load time |	slowpageloadtime |
| Slow server response time | slowserverresponsetime |
| Long dependency duration | longdependencyduration |
| Degradation in server response time | degradationinserverresponsetime |
| Degradation in dependency duration | degradationindependencyduration |
| Degradation in trace severity ratio (preview) | extension_traceseveritydetector |
| Abnormal rise in exception volume (preview) | extension_exceptionchangeextension |
| Potential memory leak detected (preview) | extension_memoryleakextension |
| Potential security issue detected (preview) | extension_securityextensionspackage |
| Abnormal rise in daily data volume (preview) | extension_billingdatavolumedailyspikeextension |

### Failure Anomalies alert rule

This Azure Resource Manager template demonstrates configuring a Failure Anomalies alert rule with a severity of 2.

> [!NOTE]
> Failure Anomalies is a global service therefore rule location is created on the global location.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "microsoft.alertsmanagement/smartdetectoralertrules",
            "apiVersion": "2019-03-01",
            "name": "Failure Anomalies - my-app",
            "location": "global", 
            "properties": {
                  "description": "Failure Anomalies notifies you of an unusual rise in the rate of failed HTTP requests or dependency calls.",
                  "state": "Enabled",
                  "severity": "2",
                  "frequency": "PT1M",
                  "detector": {
                  "id": "FailureAnomaliesDetector"
                  },
                  "scope": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/MyResourceGroup/providers/microsoft.insights/components/my-app"],
                  "actionGroups": {
                        "groupIds": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourcegroups/MyResourceGroup/providers/microsoft.insights/actiongroups/MyActionGroup"]
                  }
            }
        }
    ]
}
```

> [!NOTE]
> This Azure Resource Manager template is unique to the Failure Anomalies alert rule and is different from the other classic Smart Detection rules described in this article. If you want to manage Failure Anomalies manually this is done in Azure Monitor Alerts whereas all other Smart Detection rules are managed in the Smart Detection pane of the UI.

## Next Steps

Learn more about automatically detecting:

- [Failure anomalies](./proactive-failure-diagnostics.md)
- [Memory Leaks](./proactive-potential-memory-leak.md)
- [Performance anomalies](./proactive-performance-diagnostics.md)