---
title: 为点到站点生成和导出证书： Linux： CLI
description: 使用 Linux (strongSwan) CLI 创建自签名根证书、导出公钥和生成客户端证书。
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: alzam
ms.openlocfilehash: 0be5bb042649b0fe425077b5b3feb3cea728218c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "89394000"
---
# <a name="generate-and-export-certificates"></a>生成并导出证书

点到站点连接使用证书进行身份验证。 本文说明如何使用 Linux CLI 和 strongSwan 创建自签名根证书以及生成客户端证书。 如果正在查找不同的证书说明，请参阅 [Powershell](vpn-gateway-certificates-point-to-site.md) 或 [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) 文章。 有关如何使用 GUI 而不是 CLI 安装 strongSwan 的信息，请参阅[客户端配置](point-to-site-vpn-client-configuration-azure-cert.md#install)一文中的步骤。

## <a name="install-strongswan"></a>安装 strongSwan

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>生成并导出证书

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>后续步骤

继续进行点到站点配置以[创建和安装 VPN 客户端配置文件](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)。
