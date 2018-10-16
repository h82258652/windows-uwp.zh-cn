---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: 创建已知用户组
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 目标组, 客户, 外部测试版组, 用户组, 已知用户
ms.localizationpriority: medium
ms.openlocfilehash: e15b5a4a2f76cbfc33db593c3110ac6f2d054b5b
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4681558"
---
# <a name="create-known-user-groups"></a>创建已知用户组

通过使用与其 Microsoft 帐户关联的电子邮件地址，可借助已知用户组向某个组添加特定用户。 这些已知用户组最常用于通过[软件包外部测试版](package-flights.md)向选定的一组人分发特定软件包，或向[私人受众](choose-visibility-options.md#audience)分发提交。 它们还可用于进行市场活动，如向一组特定客户发送[目标通知](send-push-notifications-to-your-apps-customers.md)或[定向优惠](use-targeted-offers-to-maximize-engagement-and-conversions.md)。

为了被视为组成员，每个用户必须使用与所提供的电子邮件地址关联的 Microsoft 帐户在应用商店中进行身份验证。 若要通过软件包外部测试版下载应用，组成员必须使用支持软件包外部测试版的 Windows 10 版本（Windows.Desktop 版本 10586 或更高版本；Windows.Mobile 版本 10586.63 或更高版本；或 Xbox One）。 对于私人受众提交，组成员必须使用 Windows 10 版本 1607 或更高版本（包括 Xbox One）。

## <a name="to-create-a-known-user-group"></a>创建已知用户组

1. 在 Windows 开发人员中心仪表板中，展开左侧导航菜单中的**参与**，然后选择**客户组**。 
2. 在**我的客户组**部分中选择**创建新组**。
3. 在下一页上，在**组名称**框中，输入组名称。
4. 确保选择**已知用户组**单选按钮。
5. 输入要添加到组中的用户的电子邮件地址。 必须包括至少一个电子邮件地址，最多 10,000 个。 你可以直接将电子邮件地址输入该字段中（使用空格、逗号、分号或换行符分隔），或者单击**导入 .csv** 链接以从 .csv 文件中的电子邮件地址列表创建外部测试版组。
6. 选择**保存**。

该组现在可供使用。

还可以通过从[软件包外部测试版](package-flights.md)创建页面中选择**创建外部测试版组**来创建已知用户组。 请注意，如果执行此操作，则需要重新输入在软件包外部测试版创建页面中已提供的任何信息。

> [!IMPORTANT]
> 结合使用已知用户组和软件包外部测试版时，确保你已获取要添加到组的用户的任何必要许可，并且他们知道会获取不同于非外部测试版提交的软件包。 

## <a name="to-edit-a-known-user-group"></a>编辑已知用户组

创建已知用户组之后，则无法将其从仪表板中删除（或更改其名称），但可以随时编辑其成员身份。

若要查看和编辑已知用户组，展开左侧导航菜单中的**参与**菜单，然后选择**客户组**。 在**我的客户组**下，选择要编辑的组名称。 通过在创建新的外部测试版时选择**查看并管理现有组**，或者从软件包外部测试版的概述页选择组名称，还可以从软件包外部测试版创建页面中编辑已知用户组。 

选择要编辑的组后，可以在字段中直接添加或删除电子邮件地址。

对于较大的更改，请选择**导出 .csv**，以将组成员身份信息保存为 .csv 文件。 在此文件中进行更改，然后单击**导入 .csv**，以使用新版本更新组成员身份。

请注意，实现组成员身份更改可能需要 30 分钟。 无需通过发布新提交的方式来让新组成员利用软件包外部测试版或私人受众访问你的提交；更改实施后他们自然可进行访问。 






