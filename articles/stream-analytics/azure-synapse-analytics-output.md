---
title: Azure 流分析中的 azure Synapse Analytics 输出
description: 本文介绍 Azure Synapse Analytics 作为 Azure 流分析的输出。
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 0b3bec9c4d4476b95279e35953ff89177f4488d4
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93305841"
---
# <a name="azure-synapse-analytics-output-from-azure-stream-analytics"></a>Azure 流分析中的 azure Synapse Analytics 输出

[Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics)（旧称为 SQL 数据仓库）是一种无限制的分析服务，它将企业数据仓库和大数据分析结合在一起。 

Azure 流分析作业可以输出到 Azure Synapse 分析中的专用 SQL 池表，并可处理高达 200MB/秒的吞吐量速率。这为工作负荷（例如报告和仪表板建设）支持最苛刻的实时分析和热路径数据处理需求。  

专用 SQL 池表必须存在，然后才能将其作为输出添加到流分析作业。 表架构必须与作业输出中的字段及类型匹配。 

若要使用 Azure Synapse 作为输出，需要确保已配置存储帐户。 导航到“存储帐户设置”，以配置存储帐户。 仅允许支持表的存储帐户类型：常规用途 V2 和常规用途 V1。 仅选择标准层。 不支持高级层。

## <a name="output-configuration"></a>输出配置

下表列出了用于创建 Azure Synapse Analytics 输出的属性名称及其说明。

|属性名称|说明|
|-|-|
|输出别名 |在查询中使用的友好名称，用于将查询输出定向到此数据库。 |
|数据库 |要向其发送输出的专用 SQL 池名称。 |
|服务器名称 |Azure Synapse 服务器名称。  |
|用户名 |对数据库拥有写入访问权限的用户名。 流分析仅支持 SQL 身份验证。 |
|密码 |用于连接到数据库的密码。 |
|表  | 将写入输出的表名称。 表名称区分大小写。 此表的架构应与字段数量以及作业输出生成的字段类型完全匹配。|

## <a name="next-steps"></a>后续步骤

* [快速入门：使用 Azure 门户创建流分析作业](stream-analytics-quick-create-portal.md)
* [快速入门：使用 Azure CLI 创建 Azure 流分析作业](quick-create-azure-cli.md)
* [快速入门：使用 ARM 模板创建 Azure 流分析作业](quick-create-azure-resource-manager.md)
* [快速入门：使用 Azure PowerShell 创建流分析作业](stream-analytics-quick-create-powershell.md)
* [快速入门：使用 Visual Studio 创建 Azure 流分析作业](stream-analytics-quick-create-vs.md)
* [快速入门：在 Visual Studio Code 中创建 Azure 流分析作业](quick-create-visual-studio-code.md)