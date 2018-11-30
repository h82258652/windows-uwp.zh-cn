---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: 使用 Visual Studio 测试 Surface Hub 应用
description: Visual Studio 模拟器提供了用于设计、开发、调试以及测试 UWP 应用（包括针对 Surface Hub 生成的应用）的环境。
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b40fd56a85be6dce441324a427790cda28f9d7ac
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329027"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>使用 Visual Studio 测试 Surface Hub 应用
Visual Studio 模拟器提供了你可以在其中设计、开发、调试以及测试通用 Windows 平台 \(UWP\) 应用（包括针对 Microsoft Surface Hub 生成的应用）的环境。 模拟器不使用相同的用户界面与 Surface Hub，但它可用于测试你的应用的外观和行为与 Surface Hub 的屏幕大小和分辨率。

有关详细信息的模拟器工具一般情况下，请参阅[在模拟器中运行的 UWP 应用](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator)。

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>将 Surface Hub 分辨率添加到模拟器
若要将 Surface Hub 分辨率添加到模拟器，请执行以下操作：

1. 创建配置适用于 55 英寸 Surface Hub 通过将以下 XML 代码保存到名为*于 SurfaceHub55.xml*的文件。  

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

2. 创建配置适用于 84 英寸 Surface Hub 通过将以下 XML 代码保存到名为*于 SurfaceHub84.xml*的文件。

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

3. 将这两个 XML 文件复制到 *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;version number&gt;\HardwareConfigurations* 中。

   > [!NOTE]
   > 将文件保存到此文件夹中需要管理权限。

4. 在 Visual Studio 模拟器中运行应用。 单击调色板上的“更改分辨率”**** 按钮，然后从列表中选择 Surface Hub 配置。

    ![Visual Studio 模拟器分辨率](images/vs-simulator-resolutions.png)

   > [!TIP]
   > 更好地[打开平板电脑模式](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet)模拟 Surface Hub 的体验。

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>从 Visual Studio 将应用部署到 Surface Hub 设备
手动将应用部署到 Surface Hub 是一个简单的过程。

### <a name="enable-developer-mode"></a>启用开发人员模式
默认情况下，Surface Hub 仅从 Microsoft Store 安装应用。 若要安装由其他来源签名的应用，必须启用开发人员模式。

> [!NOTE]
> 启用开发人员模式后，你将需要重置 Surface Hub，如果你想要再次将其禁用。 重置设备将删除所有本地用户文件和配置，然后重新安装 Windows。

1. 从 Surface Hub 的“开始”**** 菜单中，打开“设置”应用。

   > [!NOTE]
   > 访问 Surface Hub 上的设置应用需要管理权限。

2. 导航到**更新和安全 \ > 适用于开发人员**。

3. 选择“开发人员模式”**** 并接受警告提示。

### <a name="deploy-your-app-from-visual-studio"></a>从 Visual Studio 部署应用
部署过程的详细信息一般情况下，请参阅[部署和调试 UWP 应用](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)。

   > [!NOTE]
   > 此功能需要 Visual Studio 2015 更新 1 个或更高版本，但我们建议你使用最新的最新版本的 Visual Studio。 你的所有最新的开发和安全更新，将 gibe 保持最新的 Visual Studio 实例。

1. 导航到“开始调试”**** 按钮旁边的调试目标下拉列表，然后选择“远程计算机”****。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 调试目标下拉列表](images/vs-debug-target.png)

2. 输入 Surface Hub 的 IP 地址。 确保选择“通用”**** 身份验证模式。

   > [!TIP] 
   > 在启用开发人员模式后，你可以在欢迎屏幕上找到 Surface Hub 的 IP 地址。

3. 选择**开始调试 (F5)** 部署和调试你的应用在 Surface Hub 中，或者在按住 Ctrl + F5 仅部署应用。

   > [!TIP]
   > 如果 Surface Hub 显示欢迎屏幕，请选择任意按钮以清除它。
