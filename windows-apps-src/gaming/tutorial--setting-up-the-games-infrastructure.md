---
author: mtoepke
title: "设置游戏项目"
description: "装配游戏的第一步是在 Microsoft Visual Studio 中设置项目，以便最大限度减少需要执行的代码基础结构工作量。"
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: fd8e676e66c1df530aca41e05f2ea68d96d01a32

---

# 设置游戏项目


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

装配游戏的第一步是在 Microsoft Visual Studio 中设置项目，以便最大限度减少需要执行的代码基础结构工作量。 通过使用正确的模板和专门为游戏开发配置项目，可以节省大量的时间和减少不必要的麻烦。 我们将指导你完成设置和配置简单游戏项目的步骤。

## 目标


-   了解如何在 Visual Studio 中设置 Direct3D 游戏项目。

## 设置游戏项目


你可以仅使用一个方便的文本编辑器、几个示例和一些原始创意，从头编写一款游戏。 但这可能不是最高效地使用时间的方法。 如果你不熟悉通用 Windows 平台 (UWP) 开发，那么何不让 Visual Studio 帮你完成一些任务？ 下面是让你的项目隆重快速开始所需的操作。

## 1. 选取正确的模板


Visual Studio 模板是一个设置和代码文件集合，专用于基于首选语言和技术的特定类型的应用。 在 Microsoft Visual Studio 2015 中，你可以找到许多模板，这可以大大简化游戏和图形应用的开发。 如果不使用模板，则必须亲自开发大量的基本图形呈现和显示框架，这对于游戏开发新手而言可能是一件苦差事。

适用于本教程的模板是标题为“DirectX 11 应用(通用 Windows)”的模板。 在 Visual Studio 2015 中，依次单击“文件...”****&gt;“新建项目”****，然后：

1.  从“模板”****中，依次选择“Visual C++”****、“Windows”****、“通用”****。
2.  在中心窗格中，选择“DirectX 11 应用（通用 Windows）”****。
3.  为你的游戏项目命名，并单击“确定”****。

![选择“Direct3D 应用程序”模板](images/simple-dx-game-vs-new-proj.png)

此模板为你提供了采用 DirectX 和 C++ 的 UWP 应用的基本框架。 继续用 F5 生成和运行该模板！ 查看浅灰蓝色屏幕。 花些时间查看模板提供的代码。 该模板创建包含使用 DirectX 和 C++ 的 UWP 应用的基本功能的多个代码文件。 我们将在[第 3 步](#3-review-the-included-libraries-and-headers)中更详细地讨论其他代码文件。 现在，让我们快速浏览 **App.h**。

```cpp
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        // Application lifecycle event handlers.
        void OnActivated(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView, Windows::ApplicationModel::Activation::IActivatedEventArgs^ args);
        void OnSuspending(Platform::Object^ sender, Windows::ApplicationModel::SuspendingEventArgs^ args);
        void OnResuming(Platform::Object^ sender, Platform::Object^ args);

        // Window event handlers.
        void OnWindowSizeChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::WindowSizeChangedEventArgs^ args);
        void OnVisibilityChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::VisibilityChangedEventArgs^ args);
        void OnWindowClosed(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::CoreWindowEventArgs^ args);

        // DisplayInformation event handlers.
        void OnDpiChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnOrientationChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnDisplayContentsInvalidated(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);

    private:
        std::shared_ptr<DX::DeviceResources> m_deviceResources;
        std::unique_ptr<MyAwesomeGameMain> m_main;
        bool m_windowClosed;
        bool m_windowVisible;
    };
```

在实现定义视图提供程序的 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) 接口时，创建这 5 个方法：[**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)、[**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)、[**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)、[**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 和 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)。 这些方法由启动游戏时创建的应用单一实例运行，并加载应用的所有资源以及连接相应的事件处理程序。

**main** 方法在 **App.cpp** 源文件中。 它如下所示：

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

现在，它从视图提供程序工厂（**Direct3DApplicationSource**，在 **App.h** 中定义）创建 Direct3D 视图提供程序的一个实例，并将其传递到应用单一实例以运行 ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469))。 这意味着游戏的起始点位于 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法的实现的主体中，在此例中为 **App::Run**。 代码如下：

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

如果游戏窗口没有关闭，则发送所有事件，更新计时器，并呈现和提供图形管道的结果。 我们将在[定义游戏的 UWP 框架](tutorial--building-the-games-metro-style-app-framework.md)和[装配呈现管道](tutorial--assembling-the-rendering-pipeline.md)中详细讨论此内容。 现在，你应了解 UWP DirectX 游戏的基本代码结构。

## 2. 检查和更新 package.appxmanifest 文件


代码文件并不是模板的全部内容。 **package.appxmanifest** 文件包含有关项目的元数据，这些元数据用于打包和启动游戏以及提交到 Windows 应用商店。 它还包含玩家系统用于提供对运行游戏所需的系统资源的访问权限的重要信息。

通过在“解决方案资源管理器”****中双击 **package.appxmanifest** 文件启动“清单设计器”****。 你可以看到此视图：

![package.appx 清单编辑器。](images/simple-dx-game-vs-app-manifest.png)

有关 **package.appxmanifest** 文件和打包的详细信息，请参阅[清单设计器](https://msdn.microsoft.com/library/windows/apps/br230259.aspx)。 现在，请看一下“功能”****选项卡并查看提供的选项。

![Direct3D 应用的默认功能。](images/simple-dx-game-vs-capabilities.png)

如果不选择游戏使用的功能（如用于全球高分榜的“Internet”****），你将无法访问相应的资源或功能。 在创建新游戏时，请确保选择了运行游戏所需的功能！

现在，我们看一下 **DirectX 11 应用（通用 Windows）**模板附带的其他文件。

## 3. 查看包括的库和头文件


有一些文件我们还没有碰到过。 这些文件提供 Direct3D 游戏开发场景通用的附加工具和支持。

| 模板源文件         | 说明                                                                                                                                                                                                            |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StepTimer.h                  | 定义用于游戏或交互呈现应用的高分辨率计时器。                                                                                                                                       |
| Sample3DSceneRenderer.h/.cpp | 定义用于将 Direct3D 交换链和图形适配器连接到使用 DirectX 的 UWP 的基本呈现器实现。                                                                                            |
| DirectXHelper.h              | 实现单个方法 **DX::ThrowIfFailed**，该方法可将 DirectX API 返回的错误 HRESULT 值转换为 Windows 运行时异常。 使用此方法为调试 DirectX 错误放置断点。 |
| pch.h/.cpp                   | 包含所有 Windows 系统中包括的 Direct3D 应用使用的 API，包括 DirectX 11 API。                                                                                                           |
| SamplePixelShader.hlsl       | 包含最基本的像素着色器的高级着色器语言 (HLSL) 代码。                                                                                                                                     |
| SampleVertexShader.hlsl      | 包含最基本的顶点着色器的高级着色器语言 (HLSL) 代码。                                                                                                                                    |

 

### 后续步骤

到目前为止，你可以创建使用 DirectX 的 UWP 游戏项目，并确定 DirectX 11 应用（通用 Windows）提供的组件和文件。

在下一教程[定义游戏的 UWP 框架](tutorial--building-the-games-metro-style-app-framework.md)中，我们将介绍完成的游戏并检查它如何使用和扩展模板提供的许多概念和组件。

 

 







<!--HONumber=Aug16_HO3-->


