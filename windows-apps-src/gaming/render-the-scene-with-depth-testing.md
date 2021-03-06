---
title: 通过深度测试呈现场景
description: 通过向顶点（或几何图形）着色器和像素着色器中添加深度测试来创建阴影效果。
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 呈现, 场景, 深度测试, direct3d, 阴影
ms.localizationpriority: medium
ms.openlocfilehash: d1c2c4e5d45b28c318085f4ce257b587f23f1426
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368098"
---
# <a name="render-the-scene-with-depth-testing"></a>通过深度测试呈现场景




通过向顶点（或几何图形）着色器和像素着色器中添加深度测试来创建阴影效果。 第 3 部分[演练：实现在 Direct3D 11 中使用深度缓冲区的卷影卷](implementing-depth-buffers-for-shadow-mapping.md)。

## <a name="include-transformation-for-light-frustum"></a>包括用于光锥的转换


顶点着色器需要计算每个顶点的转换光线空间位置。 使用常量缓冲区提供光线空间模型、视图以及投影矩阵。 也可以使用该常量缓冲区为光线计算提供光线位置和法线。 在深度测试期间将使用光线空间中的转换位置。

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

接下来，像素着色器将使用顶点着色器提供的差值光线空间位置来测试像素是否处于阴影中。

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>测试位置是否位于光锥之内


首先，通过将 X 和 Y 坐标规范化来检查像素是否位于视锥之内。 如果两个类型都在范围内\[0，1\] ，那么很可能要在卷影的像素。 否则，你可以跳过深度测试。 通过调用 [Saturate](https://docs.microsoft.com/windows/desktop/direct3dhlsl/saturate) 并将结果与原始值进行比较来快速对着色器进行此项测试。

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>针对阴影映射进行深度测试


使用示例比较函数（[SampleCmp](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmp) 或 [SampleCmpLevelZero](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)）来针对深度映射测试像素在光线空间中的深度。 计算标准化光线空间深度值（即 `z / w`），并将该值传递给比较函数。 由于我们使用 LessOrEqual 示例比较测试，则比较测试通过时固有函数会返回零；这表示像素位于阴影中。

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>计算阴影内外的照明


如果像素不在阴影内，则像素阴影应计算直接照明并将其添加到像素值中。

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

否则，像素阴影将使用环境照明计算像素值。

```cpp
return float4(input.color * ambient, 1.f);
```

在本演练的下一部分中，了解如何[在硬件范围内支持阴影映射](target-a-range-of-hardware.md)。

 

 




