---
title: 数据库迁移方案状态
titleSuffix: Azure Database Migration Service
description: 详细了解 Azure 数据库迁移服务支持的迁移方案的状态。
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 07/08/2020
ms.openlocfilehash: cc3323a3ae2d604557d7e09b38252d63f17c5470
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92319910"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Azure 数据库迁移服务支持的迁移方案的状态

Azure 数据库迁移服务旨在支持脱机（一次性）和联机（连续同步）迁移的各种迁移方案（源/目标对）。 随着时间的推移，Azure 数据库迁移服务提供的方案范围也在不断扩大。 将定期添加新的方案。 本文介绍 Azure 数据库迁移服务当前支持的每个迁移方案和状态（个人预览版、公共预览版或正式版）。

## <a name="offline-versus-online-migrations"></a>脱机和联机迁移

使用 Azure 数据库迁移服务可以执行脱机或联机迁移。 通过“脱机”迁移，应用程序停机时间在迁移开始的同时开始。 若要限制迁移完成时交接到新环境所需的停机时间，请使用联机迁移。 建议对脱机迁移进行测试，以便确定其停机时间是否可以接受；如果不能接受，请执行联机迁移。

## <a name="migration-scenario-status"></a>迁移方案状态

Azure 数据库迁移服务支持的迁移方案的状态随时间而变化。 一般而言，方案首先在**个人预览版**中发布。 参与个人预览需要客户通过 [DMS 预览站点](https://aka.ms/dms-preview)提交提名。 个人预览完成后，方案状态将更改为**公共预览版**。 Azure 数据库迁移服务用户可以直接从用户界面试用公共预览版的迁移方案。 不需要注册。  但是，公共预览版的迁移方案不一定在所有区域中都可用，并且可能会在最终版本发布之前进行其他更改。 经过公共预览后，方案状态将更改为**正式版**。 正式版 (GA) 是最终发布状态，其功能是完整的且可供所有用户访问。

## <a name="migration-scenario-support"></a>迁移方案支持

下表显示使用 Azure 数据库迁移服务时支持的迁移方案。

> [!NOTE]
> 如果在用户界面中列出了如下所示的方案，请联系 [请求 Azure 数据库迁移](mailto:AskAzureDatabaseMigrations@service.microsoft.com) 别名获取其他信息。

> [!IMPORTANT]
> 若要在个人预览版中查看 Azure 数据库迁移服务当前支持的所有方案，请参阅 [DMS 预览站点](https://aka.ms/dms-preview)。

### <a name="offline-one-time-migration-support"></a>脱机（一次性）迁移支持

下表显示了 Azure 数据库迁移服务对脱机迁移的支持。

| 目标  | Source | 支持 | 状态 |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL | X |  |
|   | Oracle | X |   |
| **Azure SQL VM** | SQL Server | ✔ | GA |
|   | Oracle | X |   |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Azure DB for MySQL** | MySQL | X |   |
|   | RDS MySQL | X |   |
| **Azure DB for PostgreSQL（单一服务器）** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |
| **Azure DB for PostgreSQL-超大规模 (Citus) ** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |

### <a name="online-continuous-sync-migration-support"></a>联机（连续同步）迁移支持

下表显示了 Azure 数据库迁移服务对联机迁移的支持。

| 目标  | Source | 支持 | 状态 |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle | X |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle | X |  |
| **Azure SQL VM** | SQL Server | X |   |
|   | Oracle  | X |  |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Azure DB for MySQL** | MySQL | ✔ | GA |
|   | RDS MySQL | ✔ | GA |
| **Azure DB for PostgreSQL（单一服务器）** | PostgreSQL | ✔ | GA |
|   | Azure DB for PostgreSQL-单一服务器 | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |
|   | Oracle | ✔ | 公共预览版 |
| **Azure DB for PostgreSQL-超大规模 (Citus) ** | PostgreSQL | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |


## <a name="next-steps"></a>后续步骤

有关 Azure 数据库迁移服务和区域可用性的概述，请参阅[什么是 Azure 数据库迁移服务](dms-overview.md)一文。
