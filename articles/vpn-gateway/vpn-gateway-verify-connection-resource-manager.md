---
title: Azure VPN 网关：验证网关连接
description: 本文介绍如何验证虚拟网络 VPN 网关连接。
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: devx-track-azurecli
ms.openlocfilehash: fef9eb49b10008c86ee044a199ae69a43585f4f3
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217917"
---
# <a name="verify-a-vpn-gateway-connection"></a>验证 VPN 网关连接

本文介绍如何验证经典部署模型和 Resource Manager 部署模型的 VPN 网关连接。

## <a name="azure-portal"></a>Azure 门户

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

若要使用 PowerShell 验证资源管理器部署模型的 VPN 网关连接，请安装最新版本的 [Azure 资源管理器 PowerShell cmdlet](/powershell/azure/)。

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 验证 Resource Manager 部署模型的 VPN 网关连接，请安装最新版本的 [CLI 命令](https://docs.microsoft.com/cli/azure/install-azure-cli)（2.0 或更高版本）。

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="azure-portal-classic"></a>Azure 门户（经典）

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell（经典）

若要使用 PowerShell 验证经典部署模型的 VPN 网关连接，请安装最新版本的 Azure PowerShell cmdlet。 请务必下载并安装 [Service Management](/powershell/azure/servicemanagement/install-azure-ps?#azure-service-management-cmdlets) 模块。 使用“Add-AzureAccount”登录经典部署模型。

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>后续步骤

* 可以将虚拟机添加到虚拟网络。 请参阅 [创建虚拟机](../virtual-machines/windows/quick-create-portal.md) 以获取相关步骤。
