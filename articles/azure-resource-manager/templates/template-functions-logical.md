---
title: 模板函数 - 逻辑
description: 介绍 Azure 资源管理器模板中用于确定逻辑值的函数。
ms.topic: conceptual
ms.date: 10/12/2020
ms.openlocfilehash: ede41bd6c03eb7a01ae63526810d0310f31e4014
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978503"
---
# <a name="logical-functions-for-arm-templates"></a>ARM 模板的逻辑函数

资源管理器提供了多个用于在 Azure 资源管理器 (ARM) 模板中进行比较的函数。

* [and](#and)
* [bool](#bool)
* [false](#false)
* [if](#if)
* [not](#not)
* [or](#or)
* [true](#true)

## <a name="and"></a>and

`and(arg1, arg2, ...)`

检查所有参数值是否均为 true。

### <a name="parameters"></a>parameters

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |boolean |要检查是否为 true 的第一个值。 |
| arg2 |是 |boolean |要检查是否为 true 的第二个值。 |
| 其他参数 |否 |boolean |用于检查是否为 true 的其他参数。 |

### <a name="return-value"></a>返回值

如果所有值均为 true，则返回 True；否则返回 False**** ****。

### <a name="examples"></a>示例

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json)演示如何使用逻辑函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="bool"></a>bool

`bool(arg1)`

将参数转换为布尔值。

### <a name="parameters"></a>parameters

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |字符串或整数 |要转换为布尔值的值。 |

### <a name="return-value"></a>返回值

转换后的值的布尔值。

### <a name="remarks"></a>注解

你还可以使用 [true ( # B1 ](#true) 和 [False ( # B3 ](#false) 获取布尔值。

### <a name="examples"></a>示例

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json)演示如何对字符串或整数使用 bool。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

上述示例中使用默认值的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

## <a name="false"></a>false

`false()`

返回 false。

### <a name="parameters"></a>参数

False 函数不接受任何参数。

### <a name="return-value"></a>返回值

始终为 false 的布尔值。

### <a name="example"></a>示例

下面的示例返回一个错误的输出值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "falseOutput": {
            "value": "[false()]",
            "type" : "bool"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| falseOutput | Bool | False |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

根据条件为 true 或 false 返回值。

### <a name="parameters"></a>parameters

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| condition |是 |boolean |要检查是为 true 还是为 false 的值。 |
| trueValue |是 | 字符串、int、对象或数组 |条件为 true 时返回的值。 |
| falseValue |是 | 字符串、int、对象或数组 |条件为 false 时返回的值。 |

### <a name="return-value"></a>返回值

如果第一个参数为 True，则返回第二个参数；否则返回第三个参数****。

### <a name="remarks"></a>备注

条件为 **True** 时，仅评估 true 值。 条件为 **False** 时，仅评估 false 值。 使用 **if** 函数时，可以包含仅在特定条件下有效的表达式。 例如，可以引用一个资源，该资源在某个条件下存在，在另一个条件下不存在。 以下部分显示了一个条件性评估表达式的示例。

### <a name="examples"></a>示例

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json)演示如何使用 `if` 函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| yesOutput | String | 是 |
| noOutput | String | 否 |
| objectOutput | Object | { "test": "value1" } |

以下[示例模板](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json)演示了如何将此函数与仅在特定条件下有效的表达式配合使用。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>not

`not(arg1)`

将布尔值转换为其相反值。

### <a name="parameters"></a>parameters

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |boolean |要转换的值。 |

### <a name="return-value"></a>返回值

参数为 False 时返回 True**** ****。 参数为 True 时返回 False**** ****。

### <a name="examples"></a>示例

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json)演示如何使用逻辑函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json)结合使用 **not** 和 [equals](template-functions-comparison.md#equals)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

## <a name="or"></a>或

`or(arg1, arg2, ...)`

检查任何参数值是否为 true。

### <a name="parameters"></a>parameters

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |boolean |要检查是否为 true 的第一个值。 |
| arg2 |是 |boolean |要检查是否为 true 的第二个值。 |
| 其他参数 |否 |boolean |用于检查是否为 true 的其他参数。 |

### <a name="return-value"></a>返回值

如果任何值为 true，则返回 True；否则返回 False**** ****。

### <a name="examples"></a>示例

以下[示例模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json)演示如何使用逻辑函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | Value |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="true"></a>是

`true()`

返回 true。

### <a name="parameters"></a>参数

True 函数不接受任何参数。

### <a name="return-value"></a>返回值

始终为 true 的布尔值。

### <a name="example"></a>示例

下面的示例返回真实的输出值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueOutput": {
            "value": "[true()]",
            "type" : "bool"
        }
    }
}
```

前述示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| trueOutput | Bool | True |

## <a name="next-steps"></a>后续步骤

* 有关 Azure 资源管理器模板中各部分的说明，请参阅[了解 ARM 模板的结构和语法](template-syntax.md)。

