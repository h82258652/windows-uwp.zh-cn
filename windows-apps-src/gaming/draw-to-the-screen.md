---
author: mtoepke
title: 绘制到屏幕
description: 最终，我们会移植可将旋转立方体绘制到屏幕的代码。
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 图形
ms.localizationpriority: medium
ms.openlocfilehash: 0050b854b6c8c02cd6eda5e4903fe07ee25d521d
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5982981"
---
# <a name="draw-to-the-screen"></a>绘制到屏幕




**重要的 API**

-   [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)

最终，我们会移植可将旋转立方体绘制到屏幕的代码。

在 OpenGL ES 2.0 中，你的绘制上下文被定义为 EGLContext 类型，该类型包含窗口和图面参数以及绘制到呈现目标所需的资源，将使用呈现目标来编写显示到窗口的最终图像。 你将使用此上下文来配置图形资源，以便在显示器上正确显示着色器管道的结果。 其中一个主要资源是“后台缓冲区”（或“帧缓冲区对象”），它包含可显示到显示器的最终复合呈现目标。

对于 Direct3D，配置图形资源以便绘制到显示器的过程说教性较强，并且需要相当多的 API。 （但是 Microsoft Visual Studio Direct3D 模板可以大大简化这个过程！）若要获取上下文（称为 Direct3D 设备上下文），必须首先获取 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 对象，然后使用该对象来创建和配置 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 对象。 结合使用这两个对象来配置绘制到显示器所需的特定资源。

简而言之，DXGI API 包含的 API 主要用于管理与图形适配器直接相关的资源，而 Direct3D 包含的 API 可用作 GPU 与在 CPU 上运行的主要程序之间的接口。

为了在该示例中进行比较，我们在下面给出了每个 API 的相关类型：

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)：提供图形设备及其资源的视觉表示。
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)：提供用于配置缓冲区以及发出呈现命令的接口。
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)：交换链类似于 OpenGL ES 2.0 中的后台缓冲区。 它是图形适配器上的内存区域，其中包含用于显示的最终呈现的图像。 它称为“交换链”，因为它包含多个可以写入以及“已进行交换”可向屏幕提供最新呈现的缓冲区。
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)：它包含 Direct3D 设备上下文在其中进行绘制且由交换链提供的 2D 位图缓冲区。 和 OpenGL ES 2.0 一样，你可以拥有多个呈现目标，其中一些目标未绑定到交换链，但用于多通道着色技术。

在该模板中，呈现器对象包含以下字段：

Direct3D 11：设备和设备上下文声明

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

下面介绍了如何将后台缓冲区配置为呈现目标以及如何将其提供给交换链。

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

Direct3D 运行时为 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 隐式创建一个 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)，它将纹理表示为交换链可以用于显示器的“后台缓冲区”。

Direct3D 设备和设备上下文以及呈现目标的初始化和配置可以在 Direct3D 模板的自定义 **CreateDeviceResources** 和 **CreateWindowSizeDependentResources** 方法中找到。

当 Direct3D 设备上下文与 EGL 和 EGLContext 类型相关时有关该上下文的详细信息，请参阅[将 EGL 代码移植到 DXGI 和 Direct3D](moving-from-egl-to-dxgi.md)。

## <a name="instructions"></a>说明

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>步骤 1：呈现场景并进行显示

更新立方体数据（在本例中，将其围绕 y 轴稍稍旋转）后，Render 方法将视口设置为绘制上下文 (EGLSurface) 的尺寸。 该上下文包含将使用配置的显示器 (EGLDisplay) 显示到窗口图面 (EGLSurface) 的颜色缓冲区。 此时，该示例更新顶点数据属性、重新绑定索引缓冲区、绘制立方体，并在着色管道绘制的颜色缓冲区中交换到显示器图面。

OpenGL ES 2.0：呈现用于显示的帧

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

在 Direct3D 11 中，该过程非常相似。 （我们假定你使用 Direct3D 模板中的视区和呈现目标配置。）

-   通过调用 [**ID3D11DeviceContext1::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/hh446790) 更新常量缓冲区（在本例中为 model-view-projection 矩阵）。
-   通过 [**ID3D11DeviceContext1::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 设置顶点缓冲区。
-   通过 [**ID3D11DeviceContext1::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) 设置索引缓冲区。
-   通过 [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) 设置特定的三角形拓扑（三角形列表）。
-   通过 [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 设置顶点缓冲区的输入布局。
-   通过 [**ID3D11DeviceContext1::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 绑定顶点着色器。
-   通过 [**ID3D11DeviceContext1::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 绑定片段着色器。
-   通过着色器发送索引的顶点，并通过 [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) 将颜色结果输出到呈现目标缓冲区。
-   通过 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 显示呈现目标缓冲区。

Direct3D 11：呈现用于显示的帧

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

调用 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 之后，你的帧会输出到配置的显示器。

## <a name="previous-step"></a>上一步


[移植 GLSL](port-the-glsl.md)

## <a name="remarks"></a>备注

该示例掩盖了很多与配置设备资源相关的复杂事项，对于通用 Windows 平台 (UWP) DirectX 应用来说更是如此。 我们建议你查看全部模板代码，尤其是执行窗口和设备资源设置和管理的部分。 UWP 应用必须支持旋转事件以及暂停/恢复事件，并且该模板演示了处理显示参数中缺少接口或更改的最佳做法。

## <a name="related-topics"></a>相关主题


* [如何：将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [移植着色器对象](port-the-shader-config.md)
* [移植 GLSL](port-the-glsl.md)
* [绘制到屏幕](draw-to-the-screen.md)

 

 




