---
author: Mtoepke
title: 在 Xbox 开发环境上设置你的 UWP
description: 在 Xbox 开发环境上设置和测试你的 UWP 的步骤。
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 2234b7d39f130da03562176f0df878701d524635
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6042679"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>在 Xbox 开发环境上设置你的 UWP

Xbox 开发环境上的通用 Windows 平台 (UWP) 由通过本地网络连接到 Xbox One 控制台的开发电脑组成。
开发电脑需要 Windows 10、Visual Studio 2017 或 Visual Studio 2015 Update 3、Windows 10 SDK 内部版本 14393 或更高版本以及各种支持工具。


本文介绍了设置和测试你的开发环境的步骤。

## <a name="visual-studio-setup"></a>Visual Studio 安装程序

1. 安装 Visual Studio 2017，Visual Studio 2015 更新 3 或 Visual Studio 的最新版本。 有关详细信息以及安装方式，请参阅[适用于 Windows 10 的下载和工具](https://dev.windows.com/downloads)。 我们建议你使用 Visual Studio 的最新版本，以便你可以为开发人员和安全接收最新的更新。

2. 如果要安装 Visual Studio 2017，请确保选择**通用 Windows 平台开发**工作负载。 如果你是 C++ 开发人员，请确保还要在右侧的**摘要**窗格中选择**通用 Windows 平台开发**下的 **C++ 通用 Windows 平台工具**复选框。 它不是默认安装的一部分。

    ![安装 Visual Studio 2017](images/development-environment-setup-1.png)

    如果要安装 Visual Studio 2015 Update 3，请确保选中**通用 Windows 应用开发工具**复选框。

    ![安装 Visual Studio 2015 Update 2](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Windows 10 SDK 安装程序

安装最新的 Windows 10 SDK。 它将随 Visual Studio 安装程序提供，但如果你想单独下载它，请参阅 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。


## <a name="enabling-developer-mode"></a>启用开发人员模式

在从开发电脑部署应用之前，你必须启用开发人员模式。 在**设置**应用中，导航到**更新和安全** / **对于开发人员**，在**使用开发人员功能**下选择**开发人员模式**。

## <a name="setting-up-your-xbox-one"></a>设置 Xbox One

在可以将应用部署到你的 Xbox One 之前，必须先在该主机上进行用户登录。 可以使用你的现有 Xbox Live 帐户，也可以在开发人员模式下为你的主机创建新帐户。 

## <a name="create-your-first-app"></a>创建你的第一个应用

1. 确保你的开发电脑与目标 Xbox One 主机在同一个本地网络上。 通常，这意味着它们应使用相同的路由器并位于同一个子网上。 建议使用有线网络连接。

2. 确保你的 Xbox One 主机处于开发人员模式下。  有关详细信息，请参阅 [Xbox One 开发人员模式激活](devkit-activation.md)。

3. 确定要用于 UWP 应用的编程语言。

4. 在开发电脑的 Visual Studio 中，选择**新建/项目**。

5. 在**新建项目**窗口中，选择 **Windows 通用/空白应用（通用 Windows）**。

### <a name="starting-a-c-project"></a>启动 C# 项目

  ![“新建项目”对话框](images/development-environment-setup-2.png)

1. 在**新建通用 Windows 项目**对话框中，在**最低版本**下拉列表中选择内部版本 14393 或更高版本。 在**目标版本**下拉列表中选择最新的 SDK。 如果出现**开发人员模式**对话框，请单击**确定**。 将创建一个新的空白应用。

2. 为远程调试配置开发环境：

    a. 在**解决方案资源管理器**中右键单击项目，然后选择**属性**。

    b. 在**调试**选项卡上，将**平台**更改为 **x64**。 （Xbox 不再支持 x86 平台。）

    c. 在**启动选项**下，将**目标设备**更改为**远程计算机**。

    d. 在**远程计算机**中，输入系统 IP 地址或 Xbox One 主机的主机名。 有关获取 IP 地址或主机名的信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。

    e. 在**身份验证模式**下拉列表中，选择**通用（未加密协议）**。

    ![C# BlankApp 属性页](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>启动 C++ 项目

  ![C++ 项目](images/development-environment-setup-3.png)

1. 在**新建通用 Windows 项目**对话框中，在**最低版本**下拉列表中选择内部版本 14393 或更高版本。 在**目标版本**下拉列表中选择最新的 SDK。 如果出现**开发人员模式**对话框，请单击**确定**。 将创建一个新的空白应用。

2. 为远程调试配置开发环境：

   a. 在**解决方案资源管理器**中右键单击项目，然后选择**属性**。

   b. 在**调试**选项卡上，将**要启动的调试器**更改为**远程计算机**。

   c. 在**计算机名称**中，输入系统 IP 地址或 Xbox One 主机的主机名。 有关获取 IP 地址或主机名的信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。

   d. 在**身份验证类型**下拉列表中，选择**通用（未加密协议）**。

   e. 在**平台**下拉列表中选择 **x64**。

    ![C++ BlankApp 属性页](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>将你的设备与 Visual Studio 进行 PIN 配对

1. 保存你的设置，并确保你的 Xbox One 主机处于开发人员模式下。

2. 在 Visual Studio 中打开你的项目，然后按 F5。

3. 如果这是你第一次部署，你将获取来自 Visual Studio 的对话框，要求你对你的设备进行 PIN 配对。

    a. 若要获取 PIN，请从 Xbox One 主机上的主屏幕打开**开发人员主页**。

    b. 在**主页**选项卡上的**快速操作**下，选择**显示 Visual Studio pin**。
  
    ![“与 Visual Studio 配对”对话框](images/development-environment-setup-5.png)

    c. 在**与 Visual Studio 配对**对话框中输入你的 PIN。 以下 PIN 只是一个示例；你的 PIN 将有所不同。

    ![“与 Visual Studio PIN 配对”对话框](images/devhome_pin.png)

    d. 部署错误（如果有）将显示在**输出**窗口中。

恭喜，你已在 Xbox 上成功创建和部署了你的第一个 UWP 应用！

## <a name="see-also"></a>另请参阅
- [Xbox One 开发人员模式激活](devkit-activation.md)  
- [适用于 Windows 10 的下载和工具](https://dev.windows.com/downloads)  
- [Windows 预览体验计划](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Xbox One 工具简介](introduction-to-xbox-tools.md) 
- [Xbox One 上的 UWP](index.md)