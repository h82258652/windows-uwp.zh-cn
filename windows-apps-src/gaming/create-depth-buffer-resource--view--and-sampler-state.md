---
title: 创建深度缓冲区设备资源
description: 了解如何创建支持阴影卷的深度测试所需的 Direct3D 设备资源。
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, direct3d, 深度缓冲区
ms.localizationpriority: medium
ms.openlocfilehash: dfd45f620addcf7a3f6292ed2257bdfccc862cd3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368895"
---
# <a name="create-depth-buffer-device-resources"></a>创建深度缓冲区设备资源




了解如何创建支持阴影卷的深度测试所需的 Direct3D 设备资源。 第 1 部分[演练：实现在 Direct3D 11 中使用深度缓冲区的卷影卷](implementing-depth-buffers-for-shadow-mapping.md)。

## <a name="resources-youll-need"></a>所需资源


呈现阴影卷的深度映射需要以下与 Direct3D 设备有关的资源：

-   用于深度映射的资源（缓冲区）
-   资源的深度模具视图和着色器资源视图
-   比较采样器状态对象
-   光线 POV 矩阵的常量缓冲区
-   用于呈现阴影映射的视区（通常为方形视区）
-   用于启用正面剔除的呈现状态对象
-   你还将需要一个呈现状态对象（如果你尚未使用），用于切换回背面剔除。

请注意，这些资源的创建需要包含在与设备有关的资源创建例程中，这样你的呈现器便可以在安装新的设备驱动程序时或者用户将你的应用移动到与其他图形适配器连接的监视器时重新创建它们。

## <a name="check-feature-support"></a>检查功能支持


然后再创建深度映射，调用[ **CheckFeatureSupport** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport)方法在 Direct3D 设备上请求**D3D11\_功能\_D3D9\_卷影\_支持**，并提供[ **D3D11\_功能\_数据\_D3D9\_卷影\_支持**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_d3d9_shadow_support)结构。

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

如果不支持此功能，则不尝试加载着色器编译着色器模型 4 级别 9\_x 调用示例比较函数。 在大多数情况下，缺乏对该功能的支持意味着 GPU 是一款旧设备，其驱动器未更新至支持 WDDM 1.2。 如果设备支持最低功能级别 10\_0，则您可以加载示例比较着色器编译着色器模型 4\_0 改为。

## <a name="create-depth-buffer"></a>创建深度缓冲区


首先，尝试使用精度较高的深度格式创建深度映射。 首先设置匹配的着色器资源视图属性。 如果资源创建失败，例如由于设备内存不足或者存在硬件不支持的格式，请尝试精度较低的格式并更改要匹配的属性。

此步骤是可选的如果您只需要一种低精度深度格式，例如中等分辨率 Direct3D 功能级别 9 上呈现时\_1 的设备。

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

然后创建资源视图。 在深度模具视图上将 mip 片设置为零，在着色器资源视图上将 mip 级别设置为 1。 都有一个纹理尺寸为 TEXTURE2D，并且两者都需要使用匹配[ **DXGI\_格式**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)。

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>创建比较状态


现在创建比较采样器状态对象。 功能级别 9\_1 仅支持 D3D11\_比较\_较少\_相等。 [在硬件范围内支持阴影映射](target-a-range-of-hardware.md)中详细介绍了筛选选项，或者只是选取点筛选以便快速获得阴影映射。

请注意，可以指定 D3D11\_纹理\_地址\_边框寻址模式，它将在 9 的功能级别上处理\_1 的设备。 这适用于像素着色器，该着色器在进行深度测试之前不测试像素是否位于光线的视锥之内。 通过为每个边框指定 0 或 1，你可以控制位于光线的视锥之外的像素是否可以通过深度测试，从而可以控制它们是被照亮还是处于阴影之中。

有关功能级别 9\_1，以下所需的值必须设置：**MinLOD**设置为零， **MaxLOD**设置为**D3D11\_FLOAT32\_最大值**，并且**MaxAnisotropy**设置为零。

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>创建呈现器状态


现在，创建你可以用来启用正面剔除的呈现器状态。 请注意该功能级别 9\_1 的设备需要**DepthClipEnable**设置为**true**。

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

创建你可以用来启用背面剔除的呈现器状态。 如果你的呈现代码已经启用了背面剔除，那么你可以跳过这一步。

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>创建常量缓冲区


不要忘记创建一个用于从光线的角度呈现的常量缓冲区。 你还可以使用该常量缓冲区指定着色器的光线位置。 对点光源使用透视矩阵，对定向光源（太阳光）使用正交矩阵。

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

填写常量缓冲数据。 在初始化时更新常量缓冲，如果从上一帧起更改光值，则会再次更新。

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>创建视区


你需要一个单独的视区来呈现到阴影映射。 视区不是基于设备的资源；你可以在代码中的其他地方创建视区。 创建具有阴影映射的视口可便于将视口的尺寸与阴影映射的尺寸保持一致。

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

在本操作实例的下一部分中，将介绍如何通过[呈现到深度缓冲区](render-the-shadow-map-to-the-depth-buffer.md)来创建阴影映射。

 

 




