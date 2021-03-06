---
title: 使用 CLI 在 OS 磁盘之间交换
description: 使用 CLI 更改由 Azure 虚拟机使用的操作系统磁盘。
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: adcbc5b0ba0056c79576e8d4c8ded2150b206513
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "87371866"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>使用 CLI 更改由 Azure VM 使用的 OS 磁盘


如果有现有 VM，但希望将磁盘交换为备份磁盘或其他 OS 磁盘，则可使用 Azure CLI 交换 OS 磁盘。 无需删除和重新创建 VM。 甚至可在另一资源组中使用托管磁盘，只要该磁盘尚未使用。

需要停止/取消分配 VM，然后才可将该托管磁盘的资源 ID 替换为其他托管磁盘的资源 ID。 

请确保 VM 大小和存储类型与要附加的磁盘兼容。 例如，如果要使用的磁盘位于高级存储中，则 VM 需要能使用高级存储（如 DS 系列大小）。

本文需要 Azure CLI 2.0.25 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI]( /cli/azure/install-azure-cli)。 


使用 [az disk list](/cli/azure/disk) 获取资源组中的磁盘列表。

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


在交换磁盘之前，使用 [az vm stop](/cli/azure/vm) 停止\取消分配 VM。

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


使用 [az vm update](/cli/azure/vm#az-vm-update) 以及新磁盘的完整资源 ID 获取 `--osdisk` 参数 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
使用 [az vm start](/cli/azure/vm) 重启 VM：

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**后续步骤**

要创建磁盘副本，请参阅[拍摄磁盘快照](snapshot-copy-managed-disk.md)。
