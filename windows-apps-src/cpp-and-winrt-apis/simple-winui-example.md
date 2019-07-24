---
description: 本主题将指导你完成在 C++/WinRT 项目内添加对 WinUI 的简单支持的过程。
title: 一个简单的 C++WinRT Windows UI 库示例
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, Windows UI 库, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 082e7ca0684495e1f67c2fa79b448866f68a059c
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870342"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>一个简单的 C++WinRT Windows UI 库示例

本主题将指导你完成为 C++/WinRT 项目添加对 Windows UI (WinUI) 库 的简单支持的过程。

> [!NOTE]
> Windows UI (WinUI) 库工具包以 NuGet 包的形式提供，可以添加到任何现有的或新的项目，只要该项目使用 Visual Studio 即可，这一点我们会在本主题中介绍。 如需更多的背景、设置和支持信息，请参阅 [Windows UI 库入门](/uwp/toolkits/winui/getting-started)。

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>创建空白应用 (HelloWinUICppWinRT)

在 Visual Studio 中，使用“空白应用(C++/WinRT)”项目模板创建新项目，并将其命名为 *HelloWinUICppWinRT*。 

## <a name="install-the-microsoftuixaml-nuget-package"></a>安装 Microsoft.UI.Xaml NuGet 包

单击“项目”  \>  “管理 NuGet 包...”\>“浏览”，在搜索框中键入或粘贴 **Microsoft.UI.Xaml**，在搜索结果中选择该项，然后单击“安装”将包安装到项目中（还会看到许可协议提示）。   注意，只能安装 **Microsoft.UI.Xaml** 包，不要安装 **Microsoft.UI.Xaml.Core.Direct**。

## <a name="declare-winui-application-resources"></a>声明 WinUI 应用程序资源

打开 `App.xaml`，将以下标记粘贴到现有的起始和结束 **Application** 标记之间。

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>将 WinUI 控件添加到 MainPage

接下来，打开 `MainPage.xaml`。 在现有的起始 **Application** 标记中，有一些 XML 命名空间声明。 添加 XML 命名空间声明 `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`。 然后，将以下标记粘贴到现有的起始和结束 **Page** 标记之间，覆盖现有的 **StackPanel** 元素。

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>根据需要编辑 MainPage.h 和 .cpp

在 `MainPage.h` 中编辑 include，使之如下所示。

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

最后，在 `MainPage.cpp` 中删除 **MainPage::ClickHandler** 实现中的代码，因为 *myButton* 不再存在于 XAML 标记中。

现在可以生成并运行该项目了。

![简单的 C++/WinRT Windows UI 库的屏幕截图](images/winui.png)

## <a name="related-topics"></a>相关主题
* [Windows UI 库入门](/uwp/toolkits/winui/getting-started)