---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "了解如何使用 AdControl 类在适用于 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 应用中显示横幅广告。"
title: "Windows Phone Silverlight 中的 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# Windows Phone Silverlight 中的 AdControl


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本演练介绍如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 类在适用于 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 应用中显示横幅广告。

## 先决条件

*  使用 Visual Studio 2015 或 Visual Studio 2013 安装 [Microsoft 官方商城协定和盈利 SDK](http://aka.ms/store-em-sdk)。


## 添加广告程序集引用

适用于 Windows Phone Silverlight 项目的 Microsoft Advertising 程序集未随 Microsoft 官方商城协定和盈利 SDK 安装在本地。 在你开始更新代码之前，必须先使用“已连接的服务”****与 Microsoft 官方商城协定和盈利 SDK 中提供的广告中介支持下载这些程序集，然后在你的项目中引用它们。

1.  在 Visual Studio 中，依次单击“项目”****和“添加已连接的服务”****。

2.  在“添加已连接的服务”****对话框中，单击“广告中介”****，然后单击“配置”****。

3.  单击“选择广告网络”****，然后仅选择“Microsoft Advertising”****。

    此时，所有必要的 Microsoft Advertising 程序集（适用于 Silverlight）均已通过 NuGet 程序包下载到你的本地项目，并且对这些程序集的引用会自动添加到项目。 此外，对该广告中介程序集的引用也已添加到你的项目。 你将在稍后的步骤中删除该广告中介程序集，因为在这种情况下不需要。

4.  在“选择广告网络”****对话框中，单击“确定”****。 在随后的“获取状态”****确认页面中，再次单击“确定”****，最后单击“添加”****以关闭“广告中介”****对话框。

5.  在“解决方案资源管理器”****中，展开“引用”****节点。 右键单击“Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising”****，然后单击“删除”****。 在这种情况下不需要此程序集引用。

## 对你的应用进行编码


1.  将以下功能添加到 WMAppManifest.xml 文件中的“功能”****节点。

    ``` syntax
    <Capability Name="ID_CAP_IDENTITY_USER"/>
    <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
    <Capability Name="ID_CAP_PHONEDIALER"/>
    ```

    在此示例中，你的“功能”****节点如下所示：

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

2.  （可选）保存你的项目。 单击“全部保存”****图标，或者在“文件”****菜单下，单击“全部保存”****。

3.  将“Internet (客户端和服务器)”****功能添加到项目中的 Package.appxmanifest 文件。 在“解决方案资源管理器”****中，双击“Package.appxmanifest”****。

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    在“编辑器”****中，打开“功能”****选项卡。 选中“Internet (客户端和服务器)”****框。

4.  （可选）保存你的项目。 单击“全部保存”****图标，或者在“文件”****菜单下，单击“全部保存”****。

5.  修改 MainPage.xaml 文件中的 Silverlight 标记，以包含 **Microsoft.Advertising.Mobile.UI** 命名空间。

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    你的页面标头将具有以下代码：

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  在“网格”****标记中，为 **AdControl** 添加以下代码。 将 **ApplicationId** 和 **AdUnitId** 属性分配给[测试模式值](test-mode-values.md)中提供的测试值，然后调整 **Height** 和 **Width** 属性以适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    > **注意**  
    在提交应用之前，你需要将这些测试 **ApplicationId** 和 **AdUnitId** 值替换为实时值。

    ``` syntax
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

## 使用开发人员中心发布带有实时广告的应用


1.  在“开发人员中心仪表板”中，转到应用的“盈利”****&gt;“利用广告来盈利”****页面，然后[创建独立的 Microsoft Advertising 单元](../publish/monetize-with-ads.md)。 对于广告单元类型，请指定“横幅”****。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值（**applicationId** 和 **adUnitId**）替换为你在开发人员中心生成的实时值。

3.  使用开发人员中心仪表板[将应用提交](../publish/app-submissions.md)到应用商店。

4.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。


 



<!--HONumber=Jun16_HO4-->


