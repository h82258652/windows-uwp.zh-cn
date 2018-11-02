---
author: mtoepke
title: 打包你的通用 Windows 平台 (UWP) DirectX 游戏
description: 较大的通用 Windows 平台 (UWP) 游戏可能会轻易地膨胀得很大，尤其是那些支持具有特定于区域的资源的多语言游戏或具有可选的高清晰度资源的游戏。
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 打包
ms.localizationpriority: medium
ms.openlocfilehash: 252f67a3cb307f10b1a973a17144f211c9c676b0
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5976555"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>打包你的通用 Windows 平台 (UWP) DirectX 游戏

较大的通用 Windows 平台 (UWP) 游戏可能会轻易地膨胀得很大，尤其是那些支持具有特定于区域的资源的多语言游戏或具有可选的高清晰度资源的游戏。 在本主题中，了解如何使用应用包和应用程序包来自定义应用，以使你的客户仅收到真正需要的资源。

除了应用包模型，windows 10 还支持应用程序包可组合在一起的包的两种类型：

-   应用包包括特定于平台的可执行文件和库。 通常，UWP 游戏最多可有 3 个应用包： 分别适用于 x86、x64 和 ARM CPU 体系结构。 特定于该硬件平台的所有代码和数据都必须包含在它的应用包内。 应用包还应该包含所有核心资源，以便可以在保真度和性能的基准级别运行游戏。
-   资源包包含可选的或展开的独立于平台的数据，例如游戏资源（纹理、网格、声音和文本）。 一个 UWP 游戏可以具有一个或多个资源包，包括适用于以下对象的资源包：高清晰度资源或纹理、DirectX 功能级别高于 11+ 的资源，或者特定于语言的资源。

有关应用程序包和应用包的详细信息，请阅读[定义应用资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)。

虽然你可以将所有内容都放置在应用包中，但这非常低效和多余。 为什么要为每个平台 （尤其对于可能不会使用该文件的 ARM 平台）复制三次相同大小的纹理文件？ 我们应该尝试最大程度地减少客户需要下载的内容， 并使他们可以更快速地开始玩游戏、节省其设备上的空间，并且避免可能产生的按流量计费的带宽成本。

若要使用 UWP 应用安装程序中的此功能，重要的是需要在早期游戏开发时考虑用于应用和资源打包的目录布局和文件命名约定， 以便你的工具和源可以正确输出它们，同时使打包变得简单。 当你开发或配置资源创建、管理工具和脚本，以及编写用于加载或引用资源的代码时，都请遵循此文档中概括的规则。

## <a name="why-create-resource-packs"></a>为什么要创建资源包？


当你创建应用时，尤其是创建可以在许多区域设置中或在广泛的 UWP 硬件平台中出售的游戏应用时， 经常需要包含许多文件的多个版本来支持这些区域设置或平台。 例如，如果你要在 美国和日本发布游戏，你可能需要一组用于“en-us”区域设置的采用英语的语音文件，以及另一组用于“jp-jp”区域设置的采用日语的语音文件。 或者，如果你希望针对 ARM 设备以及 x86 和 x64 平台使用游戏中的图像，则必须将相同的图像资源上载三次，每种 CPU 体系结构各一次。

此外，如果你的游戏具有许多高清晰度资源，并且这些资源不适用于较低 DirectX 功能级别的平台，为什么要将其包含在 基准应用包中，并要求你的用户下载设备不会使用的大量组件呢？ 将这些高清晰度资源分隔到可选的资源包意味着， 具有可以支持高清晰度资源的设备的客户可以通过带宽成本（可能按流量计费）来获取它们， 而不具有高端设备的客户可以通过较低网络使用成本更快速地获取游戏。

用于游戏资源包的候选内容包括：

-   特定于国际区域设置的资源（本地化的文本、音频或图像）
-   适用于不同设备比例系数（1.0x、1.4x 和 1.8x）的高分辨率资源
-   用于较高的 DirectX 功能级别（9、10 和 11）的高清晰度资源

所有内容都在作为 UWP 项目的一部分的 package.appxmanifest 中定义，并位于最终包的目录结构中。 由于新的 Visual Studio UI，如果你遵循此文档中的过程，应该不需要手动编辑它。

> **重要** **Windows.ApplicationModel.Resources**通过处理加载和管理这些资源的 \ * Api。 如果你使用这些应用模型资源 API 来加载适用于区域设置、比例系数或 DirectX 功能级别的正确文件，你不需要使用显式文件路径来加载资源；相反，你可以只使用所需资源的一般化文件名来提供资源 API，并让资源管理系统获取适用于用户的当前平台和区域设置配置（同样可以使用相同的 API 直接指定）的资源的正确变体。

 

可以使用以下两种基本方式之一来指定用于资源打包的资源：

-   资产文件具有相同的文件名，并且资源包的特定版本均放置在特定的命名目录中。 系统将保留这些目录名称。 例如，\\en-us、\\scale-140、\\dxfl-dx11。
-   虽然资源文件采用任意名称存储在文件夹中，但这些文件名中都带有共同的标签，其中附加了系统保留的字符串以表示语言或其他限定符。 具体来说，限定符字符串会紧跟一般化文件名后的下划线字符（“\_”）。 例如，\\assets\\menu\_option1\_lang-en-us.png、\\assets\\menu\_option1\_scale-140.png、\\assets\\coolsign\_dxfl-dx11.dds。 你也可以合并这些字符串。 例如，\\assets\\menu\_option1\_scale-140\_lang-en-us.png。
    > **注意**当用于文件名而不是单独用于目录名称，语言限定符必须采用形式"lang-<tag>"，例如"lang-en-us"[定制语言、 比例和其他限定符的资源](../app-resources/tailor-resources-lang-scale-contrast.md)中所述。

     

可以合并目录名称以在资源打包时实现额外特定性。 但是，它们不能是多余的。 例如，\\en-us\\menu\_option1\_lang-en-us.png 就是多余的。

只要每个资源目录中的目录结构相同，你就可以在资源目录下指定所需的任何非保留的子目录名称。 例如，\\\dxfl-dx10\\assets\\textures\\coolsign.dds。 当你加载或引用资源时，路径名必须一般化，从而删除任何用于语言、缩放或 DirectX 功能级别的限定符，无论它们位于文件夹节点还是位于文件名中。 例如，若要在代码中引用具有一个 \\dxfl-dx10\\assets\\textures\\coolsign.dds 变体的资源，请使用 \\assets\\textures\\coolsign.dds。 同样，若要引用具有变体 \\images\\background\_scale-140.png 的资源，请使用 \\images\\background.png。

下面是保留的目录名称和文件名下划线字符前缀：

| 资产类型                   | 资源包目录名称                                                                                                                  | 资源包文件名后缀                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| 本地化资源             | 所有可能的语言，或者 windows 10 的语言和区域设置的组合。 （文件夹名称中不需要限定符前缀“lang-”。） | “\_”，后跟语言、区域设置，或者语言-区域设置说明符。 例如，分别为“\_en”、“\_us”或“\_en-us”。 |
| 比例系数资源        | scale-100、scale-140、scale-180。 它们分别用于 1.0x、1.4x 和 1.8x UI 比例系数。                                     | “\_”，后跟“scale-100”、“scale-140”或“scale-180”。                                                                    |
| DirectX 功能级别资源 | dxfl-dx9、dxfl-dx10 和 dxfl-dx11。 它们分别用于 DirectX 9、10 和 11 功能级别。                                     | “\_”，后跟“dxfl-dx9”、“dxfl-dx10”或“dxfl-dx11”。                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>定义本地化语言资源包


特定于区域设置的文件均放置在以语言命名的项目目录中（例如，“en”）。

在配置你的应用来支持用于多种语言的本地化资源时，你应该：

-   为希望支持的每种语言和区域设置（例如，en-us、jp-jp、zh-cn 和 fr-fr 等）创建应用子目录（或文件版本）。
-   在开发期间，请将所有资产（例如，本地化的音频文件、纹理和菜单图形）的副本放置在相应的语言区域设置子目录中，即使它们在各种语言或区域设置中没有差别。 为了提供最佳用户体验，请确保如果用户未获取适用于其区域设置的可用语言资源包（或者，如果他们在下载和安装后意外删除了它），将向用户发出警报。
-   请确保每个资源或字符串资源文件 (.resw) 在每个目录中具有相同名称。 例如，menu\_option1.png 在 \\en-us 和 \\jp-jp 目录中应该具有相同名称，即使该文件的内容适用于不同的语言。 在本例中，你会看到它们的名称为 \\en-us\\menu\_option1.png 和 \\jp-jp\\menu\_option1.png。
    > **注意**你可以选择将区域设置附加到文件名，并将它们存储在相同的目录中;例如，\\assets\\menu\_option1\_lang-en-us.png、 \\assets\\menu\_option1\_lang-jp-jp.png。

     

-   在 [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) 和 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中使用 API 来为你的应用指定和加载特定于区域设置的资源。 同样，使用不包含指定区域设置的资源引用，因为这些 API 将根据用户设置确定正确的区域设置，然后为用户检索正确的资源。
-   在 Microsoft Visual Studio2015，选择**项目-> 应用商店-> 创建应用包...** 然后创建此程序包。

## <a name="defining-scaling-factor-resource-packs"></a>定义比例系数资源包


Windows 10 提供三个用户界面比例系数： 1.0 x、 1.4 x 和 1.8 x。 在安装期间基于许多组合因素为每个屏幕设置比例值： 屏幕大小、屏幕分辨率，以及假定的用户与屏幕的平均距离。 用户也可调整比例因子以提高可读性。 为了尽可能提供最佳体验，你的游戏应该可以感知 DPI 和比例因子。 此类感知的部分涵义是为三种比例系数分别创建重要可见资源的版本。 它还包含指针交互和点击测试！

在配置你的应用来支持用于不同的 UWP 应用比例系数的资源包时，你应该：

-   为要支持的每种比例系数（scale-100、scale-140 和 scale-180）创建应用子目录（或文件版本）。
-   在开发期间，请将所有资源的具有合适比例系数的副本放置在每个比例系数资源目录中，即使它们在不同的比例系数之间没有差别。
-   请确保每个资源在每个目录中具有相同名称。 例如，menu\_option1.png 在 \\scale-100 和 \\scale-180 目录中应该具有相同名称，即使该文件的内容不同。 在本例中，你会看到它们的名称为 \\scale-100\\menu\_option1.png 和 \\scale-140\\menu\_option1.png。
    > **注意**同样，可以选择将比例系数后缀附加到文件名，并将它们存储在相同的目录中;例如，\\assets\\menu\_option1\_scale-100.png、 \\assets\\menu\_option1\_scale-140.png。

     

-   在 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中使用 API 来加载资源。 资源引用应该一般化（无后缀），且不带特定的比例变体。 系统将检索适用于屏幕和用户设置的适当的比例资源。
-   在 Visual Studio2015，选择**项目-> 应用商店-> 创建应用包...** 然后创建此程序包。

## <a name="defining-directx-feature-level-resource-packs"></a>定义 DirectX 功能级别资源包


DirectX 功能级别与用于之前和当前版本的 DirectX（特别是 Direct3D）的 GPU 功能集对应。 包括着色器模型规格和功能、 着色器语言支持、纹理压缩支持和整体图形管道功能。

你的基准应用包应使用基准纹理压缩格式：BC1、BC2 或 BC3。 任何 UWP 设备 （从低端的 ARM 平台到专用的多 GPU 工作站和媒体计算机）都可以使用这些格式。

DirectX 功能级别 10 或更高级别所支持的纹理格式应该添加到资源包中，以节省本地磁盘空间和下载带宽。 这使你可以使用用于级别 11 的更高级的压缩方案，例如 BC6H 和 BC7。 （有关更多详细信息，请参阅 [Direct3D 11 中的纹理块压缩](https://msdn.microsoft.com/library/windows/desktop/hh308955)。）这些格式对于现代 GPU 支持的高分辨率的纹理资源更有效，使用它们可以在高端平台上改进你的游戏的外观、性能和空间要求。

| DirectX 功能级别 | 支持的纹理压缩 |
|-----------------------|-------------------------------|
| 9                     | BC1、BC2、BC3                 |
| 10                    | BC4、BC5                      |
| 11                    | BC6H、BC7                     |

 

同样，每个 DirectX 功能级别支持不同的着色器模型版本。 可以为每个功能级别创建编译的着色器资源，这些资源可以包含在 DirectX 功能级别资源包中。 此外，一些较高版本的着色器模型可以使用法线贴图等资源，但较早版本的着色器模型不能使用。 这些特定于着色器模型的资源也应该包含在 DirectX 功能级别资源包中。

因为资源机制主要侧重于资源支持的纹理格式，所以它仅支持 3 种整体功能级别。 如果你需要为类似于 DX9\_1 与 DX9\_3 的子级别（点版本）设置独立的着色器，你的资源管理和呈现代码必须明确处理它们。

在配置你的应用来支持用于不同的 DirectX 功能级别的资源包时，你应该：

-   为你要支持的每个 DirectX 功能级别（dxfl-dx9、dxfl-dx10 和 dxfl-dx11）创建应用子目录（或文件版本）。
-   在开发期间，将特定于功能级别的资源放置在各个功能级别资源目录中。 与区域设置和比例系数不同，你的游戏中的每个功能级别都可以具有不同的呈现代码分支，并且如果纹理、编译着色器或其他资源仅用于一个受支持的功能级别，或者仅用于所有受支持的功能级别的子集，则仅将相应资源放置在使用它们的功能级别的目录中。 对于在所有功能级别上加载的资源，请确保每个功能级别资源目录中都包含其具有相同名称的版本。 例如，对于名为“coolsign.dds”的功能级别独立纹理，请将使用 BC3 格式压缩的版本放置到 \\dxfl-dx9 目录中，并将使用 BC7 格式压缩的版本放置到 \\dxfl-dx11 目录中。
-   确保各个目录中的每个资源（如果它可用于多种功能级别）都具有相同的名称。 例如，coolsign.dds 在 \\dxfl-dx9 和 \\dxfl-dx11 目录中应该具有相同名称，即使该文件的内容不相同。 在本例中，你会看到它们的名称为 \\dxfl-dx9\\coolsign.dds 和 \\dxfl-dx11\\coolsign.dds。
    > **注意**同样，可以选择性地将功能级别后缀附加到文件名，并将它们存储在相同的目录中;例如，\\textures\\coolsign\_dxfl-dx9.dds、 \\textures\\coolsign\_dxfl-dx11.dds。

     

-   在配置你的图形资源时，声明受支持的 DirectX 功能级别。
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   在 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中使用 API 来加载资源。 资源引用应该一般化（无后缀），且不带功能级别。 然而，与语言和规模不同，系统不会自动确定哪个功能级别最适合给定屏幕；你需要根据代码逻辑自行确定。 确定后，请使用 API 通知首选功能级别的操作系统。 然后，系统可以根据该首选项检索正确资源。 下面的代码示例显示了如何将当前平台的 DirectX 功能级别通知你的应用：
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **注意**在代码中，直接按名称 （或功能级别目录下的路径） 将纹理加载。 不要包含功能级别目录名称或后缀。 例如，加载“textures\\coolsign.dds”，而不是“dxfl-dx11\\textures\\coolsign.dds”或“textures\\coolsign\_dxfl-dx11.dds”。

     

-   现在，使用 [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) 查找匹配当前 DirectX 功能级别的文件。 **ResourceManager** 将返回 [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/br206089)，你可以使用 [**ResourceMap::GetValue**](https://msdn.microsoft.com/library/windows/apps/br206098)（或 [**ResourceMap::TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj655438)）和提供的 [**ResourceContext**](https://msdn.microsoft.com/library/windows/apps/br206064) 查询它。 这会返回最匹配 DirectX 功能级别的 [**ResourceCandidate**](https://msdn.microsoft.com/library/windows/apps/br206051)，该功能级别已通过调用 [**SetGlobalQualifierValue**](https://msdn.microsoft.com/library/windows/apps/mt622101) 指定。
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   在 Visual Studio2015，选择**项目-> 应用商店-> 创建应用包...** 然后创建此程序包。
-   请确保在 package.appxmanifest 清单设置中启用应用程序包。

## <a name="related-topics"></a>相关主题


* [定义应用资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)
* [打包应用](https://msdn.microsoft.com/library/windows/apps/mt270969)
* [应用包生成工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767)

 

 




