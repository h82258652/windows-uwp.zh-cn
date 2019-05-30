---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: 使用 Visual Studio 测试 Surface Hub 应用
description: Visual Studio 模拟器提供了用于设计、开发、调试以及测试 UWP 应用（包括针对 Surface Hub 生成的应用）的环境。
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a846aab77fad6f087057fe930d11ac781ed7171
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362198"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>使用 Visual Studio 测试 Surface Hub 应用
Visual Studio 模拟器提供了你可以在其中设计、开发、调试以及测试通用 Windows 平台 \(UWP\) 应用（包括针对 Microsoft Surface Hub 生成的应用）的环境。 模拟器不使用相同的用户界面为 Surface Hub，但它可用于测试您的应用程序的外观和行为与 Surface Hub 屏幕大小和分辨率。

有关详细信息模拟器工具一般情况下，请参阅[在模拟器中的运行 UWP 应用](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator)。

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>将 Surface Hub 分辨率添加到模拟器
若要将 Surface Hub 分辨率添加到模拟器，请执行以下操作：

1. 为 55"创建配置通过将以下 XML 代码保存到名为的文件的 Surface Hub *HardwareConfigurations SurfaceHub55.xml*。  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. 为 84"创建配置通过将以下 XML 代码保存到名为的文件的 Surface Hub *HardwareConfigurations SurfaceHub84.xml*。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. 将复制到的两个 XML 文件*C:\Program Files (x86) \Common Files\Microsoft Shared\Windows 模拟器\\&lt;版本号&gt;\HardwareConfigurations*。

   > [!NOTE]
   > 管理权限才可将文件保存到此文件夹。

4. 在 Visual Studio 模拟器中运行应用。 单击调色板上的“更改分辨率”  按钮，然后从列表中选择 Surface Hub 配置。

    ![Visual Studio 模拟器分辨率](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [打开 Tablet 模式](https://windows.microsoft.com/windows-10/getstarted-like-a-tablet)以更好地模拟 Surface Hub 的体验。

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>从 Visual Studio 将应用部署到 Surface Hub 设备
手动将应用部署到 Surface Hub 是一个简单的过程。

### <a name="enable-developer-mode"></a>启用开发人员模式
默认情况下，Surface Hub 仅从 Microsoft Store 安装应用。 若要安装由其他来源签名的应用，必须启用开发人员模式。

> [!NOTE]
> 启用开发人员模式后，你将需要重置 Surface Hub，如果你想要再次禁用。 重置设备将删除所有本地用户文件和配置，然后重新安装 Windows。

1. 从 Surface Hub 的“开始”  菜单中，打开“设置”应用。

   > [!NOTE]
   > 访问 Surface Hub 上的设置应用程序所需管理权限。

2. 导航到**更新和安全\>面向开发人员**。

3. 选择“开发人员模式”  并接受警告提示。

### <a name="deploy-your-app-from-visual-studio"></a>从 Visual Studio 部署应用
有关部署过程的详细信息一般情况下，请参阅[部署和调试 UWP 应用](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

   > [!NOTE]
   > 此功能需要 Visual Studio 2015 Update 1 或更高版本，但是，我们建议使用最新的最新版本的 Visual Studio。 最新的 Visual Studio 实例将 gibe 您所有最新的开发和安全更新。

1. 导航到“开始调试”  按钮旁边的调试目标下拉列表，然后选择“远程计算机”  。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 调试目标下拉列表](images/vs-debug-target.png)

2. 输入 Surface Hub 的 IP 地址。 确保选择“通用”  身份验证模式。

   > [!TIP] 
   > 启用开发人员模式后，你可以在欢迎屏幕上找到 Surface Hub 的 IP 地址。

3. 选择**启动调试 (F5)** 部署和调试 Surface Hub 上的应用，或按 Ctrl + F5，只需将应用部署。

   > [!TIP]
   > 如果 Surface Hub 显示欢迎屏幕，请取消其选择任何按钮。
