---
title: 教程 - 在 Azure IoT Central 应用程序中创建和管理规则
description: 本教程介绍如何通过 Azure IoT Central 规则以近实时方式监视设备并自动调用操作（例如在触发规则时发送电子邮件）。
author: dominicbetts
ms.author: dobett
ms.date: 04/06/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 555da74da65f3b1897a276cf819a263334cfa053
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "80999057"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>教程：在 Azure IoT Central 应用程序中创建规则并设置通知

*本文适用于操作员、构建者和管理员。*

可以使用 Azure IoT Central 对连接的设备进行远程监视。 使用 Azure IoT Central 规则，可以近乎实时地监视设备并自动调用操作（例如，发送电子邮件）。 本文介绍如何创建规则来监视设备发送的遥测数据。

设备使用遥测从设备发送数值数据。 选定的设备遥测超过指定阈值时会触发规则。

在本教程中，你将创建一个规则，用于在模拟环境传感器设备中的温度超过 70&deg; F 时发送电子邮件。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
>
> * 创建规则
> * 添加电子邮件操作

## <a name="prerequisites"></a>先决条件

在开始之前，请完成[创建 Azure IoT Central 应用程序](./quick-deploy-iot-central.md)和[将模拟设备添加到 IoT Central 应用程序](./quick-create-simulated-device.md)快速入门，以创建要使用的 MXChip IoT DevKit 设备模板  。

## <a name="create-a-rule"></a>创建规则

若要创建遥测规则，设备模板必须至少包含一个遥测值。 本教程使用模拟的 MXChip IoT DevKit 设备来发送温度和湿度遥测数据  。 在[将模拟设备添加到 IoT Central 应用程序](./quick-create-simulated-device.md)快速入门中，你已添加此设备模板并创建了模拟设备。 该规则监视设备报告的温度，并在温度超过 70 度时发送电子邮件。

1. 在左窗格中，选择“规则”  。

1. 如果尚未创建任何规则，则会看到以下屏幕：

    ![尚无规则](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. 选择“+”添加新规则  。

1. 输入名称“温度监视器”以标识规则，然后按 Enter  。

1. 选择“MXChip IoT DevKit”  设备模板。 默认情况下，此规则自动应用到与此设备模板关联的所有设备。 要筛选设备的子集，请选择“+ 筛选器”，然后使用设备属性来标识设备  。 要禁用规则，请切换规则标头中的“启用/禁用”按钮  ：

    ![筛选器和启用](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>配置规则条件

条件可定义规则监视的条件。 在本教程中，将配置在温度超过 70&deg; F 时触发的规则。

1. 在“遥测”下拉列表中选择“温度”   。

1. 接下来，选择“大于”作为“运算符”，并输入“70”作为“值”     。

    ![条件](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. （可选）可以设置“时间聚合”  。 选择时间聚合时，还必须从聚合下拉列表中选择一种聚合类型（例如，平均值或求和）。

    * 如无聚合，此规则将对每个满足此条件的遥测数据点触发。 例如，如果将规则配置为在温度高于 70 度时触发，则当设备温度超过此值时，该规则会几乎立即触发。
    * 使用聚合时，如果时间窗口中遥测数据点的聚合值满足条件，将触发规则。 例如，如果将规则配置为在温度高于 70 时触发，且平均时间聚合为 10 分钟，则当设备报告在过去 10 分钟间隔计算得出的平均温度超过 70 度时，将触发该规则。

     ![聚合条件](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

选择“+ 条件”，将多个条件添加到规则中  。 如果指定多个条件，必须满足所有条件才能触发该规则。 每个条件都由一个隐式 `AND` 子句联接。 如果在多个条件下使用时间聚合，则必须聚合所有遥测值。

### <a name="configure-actions"></a>配置操作

定义条件后，可以设置触发规则时要执行的操作。 此规则中指定的所有条件评估结果均为 true 时会调用操作。

1. 在“操作”部分中选择“+ 电子邮件”   。

1. 输入“温度警告”作为操作的显示名称，在“收件人”字段中输入电子邮件地址，然后输入“你应检查设备!”    作为显示在电子邮件正文中的注释。

    > [!NOTE]
    > 电子邮件只发送给那些已添加到应用程序中并已至少登录一次的用户。 详细了解 Azure IoT Central 中的[用户管理](howto-administer.md)。

   ![配置操作](media/tutorial-create-telemetry-rules/configure-action1.png)

1. 要保存操作，请选择“完成”  。 可以向规则添加多个操作。

1. 若要保存规则，请选择“保存”。  此规则在数分钟内即可生效，然后开始监视发送到应用程序的遥测数据。 满足规则中指定的条件时，规则会触发配置的电子邮件操作。

一段时间后，规则触发时，你将收到一封电子邮件：

![示例电子邮件](media/tutorial-create-telemetry-rules/email.png)

## <a name="delete-a-rule"></a>删除规则

如果不再需要某项规则，可打开该规则并选择“删除”，以将其删除。 

## <a name="enable-or-disable-a-rule"></a>启用或禁用规则

选择要启用或禁用的规则。 切换规则中的“启用/禁用”按钮，为该规则范围内的所有设备启用或禁用该规则  。

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>对特定的设备启用或禁用规则

选择要自定义的规则。 在“目标设备”部分使用一个或多个筛选器，将规则范围缩小到要监视的设备  。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：

* 创建基于遥测的规则
* 添加操作

现在，你已定义了基于阈值的规则，建议接下来学习如何：

> [!div class="nextstepaction"]
> [配置连续数据导出](./howto-export-data.md)。
