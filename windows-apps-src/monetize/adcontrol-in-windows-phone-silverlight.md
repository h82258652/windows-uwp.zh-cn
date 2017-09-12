---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "了解如何使用 AdControl 类在适用于 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 应用中显示横幅广告。"
title: "Windows Phone Silverlight 中的 AdControl"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 , uwp, 广告, AdControl, Silverlight, Windows Phone"
ms.openlocfilehash: f1582639757abfb6de156bf88ce8af71ba3eaacd
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>Windows Phone Silverlight 中的 AdControl

本演练介绍了如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 类在适用于 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 应用中显示横幅广告。

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>用于 Windows Phone 8.x Silverlight 项目的广告支持

某些开发人员方案已不再受 Windows Phone 8.x Silverlight 项目支持。 有关详细信息，请参阅下表。

|  平台版本  |  现有项目    |   新建项目  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  如果现有的 Windows Phone 8.0 Silverlight 项目已在使用之前版本的通用广告客户端 SDK 或 Microsoft Advertising 的 **AdControl** 或 **AdMediatorControl**，并且此应用已在 Windows 应用商店中发布，则可以修改和重建该项目，并且可以在设备上调试或测试更改。 不支持在仿真器中调试或测试项目。  |  不支持。  |
| Windows Phone 8.1 Silverlight    |  如果现有的 Windows Phone 8.1 Silverlight 项目使用之前 SDK 的 **AdControl** 或 **AdMediatorControl**，则可以修改和重建该项目。 但是，若要调试或测试应用，必须在仿真器中运行该应用，并且将[测试模式值](test-mode-values.md)用于应用程序 ID 和广告单元 ID。 在不受支持的设备上调试或测试应用。  |   可以将 **AdControl** 或 **AdMediatorControl** 添加到新的 Windows Phone 8.1 Silverlight 项目。 但是，若要调试或测试应用，必须在仿真器中运行该应用，并且将[测试模式值](test-mode-values.md)用于应用程序 ID 和广告单元 ID。 在不受支持的设备上调试或测试应用。 |

## <a name="add-the-advertising-assemblies-to-your-project"></a>将广告程序集添加到你的项目

若要开始使用，将包含适用于 Windows Phone Silverlight 的 Microsoft Advertising 程序集的 NuGet 包下载并安装到你的项目中。

1.  在 Visual Studio 中打开你的项目。

2.  单击**工具**、指向 **NuGet 包管理器**，然后单击**程序包管理器控制台**。

3.  在**程序包管理器控制台**窗口中，输入以下命令之一。

  * 如果你的项目面向 Windows Phone 8.0，请输入此命令。

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * 如果你的项目面向 Windows Phone 8.1，请输入此命令。

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    输入该命令后，所有必要的 Microsoft Advertising 程序集（适用于 Silverlight）均已通过 NuGet 包下载到你的本地项目，并且对这些程序集的引用会自动添加到项目。

## <a name="code-your-app"></a>对你的应用进行编码


1.  将以下功能添加到 WMAppManifest.xml 文件中的**功能**节点。

  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  在此示例中，你的**功能**节点如下所示：

  ``` syntax
  <Capabilities>
      <Capability Name="ID_CAP_NETWORKING"/>
      <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
      <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
      <Capability Name="ID_CAP_SENSORS"/>
      <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
      <Capability Name="ID_CAP_IDENTITY_USER"/>
      <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
      <Capability Name="ID_CAP_PHONEDIALER"/>
  </Capabilities>
  ```

2.  （可选）保存你的项目。 单击**全部保存**图标，或者在**文件**菜单下，单击**全部保存**。

3.  将 **Internet (客户端和服务器)**功能添加到项目中的 Package.appxmanifest 文件。 在**解决方案资源管理器**中，双击 **Package.appxmanifest**。

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    在**编辑器**中，打开**功能**选项卡。 选中 **Internet (客户端和服务器)**框。

4.  （可选）保存你的项目。 单击**全部保存**图标，或者在**文件**菜单下，单击**全部保存**。

5.  修改 MainPage.xaml 文件中的 Silverlight 标记，以包含 **Microsoft.Advertising.Mobile.UI** 命名空间。

  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  你的页面标头将具有以下代码：

  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  在**网格**标记中，为 **AdControl** 添加以下代码。 将 **ApplicationId** 和 **AdUnitId** 属性分配给[测试模式值](test-mode-values.md)中提供的测试值，然后调整 **Height** 和 **Width** 属性以适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)之一。

  > **注意**&nbsp;&nbsp;在提交应用之前，你需要将这些测试 **ApplicationId** 和 **AdUnitId** 值替换为实时值。

  ``` xml
  <Grid x:Name="ContentPanel" Grid.Row="1">
      <UI:AdControl
          ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
          AdUnitId="10865270"
          HorizontalAlignment="Left"
          Height="80"
          VerticalAlignment="Top"
          Width="480"/>
  </Grid>
  ```

7.  生成并运行项目。 确认你的应用显示广告，类似于以下内容：

  ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## <a name="release-your-app-with-live-ads-using-dev-center"></a>使用开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的**盈利**&gt;**通过广告盈利**页面，然后[创建独立的广告单元](../publish/monetize-with-ads.md)。 对于广告单元类型，请指定**横幅**。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值（**applicationId** 和 **adUnitId**）替换为你在开发人员中心生成的实时值。

3.  使用开发人员中心仪表板 [提交应用](../publish/app-submissions.md) 至应用商店。

4.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。


 
