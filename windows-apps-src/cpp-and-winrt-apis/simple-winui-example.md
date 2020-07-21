---
description: 本主题将指导你完成在 C++/WinRT 项目内添加对 WinUI 的简单支持的过程。
title: 一个简单的 C++WinRT Windows UI 库示例
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, Windows UI 库, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 8242055e3c448e2720226859f2ea10e1ae54794f
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437131"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>一个简单的 C++WinRT Windows UI 库示例

本主题详述如何向 C++/WinRT 项目添加对 [Windows UI (WinUI) 库](https://github.com/Microsoft/microsoft-ui-xaml)的简单支持。 顺便说一下，Windows UI 库本身是使用 C++/WinRT 编写的。

> [!NOTE]
> Windows UI (WinUI) 库工具包以 NuGet 包的形式提供，可以添加到任何现有的或新的项目，只要该项目使用 Visual Studio 即可，这一点我们会在本主题中介绍。 如需更多的背景、设置和支持信息，请参阅 [Windows UI 库入门](/uwp/toolkits/winui/getting-started)。

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>创建空白应用 (HelloWinUICppWinRT)

在 Visual Studio 中，使用“空白应用(C++/WinRT)”项目模板创建新项目。 请确保使用的是“(C++/WinRT)”模板，而不是“(通用 Windows)”模板。 

将新项目的名称设置为 *HelloWinUICppWinRT*，（使文件夹结构与此演练匹配），然后取消选中“将解决方案和项目放在同一目录中”。

## <a name="install-the-microsoftuixaml-nuget-package"></a>安装 Microsoft.UI.Xaml NuGet 包

单击“项目”\>“管理 NuGet 包...”\>“浏览”，在搜索框中键入或粘贴 **Microsoft.UI.Xaml**，在搜索结果中选择该项，然后单击“安装”，将包安装到项目中（还会看到许可协议提示）。  注意，只能安装 **Microsoft.UI.Xaml** 包，不要安装 **Microsoft.UI.Xaml.Core.Direct**。

## <a name="declare-winui-application-resources"></a>声明 WinUI 应用程序资源

打开 `App.xaml`，将以下标记粘贴到现有的起始和结束 **Application** 标记之间。

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>将 WinUI 控件添加到 MainPage

接下来，打开 `MainPage.xaml`。 在现有的起始 **Page** 标记中，有一些 XML 命名空间声明。 添加 XML 命名空间声明 `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`。 然后，将以下标记粘贴到现有的起始和结束 **Page** 标记之间，覆盖现有的 **StackPanel** 元素。

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-pchh-as-necessary"></a>根据需要编辑 pch.h

将 NuGet 包（例如此前添加的 **Microsoft.UI.Xaml** 包）添加到某个 C++/WinRT 项目并生成该项目时，此工具会在项目的 `\Generated Files\winrt` 文件夹中生成一组投影头文件。 如果已按照演练进行操作，则现在会有一个 `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt` 文件夹。 若要将这些头文件引入项目中，以便解析对这些新类型的引用，可以进入预编译的头文件（通常为 `pch.h`）中，将它们包括进去。

只需包含与使用的类型对应的标头。 但下面是一个示例，其中包含为 Microsoft.UI.Xaml 包所有生成的头文件。

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>编辑 MainPage.cpp

在 `MainPage.cpp` 中删除 **MainPage::ClickHandler** 实现中的代码，因为 *myButton* 不再存在于 XAML 标记中。

现在可以生成并运行该项目了。

![简单的 C++/WinRT Windows UI 库的屏幕截图](images/winui.png)

## <a name="related-topics"></a>相关主题
* [Windows UI 库入门](/uwp/toolkits/winui/getting-started)