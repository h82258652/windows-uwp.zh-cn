---
author: TylerMSFT
title: 处理 URI 激活
description: 了解如何将应用注册为统一资源标识符 (URI) 方案名称的默认处理程序。
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c7286493e08fd62ad4b207d0e014dd4fbd5318
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3961021"
---
# <a name="handle-uri-activation"></a>处理 URI 激活

**重要的 API**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

了解如何将应用注册为统一资源标识符 (URI) 方案名称的默认处理程序。 Windows 桌面应用和通用 Windows 平台 (UWP) 应用都可以注册为 URI 方案名称的默认处理程序。 如果用户将你的应用选作 URI 方案名称的默认处理程序，则每次启动该类型 URI 时都将激活你的应用。

如果希望处理该类型 URI 方案的所有 URI 启动，建议你仅针对 URI 方案名称进行注册。 如果选择了针对某个 URI 方案名称进行注册，则为该 URI 方案激活应用时必须向最终用户提供预期的功能。 例如，针对 mailto: URI 方案名称注册的应用应该能够打开新的电子邮件，以便用户编写新电子邮件。 有关 URI 关联的详细信息，请参阅[文件类型和 URI 指南和清单](https://msdn.microsoft.com/library/windows/apps/hh700321)。

这些步骤介绍了如何注册自定义 URI 方案名称 `alsdk://`，以及在用户启动 `alsdk://` URI 时如何激活你的应用。

> [!NOTE]
> 在 UWP 应用中，保留某些 URI 和文件扩展名用于内置应用和操作系统。 使用保留的 URI 或文件扩展名注册应用的尝试将被忽略。 参阅[保留 URI 方案名称和文件类型](reserved-uri-scheme-names.md)以获取无法注册为 UWP 应用的 URI 方案（因为它们为保留或禁止的文件类型）的字母顺序列表。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>步骤 1：指定程序包清单中的扩展点

应用仅接收程序包清单中列出的 URI 方案名称的激活事件。 下面是指示应用处理 `alsdk` URI 方案名称的方式。

1. 在**解决方案资源管理器**中，双击 package.appxmanifest 以打开清单设计器。 选择**声明**选项卡，并在**可用声明**下拉列表中选择**协议**，然后单击**添加**。

    以下是该协议的清单设计器中每个可以填写的字段的简短描述（有关详细信息，请参阅 [**AppX 程序包清单**](https://msdn.microsoft.com/library/windows/apps/dn934791)）：

| 字段 | 说明 |
|-------|-------------|
| **徽标** | 指定用于标识**控制面板**的[设置默认程序](https://msdn.microsoft.com/library/windows/desktop/cc144154)中 URI 方案名称的徽标。 如果未指定徽标，则使用应用的小徽标。 |
| **显示名称** | 指定用于标识**控制面板**的[设置默认程序](https://msdn.microsoft.com/library/windows/desktop/cc144154)中 URI 方案名称的显示名称。 |
| **名称** | 为 URI 方案选择名称。 |
|  | **注意**  “名称”必须全部为小写字母。 |
|  | **保留和禁止的文件类型** 请参阅[保留 URI 方案名称和文件类型](reserved-uri-scheme-names.md)以获取不能注册为 UWP 应用的 URI 方案（因为它们是保留或禁止的文件类型）的字母顺序列表。 |
| **可执行文件** | 为该协议指定默认的启动可执行文件。 如果未指定，将使用应用的可执行文件。 如果已指定，字符串长度必须是 1 到 256 个字符、必须以“.exe”结尾，并且不能包含以下字符：&gt;、&lt;、:、"、&#124;、? 或 \*。 如果已指定，还将使用**入口点**。 如果未指定**入口点**，将使用为应用定义的入口点。 |
| **入口点** | 指定用于处理协议扩展的任务。 这通常是 Windows 运行时类型的完全命名空间限定名称。 如果未指定，将使用应用的入口点。 |
| **“开始”页面** | 处理扩展点的网页。 |
| **资源组** | 可用于一起分组扩展激活以实现资源管理目的的标记。 |
| **所需视图**（仅适用于 Windows） | 指定**所需视图**字段以指示当为该 URI 方案名称启动应用的窗口时所需的空间量。 **所需视图**的可能值为 **Default**、**UseLess**、**UseHalf**、**UseMore** 或 **UseMinimum**。<br/>**注意**  Windows 在确定目标应用的最终窗口尺寸时会考虑多个不同因素，例如源应用的首选项、屏幕上的应用数量、屏幕方向等。 设置**所需视图**并不保证为目标应用设定具体的窗口化行为。<br/>**移动设备系列：所需视图**在移动设备系列上不受支持。 |

2. 输入 `images\Icon.png` 作为**徽标**。
3. 输入 `SDK Sample URI Scheme` 作为**显示名称**
4. 输入 `alsdk` 作为**名称**。
5. 按 Ctrl+S 保存对 package.appxmanifest 的更改。

    这会向程序包清单中添加一个类似于此的 [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) 元素。 **windows.protocol** 类别指示应用处理 `alsdk` URI 方案名称。

```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
```

## <a name="step-2-add-the-proper-icons"></a>步骤 2：添加适当的图标

成为 URI 方案名称默认应用的应用在整个系统的多个位置中显示其图标，例如在“默认程序”控制面板中。 为实现此目的，请在项目中包含一个 44x44 的图标。 匹配应用磁贴徽标的外观，并使用应用的背景色而不是使图标透明。 将徽标扩展到边缘而无需填充。 在白色背景上测试你的图标。 有关图标的更多详细信息，请参阅[磁贴和图标资源指南](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets)。

## <a name="step-3-handle-the-activated-event"></a>步骤 3：处理激活的事件

[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件处理程序接收所有激活事件。 **Kind** 属性指示激活事件的类型。 此示例设置为处理 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol) 激活事件。

```csharp
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
  {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```

```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
 End If
End Sub
```

```cppwinrt
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
    {
        auto protocolActivatedEventArgs{ args.as<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs>() };
        // TODO: Handle URI activation  
        auto receivedURI{ protocolActivatedEventArgs.Uri().RawUri() };
    }
}
```

```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   }
}
```

> [!NOTE]
> 通过协议合约启动后，请确保“后退”按钮可使用户返回到已启动应用的屏幕，而不是应用的早期内容。

下面代码以编程方式通过应用的 URI 启动应用：

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

有关如何通过 URI 启动应用的详细信息，请参阅[启动 URI 的默认应用](launch-default-app.md)。

建议应用为打开新页面的每个激活事件创建一个新的 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 通过此方式，新 XAML **Frame** 的导航 Backstack 将不包含应用暂停时可能在当前窗口中显示的所有早期内容。 确定针对启动和文件合约使用单个 XAML **Frame** 的应用在导航到新页面之前，应该清除 **Frame** 导航日志上的页面。

通过协议激活启动后，应用应该考虑包括允许用户返回到应用顶部页面的 UI。

## <a name="remarks"></a>备注

任何应用或网站（包括恶意应用或网站）都可以使用你的 URI 方案名称。 因此，在 URI 中获得的任何数据都可能来自不受信任的来源。 建议千万不要基于在 URI 中接收的参数执行永久性操作。 例如，可以使用 URI 参数将应用启动到用户的帐户页面，但建议永远不要将其用于直接修改用户的帐户。

> [!NOTE]
> 如果为应用创建新的 URI 方案名称，请确保遵循 [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550) 中的指南。 这样会确保你的名称符合 URI 方案的标准。

> [!NOTE]
> 通过协议合约启动后，请确保“后退”按钮可使用户返回到已启动应用的屏幕，而不是应用的早期内容。

我们建议应用为打开新 URI 目标的每个激活事件创建一个新的 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 通过此方式，新 XAML **Frame** 的导航 Backstack 将不包含应用暂停时可能在当前窗口中显示的所有早期内容。

如果你确定希望你的应用针对启动和协议合约使用单个 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，在导航到新页面之前，应该清除 **Frame** 导航日志上的页面。 通过协议合约启动后，请考虑在应用中包括允许用户返回到应用顶部的 UI。

## <a name="related-topics"></a>相关主题

### <a name="complete-sample-app"></a>完整的示例应用

- [关联启动示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>概念

- [默认程序](https://msdn.microsoft.com/library/windows/desktop/cc144154)
- [文件类型和 URI 关联模型](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>任务

- [启动 URI 的默认应用](launch-default-app.md)
- [处理文件激活](handle-file-activation.md)

### <a name="guidelines"></a>指南

- [文件类型和 URI 的指南](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>参考

- [AppX 程序包清单](https://msdn.microsoft.com/library/windows/apps/dn934791)
- [Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224742)
- [Windows.UI.Xaml.Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)
