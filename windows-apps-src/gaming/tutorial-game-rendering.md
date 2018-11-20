---
author: joannaleecy
title: 设置
description: 了解如何装配显示图形的呈现管道。 游戏呈现、设置和准备数据。
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 呈现
ms.localizationpriority: medium
ms.openlocfilehash: 134c9005a796f52fb61ba628c0a85c8dbd875442
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295142"
---
# <a name="rendering-framework-ii-game-rendering"></a>呈现框架 II：游戏呈现

在[呈现框架 I](tutorial--assembling-the-rendering-pipeline.md) 中，我们介绍了如何获取场景信息并呈现在显示屏幕上。 现在，我们将后退一步，了解如何准备要呈现的数据。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

快速回顾一下目标。 了解如何设置基本呈现框架，以便为 UWP DirectX 游戏显示图形输出。 我们可以大致将其归纳为以下三个步骤。

 1. 与图形界面建立连接
 2. 准备：创建绘制图形所需的资源
 3. 显示图形：呈现框架

[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)介绍图形的呈现方式，涵盖步骤 1 和 3。 

本文介绍如何设置此框架的其他部分以及准备在发生呈现前所需的数据，即该流程的步骤 2。

## <a name="design-the-renderer"></a>设计呈现器

呈现器负责创建和维护用于生成游戏视觉显示的所有 D3D11 和 D2D 对象。 __GameRenderer__ 类是此示例游戏的呈现器，满足该游戏的呈现需求。

以下是可用于帮助你设计游戏呈现器的一些概念：
* 由于 Direct3D 11 API 定义为 [COM](https://msdn.microsoft.com/library/windows/desktop/ms694363.aspx) API，因此必须提供对这些 API 定义的对象的 [ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) 引用。 当应用终止时这些对象的最后引用超出范围时，将自动释放这些对象。 有关详细信息，请参阅 [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr)。 这些对象的示例：常量缓冲区、着色器对象 - [顶点着色器](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)、[像素着色器](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)以及着色器资源对象。
* 在此类中定义常量缓冲区是为了保留呈现所需的各种数据。
    * 使用具有不同频率的多个常量缓冲区是为了减少每帧必须发送到 GPU 的数据量。 该示例基于常量必须更新的频率将它们分为不同的缓冲区。 这是 Direct3D 编程的最佳做法。 
    * 在此游戏示例中，定义了 4 个常量缓冲区。
        1. __m\_constantBufferNeverChanges__ 包含照明参数。 它在 __FinalizeCreateGameDeviceResources__ 方法中设置一次，此后不再更改。
        2. __m\_constantBufferChangeOnResize__ 包含投影矩阵。 投影矩阵依赖于窗口的大小和纵横比。 它在 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method) 方法中设置一次，然后在使用 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 方法加载资源后进行更新。 如果在 3D 中呈现，也会每帧更改两次。
        3. __m\_constantBufferChangesEveryFrame__ 包含视图矩阵。 此矩阵依赖于相机位置和观看方向（投影的法线），并且使用 __Render__ 方法每帧仅更改一次。 之前在__呈现框架 I：呈现简介__中的 [__GameRenderer::Render__方法](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method)中进行了讨论。
        4. __m\_constantBufferChangesEveryPrim__ 包含每个基元的模型矩阵和材料属性。 模型矩阵将顶点从本地坐标转换为世界坐标。 这些常量特定于每个基元并在每次绘图调用时更新。 之前在__呈现框架 I：呈现简介__中的[基元呈现](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering)下进行了讨论。
* 保持基元纹理的着色器资源对象也在此类中定义。
    * 一些纹理已预先定义（[DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991.aspx) 是可用来存储压缩和未压缩纹理的文件格式。 DDS 纹理用于游戏世界的墙壁和地面以及弹药范围。）
    * 在此游戏示例中，着色器资源对象为：__m\_sphereTexture__、__m\_cylinderTexture__、__m\_ceilingTexture__、__m\_floorTexture__、__m\_wallsTexture__。
* 在此类中定义的着色器对象用于计算基元和纹理。 
    * 在此游戏示例中，着色器对象为 __m\_vertexShader__、__m\_vertexShaderFlat__ 和 __m\_pixelShader__、__m\_pixelShaderFlat__。
    * 顶点着色器处理基元和基本照明，像素着色器（有时称为碎片着色器）处理纹理和任何每像素效果。
    * 这些着色器有两个版本（即规则和平面）来呈现不同的基元。 具有不同版本的原因是平面版本要简单得多，不实现反射高光或任何每像素照明效果。 它们用于墙壁，并且使电量较低的设备的呈现速度更快。

## <a name="gamerendererh"></a>GameRenderer.h

现在我们来看一下此游戏示例的呈现器类对象中的代码，并将其与 DirectX 11 应用模板中提供的 __Sample3DSceneRenderer.h__ 比较。

```cpp
// Class object handling the rendering of the game
ref class GameRenderer
{
internal:
    GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources);
    
    // Compared with Sample3DSceneRenderer.h in the DirectX 11 App template sample. 
    
    // These methods are present.
    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();

    // Added: Async related methods to prepare 3D game objects for rendering
    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();
    // --- end of async related methods section

    // Added: Methods for rendering overlay
    Simple3DGameDX::IGameUIControl^ GameUIControl()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay->Visible(); }
    // --- end of rendering overlay section

    //...
    protected private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>                m_deviceResources;

    // ...

    // Shader resource objects
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>构造函数

接下来，我们来检查此游戏示例的 __GameRenderer__ 构造函数，并将其与 DirectX 11 应用模板中提供的 __Sample3DSceneRenderer__ 构造函数比较。

```cpp
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources) : //...
{
    // Compared with Sample3DSceneRenderer::Sample3DSceneRenderer in the DirectX 11 App template sample. 
    
    // Added: Create a new GameHud object to rendered text on the top left corner of the screen
    m_gameHud = ref new GameHud(
        deviceResources,
        "Windows platform samples",
        "DirectX first-person game sample"
        );
    //--- end of new GameHud object section
        
    // Added: Game info rendered as an overlay on the top right corner of the screen (eg. Hits, Shots, Time)    
    m_gameInfoOverlay = ref new GameInfoOverlay(deviceResources);
    //--- end of game info rendered as overlay section

    // These methods are present.
    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>创建和加载 DirectX 图形资源

在此游戏示例（以及 Visual Studio 的 __DirectX 11 应用（通用 Windows）__ 模板）中，使用从 __GameRenderer__ 构造函数调用的这两种方法实施游戏资源的创建和加载：

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method)

## <a name="createdevicedependentresources-method"></a>CreateDeviceDependentResources 方法

在 DirectX 11 应用模板中，此方法用于异步加载顶点和像素着色器、创建着色器和常量缓冲区，以及使用含有位置和颜色信息的顶点创建网格。 

在此示例游戏中，这些场景对象的操作在 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 和 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 方法中进行拆分。 

在此游戏示例中，哪些内容进入此方法？

* 实例化变量 (__m\_gameResourcesLoaded__ = false and __m\_levelResourcesLoaded__ = false)，指示在继续执行呈现以前是否已加载资源，因为我们正异步加载这些资源。 
* 由于 HUD 和覆盖呈现分别位于单独的类对象中，因此在此处调用 __GameHud::CreateDeviceDependentResources__ 和 __GameInfoOverlay::CreateDeviceDependentResources__ 方法。

下面是适用于 __GameRenderer::CreateDeviceDependentResources__ 的代码。

```cpp
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate if resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud->CreateDeviceDependentResources();
    m_gameInfoOverlay->CreateDeviceDependentResources();
}
```
下表列出了用于创建和加载资源的方法。 __CreateGameDeviceResourcesAsync__ 和 __FinalizeCreateGameDeviceResources__ 被添加到示例游戏中，以便异步加载资源。

|原始 DirectX 11 应用模板           |示例游戏                                                                |
|-------------------------------------------|---------------------------------------------------------------------------|
|CreateDeviceDependentResources             |CreateDeviceDependentResources                                             |
|                                           | - CreateGameDeviceResourcesAsync（已添加）                                  |
|                                           | - FinalizeCreateGameDeviceResources（已添加）                               |
|CreateWindowSizeDependentResources         |CreateWindowSizeDependentResources                                         |

在深入研究用于创建和加载资源的其他方法之前，我们先来创建呈现器并检查它是否适合游戏循环。

## <a name="create-the-renderer"></a>创建呈现器

__GameRenderer__ 在 __GameMain__ 构造函数中创建。 它还调用另外两种方法，[__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 和 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method)，添加这两种方法是为了帮助创建和加载资源。

```cpp

GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) : // ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // These methods are used in the DirectX 11 App template to create the class objects used for rendering. 
    // But are replaced in this game sample with GameRenderer as shown below.
    // m_sceneRenderer = std::unique_ptr<Sample3DSceneRenderer>(new Sample3DSceneRenderer(m_deviceResources));
    // m_fpsTextRenderer = std::unique_ptr<SampleFpsTextRenderer>(new SampleFpsTextRenderer(m_deviceResources));
    
    // Creation of GameRenderer
    m_renderer = ref new GameRenderer(m_deviceResources);
    
    //...

     create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();
    
    //...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>CreateGameDeviceResourcesAsync 方法

由于我们正异步加载游戏资源，因此从 __create\_task__ 循环中的 __GameMain__ 构造函数方法中调用 __CreateGameDeviceResourcesAsync__。
        
__CreateDeviceResourcesAsync__ 是作为一组单独的异步任务运行以加载游戏资源的一种方法。 由于该方法预计将在单独的线程上运行，因此它只能访问 Direct3D 11 设备方法（在 __ID3D11Device__ 上定义），而不能访问设备上下文方法（在 __ID3D11DeviceContext__ 上定义的方法），所以它不能执行任何呈现。

__FinalizeCreateGameDeviceResources__ 方法在主线程上运行，并且具有 Direct3D 11 设备上下文方法的访问权限。

原则上：
* 只能使用 __CreateGameDeviceResourcesAsync__ 中的 __ID3D11Device__ 方法，因为它们无线程限制，这意味着它们可以在任何线程上运行。 此外，预计它们不会在创建 __GameRenderer__ 所在的相同线程上运行。 
* 此处不要使用 __ID3D11DeviceContext__ 中的方法，因为他们需在一个线程上运行，且该线程与 __GameRenderer__ 的线程相同。
* 使用此方法创建常量缓冲区。
* 使用此方法将纹理（如 .dds 文件）和着色器信息（如 .cso 文件）加载到[着色器](tutorial--assembling-the-rendering-pipeline.md#shaders)中。

此方法用于：
* 创建 4 个[常量缓冲区](tutorial--assembling-the-rendering-pipeline.md#buffer)：__m\_constantBufferNeverChanges__、__m\_constantBufferChangeOnResize__、__m\_constantBufferChangesEveryFrame__、__m\_constantBufferChangesEveryPrim__
* 创建封装用于纹理的采样信息的[采样器状态](tutorial--assembling-the-rendering-pipeline.md#sampler-state)对象
* 创建一个包含由该方法创建的所有异步任务的任务组。 它等待所有这些异步任务完成，然后调用 __FinalizeCreateGameDeviceResources__。
* 使用[基本加载器](tutorial--assembling-the-rendering-pipeline.md#basicloader)创建一个加载器。 将加载器的异步加载操作作为任务添加到之前创建的任务组。
* __BasicLoader::LoadShaderAsync__ 和 __BasicLoader::LoadTextureAsync__ 等方法用于加载：
    * 编译的着色器对象（VertextShader.cso、VertexShaderFlat.cso、PixelShader.cso 和 PixelShaderFlat.cso）。 有关详细信息，请转到[各种着色器文件格式](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats)。
    * 游戏特定纹理（Assets\\seafloor.dds、metal_texture.dds、cellceiling.dds、cellfloor.dds、cellwall.dds)。

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC.
    // For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476092.aspx
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));
    
    bd.Usage = D3D11_USAGE_DEFAULT;
    // ...
    
    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476501.aspx
    
    DX::ThrowIfFailed(
        d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges) 
        );
    // ...
    
    // Define D3D11_SAMPLER_DESC. For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476207.aspx
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. 
    // For API ref, go to: https://msdn.microsoft.com/en-us/library/windows/desktop/aa366920(v=vs.85).aspx
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    // ...
    
    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    DX::ThrowIfFailed(
        d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures (resources).
    
    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. 
    // For more info, go to: https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader
    BasicLoader^ loader = ref new BasicLoader(d3dDevice);

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the BasicLoader::LoadShaderAsync method
    // Push these method calls into a list of tasks
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure the previous versions are set to NULL before any of the textures are loaded.
    m_sphereTexture = nullptr;
    // ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks
    tasks.push_back(loader->LoadTextureAsync("Assets\\seafloor.dds", nullptr, &m_sphereTexture));
    // ...
    
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources by introducing a delay.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    return when_all(tasks.begin(), tasks.end());
}
```

## <a name="finalizecreategamedeviceresources-method"></a>FinalizeCreateGameDeviceResources 方法

__FinalizeCreateGameDeviceResources__ 方法在 __CreateGameDeviceResourcesAsync__ 方法中的所有加载资源任务完成后调用。 

* 使用光线位置和颜色初始化 constantBufferNeverChanges。 通过对 __ID3D11DeviceContext::UpdateSubresource__ 的设备上下文方法调用将初始数据加载到常量缓冲区中。
* 异步加载的资源已经完成加载，因此这时可将其与相应的游戏对象关联。
* 对于每个游戏对象，使用已加载的纹理创建网格和材料。 然后将网格和材料与游戏对象关联。
* 对于目标游戏对象，不从纹理文件中加载由顶部带有数值的彩色同心环组成的纹理。 相反，可以使用 __TargetTexture.cpp__ 中的代码按顺序生成。 __TargetTexture__ 类创建在初始化时间将纹理绘制到屏幕外资源所需的资源。 再将生成的纹理与相应的目标游戏对象关联。

__FinalizeCreateGameDeviceResources__ 和 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) 在以下方面共享代码的相似部分：
* 使用 __SetProjParams__ 确保摄像头具有正确的投影矩阵。 有关详细信息，请转到[相机和坐标空间](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space)。
* 通过将放大倍数的 3D 旋转矩阵发布到相机投影矩阵的方式处理屏幕旋转。 然后使用生成的投影矩阵更新 __ConstantBufferChangeOnResize__ 常量缓冲区。
* 设置 __m\_gameResourcesLoaded__ __布尔__全局变量，以指示资源现已加载到缓冲区，可随时执行下一步。 回想一下，我们首先通过 __GameRenderer::CreateDeviceDependentResources__ 方法在 __GameRenderer__ 的构造函数方法中将此变量初始化为 __FALSE__。 
* 当此 __m\_gameResourcesLoaded__ 为 __TRUE__ 时，可以呈现场景对象。 在__呈现框架 I：呈现简介__文章的 [__GameRenderer::Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method) 方法部分对此进行了介绍。

```cpp
// When creating this sample game using the DirectX 11 App template, this method needs to be created.
// This new method is called from GameMain constructor in the .then loop.
// Make sure the 2D rendering is occurring on the same thread as the main rendering.
// Note: Helper class .h and .cpp files used in this method are located in the SharedContent/cpp/GameContent folder
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize constantBufferNeverChanges with the light positions and color.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();
    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    // ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.Get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    MeshObject^ cylinderMesh = ref new CylinderMesh(d3dDevice, 26);
    // ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    // ...
    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {

        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            (*object)->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            (*object)->Mesh(ref new WorldFloorMesh(d3dDevice));
        }
        // ...
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        // ...
    }


    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera()->Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>CreateWindowSizeDependentResource 方法

每当窗口大小、方向、启用了立体声的呈现或分辨率更改时，调用 CreateWindowSizeDependentResources 方法。 在示例游戏中，它会更新__ConstantBufferChangeOnResize__中的投影矩阵。

窗口大小资源按照此方法进行更新： 
* 应用框架获取表示窗口状态更改的多个可能事件的其中一个。 
* 你的主要游戏循环随后获得该事件通知并调用主类 (__GameMain__) 实例上的 __CreateWindowSizeDependentResources__，再调用游戏呈现器 (__GameRenderer__) 类中的 __CreateWindowSizeDependentResources__ 实现。
* 该方法的主要工作是确保视觉元素不会由于窗口属性的更改变得令人困惑或无效。

在此游戏示例中，许多方法调用与 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 方法相同。 有关代码演练，请转到上一部分。

游戏 HUD 和覆盖窗口大小呈现调整包含在[添加用户界面](#tutorial--adding-a-user-interface)中。

```cpp
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{

    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud->CreateWindowSizeDependentResources();

    // ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    // ...

    m_gameInfoOverlay->CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                )
            );

        d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}
```

## <a name="next-steps"></a>后续步骤

这是实现游戏的图形呈现框架的基本过程。 你的游戏越大，你就需要越多的抽象来处理对象类型和动画行为层次结构。 你需要实施更加复杂的方法来加载和管理网格和纹理等资产。 接下来，我们来了解如何[添加用户界面](tutorial--adding-a-user-interface.md)。