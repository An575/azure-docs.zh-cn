---
title: 教程 - 通过 Azure CLI 创建和使用规模集的磁盘
description: 了解如何通过 Azure CLI 对虚拟机规模集创建和使用托管磁盘，包括如何添加、准备、列出和分离磁盘。
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 03/27/2018
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: a7e9e1fa567ae282a4472fa728e53e720bf8ff6f
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367918"
---
# <a name="tutorial-create-and-use-disks-with-virtual-machine-scale-set-with-the-azure-cli"></a>教程：通过 Azure CLI 对虚拟机规模集创建和使用磁盘
虚拟机规模集使用磁盘来存储 VM 实例的操作系统、应用程序和数据。 创建和管理规模集时，请务必选择适用于所需工作负荷的磁盘大小和配置。 本教程介绍如何创建和管理 VM 磁盘。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * OS 磁盘和临时磁盘
> * 数据磁盘数
> * 标准磁盘和高级磁盘
> * 磁盘性能
> * 附加和准备数据磁盘

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

如果选择在本地安装和使用 CLI，本教程要求运行 Azure CLI 2.0.29 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI]( /cli/azure/install-azure-cli)。


## <a name="default-azure-disks"></a>默认 Azure 磁盘
创建或缩放规模集时，会自动将两个磁盘附加到每个 VM 实例。

**操作系统磁盘** - 操作系统磁盘大小可达 2 TB，并可托管 VM 实例的操作系统。 默认情况下，OS 磁盘标记为“/dev/sda”  。 已针对 OS 性能优化了 OS 磁盘的磁盘缓存配置。 由于此配置，OS 磁盘不应  托管应用程序或数据。 对于应用程序和数据，请使用数据磁盘，本文后面会对其进行详细介绍。

**临时磁盘** - 临时磁盘使用 VM 实例所在的 Azure 主机上的固态硬盘。 这些磁盘具有高性能，可用于临时数据处理等操作。 但是，如果将 VM 实例移到新的主机，临时磁盘上存储的数据都会删除。 临时磁盘的大小由 VM 实例大小决定。 临时磁盘标记为“/dev/sdb”  ，且装载点为 /mnt  。

### <a name="temporary-disk-sizes"></a>临时磁盘大小
| 类型 | 常见大小 | 临时磁盘大小上限 (GiB) |
|----|----|----|
| [常规用途](../virtual-machines/sizes-general.md) | A、B、D 系列 | 1600 |
| [计算优化](../virtual-machines/sizes-compute.md) | F 系列 | 576 |
| [内存优化](../virtual-machines/sizes-memory.md) | D、E、G、M 系列 | 6144 |
| [存储优化](../virtual-machines/sizes-storage.md) | L 系列 | 5630 |
| [GPU](../virtual-machines/sizes-gpu.md) | N 系列 | 1440 |
| [高性能](../virtual-machines/sizes-hpc.md) | A 和 H 系列 | 2000 |


## <a name="azure-data-disks"></a>Azure 数据磁盘
可添加额外的数据磁盘，用于安装应用程序和存储数据。 在任何需要持久和灵敏数据存储的情况下，都应使用数据磁盘。 每个数据磁盘的最大容量为 4 TB。 VM 实例的大小决定可附加的数据磁盘数。 可以为每个 VM vCPU 附加两个数据磁盘，直至达到每个虚拟机 64 个磁盘的绝对上限。

## <a name="vm-disk-types"></a>VM 磁盘类型
Azure 提供两种类型的磁盘。

### <a name="standard-disk"></a>标准磁盘
标准存储受 HDD 支持，可以在确保性能的同时提供经济高效的存储。 标准磁盘适用于经济高效的开发和测试工作负荷。

### <a name="premium-disk"></a>高级磁盘
高级磁盘由基于 SSD 的高性能、低延迟磁盘提供支持。 建议对运行生产工作负荷的 VM 使用这些磁盘。 高级存储支持 DS 系列、DSv2 系列、GS 系列和 FS 系列 VM。 选择磁盘大小时，大小值将向上舍入到下一类型。 例如，如果磁盘大小小于 128 GB，则磁盘类型为 P10。 如果磁盘大小介于 129 GB 和 512 GB 之间，则大小为 P20。 如果超过 512 GB，则大小为 P30。

### <a name="premium-disk-performance"></a>高级磁盘性能
|高级存储磁盘类型 | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 磁盘大小（向上舍入） | 32 GB | 64 GB | 128 GB | 512 GB | 1,024 GB (1 TB) | 2,048 GB (2 TB) | 4,095 GB (4 TB) |
| 每个磁盘的最大 IOPS | 120 | 240 | 500 | 2,300 | 5,000 | 7,500 | 7,500 |
每个磁盘的吞吐量 | 25 MB/秒 | 50 MB/秒 | 100 MB/秒 | 150 MB/秒 | 200 MB/秒 | 250 MB/秒 | 250 MB/秒 |

尽管上表确定了每个磁盘的最大 IOPS，但还可通过条带化多个数据磁盘实现更高级别的性能。 例如，Standard_GS5 VM 最多可实现 80,000 IOPS。 若要详细了解每个 VM 的最大 IOPS，请参阅 [Linux VM 大小](../virtual-machines/sizes.md)。


## <a name="create-and-attach-disks"></a>创建并附加磁盘
可以在创建规模集时创建和附加磁盘，也可以对现有的规模集创建和附加磁盘。

从 API 版本 `2019-07-01` 开始，可以使用 [storageProfile.osDisk.diskSizeGb](/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosdisk) 属性设置虚拟机规模集中 OS 磁盘的大小。 预配后，可能需要对磁盘进行扩展或重新分区，以利用整个空间。 在[此处](../virtual-machines/windows/expand-os-disk.md#expand-the-volume-within-the-os)详细了解如何扩展磁盘。

### <a name="attach-disks-at-scale-set-creation"></a>创建规模集时附加磁盘
首先，使用 [az group create](/cli/azure/group) 命令创建资源组。 在此示例中，在“eastus”  区域中创建了名为“myResourceGroup”  的资源组。

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

请使用 [az vmss create](/cli/azure/vmss) 命令创建虚拟机规模集。 以下示例创建名为“myScaleSet”  的规模集，并生成 SSH 密钥（如果不存在）。 两个磁盘都是 `--data-disk-sizes-gb` 参数创建的。 第一个磁盘的大小为 *64* GB，第二个磁盘的大小为 *128* GB：

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 64 128
```

创建和配置所有的规模集资源和 VM 实例需要几分钟时间。

### <a name="attach-a-disk-to-existing-scale-set"></a>将磁盘附加到现有规模集
还可以将磁盘附加到现有的规模集。 使用在上一步创建的规模集通过 [az vmss disk attach](/cli/azure/vmss/disk) 添加另一磁盘。 以下示例附加另一个 *128* GB 的磁盘：

```azurecli-interactive
az vmss disk attach \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --size-gb 128
```


## <a name="prepare-the-data-disks"></a>准备数据磁盘
已创建并附加到规模集 VM 实例的磁盘是原始磁盘。 将磁盘用于数据和应用程序之前，必须准备磁盘。 若要准备磁盘，需要创建分区、创建文件系统，并将其装载。

若要跨规模集中的多个 VM 实例自动完成此过程，可以使用 Azure 自定义脚本扩展。 此扩展可以在每个 VM 实例上以本地方式执行脚本，以便完成各种任务，例如准备附加的数据磁盘。 有关详细信息，请参阅[自定义脚本扩展概述](../virtual-machines/extensions/custom-script-linux.md)。

以下示例在每个 VM 实例上执行来自 GitHub 示例存储库的脚本，使用的是 [az vmss extension set](/cli/azure/vmss/extension) 命令，该命令用于准备所有原始的附加数据磁盘：

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

若要确认磁盘是否已正确地准备好，请通过 SSH 连接到某个 VM 实例。 使用 [az vmss list-instance-connection-info](/cli/azure/vmss) 列出规模集的连接信息：

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```

使用自己的公共 IP 地址和端口号连接到第一个 VM 实例，如以下示例所示：

```console
ssh azureuser@52.226.67.166 -p 50001
```

检查 VM 实例上的分区，如下所示：

```bash
sudo fdisk -l
```

以下示例输出显示附加到 VM 实例的有三个磁盘 - */dev/sdc*、 */dev/sdd* 和 */dev/sde*。 每个这样的磁盘都是单个分区使用所有的可用空间：

```bash
Disk /dev/sdc: 64 GiB, 68719476736 bytes, 134217728 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xa47874cb

Device     Boot Start       End   Sectors Size Id Type
/dev/sdc1        2048 134217727 134215680  64G 83 Linux

Disk /dev/sdd: 128 GiB, 137438953472 bytes, 268435456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd5c95ca0

Device     Boot Start       End   Sectors  Size Id Type
/dev/sdd1        2048 268435455 268433408  128G 83 Linux

Disk /dev/sde: 128 GiB, 137438953472 bytes, 268435456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x9312fdf5

Device     Boot Start       End   Sectors  Size Id Type
/dev/sde1        2048 268435455 268433408  128G 83 Linux
```

检查 VM 实例上的文件系统和装载点，如下所示：

```bash
sudo df -h
```

以下示例输出表明，三个磁盘已将其文件系统正确地装载在 */datadisks* 下：

```output
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.3G   28G   5% /
/dev/sdb1        50G   52M   47G   1% /mnt
/dev/sdc1        63G   52M   60G   1% /datadisks/disk1
/dev/sdd1       126G   60M  120G   1% /datadisks/disk2
/dev/sde1       126G   60M  120G   1% /datadisks/disk3
```

规模集中每个 VM 实例上的磁盘是以同一方式自动准备的。 规模集进行纵向扩展时，所需数据磁盘会附加到新的 VM 实例。 自定义脚本扩展也会运行，以便自动准备磁盘。

关闭与 VM 实例的 SSH 连接：

```bash
exit
```


## <a name="list-attached-disks"></a>列出附加的磁盘
若要查看附加到规模集的磁盘的相关信息，请使用 [az vmss show](/cli/azure/vmss) 命令并对 *virtualMachineProfile.storageProfile.dataDisks* 进行查询：

```azurecli-interactive
az vmss show \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --query virtualMachineProfile.storageProfile.dataDisks
```

还会显示有关磁盘大小、存储层和 LUN（逻辑单元号）的信息。 以下示例输出显示了有关三个附加到规模集的数据磁盘的详细信息：

```json
[
  {
    "additionalProperties": {},
    "caching": "None",
    "createOption": "Empty",
    "diskSizeGb": 64,
    "lun": 0,
    "managedDisk": {
      "additionalProperties": {},
      "storageAccountType": "Standard_LRS"
    },
    "name": null
  },
  {
    "additionalProperties": {},
    "caching": "None",
    "createOption": "Empty",
    "diskSizeGb": 128,
    "lun": 1,
    "managedDisk": {
      "additionalProperties": {},
      "storageAccountType": "Standard_LRS"
    },
    "name": null
  },
  {
    "additionalProperties": {},
    "caching": "None",
    "createOption": "Empty",
    "diskSizeGb": 128,
    "lun": 2,
    "managedDisk": {
      "additionalProperties": {},
      "storageAccountType": "Standard_LRS"
    },
    "name": null
  }
]
```


## <a name="detach-a-disk"></a>分离磁盘
不再需要某个给定的磁盘时，可以将其从规模集中分离。 该磁盘会从规模集的所有 VM 实例中删除。 若要从规模集中分离某个磁盘，请使用 [az vmss disk detach](/cli/azure/vmss/disk) 并指定磁盘的 LUN。 LUN 显示在上一部分的 [az vmss show](/cli/azure/vmss) 命令的输出中。 以下示例从规模集分离 LUN *2*：

```azurecli-interactive
az vmss disk detach \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --lun 2
```


## <a name="clean-up-resources"></a>清理资源
若要删除规模集和磁盘，请使用 [az group delete](/cli/azure/group) 删除资源组及其所有资源。 `--no-wait` 参数会使光标返回提示符处，不会等待操作完成。 `--yes` 参数将确认是否希望删除资源，不会显示询问是否删除的额外提示。

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>后续步骤
本教程介绍了如何通过 Azure CLI 创建和使用规模集的磁盘：

> [!div class="checklist"]
> * OS 磁盘和临时磁盘
> * 数据磁盘数
> * 标准磁盘和高级磁盘
> * 磁盘性能
> * 附加和准备数据磁盘

请继续学习下一教程，了解如何对规模集 VM 实例使用自定义映像。

> [!div class="nextstepaction"]
> [对规模集 VM 实例使用自定义映像](tutorial-use-custom-image-cli.md)