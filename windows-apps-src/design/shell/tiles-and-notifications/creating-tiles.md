---
Description: 磁贴是应用在“开始”菜单上的表示形式。 每个应用都有一个磁贴。 在 Microsoft Visual Studio 中创建新的 Windows 应用应用程序项目时，该项目包含显示应用名称和徽标的默认磁贴。
title: 适用于 Windows 应用的磁贴
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0882ac67766bc2ce037133cf8a39b5393f616e13
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970992"
---
# <a name="tiles-for-windows-apps"></a>适用于 Windows 应用的磁贴

 

*磁贴*是应用在 "开始" 菜单上的表示形式。 每个应用都有一个磁贴。 在 Microsoft Visual Studio 中创建新的 Windows 应用应用程序项目时，该项目包含显示应用名称和徽标的默认磁贴。应用首次安装时，Windows 将显示此磁贴。 应用安装后，可通过通知更改磁贴内容，例如，可更改磁贴以将新信息传递给用户（如头条新闻或最近未读邮件的主题）。

## <a name="configure-the-default-tile"></a>配置默认磁贴


当在 Visual Studio 中创建新项目时，它将创建显示应用名称和徽标的简单默认磁贴。

若要编辑磁贴，请双击主 UWP 项目中的 **Package.appxmanifest** 文件以打开设计器（或者右键单击该文件，然后选择“查看代码”）。

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

应更新以下项：

-   DisplayName：将要在磁贴上显示的名称替换此值。
-   ShortName：由于磁贴上容纳显示名称的空间有限，我们也建议指定 ShortName，以确保应用名称不会被截断。
-   徽标图像：

    应用自己的图像替换这些图像。 可选择为不同的缩放提供图像，但无需为所有缩放提供。 若要确保应用在一系列设备上具有良好的外观，我们建议提供每个图像的 100%、200% 和 400% 缩放版本。 若要详细了解如何生成这些资产，请参阅[应用程序图标和徽标](/windows/uwp/design/style/app-icons-and-logos)。

    缩放的图像应遵循此命名约定：
    
    *&lt;&gt;**映像名称&gt; &lt;* 系数。*图像文件扩展名&gt; &lt;* 

    例如：SplashScreen.scale-100.png

    引用该映像时，请将其引用为* &lt;映像名称&gt;*。*图像文件扩展名&gt; （在本示例中为 "SplashScreen"）。 &lt;* 系统将自动从你提供的图像中为设备选择相应的缩放图像。

-   虽然不是必须的，但我们强烈建议你提供适用于宽磁贴和大磁贴的徽标，以便用户可以将应用磁贴大小调整到这些大小。 若要提供这些附加图像，可创建 **DefaultTile** 元素并使用 **Wide310x150Logo** 和 **Square310x310Logo** 属性指定附加图像：
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>使用通知自定义磁贴


应用安装后，可使用通知自定义磁贴。 可在首次启动应用或响应事件（如推送通知）时执行此操作。

若要了解如何发送磁贴通知，请参阅[发送本地磁贴通知](sending-a-local-tile-notification.md)。
