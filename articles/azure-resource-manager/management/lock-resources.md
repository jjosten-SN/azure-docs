---
title: Protect your Azure resources with a lock
description: You can safeguard Azure resources from updates or deletions by locking all users and roles.
ms.topic: conceptual
ms.date: 05/13/2022
ms.custom: devx-track-azurecli, devx-track-azurepowershell
---

# Lock your resources to protect your infrastructure

As an administrator, you can lock an Azure subscription, resource group, or resource to protect them from accidental user deletions and modifications. The lock overrides any user permissions.

You can set locks that prevent either deletions or modifications. In the portal, these locks are called **Delete** and **Read-only**. In the command line, these locks are called **CanNotDelete** and **ReadOnly**. In the left navigation panel, the subscription lock feature's name is **Resource locks**, while the resource group lock feature's name is **Locks**.  

- **CanNotDelete** means authorized users can read and modify a resource, but they can't delete it.
- **ReadOnly** means authorized users can read a resource, but they can't delete or update it. Applying this lock is similar to restricting all authorized users to the permissions that the **Reader** role provides.

Unlike role-based access control (RBAC), you use management locks to apply a restriction across all users and roles. To learn about setting permissions for users and roles, see [Azure RBAC](../../role-based-access-control/role-assignments-portal.md).

## Lock inheritance

When you apply a lock at a parent scope, all resources within that scope inherit the same lock. Even resources you add later inherit the same parent lock. The most restrictive lock in the inheritance takes precedence.

If you have a **Delete** lock on a resource and attempt to delete its resource group, the feature blocks the whole delete operation. Even if the resource group or other resources in the resource group are unlocked, the deletion doesn't happen. You never have a partial deletion.

When you [cancel an Azure subscription](../../cost-management-billing/manage/cancel-azure-subscription.md#what-happens-after-subscription-cancellation):
* A resource lock doesn't block the subscription cancellation.
* Azure preserves your resources by deactivating them instead of immediately deleting them.
* Azure only deletes your resources permanently after a waiting period.

## Understand scope of locks

> [!NOTE]
> Locks only apply to control plane Azure operations and not to data plane operations. 

Azure control plane operations go to `https://management.azure.com`. Azure data plane operations go to your service instance, such as `https://myaccount.blob.core.windows.net/`. See [Azure control plane and data plane](control-plane-and-data-plane.md). To discover which operations use the control plane URL, see the [Azure REST API](/rest/api/azure/).

The distinction means locks protect a resource from changes, but they don't restrict how a resource performs its functions. A ReadOnly lock, for example, on an SQL Database logical server, protects it from deletions or modifications. It allows you to create, update, or delete data in the server database. Data plane operations allow data transactions. These requests don't go to `https://management.azure.com`.

## Considerations before applying your locks

Applying locks can lead to unexpected results. Some operations, which don't seem to modify a resource, require blocked actions. Locks prevent the POST method from sending data to the Azure Resource Manager (ARM) API. Some common examples of blocked operations are:

- A read-only lock on a **storage account** prevents users from listing the account keys. A POST request handles the Azure Storage [List Keys](/rest/api/storagerp/storageaccounts/listkeys) operation to protect access to the account keys. The account keys provide complete access to data in the storage account. When a read-only lock is configured for a storage account, users who don't have the account keys need to use Azure AD credentials to access blob or queue data. A read-only lock also prevents the assignment of Azure RBAC roles that are scoped to the storage account or to a data container (blob container or queue). 

- A read-only lock on a **storage account** protects RBAC assignments scoped for a storage account or a data container (blob container or queue).

- A cannot-delete lock on a **storage account** doesn't protect account data from deletion or modification. It only protects the storage account from deletion. If a request uses [data plane operations](control-plane-and-data-plane.md#data-plane), the lock on the storage account doesn't protect blob, queue, table, or file data within that storage account. If the request uses [control plane operations](control-plane-and-data-plane.md#control-plane), however, the lock protects those resources.

  If a request uses [File Shares - Delete](/rest/api/storagerp/file-shares/delete), for example, which is a control plane operation, the deletion fails. If the request uses [Delete Share](/rest/api/storageservices/delete-share), which is a data plane operation, the deletion succeeds. We recommend that you use a control plane operation.

- A read-only lock on a **storage account** doesn't prevent its data from deletion or modification. It also doesn't protect its blob, queue, table, or file data.

- A read-only lock on an **App Service** resource prevents Visual Studio Server Explorer from displaying files for the resource because that interaction requires write access.

- A read-only lock on a **resource group** that contains an **App Service plan** prevents you from [scaling up or out of the plan](../../app-service/manage-scale-up.md).

- A read-only lock on a **resource group** that contains a **virtual machine** prevents all users from starting or restarting a virtual machine. These operations require a POST method request.

- A read-only lock on a **resource group** that contains an **automation account** prevents all runbooks from starting. These operations require a POST method request.

- A cannot-delete lock on a **resource group** prevents Azure Resource Manager from [automatically deleting deployments](../templates/deployment-history-deletions.md) in the history. If you reach 800 deployments in the history, your deployments fail.

- A cannot-delete lock on the **resource group** created by **Azure Backup Service** causes backups to fail. The service supports a maximum of 18 restore points. When locked, the backup service can't clean up restore points. For more information, see [Frequently asked questions-Back up Azure VMs](../../backup/backup-azure-vm-backup-faq.yml).

- A cannot-delete lock on a **resource group** prevents **Azure Machine Learning** from autoscaling [Azure Machine Learning compute clusters](../../machine-learning/concept-compute-target.md#azure-machine-learning-compute-managed) to remove unused nodes.

- A read-only lock on a **Log Analytics workspace** prevents **User and Entity Behavior Analytics (UEBA)** from being enabled.

- A read-only lock on a **subscription** prevents **Azure Advisor** from working correctly. Advisor is unable to store the results of its queries.

- A read-only lock on an **Application Gateway** prevents you from getting the backend health of the application gateway. That [operation uses a POST method](/rest/api/application-gateway/application-gateways/backend-health), which a read-only lock blocks.

- A read-only lock on an Azure Kubernetes Service (AKS) cluster limits how you can access cluster resources through the portal. A read-only lock prevents you from using the AKS cluster's Kubernetes resources section in the Azure portal to choose a cluster resource. These operations require a POST method request for authentication.

## Who can create or delete locks

To create or delete management locks, you need access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions. Only the **Owner** and the **User Access Administrator** built-in roles can create and delete management locks. You can create a custom role with the required permissions.

## Managed applications and locks

Some Azure services, such as Azure Databricks, use [managed applications](../managed-applications/overview.md) to implement the service. In that case, the service creates two resource groups. One is an unlocked resource group that contains a service overview. The other is a locked resource group that contains the service infrastructure.

If you try to delete the infrastructure resource group, you get an error stating that the resource group is locked. If you try to delete the lock for the infrastructure resource group, you get an error stating that the lock can't be deleted because a system application owns it.

Instead, delete the service, which also deletes the infrastructure resource group.

For managed applications, choose the service you deployed.

![Select service](./media/lock-resources/select-service.png)

Notice the service includes a link for a **Managed Resource Group**. That resource group holds the infrastructure and is locked. You can only delete it indirectly.

![Show managed group](./media/lock-resources/show-managed-group.png)

To delete everything for the service, including the locked infrastructure resource group, choose **Delete** for the service.

![Delete service](./media/lock-resources/delete-service.png)

## Configure locks

### Portal

[!INCLUDE [resource-manager-lock-resources](../../../includes/resource-manager-lock-resources.md)]

### Template

When using an ARM template or Bicep file to deploy a lock, it's good to understand how the deployment scope and the lock scope work together. To apply a lock at the deployment scope, such as locking a resource group or a subscription, leave the scope property unset. When locking a resource, within the deployment scope, set the scope property on the lock.

The following template applies a lock to the resource group it's deployed to. Notice there isn't a scope property on the lock resource because the lock scope matches the deployment scope. Deploy this template at the resource group level.

# [JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/locks",
      "apiVersion": "2016-09-01",
      "name": "rgLock",
      "properties": {
        "level": "CanNotDelete",
        "notes": "Resource group should not be deleted."
      }
    }
  ]
}
```

# [Bicep](#tab/bicep)

```bicep
resource createRgLock 'Microsoft.Authorization/locks@2016-09-01' = {
  name: 'rgLock'
  properties: {
    level: 'CanNotDelete'
    notes: 'Resource group should not be deleted.'
  }
}
```

---

To create a resource group and lock it, deploy the following template at the subscription level.

# [JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2021-04-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2021-04-01",
      "name": "lockDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Authorization/locks",
              "apiVersion": "2016-09-01",
              "name": "rgLock",
              "properties": {
                "level": "CanNotDelete",
                "notes": "Resource group and its resources should not be deleted."
              }
            }
          ],
          "outputs": {}
        }
      }
    }
  ],
  "outputs": {}
}
```

# [Bicep](#tab/bicep)

The main Bicep file creates a resource group and uses a [module](../bicep/modules.md) to create the lock.

```Bicep
targetScope = 'subscription'

param rgName string
param rgLocation string

resource createRg 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: rgName
  location: rgLocation
}

module deployRgLock './lockRg.bicep' = {
  name: 'lockDeployment'
  scope: resourceGroup(createRg.name)
}
```

The module uses a Bicep file named _lockRg.bicep_ that adds the resource group lock.

```bicep
resource createRgLock 'Microsoft.Authorization/locks@2016-09-01' = {
  name: 'rgLock'
  properties: {
    level: 'CanNotDelete'
    notes: 'Resource group and its resources should not be deleted.'
  }
}
```

---

When applying a lock to a **resource** within the resource group, add the scope property. Set the scope to the name of the resource to lock.

The following example shows a template that creates an app service plan, a website, and a lock on the website. The lock's scope is set to the website.

# [JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "hostingPlanName": {
      "type": "string"
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2020-12-01",
      "name": "[parameters('hostingPlanName')]",
      "location": "[parameters('location')]",
      "sku": {
        "tier": "Free",
        "name": "f1",
        "capacity": 0
      },
      "properties": {
        "targetWorkerCount": 1
      }
    },
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2020-12-01",
      "name": "[variables('siteName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      }
    },
    {
      "type": "Microsoft.Authorization/locks",
      "apiVersion": "2016-09-01",
      "name": "siteLock",
      "scope": "[concat('Microsoft.Web/sites/', variables('siteName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
      ],
      "properties": {
        "level": "CanNotDelete",
        "notes": "Site should not be deleted."
      }
    }
  ]
}
```

# [Bicep](#tab/bicep)

```Bicep
param hostingPlanName string
param location string = resourceGroup().location

var siteName = concat('ExampleSite', uniqueString(resourceGroup().id))

resource serverFarm 'Microsoft.Web/serverfarms@2020-12-01' = {
  name: hostingPlanName
  location: location
  sku: {
    tier: 'Free'
    name: 'f1'
    capacity: 0
  }
  properties: {
    targetWorkerCount: 1
  }
}

resource webSite 'Microsoft.Web/sites@2020-12-01' = {
  name: siteName
  location: location
  properties: {
    serverFarmId: serverFarm.name
  }
}

resource siteLock 'Microsoft.Authorization/locks@2016-09-01' = {
  name: 'siteLock'
  scope: webSite
  properties:{
    level: 'CanNotDelete'
    notes: 'Site should not be deleted.'
  }
}
```

---

### Azure PowerShell

You lock deployed resources with Azure PowerShell by using the [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) command.

To lock a resource, provide the name of the resource, its resource type, and its resource group name.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

To lock a resource group, provide the name of the resource group.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

To get information about a lock, use [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock). To get all the locks in your subscription, use:

```azurepowershell-interactive
Get-AzResourceLock
```

To get all locks for a resource, use:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

To get all locks for a resource group, use:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

To delete a lock for a resource, use:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

To delete a lock for a resource group, use:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup).LockId
Remove-AzResourceLock -LockId $lockId
```

### Azure CLI

You lock deployed resources with Azure CLI by using the [az lock create](/cli/azure/lock#az-lock-create) command.

To lock a resource, provide the name of the resource, its resource type, and its resource group name.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

To lock a resource group, provide the name of the resource group.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

To get information about a lock, use [az lock list](/cli/azure/lock#az-lock-list). To get all the locks in your subscription, use:

```azurecli
az lock list
```

To get all locks for a resource, use:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

To get all locks for a resource group, use:

```azurecli
az lock list --resource-group exampleresourcegroup
```

To delete a lock for a resource, use:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

To delete a lock for a resource group, use:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup  --output tsv --query id)
az lock delete --ids $lockid
```

### REST API

You can lock deployed resources with the [REST API for management locks](/rest/api/resources/managementlocks). The REST API lets you create and delete locks and retrieve information about existing locks.

To create a lock, run:

```http
PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}
```

The scope could be a subscription, resource group, or resource. The lock name can be whatever you want to call it. For the API version, use **2016-09-01**.

In the request, include a JSON object that specifies the lock properties.

```json
{
  "properties": {
  "level": "CanNotDelete",
  "notes": "Optional text notes."
  }
}
```

## Next steps

- To learn about logically organizing your resources, see [Using tags to organize your resources](tag-resources.md).
- You can apply restrictions and conventions across your subscription with customized policies. For more information, see [What is Azure Policy?](../../governance/policy/overview.md).
- For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](/azure/architecture/cloud-adoption-guide/subscription-governance).