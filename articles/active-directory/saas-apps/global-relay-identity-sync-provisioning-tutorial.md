---
title: 教程：为自动用户预配配置全局中继标识同步 Azure Active Directory |Microsoft Docs
description: 了解如何从 Azure AD 自动预配和取消预配用户帐户。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 0c4a3bf0-d0a6-4eab-909b-6cf9f9234e4c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2020
ms.author: Zhchia
ms.openlocfilehash: 9445004170fb06a3d563982da51f89535464e328
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92927985"
---
# <a name="tutorial-configure-global-relay-identity-sync-for-automatic-user-provisioning"></a>教程：为自动用户预配配置全局中继标识同步

本教程介绍了在全局中继标识同步和 Azure Active Directory (Azure AD) 配置自动用户预配时需要执行的步骤。 配置时，Azure AD 会自动将用户和组预配和取消预配到使用 Azure AD 预配服务的全局中继标识同步。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在全局中继标识同步中创建用户
> * 如果用户不再需要访问，请在全局中继标识同步中删除用户
> * 使用户属性在 Azure AD 和全局中继标识同步之间保持同步
> * 在全局中继标识同步中预配组和组成员身份


> [!NOTE]
> 全局中继标识同步预配连接器利用了由于安全问题而不再受支持的 SCIM 授权方法。 正在进行的工作是通过全局中继来转换为更安全的授权方法。

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 中[有权](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)配置预配的用户帐户（例如应用管理员、云应用管理员、应用所有者或全局管理员）。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
3. 确定 [Azure AD 和全局中继标识同步之间要映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。 

## <a name="step-2-configure-global-relay-identity-sync-to-support-provisioning-with-azure-ad"></a>步骤 2. 配置全局中继标识同步，以支持 Azure AD 的预配

请联系全局中继标识同步代表来接收租户 url。 此值将在 Azure 门户的全局中继标识同步应用程序的 "预配" 选项卡的 " **租户 URL** " 字段中输入。

## <a name="step-3-add-global-relay-identity-sync-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加全局中继标识同步

从 Azure AD 应用程序库添加全局中继标识同步，开始管理预配到全局中继标识同步。在 [此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)了解有关从库中添加应用程序的详细信息。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

使用 Azure AD 预配服务，可以根据对应用程序的分配和/或用户/组的属性来限定谁在预配范围内。 如果选择根据分配来查看要将谁预配到应用，则可以使用以下[步骤](../manage-apps/assign-user-or-group-access-portal.md)将用户和组分配给应用程序。 如果选择仅根据用户或组的属性来限定要对谁进行预配，可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 先小部分测试。 在向全员推出之前，请先使用少量的用户和组进行测试。 如果预配范围设置为分配的用户和组，则可以先尝试将一两个用户或组分配到应用。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-global-relay-identity-sync"></a>步骤 5。 配置全局中继标识同步的自动用户预配 

本部分将指导你完成以下步骤：配置 Azure AD 预配服务，以便基于 Azure AD 中的用户和/或组分配在全局中继标识同步应用中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-global-relay-identity-sync-in-azure-ad"></a>若要为 Azure AD 中的全局中继标识同步配置自动用户预配，请执行以下操作：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择 " **全局中继标识同步** "。

    ![应用程序列表中的全局中继标识同步链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![“预配”选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![自动设置选项卡](common/provisioning-automatic.png)

5. 在 " **管理员凭据** " 部分中，输入全局中继标识同步 **租户 url** 。 单击 " **测试连接** " 以确保 Azure AD 可以连接到全局中继标识同步。如果连接失败，请确保全局中继标识同步帐户具有管理员权限，并与你的全局中继代表联系以解决此问题。

    ![授权按钮](media/global-relay-identity-sync-provisioning-tutorial/authorization.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到全局中继标识同步** "。

9. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到全局中继标识同步的用户属性。 选为 " **匹配** " 属性的属性用于匹配全局中继标识同步中的用户帐户以执行更新操作。 如果选择更改 [匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保全局中继标识同步 API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|
   |---|---|
   |userName|字符串|
   |活动|Boolean|
   |displayName|字符串|
   |title|字符串|
   |preferredLanguage|字符串|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |name.formatted|字符串|
   |addresses[type eq "work"].formatted|字符串|
   |addresses[type eq "work"].streetAddress|字符串|
   |emails[type eq "work"].value|字符串|
   |addresses[type eq "work"].locality|字符串|
   |addresses[type eq "work"].region|字符串|
   |addresses[type eq "work"].postalCode|字符串|
   |addresses[type eq "work"].country|字符串|
   |地址 [type eq "other"]。格式|字符串|
   |phoneNumbers[type eq "work"].value|字符串|
   |phoneNumbers[type eq "mobile"].value|字符串|
   |phoneNumbers[type eq "fax"].value|字符串|
   |externalId|字符串|
   |name.honorificPrefix|String|
   |名称. honorificSuffix|字符串|
   |nickName|String|
   |userType|String|
   |区域设置|字符串|
   |timezone|字符串|
   |电子邮件 [类型 eq "home"]。值|String|
   |电子邮件 [type eq "other"]。值|String|
   |phoneNumbers [type eq "home"]。值|String|
   |phoneNumbers [type eq "other"]。值|String|
   |phoneNumbers [type eq "呼机"]。值|String|
   |地址 [type eq "home"]. streetAddress|String|
   |地址 [类型 eq "home"]。位置|String|
   |地址 [类型 eq "home"]。区域|String|
   |地址 [类型 eq "home"]|String|
   |地址 [类型 eq "home"]。国家/地区|String|
   |地址 [类型 eq "home"]。格式|String|
   |地址 [type eq "other"]. streetAddress|String|
   |地址 [type eq "other"]。位置|String|
   |地址 [type eq "other"]。区域|String|
   |地址 [type eq "other"]|String|
   |地址 [type eq "other"]|String|
   |角色 [主 eq "True"]。显示|String|
   |role [primary eq "True"]。类型|字符串|
   |roles[primary eq "True"].value|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|参考|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： proxyAddresses|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute1|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute2|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute3|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute4|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute5|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute6|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute7|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute8|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute9|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute10|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute11|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute12|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute13|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute14|String|
   |urn： ietf： params： scim：架构：扩展： GlobalRelay：2.0： User： extensionAttribute15|String|



10. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 组同步到全局中继标识同步** "。

11. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到全局中继标识同步的组属性。 选为 " **匹配** " 属性的属性用于匹配全局中继标识同步中的组以执行更新操作。 选择“保存”按钮以提交任何更改。

      |Attribute|类型|
      |---|---|
      |displayName|字符串|
      |members|参考|

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为全局中继标识同步启用 Azure AD 预配服务，请在 " **设置** " 部分中将 " **预配状态** " 更改为 **"打开** "。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 " **设置** " 部分的 " **范围** " 中选择所需的值，定义要预配到全局中继标识同步的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)了解有关隔离状态的详细信息。  

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)
