---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: 创建 Hello World 应用在 C + + CX (windows 10)
description: Microsoft Visual Studio2017，则可以使用 C + + CX 来开发可在 windows 10，包括可在运行 windows 10 的手机上运行的应用。 这些应用具有使用 Extensible Application Markup Language (XAML) 定义的 UI。
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6954f935440f75a728c3f3601ade884bbee7b6bc
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897937"
---
# <a name="create-a-hello-world-app-in-ccx"></a>创建"Hello world"应用在 C + + CX

> [!IMPORTANT]
> 此教程使用 C + + CX。 Microsoft 已发布了 C + + WinRT： 的完全标准新式 C + + 17 语言投影的 Windows 运行时 (WinRT) Api。 有关此语言的详细信息，请参阅[C + + WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)。 

Microsoft Visual Studio2017，则可以使用 C + + CX 开发的应用的 UI 定义 Extensible Application Markup Language (XAML) 中的 windows 10 上运行的应用。

> [!NOTE]
> 本教程使用 Visual Studio Community 2017。 如果使用的是不同版本的 Visual Studio，则其外观可能稍有不同。

## <a name="before-you-start"></a>开始之前

-   若要完成本教程中，必须运行 windows 10 的计算机上使用 Visual StudioCommunity 2017 或 Visual Studio2017 非社区版之一。 若要进行下载，请参阅[获取工具](http://go.microsoft.com/fwlink/p/?LinkId=532666)。
-   我们假设你已基本了解 C + + CX，XAML，并在此[XAML 概述](https://msdn.microsoft.com/library/windows/apps/Mt185595)中的概念。
-   我们假定你正在使用 Visual Studio 中的默认窗口布局。 若要重置为默认布局，请在菜单栏上，依次选择**窗口** > **重置窗口布局**。

## <a name="comparing-c-desktop-apps-to-windows-apps"></a>将 C++ 桌面应用与 Windows 应用比较

如果你习惯使用 C++ 编写 Windows 桌面程序，你可能会发现编写适用于 UWP 的应用在某些方面与此类似，但在其他一些方面则需要了解更多知识。

### <a name="whats-the-same"></a>相同之处

-   你可以使用 STL、 CRT （有某些例外） 以及任何其他 c + + 库，只要代码仅调用可以从 Windows 运行时环境访问的 Windows 函数。

-   如果你习惯可视设计器，你仍然可以使用内置于 Microsoft Visual Studio 的设计器，或者你可以使用功能更加完整的 Blend for Visual Studio。 如果你习惯手动编写 UI 代码，则可以手动编写 XAML 的代码。

-   你仍然可以创建使用 Windows 操作系统类型和你自己的自定义类型的应用。

-   你仍然可以使用 Visual Studio 调试器、探查器和其他开发工具。

-   你仍然可以通过使用 Visual C++ 编译器创建用来编译原生机器代码的应用。 UWP 应用中的 C + + CX 不在托管的运行时环境中执行。

### <a name="whats-new"></a>新增功能有哪些？

-   UWP 应用和通用 Windows 应用的设计准则与桌面应用的设计准则有很大差别。 设计的重点不再是窗口边框、标签和对话框等。 内容才是最重要的。 出色的通用 Windows 应用从最开始的规划阶段就严格遵循这些准则。

-   你将使用 XAML 定义整个 UI。 在 Universal Windows App 中，UI 与核心程序逻辑之间的分离远比在 MFC 或 Win32 应用中更为清晰。 你在代码文件中处理行为的同时，其他用户可以在 XAML 文件中处理 UI 的外观。

-   尽管在 Windows 设备上 Win32 仍然可用于某些功能，但你将主要面向一个易于导航且面向对象的全新 API（即 Windows 运行时）进行编程。

-   使用 C++/CX 使用和创建 Windows 运行时对象。 C++/CX 支持 C++ 异常处理、代理、事件和动态创建的对象的自动引用计数。 使用 C++/CX 时，基础 COM 和 Windows 体系结构的详细信息从应用代码中隐藏。 有关详细信息，请参阅 [C++/CX 语言参考](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)。

-   你的应用将编译为一个程序包，其中还包含有关你的应用所包含的类型、它使用的资源以及它需要的功能（文件访问、Internet 访问和相机访问等）的元数据。

-   在 Microsoft Store 和 Windows Phone Store 中，应用通过一个验证流程确定为安全之后，即可面向无数潜在客户发布。

## <a name="hello-world-store-app-in-ccx"></a>Hello World 应用商店应用 C + + CX

我们的第一个应用是“Hello World”，它演示了交互性、布局和样式的一些基本功能。 我们将通过 Universal Windows App 项目模板创建应用。 如果你已开发 Windows8.1 和之前的 Windows Phone 8.1 应用，你可能还记得你必须具有三个项目在 Visual Studio 中，一个用于 Windows 应用，一个用于手机应用，另一个带有共享代码。 Windows 10 通用 Windows 平台 (UWP) 使只需一个项目，在所有设备，包括运行 windows 10 设备，如平板电脑、 手机、 VR 设备等的台式机和笔记本电脑计算机上运行。

我们将从基础开始：

-   如何在 Visual Studio2017 中创建通用 Windows 项目。

-   如何了解创建的项目和文件。

-   如何了解 VisualC + + 组件扩展中的扩展 (C + + CX)，以及何时使用它们。

**首先，在 Visual Studio 中创建一个解决方案**

1.  在 Visual Studio 的菜单栏上，依次选择**文件** > **新建** > **项目**。

2.  在**新建项目**对话框的左侧窗格中，依次展开**已安装** > **Visual C++** > **Windows 通用**。

> [!NOTE]
> 系统将会提示你安装用于 C++ 开发的 Windows 通用工具。

3.  在中心窗格中，选择**空白应用（通用 Windows）**。

   （如果你没有看到这些选项，请确保已安装通用 Windows 应用开发工具。 有关详细信息，请参阅[准备工作](get-set-up.md)。）

4.  输入项目名称。 我们将其命名为 HelloWorld。

 ![C + + CX 新建项目对话框中的项目模板 ](images/vs2017-uwp-01.png)

5.  选择**确定**按钮。

> [!NOTE]
> 如果你是首次使用 Visual Studio，则可能会看到要求启用**开发人员模式**的“设置”对话框。 开发人员模式是一种用于启用某些功能（如允许直接运行应用，而不是只能从应用商店运行）的特殊设置。 有关更多信息，请阅读[启用设备进行开发](enable-your-device-for-development.md)。 若要继续使用本指南，请选择**开发人员模式**，然后单击**是**并关闭对话框。

   已创建项目文件。

在继续之前，让我们看一下解决方案的内容。

![节点折叠的通用应用解决方案](images/vs2017-uwp-02.png)

### <a name="about-the-project-files"></a>关于项目文件

项目文件中的每个 .xaml 文件在同一个文件夹都有对应的 .xaml.h 文件和 .xaml.cpp 文件，在“生成的文件”文件夹中有 .g 文件和 .g.hpp 文件，该文件夹在磁盘上，但不是项目的一部分。 修改 XAML 文件以创建 UI 元素，并将其连接到数据源 (DataBinding)。 修改 .h 和 .cpp 文件以为事件处理程序添加自定义逻辑。 自动生成的文件表示 XAML 标记的转换为 C + + CX。 请勿修改这些文件，但你可以研究它们以更好地了解代码隐藏的工作原理。 基本上，生成的文件包含 XAML 根元素的部分类定义；此类是你在 \*.xaml.h 和 .cpp 文件中修改的相同类。 生成的文件将 XAML UI 子元素声明为类成员，以便你可以在你编写的代码中引用它们。 在构建时，生成的代码和你的代码合并为完整的类定义，然后进行编译。

让我们先看一下项目文件。

-   **App.xaml、App.xaml.h、App.xaml.cpp：** 代表应用程序对象，这是应用的入口点。 App.xaml 不包含特定于页面的 UI 标记，但是你可以添加希望可以从任何页面访问的 UI 样式和其他元素。 代码隐藏文件包含 **OnLaunched** 和 **OnSuspending** 事件的处理程序。 通常，你在此处添加自定义代码以在应用启动时初始化应用并在它暂停或终止时执行清理。
-   **MainPage.xaml、MainPage.xaml.h、MainPage.xaml.cpp：** 包含应用中的默认“开始”页面的 XAML 标记和代码隐藏。 它没有导航支持或内置控件。
-   **pch.h, pch.cpp：** 预编译标头文件和在你的项目中包含它的文件。 在 pch.h 中，你可以在解决方案中包含任何不经常更改且包含在其他文件中的标头。
-   **package.appxmanifest：** 描述应用所需的设备功能、应用版本信息以及其他元数据的 XML 文件。 要在**清单设计器**中打开此文件，只需双击它。
-   **HelloWorld\_TemporaryKey.pfx：** 支持在此计算机上从 Visual Studio 部署应用的密钥。

## <a name="a-first-look-at-the-code"></a>代码一览

如果你检查共享项目中的 App.xaml.h、App.xaml.cpp 中的代码，你将注意到主要是 C++ 代码看起来很熟悉。 但是，如果你不熟悉 Windows 运行时应用，或者你使用过 C++/CLI，则某些语法元素可能不那么熟悉。 以下是你将在 C++/CX 中看到的最常见的非标准语法元素：

**引用类**

几乎所有 Windows 运行时类，包括 Windows API--XAML 控件中的所有类型、应用中的页面、应用类本身，所有设备和网络对象、所有容器类型，都声明为 **ref class**。 （一些 Windows 类型是 **value class** 或 **value struct**）。 引用类可从任何语言使用。 在 C + + CX，这些类型的生存期由自动引用计数管理 （非垃圾集合），以便你永远不会明确地删除这些对象。 你也可以创建自己的引用类。

```cpp
namespace HelloWorld
{
   /// <summary>
   /// An empty page that can be used on its own or navigated to within a Frame.
   /// </summary>
   public ref class MainPage sealed
   {
      public:
      MainPage();
   };
}
```    

所有 Windows 运行时类型必须在命名空间中进行声明，而且与 ISO C++ 中不同，这些类型本身具有辅助功能修改器。 **public** 修改器使类对命名空间之外的 Windows 运行时组件可见。 **sealed** 关键字表示该类无法用作基类。 几乎已密封所有引用类；而不广泛使用类继承，因为 Javascript 无法理解它。

**ref new** 和 **^ (hats)**

通过使用 ^（乘幂号）运算符声明引用类的变量，并且使用引用新关键字实例化该对象。 然后你使用 -&gt; 运算符访问对象的实例方法，就像 C++ 指针一样。 使用 :: 运算符访问静态方法，就像 ISO C++ 中一样。

在以下代码中，我们使用完全限定的名称来实例化对象，并使用 -&gt; 运算符调用实例方法。

```cpp
Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
     ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();
      
bitmapImage->SetSource(fileStream);
```

通常，在 .cpp 文件中，我们将添加 `using namespace  Windows::UI::Xaml::Media::Imaging` 指令和自动关键字，因此相同的代码看似如下：

```cpp
auto bitmapImage = ref new BitmapImage();
bitmapImage->SetSource(fileStream);
```

**属性**

引用类可以具有属性，和在托管的语言中一样，这些属性是对使用代码显示为字段的特殊成员函数。

```cpp
public ref class SaveStateEventArgs sealed
{
   public:
   // Declare the property
   property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
   {
      Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
   }
   ...
};

   ...
   // consume the property like a public field
   void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
   {    
      if (mruToken != nullptr && !mruToken->IsEmpty())
   {
      e->PageState->Insert("mruToken", mruToken);
   }
}
```

**代理**

和在托管语言中一样，代理是包装带有特定签名的函数的引用类型。 它们经常与事件和事件处理程序一同使用

```cpp
// Delegate declaration (within namespace scope)
public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

// Event declaration (class scope)
public ref class NavigationHelper sealed
{
   public:
   event LoadStateEventHandler^ LoadState;
};

// Create the event handler in consuming class
MainPage::MainPage()
{
   auto navigationHelper = ref new Common::NavigationHelper(this);
   navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
}
```

## <a name="adding-content-to-the-app"></a>向应用添加内容

让我们来向应用添加一些内容。

**步骤 1：修改你的起始页**

1.  在 **“解决方案资源管理器”** 中，打开 MainPage.xaml。
2.  通过向根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 添加以下 XAML 来为 UI 创建控件，紧挨在其结束标记之前。 它包含 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)，后者具有一个询问用户名的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)、一个接受用户名的 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 元素、一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 和其他 **TextBlock** 元素。

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
    ```

3.  此时，你已创建一个非常基本的通用 Windows 应用。 若要查看 UWP 应用的外观，请按 F5，在调试模式下生成、部署并运行该应用。

首先会出现默认的初始屏幕。 它有一个图像（Assets\\SplashScreen.scale-100.png）以及在应用的清单文件中指定的背景色。 若要了解如何自定义初始屏幕，请参阅[添加初始屏幕](https://msdn.microsoft.com/library/windows/apps/Hh465332)。

当初始屏幕消失时，将显示你的应用。 它显示应用的主页面。

![带有控件的 UWP 应用屏幕](images/xaml-hw-app2.png)

它还做不了多少事，但是祝贺你，你已生成了你的第一款通用 Windows 平台应用！

要停止调试并关闭应用，请返回到 Visual Studio 并按 Shift+F5。

有关详细信息，请参阅[从 Visual Studio 运行应用商店应用](http://go.microsoft.com/fwlink/p/?LinkId=619619)。

在应用中，你可以在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中键入，但是单击 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 不执行任何操作。 在以后的步骤中，为按钮的 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件创建事件处理程序，该事件会显示个性化问候。

## <a name="step-2-create-an-event-handler"></a>步骤 2：创建事件处理程序

1.  在 MainPage.xaml 的 XAML 或设计视图中，在之前添加的 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) 中选择“Say Hello”[**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。
2.  按 F4 打开**属性窗口**，然后选择“事件”按钮（![事件按钮](images/eventsbutton.png)）。
3.  查找 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件。 在其文本框中，键入处理 **Click** 事件的函数名称。 对于本示例，请键入“Button\_Click”。

    ![属性窗口, 事件视图](images/xaml-hw-event.png)

4.  按 Enter。 事件处理程序方法在 MainPage.xaml.cpp 中创建并已打开，因此你可以添加在事件出现时执行的代码。

   同时，在 MainPage.xaml 中，更新 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 的 XAML 以声明 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件处理程序，如下所示：

    ```xaml
    <Button Content="Say &quot;Hello&quot;" Click="Button_Click"/>
    ```

   你可以只是手动将其添加到 xaml 代码，如果设计器未加载这可能会很有用。 如果你手动输入此信息，请键入“Click”，然后让 IntelliSense 弹出用于添加新事件处理程序的选项。 这样，Visual Studio 将创建必要的方法声明和存根。

   如果在呈现期间发生未经处理的异常，则设计器将无法加载。 在设计器中呈现涉及到运行该页面的设计时版本。 它可能有助于禁用运行用户代码。 你可以通过在 **“工具, 选项”** 对话框中更改设置来执行此操作。 在 **“XAML 设计器”** 下，取消选中 **“在 XAML 设计器中运行项目代码 (如果支持)”**。

5.  在 MainPage.xaml.cpp 中，将以下代码添加到刚创建的 **Button\_Click** 事件处理程序。 此代码从 `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 控件检索用户名并使用该用户名创建问候语。 `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 将显示相关结果。

    ```cpp
    void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
    {
        greetingOutput->Text = "Hello, " + nameInput->Text + "!";
    }
    ```

6.  将项目设置为启动项，然后按 F5 生成并运行应用。 当你在文本框中键入姓名并单击按钮时，应用会显示个性化问候。

![显示消息的应用屏幕](images/xaml-hw-app4.png)

## <a name="step-3-style-the-start-page"></a>步骤 3：设置起始页的样式

### <a name="choosing-a-theme"></a>选择主题

轻松自定义应用的外观。 默认情况下，应用使用具有浅色样式的资源。 系统资源还包含浅色主题。 我们来尝试一下并看看它的外观。

**切换到深色主题**

1.  打开 App.xaml。
2.  在开始标记 [**Application**](https://msdn.microsoft.com/library/windows/apps/BR242324) 中，编辑 [**RequestedTheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme) 属性并将其值设置为 **Dark**：

    ```xaml
    RequestedTheme="Dark"
    ```

    以下是带有深色主题的完整 [**Application**](https://msdn.microsoft.com/library/windows/apps/BR242324) 标记：

    ```xaml 
        <Application
        x:Class="HelloWorld.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:HelloWorld"
        RequestedTheme="Dark">
    ```

3.  按 F5 生成并运行应用。 请注意它使用深色主题。

![带有深色主题的应用屏幕](images/xaml-hw-app3.png)

你应使用哪个主题？ 你需要的任何一个。 以下我们的观点：对于主要显示图像或视频的应用，我们建议深色主题；对于包含大量文本的应用，我们建议浅色主题。 如果你使用的是自定义配色方案，则请使用最适合应用外观和感觉的主题。 在本教程的其余部分中，我们使用屏幕截图中的浅色主题。

**注意**当应用启动并且无法在应用运行时更改应用主题。

### <a name="using-system-styles"></a>使用系统样式

此时，在 Windows 应用中，所有文本都非常小，因此很难阅读。 让我们通过应用系统样式来解决该问题。

**更改元素样式**

1.  在 Windows 项目中，打开 MainPage.xaml。
2.  在 XAML 或设计视图中，选择以前添加的“你叫什么名字？”[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)。
3.  在 **“属性”** 窗口 (**F4**) 中，选择右上角的“属性”按钮（![“属性”按钮](images/propertiesbutton.png)）。
4.  展开 **“文本”** 组，并将字体大小设置为 18 px。
5.  展开**其他**组并找到 **Style** 属性。
6.  单击属性标记（**样式**属性右侧的绿色框），然后在菜单上，依次选择**系统资源** > **BaseTextBlockStyle**。

     **BaseTextBlockStyle**为在 <root>\\Program Files\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml 中的[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)中定义的资源。

    ![属性窗口，属性视图](images/xaml-hw-style-cpp.png)

     在 XAML 设计图面中，文本外观会发生更改。 在 XAML 编辑器中，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 XAML 会进行更新：

    ```xaml
    <TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
    ```

7.  重复该过程可设置字体大小并将 **BaseTextBlockStyle** 分配到 `greetingOutput`[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 元素。

    **提示**尽管没有任何文本在此[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)中，你将指针移动到 XAML 设计图面时，蓝色轮廓会显示所在，以便你可以选择它。  

    现在的 XAML 如下所示：

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
        </StackPanel>
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" x:Name="greetingOutput"/>
    </StackPanel>
    ```

8.  按 F5 构建并运行应用。 现在它的外观如下所示：

![带有较大文本的应用屏幕](images/xaml-hw-app5.png)

### <a name="step-4-adapt-the-ui-to-different-window-sizes"></a>步骤 4：使 UI 适应不同的窗口大小

现在我们将使 UI 适应不同的屏幕大小，以使其在移动设备上外观良好。 若要执行此操作，添加 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 并设置应用于不同视觉状态的属性。

**调整 UI 布局**

1.  在 XAML 编辑器中，在根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 元素的开始标记后添加此 XAML 块。

    ```xaml
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ```

2.  在本地计算机上调试应用。 请注意，UI 外观与以前相同，除非窗口变得窄于 641 与设备无关的像素 (DIP)。
3.  在移动设备仿真器上调试应用。 请注意 UI 使用你在 `narrowState` 中定义的属性并正确显示在小屏幕上。

![带有样式文本的移动应用屏幕](images/hw10-screen2-mob.png)

如果你在以前版本的 XAML 中使用过 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)，你可能会注意到 XAML 在此处使用简化的语法。

名为 `wideState` 的 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 具有一个 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)，并且其 [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 641。 这意味着仅在窗口宽度不小于 641 DIP 的最小值时应用该状态。 你没有为此状态定义任何 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 对象，因此它会将你在 XAML 中定义的布局属性用于页面内容。

名为 `narrowState` 的第二个 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 具有一个 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)，其 [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 0。 当窗口宽度大于 0 但小于 641 DIP 时，应用此状态。 （在 641 DIP 时，应用 `wideState`。）在此状态下，定义一些 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 对象以更改 UI 中控件的布局属性：

-   将 `contentPanel` 元素的左边距从 120 降低为 20。
-   将 `inputPanel` 元素的 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation) 从 **Horizontal** 更改为 **Vertical**。
-   将 4 DIP 的上边距添加到 `inputButton` 元素。

### <a name="summary"></a>摘要

恭喜，你已完成了第一个教程！ 它教你如何将内容添加到 Windows 通用应用、如何向其添加交互功能，以及如何更改其外观。

## <a name="next-steps"></a>后续步骤

如果你有一个面向 Windows8.1 和/或 Windows Phone 8.1 的 Windows 通用应用项目，你可以将其移植到 windows 10。 虽然没有针对移植的自动操作过程，但你可以手动完成移植。 根据本主题中的指南，从新的 Windows 通用项目开始，获取最新的项目系统结构和清单文件、将你的代码文件复制到项目的目录结构中、将这些项添加到你的项目中，然后使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 重写你的 XAML。 有关详细信息，请参阅[将 Windows 运行时 8 项目移植到通用 Windows 平台 (UWP) 项目](https://msdn.microsoft.com/library/windows/apps/Mt188203)和[移植到通用 Windows 平台 (C++)](http://go.microsoft.com/fwlink/p/?LinkId=619525)。

如果你有要与 UWP 应用集成的现有 C++ 代码，例如用于为现有应用程序创建新的 UWP UI，请参阅[操作方法：在通用 Windows 项目中使用现有 C++ 代码](http://go.microsoft.com/fwlink/p/?LinkId=619623)。

