---
author: mcleblanc
description: "在开始移植过程时，你有两个选择。"
title: "将 Windows 运行时 8.x 项目移植到 UWP 项目&quot;"
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d711d981a674d1516b12ee11c379e679c45dcb60
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>将 Windows 运行时 8.x 项目移植到 UWP 项目

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


在开始移植过程时，你有两个选择。 一是编辑现有项目文件的副本，包括应用包清单（对于该选项，请参阅[将应用迁移到通用 Windows 平台应用 (UWP)](https://msdn.microsoft.com/library/mt148501.aspx) 中有关更新项目文件的信息）。 另一个是在 Visual Studio 中创建一个新的 Windows 10 项目，并将你的文件复制到其中。 本主题的第一部分描述了第二个选择，但本主题的其余部分提供了同时适用于这两个选择的其他信息。 你还可以选择将新的 Windows 10 项目保留在与你的现有项目相同的解决方案中，并使用已共享的项目共享源代码文件。 或者，你也可以将新项目保留在它自己的解决方案中，并使用 Visual Studio 中链接的文件功能共享源代码文件。

## <a name="create-the-project-and-copy-files-to-it"></a>创建项目并向其复制文件

这些步骤侧重于以下内容：在 Visual Studio 中创建一个新的 Windows 10 项目，并将你的文件复制到其中。 某些围绕着所创建的项目数和要复制的文件展开的特定步骤，将依赖于[如果你有一个通用 8.1 应用](w8x-to-uwp-root.md)及其后的部分中所述的规格和决策。 这些步骤中假设了最简单的应用场景。

1.  启动 Microsoft Visual Studio 2015 并创建新的空白应用程序（Windows 通用）项目。 有关详细信息，请参阅[使用模板（C#、C++、Visual Basic）快速启动 Windows 应用商店应用](https://msdn.microsoft.com/library/windows/apps/hh768232)。 新建项目会构建一个可在所有设备系统上运行的应用包（appx 文件）。
2.  在 Universal 8.1 App 项目中，标识要重用的所有源代码文件和视觉资产文件。 通过使用文件资源管理器，将数据模型、视图模型、视觉资产、资源词典，文件夹结构和想要重用的任何其他内容复制到新项目中。 根据需要在磁盘上复制或创建子文件夹。
3.  还可以将视图（例如，MainPage.xaml 和 MainPage.xaml.cs）复制到新项目中。 同样，也可根据需要创建新的子文件夹，并从项目中删除现有视图。 但在覆盖或删除 Visual Studio 生成的视图之前，请保留一份副本，因为在以后引用它时，这可能会很有用。 移植通用 8.1 应用的第一个阶段侧重于使其在某一设备系列上正常显示并良好运行。 之后，将侧重点转到确保视图能自行适应所有外观规格，也可以选择添加任何自适应代码以最大程度地利用特定的设备系列。
4.  在**解决方案资源管理器**中，请确保将**显示所有文件**切换为打开。 选择要复制的文件，右键单击这些文件，然后单击**包括在项目中**。 这将自动包括其所包含的文件夹。 然后，可根据需要将**“显示所有文件”**切换为关闭。 备用工作流（如果选择）旨在使用**“添加现有项”**命令，以便在 Visual Studio**“解决方案资源管理器”**中创建任何必要子文件夹。 仔细检查可见资源是否已将**生成操作**设置为**内容**，并将**复制到输出目录**设置为**不复制**。
5.  你很可能会在此阶段看到一些生成错误。 不过，如果你知道需要更改哪些内容，则可以使用 Visual Studio 的 **Find and Replace** 命令批量更改源代码；并且可以在 Visual Studio 的强制性代码编辑器中，使用上下文菜单上的 **Resolve** 和 **Organize Usings** 命令了解更多目标更改。

## <a name="maximizing-markup-and-code-reuse"></a>最大程度地重新使用标记和代码

你会发现，只需稍作重构和/或添加自适应代码（将在下文进行介绍），即可最大程度地重新使用可跨所有设备系列运行的标记和代码。 下面提供了更多详细信息。

-   无需特别考虑在所有设备系列上均通用的文件。 这些文件将由可在所有设备系列上运行的应用使用。 这包括 XAML 标记文件、强制性源代码文件和资产文件。
-   你的应用既能检测到正在运行它的设备系列，又能导航到专门为该设备系列设计的视图。 有关详细信息，请参阅[检测应用所运行的平台](w8x-to-uwp-input-and-sensors.md)。
-   在以下情况中，你可能会发现一种类似的技术也许会很有用：必须要为某个标记文件或 **ResourceDictionary** 文件（或者包含该文件的文件夹）提供一个特殊名称，以便它仅当应用在特定设备系列上运行时才自动在运行时加载。 此技术将在 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) 案例研究中进行介绍。
-   如果你仅需要支持 Windows 10，你应该能够删除通用 8.1 应用源代码中的大量条件编译指令。 请参阅本主题中的[条件编译和自适应代码](#conditional-compilation-and-adaptive-code)。
-   若要使用未在所有设备系列上提供的功能（例如打印机、扫描仪或相机按钮），你可以编写自适应代码。 请参阅本主题的[条件编译和自适应代码](#conditional-compilation-and-adaptive-code)中的第三个示例。
-   如果你想要同时支持 Windows 8.1、Windows Phone 8.1 和 Windows 10，你可以将三个项目都保留在同一个解决方案中并与共享的项目共享代码。 此外，你可以在项目之间共享源代码文件。 以下是操作方法：在 Visual Studio 中，在**解决方案资源管理器**中右键单击项目、选择**添加现有项**、选择要共享的文件，然后单击**添加为链接**。 将源代码文件存储在文件系统上的常用文件夹中，可在指向它们的链接所在的项目中看到这些文件。 不要忘记将它们添加到源控件中。
-   有关在二进制级别（而不是源代码级别）上重复使用的信息，请参阅[使用 C# 和 Visual Basic 创建 Windows 运行时组件](http://msdn.microsoft.com/library/windows/apps/xaml/br230301.aspx)。 还存在一些可移植类库，这些库支持在适用于 Windows 8.1、Windows Phone 8.1 和 Windows 10 应用 (.NET Core) 的 .NET Framework 以及整个 .NET Framework 中可用的 .NET API 子集。 可移植类库程序集为二进制形式，可与所有这些平台兼容。 使用 Visual Studio 创建一个面向可移植类库的项目。 请参阅[使用可移植类库的跨平台开发](http://msdn.microsoft.com/library/gg597391.aspx)。

## <a name="extension-sdks"></a>扩展 SDK

大多数已由通用 8.1 应用调用的 Windows 运行时 API 都在所谓的通用设备系列的 API 集中实现。 但是，部分 API 在扩展 SDK 中实现，并且 Visual Studio 只能识别由应用的目标设备系列或所引用的任何扩展 SDK 实现的 API。

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

另请参阅[应用包清单](#app-package-manifest)。

## <a name="conditional-compilation-and-adaptive-code"></a>条件编译和自适应代码

如果你为了让代码文件能同时在 Windows 8.1 和 Windows Phone 8.1 上运行而（借助 C# 预处理器指令）使用条件编译，你现在可以根据 Windows 10 中已完成的融合工作查看该条件编译。 融合意味着在 Windows 10 应用中，某些条件可能已被彻底删除。 而其他条件将更改为运行时检查，如下面的示例所示。

**注意**   如果你想要使用单个代码文件支持 Windows 8.1、Windows Phone 8.1 和 Windows 10，也可以执行此操作。 如果你在项目属性页中查找 Windows 10 项目，你将看到该项目将 WINDOWS\_UAP 定义为条件编译符号。 这样，你便可以将其与 WINDOWS\_APP 和 WINDOWS\_PHONE\_APP 结合使用。 这些示例将演示有关从通用 8.1 应用删除条件编译并用等效代码针对 Windows 10 应用进行替换的更为简单的案例。

第一个示例将演示 **PickSingleFileAsync** API（仅适用于 Windows 8.1）和 **PickSingleFileAndContinue** API（仅适用于 Windows Phone 8.1）的使用模式。

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

由于 Windows 10 在 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) API 上融合，因此可对你的代码作如下简化：

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

在此示例中，我们将处理硬件后退按钮，但仅在 Windows Phone 上才进行处理。

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

在 Windows 10 中，后退按钮事件是一个通用概念。 已在硬件或软件中实现的后退按钮都将引发 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件（即，为要处理的事件）。

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

最后一个示例与上一个示例类似。 下面我们将处理硬件相机按钮，但同样仅在已编译到 Windows Phone 应用包的代码中才进行处理。

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

在 Windows 10 中，硬件相机按钮是一个特定于移动设备系列的概念。 因为某一应用包将在所有设备上运行，所以我们将使用所谓的自适应代码，将编译时条件更改为运行时条件。 为此，我们将使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 类在运行时查询是否存在 [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 类。 由于 **HardwareButtons** 在移动扩展 SDK 中进行了定义，因此我们需要针对要编译的代码将对该 SDK 的引用添加到我们的项目。 但需要注意的是，处理程序仅在实现移动扩展 SDK 中定义的类型且属于移动设备系列的设备上执行。 因此，此代码在特意只使用存在的功能方面，原则上与通用 8.1 代码等效，尽管它采用了不同的实现方式。

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

另请参阅[检测正运行你的应用的平台](w8x-to-uwp-input-and-sensors.md)。

## <a name="app-package-manifest"></a>应用包清单

[Windows 10 中的更改内容](https://msdn.microsoft.com/library/windows/apps/dn705793)主题针对 Windows 10 列出了对程序包清单架构参考所做的更改，其中包括已添加、删除和更改的元素。 有关该架构中所有元素、属性和类型的参考信息，请参阅[元素层次结构](https://msdn.microsoft.com/library/windows/apps/dn934819)。 如果你要移植 Windows Phone 应用商店应用，或者你的应用是对 Windows Phone 应用商店应用的更新，请确保 **pm:PhoneIdentity** 元素与上一个应用的应用清单中的相应项匹配（使用由应用商店分配给应用的相同 GUID）。 这将确保要升级到 Windows 10 的应用用户将接收您的新应用作为更新而不是重复项。 请参阅 [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) 参考主题以了解更多详细信息。

你的项目中的设置（包括任何扩展 SDK 引用）决定了你的应用可以调用的 API 图面区域。 但是，你的应用包清单决定了你的客户可以从应用商店将你的应用安装在哪些实际的设备集上。 有关详细信息，请参阅 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 中的相关示例。

你可以编辑应用包清单以设置各种声明、功能和某些功能所需的其他设置。 你可以使用 Visual Studio 应用包清单编辑器来编辑它。 如果未显示**解决方案资源管理器**，请从**视图**菜单中选择它。 双击 **Package.appxmanifest**。 此时会打开“清单编辑器”窗口。 选择要更改的相应选项卡，然后进行保存。

下一主题为[疑难解答](w8x-to-uwp-troubleshooting.md)。

## <a name="related-topics"></a>相关主题

* [开发通用 Windows 平台应用](http://msdn.microsoft.com/library/dn975273.aspx)
* [使用模板（C#、C++ 和 Visual Basic）快速启动你的 Windows 应用商店应用](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [使用可移植类库的跨平台开发](http://msdn.microsoft.com/library/gg597391.aspx)

