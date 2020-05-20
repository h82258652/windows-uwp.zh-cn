---
description: 本指南展示了如何开始使用 WinUI 3 UI 创建 .NET 和 C++/Win32 桌面应用。
title: 适用于桌面应用的 WinUI 3 入门
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580764"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>适用于桌面应用的 WinUI 3.0 入门

WinUI 3.0 预览版 1 引入了新的项目模板，可以使用这些模板创建托管桌面 C#/.NET 应用和原生 C++/Win32 桌面应用并采用完全基于 WinUI 的用户界面。 使用这些项目模板创建应用时，应用程序的整个用户界面都是使用 WinUI 3.0 提供的窗口、控件和其他 UI 类型实现的。 

WinUI 3.0 预览版 1 在 Visual Studio 2019 中添加了以下**桌面版 WinUI** 项目模板：

* 面向 .NET 5 的 C# 应用和库：
  * 打包的空白应用（桌面版 WinUI）
  * 类库（桌面版 WinUI）

* C++/Win32 应用：
  * 打包的空白应用（桌面版 WinUI）

应用项目模板会生成一个 WinUI 应用项目和一个 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经过配置后可以将应用生成为适合部署的 [MSIX 程序包](https://docs.microsoft.com/windows/msix/overview)。

## <a name="prerequisites"></a>必备条件

若要使用的 WinUI 3 是针对本文中所述的桌面项目模板的，请按照以下说明来配置开发计算机：

1. 确保开发计算机上已安装 Windows 10 版本 1803（内部版本 17134）或更高版本。 适用于桌面应用的 WinUI 3 需要 1803 或更高的 OS 版本。

2. 安装 Visual Studio 2019 版本 16.7 预览版 1。 有关详细信息，请参阅[这些说明](index.md#configure-your-dev-environment)。

3. 安装 .NET 5 预览版 4 的 x64 和 x86 版本：
    * x64：[https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86：[https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. 安装 VSIX 扩展，其中包含适用于 Visual Studio 2019 的 WinUI 3.0 预览版 1 项目模板。 有关详细信息，请参阅[这些说明](index.md#visual-studio-project-templates)。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>创建适用于 C# 和 .NET 5 的 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，分别选择“C#”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI)”项目类型，然后单击“下一步”。

    ![空白应用项目模板](images/WinUI-csharp-newproject.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134），然后单击“确定”。

    ![目标版本和最低版本](images/WinUI-min-target-version.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 **App.xaml.cs** 代码文件定义一个表示你的应用实例的 `Application` 类，**MainWindow.xaml.cs** 代码文件定义一个表示应用所显示的主窗口的 `MainWindow` 类。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![应用项目](images/WinUI-csharp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经配置后可将应用生成为适合部署的 MSIX 程序包。 此项目包含应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![应用项目](images/WinUI-csharp-packageproject.png)

7. 若要向应用项目中添加新项，请在**解决方案资源管理器**中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 可以从以下项类型中进行选择：

    * **空白页面**
    * **空白窗口**
    * **自定义控件**
    * **资源字典**
    * **资源文件**
    * **用户控件**

    ![新项](images/WinUI-csharp-newitem.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>为 C++/Win32 创建 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，选择“C++”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI)”项目类型，然后单击“下一步”。

    ![空白应用项目模板](images/WinUI-cpp-newproject.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134），然后单击“确定”。

    ![目标版本和最低版本](images/WinUI-min-target-version.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 **App.xaml** 和各种**应用**代码文件定义一个表示应用实例的 `Application` 类，**MainWindow.xaml** 和各种 **MainWindow** 代码文件定义一个表示应用显示的主窗口的 `MainWindow` 类。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![应用项目](images/WinUI-cpp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经配置后可将应用生成为适合部署的 MSIX 程序包。 此项目包含应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![程序包项目](images/WinUI-cpp-packageproject.png)

7. 若要向应用项目中添加新项，请在**解决方案资源管理器**中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 可以从以下项类型中进行选择：

    * **空白页面**
    * **空白窗口**
    * **自定义控件**
    * **资源字典**
    * **资源文件**
    * **用户控件**

    ![新项](images/WinUI-cpp-newitem.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

## <a name="known-issues-and-limitations"></a>已知问题和限制

有关预览版 1 中的已知问题和限制的列表，请参阅[此部分](index.md#preview-1-limitations-and-known-issues)。

## <a name="related-topics"></a>相关主题

* [WinUI 3.0](index.md)