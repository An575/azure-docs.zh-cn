---
title: 无法登录到 Azure HDInsight 群集
description: 排查无法登录到 Azure HDInsight 中 Apache Hadoop 群集的原因
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 222c48ab2b9b718bc373e710f37f906e68bd31d4
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289011"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>方案：无法登录到 Azure HDInsight 群集

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

无法登录到 Azure HDInsight 群集。

## <a name="cause"></a>原因

原因可能有所不同。 请记住，在登录到群集或应用仪表板时，请使用“群集登录名”或 HTTP 凭据。 从远程连接时，请使用安全外壳 (SSH) 或远程桌面凭据。

## <a name="resolution"></a>解决方法

若要解决常见问题，请尝试下面的一个或多个步骤。

* 尝试在新的浏览器选项卡中以隐私模式打开群集仪表板。

* 如果忘记了 SSH 凭据，可以[在 Ambari UI 中重置凭据](../hdinsight-administer-use-portal-linux.md#change-passwords)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]