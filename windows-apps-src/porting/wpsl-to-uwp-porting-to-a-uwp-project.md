---
description: 通过在 Visual Studio 中创建新的 windows 10 项目并将你的文件复制到其中开始移植过程。
title: WindowsPhone Silverlight 将项目移植到 UWP 项目
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55c4347b85d94d183d44599f7d34bc750d34d181
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927897"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>WindowsPhone Silverlight 将项目移植到 UWP 项目


上一主题是[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。

通过在 Visual Studio 中创建新的 windows 10 项目并将你的文件复制到其中开始移植过程。

## <a name="create-the-project-and-copy-files-to-it"></a>创建项目并将文件复制到其中

1.  启动 Microsoft Visual Studio2015 并创建一个新的空白应用程序 （Windows 通用） 项目。 有关详细信息，请参阅[快速启动你的 Windows 运行时 8.x 应用使用模板 （C#、 c + +、 Visual Basic）](https://msdn.microsoft.com/library/windows/apps/hh768232)。 你的新项目会生成一个将在所有设备系列上运行的应用包（appx 文件）。
2.  在 WindowsPhone Silverlight 应用项目中，标识所有源代码文件和视觉资产文件，你想要重复使用。 通过使用文件资源管理器，将数据模型、视图模型、视觉资源、资源词典，文件夹结构和想要重复使用的任何其他内容复制到新项目中。 根据需要在磁盘上复制或创建子文件夹。
3.  还可以将视图（例如，MainPage.xaml 和 MainPage.xaml.cs）复制到新项目节点中。 同样，也可根据需要创建新的子文件夹，并从项目中删除现有视图。 但是，在覆盖或删除 Visual Studio 生成的视图之前，请保留一份副本，因为在以后引用它时，这可能会很有用。 将 WindowsPhone Silverlight 应用移植的第一个阶段侧重于一设备系列上正常工作并让其。 之后，将侧重点转到确保视图能自行适应所有外形规格，也可以选择添加任何自适应代码以最大程度地利用特定的设备系列。
4.  在**解决方案资源管理器**中，请确保将**显示所有文件**切换为打开。 选择要复制的文件，右键单击这些文件，然后单击**包括在项目中**。 这将自动包括其所包含的文件夹。 然后，可根据需要将 **“显示所有文件”** 切换为关闭。 备用工作流（如果选择）旨在使用 **“添加现有项”** 命令，以便在 Visual Studio **“解决方案资源管理器”** 中创建任何必要子文件夹。 仔细检查可见资源是否已将**生成操作**设置为**内容**，并将**复制到输出目录**设置为**不复制**。
5.  在此阶段中，命名空间和类名称之间的差异将生成大量生成错误。 例如，在打开 Visual Studio 生成的视图时，你将会看到它们的类型是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)，而不是 **PhoneApplicationPage**。 XAML 标记和命令性代码之间的许多差异将在本移植指南的以下主题中详细介绍。 只需按照以下常规步骤操作，即可快速取得进展：在 XAML 标记的命名空间前缀声明中将“clr-namespace”更改为“using”；使用[命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)主题和 Visual Studio 的 **Find and Replace** 命令以批量更改到源代码（例如，将“System.Windows”替换为“Windows.UI.Xaml”）；并通过 Visual Studio 中的强制性代码编辑器使用上下文菜单上的 **Resolve** 和 **Organize Usings** 命令，以进行更具针对性的更改。

## <a name="extension-sdks"></a>扩展 SDK

已移植应用要调用的大多数通用 Windows 平台 (UWP) API 将在 API 集（称为通用设备系列）中实现。 但是，部分 API 在扩展 SDK 中实现，因为 Visual Studio 只能识别由你的应用的目标设备系列或所引用的任何扩展 SDK 实现的 API。

如果你收到有关找不到命名空间、类型或成员的编译错误，这很可能是导致此类错误出现的原因。 打开 API 参考文档中的 API 主题并导航到“要求”部分：你可以从中了解到设备系列实现的内容。 如果这不是你的目标设备系列，但需要使相应 API 适用于你的项目，你将需要一个对该设备系列的扩展 SDK 的引用。

依次单击**项目** &gt; **添加引用** &gt; **Windows 通用** &gt; **扩展**，然后选择相应的扩展 SDK。 例如，如果要调用的 API 仅在移动设备系列中可用，且它们已在版本 10.0.x.y 中引入，请选择**适用于 UWP 的 Windows 移动版扩展**。

这将向你的项目文件添加以下引用：

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

名称和版本号需与 SDK 的安装位置所在的文件夹匹配。 例如，上述信息需与以下文件夹名称匹配：

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

除非你的应用面向可实现此 API 的设备系列，否则你将需要先使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 类测试该 API 是否存在，然后才能调用它（这称为自适应代码）。 然后，评估此条件（无论你的应用在何处运行），但其仅针对存在相应 API 的设备才评估为 True，从而可调用该 API。 仅在首次检查是否存在通用 API 后，才能使用扩展 SDK 和自适应代码。 下面的部分中提供了一些示例。

另请参阅[应用包清单](#the-app-package-manifest)。

## <a name="maximizing-markup-and-code-reuse"></a>最大程度地重新使用标记和代码

你会发现，只需稍作重构和/或添加自适应代码（将在下文进行介绍），即可最大程度地重新使用可跨所有设备系列运行的标记和代码。 下面提供了更多详细信息。

-   无需特别考虑在所有设备系列上均通用的文件。 这些文件将由可在所有设备系列上运行的应用使用。 这包括 XAML 标记文件、强制性源代码文件和资产文件。
-   你的应用既能检测到正在运行它的设备系列，又能导航到专门为该设备系列设计的视图。 有关详细信息，请参阅[检测应用所运行的平台](wpsl-to-uwp-input-and-sensors.md)。
-   在以下情况下，你可能会发现一种类似的技术也许会很有用：必须要为某个标记文件或 **ResourceDictionary** 文件（或者包含该文件的文件夹）提供一个特殊名称，以便它仅当应用在特定设备系列上运行时才自动在运行时加载。 此技术将在 [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) 案例研究中进行介绍。
-   若要使用未在所有设备系列上提供的功能（例如打印机、扫描仪或相机按钮），你可以编写自适应代码。 请参阅本主题下[条件编译和自适应代码](#conditional-compilation-and-adaptive-code)中的第三个示例。
-   如果你想要支持 WindowsPhone Silverlight 和 windows 10，则你可能能够共享项目之间的源代码文件。 以下是操作方法：在 Visual Studio 中，在**解决方案资源管理器**中右键单击项目、选择**添加现有项**、选择要共享的文件，然后单击**添加为链接**。 将源代码文件存储在文件系统上的常用文件夹中，可在指向它们的链接所在的项目中看到这些文件，不要忘记将它们添加到源控件。 如果你可以在文件中构建强制性源代码且其中的大多数代码（即使不是全部）同时适用于这两个平台，则无需具有该文件的两个副本。 尽可能将文件中任何特定于平台的逻辑打包到条件编译指令中，或者将其打包到运行时条件中（如果需要）。 请参阅下面的下一个部分和 [C# 预处理器指令](http://msdn.microsoft.com/library/ed8yd1ha.aspx)。
-   对于在二进制级别，而不是源代码级别上重复使用，存在一些可移植类库，这些库支持适用于 windows 10 应用 (.NET Core) 的 WindowsPhone Silverlight 中可用的.NET Api 子集。 可移植类库程序集为二进制形式，可与这些 .NET 平台及其他平台兼容。 使用 Visual Studio 创建一个面向可移植类库的项目。 请参阅[使用可移植类库的跨平台开发](http://msdn.microsoft.com/library/gg597391.aspx)。

## <a name="conditional-compilation-and-adaptive-code"></a>条件编译和自适应代码

如果你想要在单个代码文件中支持 WindowsPhone Silverlight 和 windows 10，你可以执行此操作。 如果你查看 windows 10 项目在项目属性页中，你将看到该项目将 WINDOWS\_UAP 定义为条件编译符号。 一般情况下，你可以使用以下逻辑执行条件编译。

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

如果你有一个 WindowsPhone Silverlight 应用和 Windows 运行时 8.x 应用之间共享的代码，则可能已经具有包含如下逻辑的源代码：

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

如果是这样，且现在想要支持除此之外的 windows 10，然后你可以这样做，太。

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

你可能已使用条件编译来限制 Windows Phone 的硬件后退按钮的处理。 在 windows 10，后退按钮事件是一个通用概念。 已在硬件或软件中实现的后退按钮都将引发 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件（即，为要处理的事件）。

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

你可能已使用条件编译来限制 Windows Phone 的硬件相机按钮的处理。 在 windows 10，硬件相机按钮是一个特定于移动设备系列的概念。 因为某一应用包将在所有设备上运行，所以我们将使用所谓的自适应代码，将编译时条件更改为运行时条件。 为此，我们将使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 类在运行时查询是否存在 [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 类。 由于 **HardwareButtons** 在移动扩展 SDK 中进行了定义，因此我们需要针对要编译的代码将对该 SDK 的引用添加到我们的项目。 但需要注意的是，处理程序仅在实现移动扩展 SDK 中定义的类型且属于移动设备系列的设备上执行。 因此，下面的代码应注意仅使用存在的功能，尽管它采用了不同于条件编译的方式来实现该程序。

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

另请参阅[检测正运行你的应用的平台](wpsl-to-uwp-input-and-sensors.md)。

## <a name="the-app-package-manifest"></a>应用包清单

你的项目中的设置（包括任何扩展 SDK 引用）决定了你的应用可以调用的 API 图面区域。 但是，你的应用包清单决定了你的客户可以从应用商店将你的应用安装在哪些实际的设备集上。 有关详细信息，请参阅 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 中的相关示例。

值得了解的是，如何编辑应用包清单，因为后面的主题将讨论如何针对各种声明、功能和某些功能所需的其他设置使用它。 你可以使用 Visual Studio 应用包清单编辑器来编辑它。 如果未显示**解决方案资源管理器**，请从**视图**菜单中选择它。 双击 **Package.appxmanifest**。 此时会打开“清单编辑器”窗口。 选择相应的选项卡来进行更改，然后保存更改。 你可能要确保已移植的应用清单中的 **pm:PhoneIdentity** 元素与要移植的应用的应用清单中的元素匹配（有关完整详细信息，请参阅 [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) 主题）。

请参阅[程序包清单架构参考的 windows 10](https://msdn.microsoft.com/library/windows/apps/dn934820)。

下一主题为[疑难解答](wpsl-to-uwp-troubleshooting.md)。

