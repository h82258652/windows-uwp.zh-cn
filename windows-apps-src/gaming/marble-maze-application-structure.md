---
author: mtoepke
title: "Marble Maze 应用程序结构"
description: "DirectX 通用 Windows 平台 (UWP) 应用与传统桌面应用程序的结构不同。"
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: e9cc290fa77471f315daf7f29f605412d2ec45cc

---

# Marble Maze 应用程序结构


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


DirectX 通用 Windows 平台 (UWP) 应用与传统桌面应用程序的结构不同。 Windows 运行时提供了接口（如 [**Windows::UI::Core::ICoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208296)），以便你可以采用更现代、面向对象的方式开发 UWP 应用，而不是使用句柄类型（如 **HWND**）和函数（如 **CreateWindow**）。 文档的这一部分介绍了如何构造 Marble Maze 应用程序代码。

> **注意** 与本文档对应的示例代码位于 [DirectX Marble Maze 游戏示例](http://go.microsoft.com/fwlink/?LinkId=624011)中。

 
## 
本文档讨论了在构造游戏代码时的一些重要事项：

-   在初始化阶段，设置游戏使用的运行时和库组件。 还要加载特定于游戏的资源。
-   UWP 应用必须在启动后的 5 秒内开始处理事件。 因此，在加载应用时仅加载必要的资源。 游戏应在后台加载大型资源并显示一个进度屏幕。
-   在游戏循环中，响应 Windows 事件，读取用户输入，更新场景对象并渲染场景。
-   使用事件处理程序响应窗口事件。 （这些事件取代了来自桌面 Windows 应用的窗口消息。）
-   使用状态机来控制游戏逻辑的流向和顺序。

##  文件整理


Marble Maze 中的一些组件只需很少或无需修改即可在任何游戏中重用。 对于你自己的游戏，可以调整这些文件所提供的结构和观点。 下表简短描述了重要的源代码文件。

| 文件                                      | 描述                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audio.h、Audio.cpp                         | 定义 **Audio** 类，可管理音频资源。                                                                                                                          |
| BasicLoader.h、BasicLoader.cpp             | 定义 **BasicLoader** 类，可提供实用程序方法来帮助加载纹理、网格和着色器。                                                                  |
| BasicMath.h                                | 定义帮助你处理顶点和矩阵数据以及计算的结构和函数。 其中许多函数与 HLSL 着色器类型兼容。                     |
| BasicReaderWriter.h、BasicReaderWriter.cpp | 定义 **BasicReaderWriter** 类，可使用 Windows 运行时在 UWP 应用中读取和写入文件数据。                                                                    |
| BasicShapes.h、BasicShapes.cpp             | 定义 **BasicShapes** 类，可提供用于创建基本形状（如立方体和球）的实用程序方法。 （Marble Maze 实现未使用这些文件）。 |
| BasicTimer.h、BasicTimer.cpp               | 定义 **BasicTimer** 类，可提供一种轻松的方式来获取总时间和已经过的时间。                                                                                         |
| Camera.h、Camera.cpp                       | 定义 **Camera** 类，可提供相机的位置和方向。                                                                                               |
| Collision.h、Collision.cpp                 | 管理弹珠与其他物体（例如迷宫）之间的碰撞信息。                                                                                                       |
| DDSTextureLoader.h、DDSTextureLoader.cpp   | 定义 **CreateDDSTextureFromMemory** 函数，可从内存缓冲区中加载 .dds 格式的纹理。                                                              |
| DirectXApp.h、DirectXApp.cpp               | 定义 **DirectXApp** 和 **DirectXAppSource** 类，可封装应用的视图（窗口、线程和事件）。                                                     |
| DirectXBase.h、DirectXBase.cpp             | 定义 **DirectXBase** 类，可提供通用于许多 DirectX UWP 应用的基础结构。                                                                            |
| DirectXSample.h                            | 定义可由 DirectX UWP 应用使用的实用工具函数。                                                                                                                      |
| LoadScreen.h、LoadScreen.cpp               | 定义 **LoadScreen** 类，可在应用初始化期间显示一个加载屏幕。                                                                                         |
| MarbleMaze.h、MarbleMaze.cpp               | 定义 **MarbleMaze** 类，可管理特定于游戏的资源并定义许多游戏逻辑。                                                                          |
| MediaStreamer.h、MediaStreamer.cpp         | 定义 **MediaStreamer** 类，可使用媒体基础帮助游戏管理音频资源。                                                                            |
| PersistentState.h、PersistentState.cpp     | 定义 **PersistentState** 类，可从备份存储读取和向备份存储写入基元数据类型。                                                                      |
| Physics.h、Physics.cpp                     | 定义 **Physics** 类，可实现弹珠与迷宫之间的力学模拟。                                                                              |
| Primitives.h                               | 定义游戏使用的几何图形类型。                                                                                                                                   |
| SampleOverlay.h、SampleOverlay.cpp         | 定义 **SampleOverlay** 类，可提供通用的 2D 和用户界面数据与操作。                                                                               |
| SDKMesh.h、SDKMesh.cpp                     | 定义 **SDKMesh** 类，可加载并呈现 SDK Mesh (.sdkmesh) 格式的网格。                                                                                |
| UserInterface.h、UserInterface.cpp         | 定义与用户界面相关的功能，例如菜单系统和高分表。                                                                        |

 

##  设计时资源格式与运行时资源格式


如果可能，使用运行时格式代替设计时格式，以更高效地加载游戏资源。

*设计时*格式是在设计资源时使用的格式。 通常，3D 设计人员使用设计时格式。 一些设计时格式也是基于文本的，以便你可在任何基于文本的编辑器中修改它们。 设计时格式可能很繁复，包含比游戏所需更多的信息。 *运行时*格式是游戏读取的二进制格式。 运行时格式通常比相应的设计时格式更紧凑，并且能更高效地加载。 这正是大多数游戏在运行时使用运行时资源的原因。

尽管游戏可直接读取设计时格式，但使用单独的运行时格式具有许多好处。 因为运行时格式常常更加紧凑，所以它们需要的磁盘空间更少，在网络上传输所需的时间也更少。 另外，运行时格式常常表示为具有对应内存的数据结构。 因此，将其加载到内存中的速度要比（举例而言）基于 XML 的文本文件快得多。 最后，因为独立的运行时格式通常是二进制编码的，所以最终用户更难修改它们。

HLSL 着色器是使用不同设计时和运行时格式的一个资源示例。 Marble Maze 使用 .hlsl 作为设计时格式，使用 .cso 作为运行时格式。 .hlsl 文件包含着色器的源代码；.cso 文件包含相应的着色器字节代码。 离线转换 .hlsl 文件并为游戏提供 .cso 文件时，可避免在游戏加载时将 HLSL 源文件转换为字节代码。

出于指令原因，Marble Maze 项目同时包含许多资源的设计时格式和运行时格式，但你在自己游戏的源项目中只需维护设计时格式，因为可在需要时将它们转换为运行时格式。 本文介绍如何将设计时格式转换为运行时格式。

##  应用程序生命周期


Marble Maze 遵循典型的 UWP 应用的生命周期。 有关 UWP 应用的生命周期的详细信息， 请参阅[应用生命周期](https://msdn.microsoft.com/library/windows/apps/mt243287)。

UWP 游戏初始化时，它通常会初始化运行时组件（例如 Direct3D、Direct2D）和它使用的任何输入、音频或物理库。 它还在游戏开始之前加载特定于游戏的必要资源。 此初始化过程在一个游戏会话中发生一次。

初始化后，游戏通常会运行*游戏循环*。 在此循环中，游戏通常执行 4 个操作：处理 Windows 事件、收集输入、更新场景对象和渲染场景。 游戏更新场景时，它可将当前的输入状态应用到场景对象并模拟力学事件，例如对象碰撞。 游戏也可执行其他活动，例如播放声音效果或通过网络发送数据。 游戏渲染场景时，它捕获场景的当前状态并将它绘制到显示设备。 以下部分更加详细地介绍这些活动。

##  添加到模板


*DirectX 11 应用（通用 Windows）*模板将创建一个可使用 Direct3D 呈现的核心窗口。 模板还包括 **DeviceResources** 类，用于创建在 UWP 应用中呈现 3D 内容所需的所有 Direct3D 设备资源。 **AppMain** 类将在每个帧上创建 **MarbleMaze** 类对象、开始加载资源、循环以更新计时器并调用 **MarbleMaze** 呈现方法。 用于此类的 **CreateWindowSizeDependentResources**、Update 和 Render 方法调用 **MarbleMaze** 类中的相应方法。 以下示例显示了 **AppMain** 构造函数创建 **MarbleMaze** 类对象的位置。 将设备资源类传递到该类，以便它可以使用 Direct3D 对象进行呈现。

```cpp
    m_marbleMaze = std::unique_ptr<MarbleMaze>(new MarbleMaze(m_deviceResources));
    m_marbleMaze->CreateWindowSizeDependentResources();
```

**AppMain** 类还开始为游戏加载延迟资源。 有关详细信息，请参阅下一部分。 **DirectXPage** 构造函数将设置事件处理程序、创建 **DeviceResources** 类，以及创建 **AppMain** 类。

当调用这些事件的处理程序时，它们会将输入传递到 **MarbleMaze** 类。

## 在后台加载游戏资源


若要确保游戏可在启动后的 5 秒内响应窗口事件，我们建议以异步方式或在后台加载游戏资源。 资源加载到后台后，游戏即可响应窗口事件。

> **注意** 也可在主菜单准备好后显示主菜单，并允许剩余资源继续在后台加载。 例如，如果用户在所有资源加载完之前从菜单选择一个选项，你可显示一个进度栏来表明正在继续加载场景资源。

 

即使游戏包含相对较少的游戏资源，以异步方式加载它们也是一种不错的做法，原因有二。 一个原因是很难保证所有资源将快速加载到所有设备和所有配置上。 另外，通过尽早整合异步加载功能，代码随时可随着功能的添加而扩展。

异步资源加载从 **AppMain::Load** 方法开始。 此方法使用 [**task Class (Concurrency Runtime)**](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx) 类在后台加载游戏资源。

```cpp
    task<void>([=]()
    {
        m_marbleMaze->LoadDeferredResources();
    });

```

**MarbleMaze** 类定义 *m\_deferredResourcesReady* 标志来表明异步加载已完成。 **MarbleMaze::LoadDeferredResources** 方法加载游戏资源，然后设置此标志。 应用的更新 (**MarbleMaze::Update**) 和呈现 (**MarbleMaze::Render**) 阶段会检查此标志。 设置此标志后，游戏会像平常一样继续运行。 如果未设置此标志，则游戏会显示加载屏幕。

有关 UWP 应用的异步编程的详细信息，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

>> > **提示** 如果要编写 Windows 运行时 C++ 库（也就是 DLL）中包含的游戏代码，请考虑是否阅读[使用 C++ 为 Windows 应用商店应用创建异步操作](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx)以了解如何创建可供应用和其他库使用的异步操作。

 

## 游戏循环


**DirectPage::OnRendering** 方法运行主要游戏循环。 在每个帧上调用此方法。

为了帮助将视图和窗口代码与特定于游戏的代码分开，我们实现了 **DirectXApp::Run** 方法来将更新和呈现调用转发到 **MarbleMaze** 对象。 **DirectPage::OnRendering** 方法还定义了游戏计时器，用于动画和力学模拟。

以下示例展示了 **DirectPage::OnRendering** 方法，其中包含主要游戏循环。 该游戏循环更新总时间和帧时间变量，然后更新和呈现场景。 这样还可确保仅在窗口可见时渲染内容。

```cpp

void DirectXPage::OnRendering(Object^ sender, Object^ args)
{
    if (m_windowVisible)
    {
        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
}
```

## 状态机


游戏通常包含一个*状态机*（也称为*有限状态机*或 FSM）来控制游戏逻辑的流和顺序。 状态机包含给定数量的状态，并且拥有在它们之间过渡的能力。 状态机通常从一个*初始*状态开始，过渡到一个或多个*中间*状态，最后可能在一个*终止*状态上结束。

游戏循环常常使用状态机，以便它可执行特定于当前游戏状态的逻辑。 Marble Maze 定义了 **GameState** 枚举，后者定义游戏的每个可能状态。

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

例如，**MainMenu** 状态定义主菜单显示和游戏未活动。 相反，**InGameActive** 状态定义游戏是活动的和菜单未显示。 **MarbleMaze** 类定义 **m\_gameState** 成员变量来保留活动的游戏状态。

**MarbleMaze::Update** 和 **MarbleMaze::Render** 方法使用 switch 语句执行当前状态的逻辑。 下面的示例展示了 **MarbleMaze::Update** 方法的这个 switch 语句的可能形式（为了演示结构，已删除了详细内容）。

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

游戏逻辑或呈现依赖于一个特定游戏状态时，我们会在本文档中着重指出。

## 处理应用和窗口事件


Windows 运行时提供了一个面向对象的事件处理系统，以便你能更轻松地管理 Windows 消息。 要在应用中处理一个事件，必须提供一个事件处理程序或事件处理方法来响应该事件。 还必须向事件源注册该事件处理程序。 此过程常常称为事件连接。

### 支持暂停、恢复和重新启动

Marble Maze 在用户离开它或 Windows 进入电量不足状态时暂停。 在用户将游戏移动到前台或 Windows 从电量不足状态中恢复时恢复游戏。 一般而言，你无需关闭应用。 Windows 可在应用处于暂停状态和 Windows 需要该应用所使用的资源（例如内存）时终止它。 Windows 会在应用即将暂停或恢复时通知它，但在应用即将被终止时不会通知它。 因此，在 Windows 通知应用它即将暂停时，应用必须能够保存在应用重新启动时还原当前用户状态所需的任何数据。 如果应用拥有需要很长时间来保存的重要用户状态，你可能还需要定期保存状态，甚至在应用收到暂停通知之前也要定期保存。 Marble Maze 出于两种原因而响应暂停和恢复通知：

1.  应用暂停时，游戏保存当前游戏状态并暂停音频播放。 应用恢复时，游戏恢复音频回放。
2.  应用关闭并在以后重新启动时，游戏从之前的状态恢复。

Marble Maze 执行以下任务来支持暂停和恢复：

-   它在游戏的关键点（例如用户到达一个检查点时）将自己的状态保存到永久存储中。
-   它通过将状态保存到持久存储来响应暂停通知。
-   它通过从持久存储加载状态来响应恢复通知。 它也会在启动期间加载以前的状态。

为了支持暂停和恢复，Marble Maze 定义了 **PersistentState** 类。 （参见 PersistentState.h 和 PersistentState.cpp）。 此类使用 [**Windows::Foundation::Collections::IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) 接口读取和写入属性。 **PersistentState** 类提供了从备份存储读取和向其写入基元数据类型（例如 **bool**、**int**、**float**、[**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) 和 [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/hh755812.aspx)）的方法。

```cpp
ref class PersistentState
{
public:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ m_settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);
    DirectX::XMFLOAT3 LoadXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 defaultValue);
    Platform::String^ LoadString(Platform::String^ key, Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

**MarbleMaze** 类保留一个 **PersistentState** 对象。 **MarbleMaze** 构造函数初始化此对象并提供本地应用程序数据存储作为备份数据存储。

```cpp
m_persistentState = ref new PersistentState();
m_persistentState->Initialize(ApplicationData::Current->LocalSettings->Values, "MarbleMaze");
```

Marble Maze 在弹珠通过一个检查点或目标时（在 **MarbleMaze::Update** 方法中）和窗口失去焦点时（在 **MarbleMaze::OnFocusChange** 方法中）保存它的状态。 如果游戏保留大量状态数据，我们建议偶尔以一种类似方式将状态保存到持久存储中，因为你只有几秒的时间来响应暂停通知。 这样，在应用收到暂停通知时，它只需保存更改的状态数据。

为了响应暂停和恢复通知，**DirectXPage** 类定义了在暂停和恢复时调用的 **SaveInternalState** 和 **LoadInternalState** 方法。 **MarbleMaze::OnSuspending** 方法处理暂停事件，**MarbleMaze::OnResuming** 方法处理恢复事件。

**MarbleMaze::OnSuspending** 方法保存游戏状态并暂停音频。

```cpp
void MarbleMaze::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

**MarbleMaze::SaveState** 方法保存游戏状态值，例如弹珠的当前位置和速度、最新的检查点和高分表。

```cpp
void MarbleMaze::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());
    m_persistentState->SaveSingle(":ElapsedTime", m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0; 
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));
    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(Platform::String::Concat(":ScoreTime", string), iter->elapsedTime);
        m_persistentState->SaveString(Platform::String::Concat(":ScoreTag", string), iter->tag);
    }
}
```

游戏恢复时，它仅需要恢复音频。 它无需从持久存储加载状态，因为状态已加载到内存中。

文档[向 Marble Maze 示例添加音频](adding-audio-to-the-marble-maze-sample.md)中介绍了游戏暂停和恢复音频的方式。

为了支持重新启动，**MarbleMaze::Initialize** 方法（在启动期间调用）调用 **MarbleMaze::LoadState** 方法。 **MarbleMaze::LoadState** 方法读取状态并将状态应用到游戏对象。 如果游戏在暂停时处于暂停或活动状态，此方法还会将当前游戏状态设置为暂停。 我们暂停游戏是为了让用户不会被意外的活动所惊扰。 如果游戏在暂停时未处在玩游戏状态，它还会调出主菜单。

```cpp
void MarbleMaze::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(":Position", m_physics.GetPosition());
    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(":Velocity", m_physics.GetVelocity());
    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(":GameState", static_cast<int>(m_gameState));
    int currentCheckpoint = m_persistentState->LoadInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(Platform::String::Concat(":ScoreTime", string), 0.0f);
        entry.tag = m_persistentState->LoadString(Platform::String::Concat(":ScoreTag", string), L"");
        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> **重要提示** Marble Maze 不会区分冷启动（也就是首次启动，而之前没有暂停事件）和从暂停状态恢复。 这是所有 UWP 应用的建议设计。

 

有关演示如何在本地应用程序数据存储中存储和检索设置和文件的更多示例，请参阅[快速入门：本地应用程序数据](https://msdn.microsoft.com/library/windows/apps/hh465118)。 有关应用程序数据的详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/library/windows/apps/mt299098)。

##  后续步骤


阅读[向 Marble Maze 示例添加可视内容](adding-visual-content-to-the-marble-maze-sample.md)，了解在使用可视资源时要记住的一些重要实践。

## 相关主题

* [向 Marble Maze 添加可视内容示例](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze 示例基础](marble-maze-sample-fundamentals.md)
* [开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


