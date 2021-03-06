---
title: 如何在 Azure VMSS 上使用 Azure CLI 配置系统分配标识和用户分配标识
description: 分步说明如何在 Azure VMSS 上使用 Azure CLI 配置系统分配标识和用户分配标识。
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 36df9d00d41f3c092320fa88772b41c9a41c6d8e
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237275"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>使用 Azure CLI 配置虚拟机规模集托管服务标识 (MSI)

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

托管服务标识为 Azure 服务提供了 Azure Active Directory 中的自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

在本文中，你将了解如何在 Azure 虚拟机规模集 (VMSS) 上使用 Azure CLI 执行以下托管服务标识操作：
- 在 Azure VMSS 上启用和禁用系统分配标识
- 在 Azure VMSS 上添加和删除用户分配标识


## <a name="prerequisites"></a>先决条件

- 如果不熟悉托管服务标识，请查阅[概述部分](overview.md)。 请务必了解[系统分配标识与用户分配标识之间的差异](overview.md#how-does-it-work)。
- 如果没有 Azure 帐户，请在继续前[注册免费帐户](https://azure.microsoft.com/free/)。
- 若要执行本文中的管理操作，帐户需要分配以下角色：
    - [虚拟机参与者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)，可创建虚拟机规模集，并从虚拟机规模集启用和删除系统和/或用户分配的托管标识。
    - [托管标识参与者](/azure/role-based-access-control/built-in-roles#managed-identity-contributor)角色，可以创建用户分配的标识。
    - [托管标识操作员](/azure/role-based-access-control/built-in-roles#managed-identity-operator)角色，可在虚拟机规模集中分配和删除用户分配的标识。
- 若要运行 CLI 脚本示例，可以使用下列三种方法：
    - 在 Azure 门户中使用 [Azure Cloud Shell](../../cloud-shell/overview.md)（见下一部分）。
    - 单击各代码块右上角的“试运行”按钮，使用嵌入的 Azure Cloud Shell。
    - 如果希望使用本地 CLI 控制台，可以[安装最新版 CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)（2.0.13 或更高版本）。 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>系统分配标识

在此部分中，你将了解如何使用 Azure CLI 为 Azure VMSS 启用和禁用系统分配标识。

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>在创建 Azure 虚拟机规模集的过程中启用系统分配标识

若要创建启用了系统分配标识的虚拟机规模集，请执行以下操作：

1. 如果在本地控制台中使用 Azure CLI，首先请使用 [az login](/cli/azure/reference-index#az_login) 登录到 Azure。 使用与要在其下部署虚拟机规模集的 Azure 订阅关联的帐户：

   ```azurecli-interactive
   az login
   ```

2. 使用 [az group create](/cli/azure/group/#az_group_create)，创建用于容纳和部署虚拟机规模集及其相关资源的[资源组](../../azure-resource-manager/resource-group-overview.md#terminology)。 如果已有要改用的资源组，则可以跳过此步骤：

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. 使用 [az vmss create](/cli/azure/vmss/#az_vmss_create) 创建虚拟机规模集。 下面的示例创建名为 *myVMSS* 且已启用系统分配标识的虚拟机规模集（应 `--assign-identity` 参数的要求）。 `--admin-username` 和 `--admin-password` 参数指定用于登录虚拟机的管理用户名和密码帐户。 针对自己的环境相应地更新这些值： 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>在现有 Azure 虚拟机规模集上启用系统分配标识

如果需要在现有 Azure 虚拟机规模集上启用系统分配标识，请执行以下操作：

1. 如果在本地控制台中使用 Azure CLI，首先请使用 [az login](/cli/azure/reference-index#az_login) 登录到 Azure。 使用与包含虚拟机规模集的 Azure 订阅关联的帐户。

   ```azurecli-interactive
   az login
   ```

2. 使用 [az vmss identity assign](/cli/azure/vmss/identity/#az_vmss_identity_assign) 命令为现有 VM 启用系统分配标识：

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>从 Azure 虚拟机规模集中禁用系统分配标识

如果某个虚拟机规模集不再需要系统分配的标识，但仍需要用户分配的标识，请使用以下命令：

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

如果某个虚拟机不再需要系统分配的标识，且没有用户分配的标识，请使用以下命令：

> [!NOTE]
> 值 `none` 区分大小写。 它必须为小写。 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

若要删除 MSI VM 扩展，请使用 [az vmss identity remove](/cli/azure/vmss/identity/#az_vmss_remove_identity) 命令从 VMSS 中删除系统分配标识：

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>用户分配标识

在此部分中，你将了解如何使用 Azure CLI 启用和删除用户分配标识。

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>在创建 Azure VMSS 的过程中分配用户分配标识

此部分介绍如何创建 VMSS 以及向 VMSS 分配用户分配标识。 如果已有要使用的 VMSS，请跳过此部分，转到下一部分。

1. 如果已有要使用的资源组，可跳过此步骤。 使用 [az group create](/cli/azure/group/#az_group_create) 创建用于容纳和部署用户分配标识的[资源组](~/articles/azure-resource-manager/resource-group-overview.md#terminology)。 请务必将 `<RESOURCE GROUP>` 和 `<LOCATION>` 参数值替换为自己的值。 :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. 使用 [az identity create](/cli/azure/identity#az-identity-create) 创建用户分配标识。  `-g` 参数指定要创建用户分配标识的资源组，`-n` 参数指定其名称。 请务必将 `<RESOURCE GROUP>` 和 `<USER ASSIGNED IDENTITY NAME>` 参数值替换为自己的值：

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   响应包含所创建的用户分配标识的详细信息，与以下示例类似。 下一步会用到分配给用户分配标识的资源 `id` 值。

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. 使用 [az vmss create](/cli/azure/vmss/#az-vmss-create) 创建 VMSS。 下面的示例创建与新用户分配标识关联的 VMSS，用 `--assign-identity` 参数指定。 请务必将 `<RESOURCE GROUP>`、`<VMSS NAME>`、`<USER NAME>`、`<PASSWORD>` 和 `<USER ASSIGNED IDENTITY ID>` 参数值替换为你自己的值。 对于 `<USER ASSIGNED IDENTITY ID>`，请使用上一步创建的用户分配标识的资源 `id` 属性： 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>向现有 Azure VM 分配用户分配标识

1. 使用 [az identity create](/cli/azure/identity#az-identity-create) 创建用户分配标识。  `-g` 参数指定要创建用户分配标识的资源组，`-n` 参数指定其名称。 请务必将 `<RESOURCE GROUP>` 和 `<USER ASSIGNED IDENTITY NAME>` 参数值替换为自己的值：

    > [!IMPORTANT]
    > 目前不支持创建名称中具有特殊字符（即下划线）的用户分配标识。 请使用字母数字字符。 稍后返回查看更新。  有关详细信息，请参阅 [FAQ 和已知问题](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
响应包含所创建的用户分配标识的详细信息，与以下示例类似。 下一步会用到分配给用户分配标识的资源 `id` 值。

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. 使用 [az vmss identity assign](/cli/azure/vmss/identity#az_vm_assign_identity) 将用户分配标识分配给 VMSS。 请务必将 `<RESOURCE GROUP>` 和 `<VMSS NAME>` 参数值替换为自己的值。 `<USER ASSIGNED IDENTITY ID>` 将为上一步创建的用户分配标识的资源 `id` 属性：

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>从 Azure 虚拟机规模集中删除用户分配的标识

若要从虚拟机规模集中删除用户分配的标识，请使用 [az vmss identity remove](/cli/azure/vmss/identity#az-vmss-identity-remove)。 请务必将 `<RESOURCE GROUP>` 和 `<VMSS NAME>` 参数值替换为自己的值。 `<MSI NAME>` 将为用户分配标识的 `name` 属性，可通过 `az vmss identity show` 在 VM 的标识部分中找到：

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

如果虚拟机规模集没有系统分配的标识，并且你想要从中删除所有用户分配的标识，请使用以下命令：

> [!NOTE]
> 值 `none` 区分大小写。 它必须为小写。

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

如果虚拟机规模集同时具有系统分配的标识和用户分配的标识，则可通过切换到仅使用系统分配的标识，删除所有用户分配的标识。 请使用以下命令：

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>后续步骤

- [托管服务标识概述](overview.md)
- 有关完整的 Azure 虚拟机规模集创建快速入门，请参阅： 

  - [使用 CLI 创建虚拟机规模集](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















