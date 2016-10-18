---
author: mtoepke
title: "在 DirectX 游戏中加载资源"
description: "大多数游戏在某些时间会从本地存储或其他一些数据流中加载资源（着色器、纹理、预先定义的网络或其他图形数据）。"
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 09221cb853b3d327b5cb60cacec109032135eabc

---

# 在 DirectX 游戏中加载资源


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

大多数游戏在某些时间会从本地存储或其他一些数据流中加载资源（着色器、纹理、预先定义的网络或其他图形数据）。 下面，让我们看一看在通用 Windows 平台 (UWP) 游戏中加载要使用的这些文件时必须考虑的一个高级视图。

例如，游戏中的多边形对象网格可能是使用其他工具创建的，并且已导出为某个特定格式。 纹理等也是一样：尽管大多数工具通常可以编写平面的未压缩的位图并且大多数图形 API 都可以理解，但这对于在游戏中的使用来说还远远不够。 下面我们将指导你完成加载三个不同类型的图形资源以便用于 Direct3D 的基本步骤：网格（模型）、纹理（位图）以及编译的着色器对象。

## 你需要了解的内容


### 技术

-   并行模式库 (ppltasks.h)

### 先决条件

-   了解基本的 Windows 运行时
-   了解异步任务
-   了解 3-D 图形编程的基本概念。

该示例还包括用于资源加载和管理的三个代码文件。 你将在本主题中遇到在这些文件中定义的代码对象。

-   BasicLoader.h/.cpp
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

可以在以下链接中查找这些示例的完整代码。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[BasicLoader 的完整代码](complete-code-for-basicloader.md)</p></td>
<td align="left"><p>转换图形网格对象并将其加载到内存中的类和方法的完整代码。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[BasicReaderWriter 的完整代码](complete-code-for-basicreaderwriter.md)</p></td>
<td align="left"><p>一般用来读取和写入二进制数据文件的类和方法的完整代码。 由 [BasicLoader](complete-code-for-basicloader.md) 类使用。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[DDSTextureLoader 的完整代码](complete-code-for-ddstextureloader.md)</p></td>
<td align="left"><p>从内存中加载 DDS 纹理的类和方法的完整代码。</p></td>
</tr>
</tbody>
</table>

 

## 说明

### 异步加载

使用并行模式库 (PPL) 中的 **task** 模板处理异步加载。 **task** 包含一个方法调用，后跟完成调用后处理异步调用结果的 lambda，通常遵循以下格式：

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`。

可以使用 **.then\(\)** 语法将任务链接在一起，以便当一个操作完成后，可以运行依赖之前操作的结果的另一个异步操作。 这样，你便可以在单独的线程上加载、转换和管理复杂的资源，而这种方式玩家几乎看不到。

有关更多详细信息，请阅读[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

现在，让我们看一看用于声明和创建异步文件加载方法（即 **ReadDataAsync**）的基本结构。

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

在该代码中，当你的代码调用上面定义的 **ReadDataAsync** 方法时，会创建一个任务来从文件系统中读取缓冲区。 完成后，链接的任务便获取该缓冲区并使用静态的 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 类型将该缓冲区中的字节流入一个数组。

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

下面是对 **ReadDataAsync** 进行的调用。 完成调用后，你的代码便会收到从提供的文件中读取的字节数组。 由于 **ReadDataAsync** 自身定义为任务，因此当返回字节数组时你可以使用 lambda 来执行特定的操作，如将该字节数据传递给可以使用该数据的 DirectX 函数。

如果你的游戏十分简单，当用户启动游戏时可以使用此类方法来加载你的资源。 可以在 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 实现的调用序列中从某些点启动主游戏之前，完成该操作。 而且，异步调用你的自由加载方法，以便游戏可以快速启动，并且玩家无需等待加载完成即可参与早期交互。

但是，你希望在所有异步加载都完成之后再正确启动游戏！ 创建用于指示何时加载完成的一些方法（如某个特定字段）并在你的加载方法上使用 lambda 来设置完成时的指示。 在启动使用这些加载的资源的任何组件之前，检查变量。

下面是游戏启动时，使用 BasicLoader.cpp 中定义的异步方法加载着色器、网格以及纹理的示例。 请注意，当所有加载方法完成时，会在游戏对象上设置一个特定字段 **m_loadingComplete**。

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

请注意，使用 &amp;&amp; 运算符聚合任务，以便设置加载完成标识的 lambda 仅在完成所有任务时触发。 注意，如果你拥有多个标志，则可能会出现争用的情况。 例如，如果 lambda 将两个标志连续设置为同一个值，那么当在设置第二个标志之前进行检查时，另一个线程可能只能看到设置的第一个标志。

你已经了解如何异步加载资源文件。 同步文件加载更简单，你可以在 [BasicReaderWriter 的完整代码](complete-code-for-basicreaderwriter.md)和 [BasicLoader 的完整代码](complete-code-for-basicloader.md)中找到它们的示例。

当然，不同的资源类型通常需要额外的处理或转换，然后才能在你的图形管道中使用它们。 让我们看一看三种特定类型的资源：网格、纹理和着色器。

### 加载网格

网格就是顶点数据，由游戏中的代码按步骤生成或者是从其他应用（如 3DStudio MAX 或 Alias WaveFront）导出到某个文件。 这些网格表示游戏中的模型，从简单的基元（如立方体和球面）到汽车、房屋和字符。 它们通常包含颜色和动画数据，具体取决于它们的格式。 我们重点介绍只包含顶点数据的网格。

若要正确加载网格，你必须知道网格文件中该数据的格式。 上面简单的 **BasicReaderWriter** 类型只是以字节流的形式读取其中的数据；它并不知道表示网格的字节数据，更不用说其他应用程序导出的特定网格格式了！ 你必须在将网格数据放入内存时执行转换。

（你应该始终尝试采用尽可能接近内部表示的格式来封装资源数据。 这样做会减少资源利用并节省时间。）

让我们从网格文件中获取字节数据。 该示例中的格式假定该文件是后缀为 .vbo 的示例特定格式。 （而且，该格式不同于 OpenGL 的 VBO 格式。）每个顶点本身都会映射到 **BasicVertex** 类型，这是在 obj2vbo 转换器工具的代码中定义的结构。 .vbo 文件中顶点数据的布局如下所示：

-   数据流的第一个 32 位（4 个字节）包含网格中的顶点数量 \(numVertices\)，表示为 uint32 值。
-   数据流的下一个 32 位（4 个字节）包含网格中的索引数量 \(numIndices\)，表示为 uint32 值。
-   此后，后续的位 \(numVertices \* sizeof\(**BasicVertex**\)\) 包含顶点数据。
-   最后一个数据位 \(numIndices \* 16\) 包含索引数据，表示为 uint16 值序列。

重点是要知道你加载的网格数据的位级布局。 而且还要确保你符合字节序。 所有 Windows 8 平台都是低字节序。

在该示例中，你从 **LoadMeshAsync** 方法中调用一个方法 CreateMesh 来执行该位级解释。

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh** 解释从文件加载的字节数据，并通过分别向 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 传递顶点和索引列表为网格创建顶点缓冲区和索引缓冲区并指定 D3D11_BIND_VERTEX_BUFFER 或 D3D11_BIND_INDEX_BUFFER。 下面是 **BasicLoader** 中使用的代码：

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

通常为你在游戏中使用的每个网格创建一个顶点/索引缓冲区对。 在哪里以及何时加载网格都由你决定。 如果你有很多网格，那么你可能只希望在游戏中的特定点上从磁盘加载某些网格，如在特定的、预先定义的加载状态期间。 对于较大的网格，如地形数据，则可以从缓存中流入顶点，但该过程比较复杂，不在本主题的范围之内。

再次了解你的顶点数据格式！ 在用于创建模型的工具中，有很多很多方法可用来表示顶点数据。 还有很多不同的方法可用来将顶点数据的输入布局表示为 Direct3D，如三角形列表和带。 有关顶点数据的详细信息，请阅读 [Direct3D 11 中的缓冲区简介](https://msdn.microsoft.com/library/windows/desktop/ff476898)和[基元](https://msdn.microsoft.com/library/windows/desktop/bb147291)。

接下来，让我们看一看如何加载纹理。

### 加载纹理

游戏中最常用的资源（以及由磁盘上和内存中的大多数文件组成的资源）就是纹理。 与网格一样，纹理可以采用各种格式，并且你可以将它们转换为加载时 Direct3D 可以使用的格式。 纹理也有各种类型，用于创建不同的效果。 可以使用纹理的 MIP 级别来改进远距离对象的外观和性能；可以使用深色和浅色映射来产生分层效果并在顶部产生基本纹理；在每个像素照明计算中使用法线贴图 。 在现代游戏中，典型场景可能会有数千个单个纹理，你的代码必须有效地管理它们！

而且与网格一样，用来使内存使用有效的特定格式有很多。 由于纹理可以轻松使用大部分 GPU（以及系统）内存，因此通常会采用某些方式对它们进行压缩。 你不需要在游戏的纹理上使用压缩，你可以使用所需的任何压缩/解压缩算法，只要为 Direct3D 着色器提供它可以理解的格式的数据即可（如 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 位图）。

Direct3D 提供对 DXT 纹理压缩算法的支持，但在玩家的图形硬件可能并不支持每个 DXT 格式. DDS 文件包含 DXT 纹理（以及其他纹理压缩格式），后缀为 .dds。

DDS 文件是包含以下信息的二进制文件：

-   包含四个字符代码值“DDS ”\(0x20534444\) 的 DWORD（幻数）。

-   文件中数据的描述。

    使用 [**DDS\_HEADER**](https://msdn.microsoft.com/library/windows/desktop/bb943982) 描述具有标头描述的数据；使用 [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) 定义像素格式。 请注意，**DDS\_HEADER** 和 **DDS\_PIXELFORMAT** 结构会替换已弃用的 DDSURFACEDESC2、DDSCAPS2 和 DDPIXELFORMAT DirectDraw 7 结构。 **DDS\_HEADER** 是等效于 DDSURFACEDESC2 和 DDSCAPS2 的二进制结构。 **DDS\_PIXELFORMAT** 是等效于 DDPIXELFORMAT 的二进制结构。

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    如果 [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) 中 **dwFlags** 的值设置为 DDPF_FOURCC 并且 **dwFourCC** 设置为“DX10”，那么将提供一个额外的 [**DDS\_HEADER\_DXT10**](https://msdn.microsoft.com/library/windows/desktop/bb943983) 结构来容纳纹理数组或无法表示为 RGB 像素格式的 DXGI 格式，如浮点格式、sRGB 格式等。当提供 **DDS\_HEADER\_DXT10** 结构时，整个数据描述将如下所示。

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   指向包含主要图面数据的字节数组的指针。
    ```cpp
    BYTE bdata[]
    ```

-   指向包含其余图面的字节数组的指针；mipmap 级别、立方体贴图中的面以及体纹理中的深度。 跟随这些链接可获得有关以下内容的 DDS 文件布局的详细信息：[纹理](https://msdn.microsoft.com/library/windows/desktop/bb205578)、[立方体贴图](https://msdn.microsoft.com/library/windows/desktop/bb205577)或[体纹理](https://msdn.microsoft.com/library/windows/desktop/bb205579)。

    ```cpp
    BYTE bdata2[]
    ```

很多工具都导出到 DDS 格式。 如果你没有将纹理导出到该格式的工具，请考虑创建一个工具。 有关 DDS 格式以及如何在代码中使用该格式的详细信息，请阅读 [DDS 编程指南](https://msdn.microsoft.com/library/windows/desktop/bb943991)。 在我们的示例中，我们将使用 DDS。

与其他资源类型一样，从文件中以字节流的形式读取数据。 加载完任务之后，lambda 调用运行代码（**CreateTexture** 方法）将字节流处理成 Direct3D 可以使用的格式。

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

在上面的代码段中，lambda 查看文件名是否具有“dds”扩展名。 如果有，则认为它是 DDS 纹理。 如果没有，则使用 Windows 图像处理组件 \(WIC\) API 发现该格式并将数据解码为位图。 无论采用哪种方法，结果都是 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 位图（或错误）。

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

当该代码完成时，你在内存中拥有一个 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)，它是从图像文件加载的。 与网格一样，你可能会在游戏和任何给定场景中拥有很多 Texture2D。 考虑为定期访问每个场景或每个级别的纹理创建缓存，而不是当游戏或级别启动时将它们全部加载。

（在 [DDSTextureLoader 的完整代码](complete-code-for-ddstextureloader.md)中全面探究在上一示例中调用的 **CreateDDSTextureFromMemory** 方法。）

此外，各个纹理或纹理“皮肤”可能会映射到特定网格多边形或图面。 该映射数据通常由工具导出，艺术家或设计人员使用它来创建模型和纹理。 确保你在加载导出的数据时捕获该信息，因为当你只需分段着色时你将使用它将正确的纹理映射到相应的表面。

### 加载着色器

着色器是编译的高级着色器语言 \(HLSL\) 文件，这些文件加载到内存中并在图形管道的特定阶段进行调用。 最常用和最基本的着色器是顶点和像素着色器，它们在场景的视区中分别处理网格和想色的各个顶点。 执行 HLSL 代码可转换几何体、应用照明效果和纹理，以及对呈现的场景执行后期处理。

Direct3D 游戏可以包含很多不同的着色器，每个着色器编译成一个单独的 CSO（编译的着色器对象，.cso）文件。 正常情况下，你没有这么多需要动态加载它们的着色器，在大多数情况下，只需在游戏启动时加载它们，或者按级别加载（如下雨效果的着色器）。

**BasicLoader** 类中的代码为不同的着色器提供了很多重载，包括顶点、几何体、像素和外壳着色器。 下面的代码以像素着色器为例进行介绍。 （你可以在 [BasicLoader 的完整代码](complete-code-for-basicloader.md)中查看完整代码。）

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

在该示例中，你使用 **BasicReaderWriter** 实例 \(**m\_basicReaderWriter**\) 以字节流的形式读取所提供的编译的着色器对象 \(.cso\) 文件。 该任务完成后，lambda 使用从文件加载的字节数据调用 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)。 你的回调必须设置某些指示加载成功的标志，并且你的代码必须在运行着色器之前检查该标志。

顶点着色器稍微有点复杂。 对于顶点着色器，你还要加载一个单独的定义顶点数据的输入布局。 可以使用下列代码来异步加载顶点着色器以及自定义顶点输入布局。 确保你从网格加载的顶点信息可以由该输入布局正确表示！

让我们在加载顶点着色器之前创建输入布局。

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

在这个特殊的布局中，每个顶点都拥有由顶点着色器处理的下列数据：

-   模型的坐标空间中的 3D 坐标位置 \(x, y, z\) 表示为一个 32 位浮点值的三元组。
-   顶点的法线向量还表示为三个 32 位浮点值。
-   转换后的 2D 纹理坐标值 \(u, v\) 表示为一个 32 位浮点值对。

这些每个顶点的输入元素称为 [HLSL 语义](https://msdn.microsoft.com/library/windows/desktop/bb509647)，并且它们是 一组定义的注册，用于在编译的着色器对象之间来回传递数据。 你的管道为加载的网格中的每个顶点运行一次顶点着色器。 语义定义运行时顶点着色器的输入（以及输出），并且在着色器的 HLSL 代码中为每个顶点的计算提供该数据。

现在，加载顶点着色器对象。

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

在该代码中，你读取顶点着色器 CSO 文件中的字节数据之后，通过调用 [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 创建顶点着色器。 之后，在同一个 lambda 中为着色器创建输入布局。

其他着色器类型（如外壳着色器和几何体着色器）可能还需要特定配置。 [BasicLoader 的完整代码](complete-code-for-basicloader.md)和 [Direct3D 资源加载示例]( http://go.microsoft.com/fwlink/p/?LinkID=265132)中提供了各种着色器加载方法的完整代码。

## 备注

此时，你应该已了解并且能够创建或修改用于异步加载常用游戏资源（如网格、纹理以及编译的着色器）的方法。

## 相关主题

* [Direct3D 资源加载示例]( http://go.microsoft.com/fwlink/p/?LinkID=265132)
* [BasicLoader 的完整代码](complete-code-for-basicloader.md)
* [BasicReaderWriter 的完整代码](complete-code-for-basicreaderwriter.md)
* [DDSTextureLoader 的完整代码](complete-code-for-ddstextureloader.md)

 

 







<!--HONumber=Aug16_HO3-->


