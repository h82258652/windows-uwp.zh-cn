---
author: awkoren
Description: "除了适用于所有 UWP 应用的常规 API 之外，还有一些扩展和 API 仅适用于已转换的桌面应用。 本文将介绍这些扩展及其使用方法。"
Search.Product: eADQiWindows 10XVcnh
title: "已转换的桌面应用扩展"
translationtype: Human Translation
ms.sourcegitcommit: aa64c39c452beb2356186789a0d8bc44f79d82d2
ms.openlocfilehash: 0ad7e8d0fe63ffbfa8668be8955859258887d6f0

---

# 已转换的桌面应用扩展

可以使用各种通用 Windows 平台 (UWP) API 增强已转换的桌面应用程序。 但是，除了适用于所有 UWP 应用的常规 API 之外，还有一些扩展和 API 仅适用于已转换的桌面应用。 这些功能主要针对诸如在用户登录时启动进程和文件资源管理器集成之类的方案，旨在平滑处理原始桌面应用和已转换的应用包之间的转换。

本文将介绍这些扩展及其使用方法。 大多数扩展都要求手动修改已转换的应用的清单文件，其中包含有关你的应用将使用的扩展声明。 若要编辑清单，请右键单击 Visual Studio 解决方案中的 **Package.appxmanifest** 文件，然后选择“查看代码”**。 

## 启动任务

启动任务允许你的应用在用户登录时自动运行一个可执行文件。 

若要声明启动任务，请将以下内容添加到应用的清单中： 

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *扩展类别*的值应始终为“windows.startupTask”。
- *扩展可执行文件*是要启动的 .exe 的相对路径。
- *扩展入口点*的值应始终为“Windows.FullTrustApplication”。
- *StartupTask TaskId* 是任务的唯一标识符。 应用可以使用此标识符调用 **Windows.ApplicationModel.StartupTask** 类中的 API，以便以编程方式启用或禁用启动任务。
- *StartupTask Enabled* 指示是启用还是禁用任务的首次启动。 启用的任务将在用户下次登录时运行（除非用户禁用它）。 
- *StartupTask DisplayName* 是出现在任务管理器中的任务名称。 此字符串使用 ```ms-resource``` 进行本地化。 

应用可以声明多个启动任务；每一个任务都将独立引发和运行。 所有启动任务都将显示在任务管理器的“启动”****选项卡下，其名称在应用的清单和应用的图标中指定。 任务管理器将自动分析任务的启动影响。 用户可以选择使用任务管理器手动禁用你的应用的启动任务；如果用户禁用某个任务，则你无法以编程方式重新启用它。

## 应用执行别名

应用执行别名使你可以为你的应用指定关键字名称。 用户或其他进程可以使用该关键字轻松启动你的应用，如同该应用原本就在 PATH 变量中（例如从 Run 或命令提示符），无需提供完整路径。 例如，如果声明别名“Foo”，用户可以从 cmd.exe 键入“Foo Bar.txt”，将使用“Bar.txt”的路径作为激活事件参数的一部分来激活你的应用。

若要指定应用执行别名，请将以下内容添加到应用的清单： 

```XML 
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe">
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *扩展类别*的值应始终为“windows.appExecutionAlias”。
- *扩展可执行文件*是调用别名时要启动的可执行文件的相对路径。
- *扩展入口点*的值应始终为“Windows.FullTrustApplication”。
- *ExecutionAlias 别名*是你的应用的短名称。 它必须始终以“.exe”扩展名结尾。 

只可以为程序包中每个应用程序指定一个应用执行别名。 如果多个应用都注册了同一个别名，系统会调用最后注册的一个应用，因此请确保选择其他应用不太可能覆盖的独特别名。

## 协议关联 

协议关联支持已转换的应用和其他程序或系统组件之间的互操作方案。 使用某个协议启动已转换的应用时，你可以指定要传递到其激活事件参数的特定参数，以使该应用具备相应行为。 请注意，参数仅适用于完全信任的已转换应用；UWP 应用无法使用参数。  

若要声明协议关联，请将以下内容添加到应用的清单：

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *扩展类别*的值应始终为“windows.protocol”。 
- *协议名称*是协议的名称。 
- *协议参数*是应用激活时向其传递用作事件参数的参数和值的列表。 请注意，如果变量可以包含文件路径，则应该用引号将该值括起来，使其在传递的路径中包含空格时不至于中断。

## 文件和文件资源管理器集成

已转换的应用具有各种选项，用于注册以处理特定文件类型，以及集成到文件资源管理器中。 这使用户可以轻松将你的应用作为其常规工作流的一部分进行访问。

若要开始使用，请首先将以下内容添加到应用的清单： 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *扩展类别*的值应始终为“windows.fileTypeAssociation”。 
- *FileTypeAssociation 名称*是唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的 ProgID。 此 ID 可用于管理将来版本的应用中的更改。 例如，如果你想要更改某个文件扩展名的图标，可以将其移动到具有不同名称的新 FileTypeAssociation。  

接下来，根据所需的特定功能向该项添加其他子元素。 可用选项如下所述。

### 支持的文件类型

应用可以指定它是否支持已打开的特定类型的文件。 如果用户右键单击某个文件并选择“打开方式”，你的应用将出现在建议列表中。

示例：

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* 是你的应用支持的扩展名。

### 上下文菜单动词命令 

用户通常通过双击打开文件。 但是，当用户右键单击某个文件时，上下文菜单会向他们呈现各种选项（称为“动词命令”），用于提供有关他们想要如何与文件进行交互（如“打开”、“编辑”、“预览”或“打印”）的其他详细信息。 

指定支持的文件类型将自动添加“打开”动词命令。 但是，应用还可以将其他自定义动词命令添加到“文件资源管理器”上下文菜单。 这些动词命令使应用可以启动某种方式，具体取决于打开文件时用户的选择。

示例： 

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot">Print</uap:Verb>
</uap2:SupportedVerbs>
```

- *动词命令 ID* 是动词命令的唯一 ID. 如果你的应用为 UWP 应用，则会将该动词命令作为应用的激活事件参数的一部分向其传递，以便应用可以相应地处理用户的选择。 如果你的应用是完全信任的已转换应用，它将改为接收参数（请参阅下一项）。 
- *动词命令参数*是与动词命令关联的实参参数和值的列表。 如果你的应用是完全信任的已转换应用，当激活该应用时会将这些参数作为其事件参数向应用传递，以便你可以针对不同激活动词命令自定义应用的行为。 如果变量可以包含文件路径，则应该用引号将该值括起来，使其在传递的路径中包含空格时不至于中断。 请注意，如果你的应用为 UWP 应用，你将无法传递参数 - 它将改为接收 ID（请参阅上一项）。 
- *动词命令扩展*指定动词命令应仅在用户右键单击文件显示上下文菜单之前按住 **Shift** 键时才显示。 如果未列出该属性，则该属性是可选的，并且默认为 *False*（例如，始终显示动词命令）。 为每个动词命令逐个指定此行为（“打开”除外，它始终为 *False*）。 
- *动词命令*是要在“文件资源管理器”上下文菜单中显示的名称。 此字符串使用 ```ms-resource``` 进行本地化。

### 多选模式

多选使你可以指定应用如何处理用户使用该应用同时打开多个文件的情形（例如，在“文件资源管理器”中选择 10 个文件并点击“打开”）。

已转换的桌面应用具有与常规桌面应用相同的三个选项。 
- *播放机*：将使用所有作为实参参数传入的选定文件激活一次应用。
- *单一*：针对第一个选定的文件激活一次应用。 忽略其他文件。 
- *文档*：针对每个选定的文件激活应用的一个新的单独实例。

可以为不同的文件类型和操作设置不同的首选项。 例如，你可能想要以*文档*模式打开“文档”**，以*播放机*模式打开“图像”**。

若要设置应用的行为，请将 *MultiSelectModel* 属性添加到清单中与文件类型和文件启动相关的元素中。 

设置支持的文件类型的模型： 

```XML
<uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
```

设置动词命令的模型：

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap:Verb>
```

如果应用未针对多选情形指定选择，则默认为*播放机*（如果用户打开的文件不超过 15 个）。 此外，如果应用为已转换的应用，则默认为*文档*。 始终将 UWP 应用启动为*播放机*。 

### 完整示例

以下完整示例中集成了许多与上述元素相关的文件和文件资源管理器： 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        <uap:SupportedFileTypes>
            <uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot">Print</uap:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## 另请参阅

- [应用包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)


<!--HONumber=Jul16_HO1-->


