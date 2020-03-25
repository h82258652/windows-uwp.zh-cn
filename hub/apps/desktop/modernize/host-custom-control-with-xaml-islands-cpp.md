---
description: 本文演示如何使用 XAML 宿主 API 在C++ Win32 应用程序中托管自定义 UWP 控件。
title: 使用 XAML 宿主 API 在C++ Win32 应用程序中托管自定义 UWP 控件
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10，uwp， C++，Win32，xaml 岛，自定义控件，用户控件，主机控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226311"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>在C++ Win32 应用程序中托管自定义 UWP 控件

本文演示如何使用[UWP xaml 宿主 API](using-the-xaml-hosting-api.md)在新C++的 Win32 应用程序中托管自定义 UWP xaml 控件。 如果你有现有C++的 Win32 应用项目，则可以修改项目的这些步骤和代码示例。

若要承载自定义 UWP XAML 控件，你将在本演练中创建以下项目和组件：

* **Windows 桌面应用程序项目**。 此项目实现本机C++ Win32 桌面应用。 你将向此项目添加代码，该项目使用 UWP XAML 宿主 API 来承载自定义 UWP XAML 控件。

* **UWP 应用项目（C++/WinRT）** 。 此项目实现自定义 UWP XAML 控件。 它还实现根元数据提供程序，用于为项目中的自定义 UWP XAML 类型加载元数据。

## <a name="requirements"></a>要求

* Visual Studio 2019 版本16.4.3 或更高版本。
* Windows 10 版本 1903 SDK （版本10.0.18362）或更高版本。
* 与 visual studio 一起安装的[/WinRT visual studio Extension （VSIX）。 C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 有关详细信息，请参阅[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)。

## <a name="create-a-desktop-application-project"></a>创建桌面应用程序项目

1. 在 Visual Studio 中，创建一个名为**MyDesktopWin32App**的新**Windows 桌面应用程序**项目。 此项目模板可用于**C++** 、 **Windows**和**桌面**项目筛选器。

2. 在**解决方案资源管理器**中，右键单击解决方案节点，单击 "重**定目标解决方案**"，选择**10.0.18362.0**或更高版本的 SDK 版本，然后单击 **"确定"** 。

3. 安装[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包，以便在项目中启用对[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)的支持：

    1. 在**解决方案资源管理器**中右键单击 " **MyDesktopWin32App** " 项目，然后选择 "**管理 NuGet 包**"。
    2. 选择 "**浏览**" 选项卡，搜索[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)包，然后安装此包的最新版本。

4. 在 "**管理 NuGet 包**" 窗口中，安装以下附加 NuGet 包：

    * 6\.0.0 （版本 v 或更高版本[）。](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 此包提供了多个生成和运行时资产，使 XAML 孤岛可以在应用中工作。
    * [XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) （版本 v 6.0.0 或更高版本）。
    * [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140)。

5. 生成解决方案并确认它已成功生成。

## <a name="create-a-uwp-app-project"></a>创建 UWP 应用项目

接下来，向解决方案添加**UWP （C++/WinRT）** 应用项目，并对此项目进行某些配置更改。 稍后在本演练中，你将向此项目添加代码，以实现自定义 UWP XAML 控件并定义 Windows 社区工具包提供的[XamlHost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)类的实例。 您的应用程序将使用此类作为根元数据提供程序，以便为自定义 UWP XAML 类型加载元数据。

1. 在**解决方案资源管理器**中，右键单击 "解决方案" 节点，然后选择 "**添加** -> "**新建项目**"。

2. 将**空白应用（C++/WinRT）** 项目添加到解决方案。 将项目命名为**MyUWPApp** ，并确保目标版本和最低版本均设置为**Windows 10 1903 版**或更高版本。

3. 将[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 包安装在**MyUWPApp**项目中：

    1. 右键单击 " **MyUWPApp** " 项目，然后选择 "**管理 NuGet 包**"。
    2. 选择 "**浏览**" 选项卡，搜索[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)包，然后安装6.0.0 或更高版本的此包。

4. 右键单击**MyUWPApp**节点，然后选择 "**属性**"。 在 " **Common Properties** ->  **C++/WinRT** " 页上，设置以下属性，然后单击 "**应用**"。

    * 将 "**详细级别**" 设置为 "**正常**"。
    * 已将**优化**设置为 "**否**"。

    完成后，"属性" 页应如下所示。

    ![C++/WinRT 项目属性](images/xaml-islands/xaml-island-cpp-1.png)

5. 在 "属性" 窗口的 "**配置属性**" -> "**常规**" 页上，将 "**配置类型**" 设置为 "**动态库（.Dll）** "，然后单击 **"确定"** 以关闭 "属性" 窗口。

    ![常规项目属性](images/xaml-islands/xaml-island-cpp-2.png)

6. 向**MyUWPApp**项目添加占位符可执行文件。 Visual Studio 生成所需的项目文件并正确生成项目时，此占位符可执行文件是必需的。

    1. 在**解决方案资源管理器**中，右键单击**MyUWPApp**项目节点，然后选择 "**添加** -> **新项**"。
    2. 在 "**添加新项**" 对话框中，在左侧页面中选择 "**实用工具**"，然后选择 "**文本文件（.txt）** "。 输入名称**placeholder** ，并单击 "**添加**"。
      ![添加文本文件](images/xaml-islands/xaml-island-cpp-3.png)
    3. 在**解决方案资源管理器**中，选择 "**占位符 .exe** " 文件。 在 "**属性**" 窗口中，确保 " **Content** " 属性设置为 " **True**"。
    4. 在**解决方案资源管理器**中，右键单击**MyUWPApp**项目中的**Appxmanifest.xml**文件，选择 "**打开方式**"，然后选择 " **XML （文本）编辑器**"，然后单击 **"确定"** 。
    5. 查找 **&lt;应用程序&gt;** 元素，并将**可执行**属性更改为 `placeholder.exe`值。 完成后， **&lt;应用程序&gt;** 元素应如下所示。

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. 保存并关闭**appxmanifest.xml**文件。

7. 在**解决方案资源管理器**中，右键单击**MyUWPApp**节点，然后选择 "**卸载项目**"。
8. 右键单击**MyUWPApp**节点，然后选择 "**编辑 MyUWPApp .vcxproj**"。
9. 找到 `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` 元素，并将其替换为以下 XML。 此 XML 紧接在元素之前添加几个新属性。

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. 保存并关闭项目文件。
11. 在**解决方案资源管理器**中，右键单击**MyUWPApp**节点，然后选择 "**重新加载项目**"。

## <a name="configure-the-solution"></a>配置解决方案

在本部分中，将更新包含两个项目的解决方案，以配置项目依赖项和生成正确生成项目所需的属性。

1. 在**解决方案资源管理器**中，右键单击 "解决方案" 节点，然后添加名为**属性**的新 XML 文件。
2. 将以下 XML 添加到**属性**文件。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. 从 "**视图**" 菜单中，单击 "**属性管理器**" （具体取决于你的配置，这可能位于 "**查看** -> **其他窗口**" 下）。
4. 在 "**属性管理器**" 窗口中，右键单击**MyDesktopWin32App** ，然后选择 "**添加现有属性表**"。 导航到刚刚添加的**属性**文件，并单击 "**打开**"。
5. 重复上述步骤，将**属性**文件添加到 "**属性管理器**" 窗口中的 " **MyUWPApp** " 项目。
6. 关闭 "**属性管理器**" 窗口。
7. 确认属性表更改已正确保存。 在**解决方案资源管理器**中，右键单击**MyDesktopWin32App**项目，然后选择 "**属性**"。 单击 "**配置属性**" -> " **Genneral**"，并确认 "**输出目录**" 和 "**中间目录**" 属性的值已添加到**属性**文件中。 你还可以为**MyUWPApp**项目确认相同的。
    ![项目属性](images/xaml-islands/xaml-island-cpp-4.png)

8. 在**解决方案资源管理器**中，右键单击 "解决方案" 节点，然后选择 "**项目依赖项**"。 在 "**项目**" 下拉列表中，确保选中 " **MyDesktopWin32App** "，并选择 "**依赖于**" 列表中的 " **MyUWPApp** "。
    ![项目依赖项](images/xaml-islands/xaml-island-cpp-5.png)

9. 单击“确定”。

## <a name="add-code-to-the-uwp-app-project"></a>将代码添加到 UWP 应用项目

你现在可以将代码添加到**MyUWPApp**项目来执行以下任务：

* 实现自定义 UWP XAML 控件。 稍后在本演练中，你将在**MyDesktopWin32App**项目中添加承载此控件的代码。
* 在 Windows 社区工具包中定义一个派生自 XamlHost 类的类型： [XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)类。 此类充当根元数据提供程序，用于加载自定义 UWP XAML 类型的元数据。

### <a name="define-a-custom-uwp-xaml-control"></a>定义自定义 UWP XAML 控件

1. 在**解决方案资源管理器**中，右键单击**MyUWPApp** ，然后选择 "**添加** -> **新项**"。 选择左窗格中的 **C++可视**节点，选择 "**空白用户控件（C++/WinRT）** "，将其命名为 " **MyUserControl**"，然后单击 "**添加**"。
2. 在 XAML 编辑器中，将**MyUserControl**文件的内容替换为以下 XAML，并保存该文件。

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>定义 XamlApplication 类

接下来，将**MyUWPApp**项目中的默认**应用**类修改为派生自 Windows 社区工具包提供的[XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)类。 稍后在本演练中，你将更新桌面项目，以将此类的实例创建为根元数据提供程序，以便为自定义 UWP XAML 类型加载元数据。

  > [!NOTE]
  > 使用 XAML 孤岛的每个解决方案只能包含一个定义 `XamlApplication` 对象的项目。 应用中的所有自定义 UWP XAML 控件共享相同的 `XamlApplication` 对象。 

1. 在**解决方案资源管理器**中，右键单击**MyUWPApp**项目中的**MainPage**文件。 单击 "**删除**"，然后单击 "**删除**" 以从项目中删除此文件 permamently。
2. 在**MyUWPApp**项目中，展开 " **app.config** " 文件。
3. 用下面的代码替换**app.xaml**、 **app.config**、 **app.config**和**应用 .idl**文件的内容了。

    * **应用 .xaml**：

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **应用 .idl**：

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.config**：

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.config**：

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. 向名为**MyUWPApp**的**项目中添加**一个新的头文件。
5. 将以下代码添加到**app.config**文件中，保存该文件，然后将其关闭。

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. 生成解决方案并确认它已成功生成。

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>将桌面项目配置为使用自定义控件类型

在**MyDesktopWin32App**应用可以在 XAML 岛中承载自定义 UWP XAML 控件之前，必须将其配置为使用**MyUWPApp**项目中的自定义控件类型。 可以通过两种方式执行此操作，并且可以在完成本演练时选择任一选项。

### <a name="option-1-package-the-app-using-msix"></a>选项1：使用 .MSIX 打包应用程序

可以在[.msix 包](https://docs.microsoft.com/windows/msix)中将应用打包，以便进行部署。 .MSIX 是适用于 Windows 的新式应用打包技术，它基于 MSI、.appx、App-v 和 ClickOnce 安装技术的组合。

1. 向解决方案添加新的[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 创建项目时，将其命名为 " **MyDesktopWin32Project** "，并选择 **"Windows 10，版本1903（10.0;版本18362）** 用于**目标版本**和**最低版本**。

2. 在打包项目中，右键单击 "**应用程序**" 节点，然后选择 "**添加引用**"。 在项目列表中，选中 " **MyDesktopWin32App** " 项目旁边的复选框，然后单击 **"确定"** 。
    ![引用项目](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> 如果选择不将应用程序打包到用于部署的[.msix 包](https://docs.microsoft.com/windows/msix)中，则运行应用的计算机必须安装有[Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) 。

### <a name="option-2-create-an-application-manifest"></a>选项2：创建应用程序清单

可以向应用程序中添加[应用程序清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)。

1. 右键单击 " **MyDesktopWin32App** " 项目，然后选择 "**添加** -> **新项**"。 
2. 在 "**添加新项**" 对话框中，单击左窗格中的 "**网站**"，然后选择 " **xml 文件（.xml）** "。 
3. 将新文件命名为**app.config** ，并单击 "**添加**"。
4. 将新文件的内容替换为以下 XML。 此 XML 将在**MyUWPApp**项目中注册自定义控件类型。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>配置其他桌面项目属性

接下来，更新**MyDesktopWin32App**项目以定义其他包含目录的宏，并配置其他属性。

1. 在**解决方案资源管理器**中，右键单击**MyDesktopWin32App**项目，然后选择 "**卸载项目**"。

2. 右键单击 " **MyDesktopWin32App （已卸载）** "，然后选择 "**编辑 MyDesktopWin32App. .vcxproj**"。

3. 将以下 XML 添加到文件末尾的结束 `</Project>` 标记之前。 然后，保存并关闭该文件。

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. 在**解决方案资源管理器**中，右键单击**MyDesktopWin32App （已卸载）** ，然后选择 "**重新加载项目**"。

5. 右键单击 " **MyDesktopWin32App**"，选择 "**属性**"，然后单击左窗格中的 " **C/C++** 节点"。 确认已根据在上一步中所做的项目文件更改定义了**其他包含目录**宏。
    ![C/C++项目设置](images/xaml-islands/xaml-island-cpp-7.png)

6. 在 "**属性页**" 对话框中，展开 "**清单工具** -> **输入和输出**"。 将 " **DPI 识别**" 属性设置为 "**每监视器高 dpi 识别**"。 如果未设置此属性，则在某些高 DPI 方案中可能会遇到清单配置错误。
    ![C/C++项目设置](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>在桌面项目中承载自定义 UWP XAML 控件

最后，你可以将代码添加到**MyDesktopWin32App**项目，以托管你之前在**MyUWPApp**项目中定义的自定义 UWP XAML 控件。

1. 在**MyDesktopWin32App**项目中，打开**framework .h**文件并注释掉以下代码行。 完成后，保存该文件。

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. 打开**MyDesktopWin32App**文件，并将此文件的内容替换为以下代码，以引用必要C++的/WinRT 标头文件。 完成后，保存该文件。

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. 打开**MyDesktopWin32App**文件，并将以下代码添加到 `Global Variables:` 部分。

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. 在同一文件中，将以下代码添加到 `Forward declarations of functions included in this code module:` 部分。

    ```cpp
    void AdjustLayout(HWND);
    ```

5. 在同一文件中，紧接在 `wWinMain` 函数中 `TODO: Place code here.` 注释之后添加以下代码。

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. 在同一文件中，将默认 `InitInstance` 函数替换为以下代码。

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. 在同一文件中，将以下新函数添加到文件末尾。

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. 在同一文件中，找到 `WndProc` 函数。 将 switch 语句中的默认 `WM_DESTROY` 处理程序替换为下面的代码。

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. 保存文件。
10. 生成解决方案并确认它已成功生成。

## <a name="test-the-app"></a>测试应用程序

运行解决方案，确认**MyDesktopWin32App**将打开并出现以下窗口。

![MyDesktopWin32App 应用](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>后续步骤

托管 XAML 孤岛的许多桌面应用程序需要处理其他方案才能提供流畅的用户体验。 例如，桌面应用程序可能需要在 XAML 孤岛中处理键盘输入，在 XAML 孤岛和其他 UI 元素之间定位导航，并更改布局。

有关处理这些方案的详细信息以及指向相关代码示例的链接，请参阅[Win32 应用中C++的 XAML 孤岛的高级方案](advanced-scenarios-xaml-islands-cpp.md)。

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 孤岛）](xaml-islands.md)
* [在C++ Win32 应用中使用 UWP XAML 宿主 API](using-the-xaml-hosting-api.md)
* [在C++ Win32 应用程序中托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [Win32 应用中C++ XAML 孤岛的高级方案](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 孤岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
