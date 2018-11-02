---
author: normesta
title: 请求地图身份验证密钥
description: 通用 Windows 应用必须先经过身份验证，然后才能在 Windows.Services.Maps 命名空间中使用 MapControl 和地图服务。
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 地图身份验证密钥, 地图控件
ms.localizationpriority: medium
ms.openlocfilehash: c42255ec42432d0674533492e141c4a48f3bb9ff
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5973330"
---
# <a name="request-a-maps-authentication-key"></a>请求地图身份验证密钥




[通用 Windows 应用](https://msdn.microsoft.com/library/windows/apps/dn894631)必须先经过验证，然后才能在 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务。 若要对你的应用进行身份验证，你必须指定地图身份验证密钥。 本主题介绍如何从[必应地图开发人员中心](https://www.bingmapsportal.com/)请求地图验证密钥并将其添加到应用。

**提示** 若要了解有关在你的应用中使用地图的详细信息，请从 GitHub 上的 [Windows-universal-samples 存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)中下载以下示例：

-   [通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="get-a-key"></a>获取密钥


使用[必应地图开发人员中心](https://www.bingmapsportal.com/)创建并管理你的通用 Windows 应用的地图验证密钥。

创建新密钥

1.  在浏览器中，导航到必应地图开发人员中心 ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/))。

2.  如果系统提示你登录，请输入你的 Microsoft 帐户，然后单击**登录**。

3.  选择要与必应地图帐户关联的帐户。 如果你想要使用自己的 Microsoft 帐户，请单击 **“是”**。 否则，请单击**使用其他帐户登录**。

4.  如果你还没有必应地图帐户，请创建一个新的必应地图帐户。 输入 **“帐户名称”**、**“联系人姓名”**、**“公司名称”**、**“电子邮件地址”** 和 **“电话号码”**。 在接受使用条款后，单击**创建**。

5.  在**我的帐户**菜单上，单击**我的密钥**。

6.  如果之前创建了密钥，请单击链接以创建新密钥。 否则继续前进到“创建密钥”窗体。

7.  完成**创建密钥**窗体，然后单击**创建**。

    -   **应用程序名称：** 你的应用程序的名称。
    -   **应用程序 URL（可选）：** 你的应用程序的 URL。
    -   **密钥类型：** 选择 **“基本”** 或 **“企业”**。
    -   **应用程序类型：** 选择 **“通用 Windows 应用”** 以便在你的通用 Windows 应用中使用。

    这是一个表格呈现内容的示例。

    ![“创建密钥”表格的示例。](images/createkeydialog.png)

8.  在单击 **“创建”** 后，新的密钥将显示在 **“创建密钥”** 表格的下方。 将该密钥复制到安全位置或立即将其添加到你的应用，如下一步中所述。

## <a name="add-the-key-to-your-app"></a>将密钥添加到你的应用


若要在你的通用 Windows 应用中使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地图服务 ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979))，则需要地图验证密钥。 将该密钥添加到地图控件和地图服务对象（如果适用）。

### <a name="to-add-the-key-to-a-map-control"></a>将密钥添加到地图控件

要验证 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)，请将 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 属性设置为身份验证密钥值。 你可以在代码中设置该属性，也可以在 XAML 标记中设置它，具体取决于你的偏好。 有关使用 **MapControl** 的详细信息，请参阅[以 2D、3D 和街景视图方式显示地图](display-maps.md)。

-   此示例将 **MapServiceToken** 设置为代码中的身份验证密钥的值。

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   此示例将 **MapServiceToken** 设置为 XAML 标记中的身份验证密钥的值。

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>将密钥添加到地图服务

若要在 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间中使用服务，请将 [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) 属性设置为身份验证密钥值。 有关使用地图服务的详细信息，请参阅[显示路线和方向](routes-and-directions.md)和[执行地理编码和反向地理编码](geocoding.md)。

-   此示例将 **ServiceToken** 设置为代码中的身份验证密钥的值。

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>相关主题

* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地图设计指南](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [版本 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 路况应用示例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
