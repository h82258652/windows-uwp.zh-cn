---
Description: You can use extensions to integrate your packaged desktop app with Windows 10 in predefined ways.
Search.Product: eADQiWindows 10XVcnh
title: 将应用与 Windows 10 集成（桌面桥）
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.localizationpriority: medium
ms.openlocfilehash: 19ae09190b916fdaae68a67a2b9c11caa20d30e2
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7693418"
---
# <a name="integrate-your-packaged-desktop-application-with-windows-10"></a>与 Windows 10 集成已打包的桌面应用程序

使用扩展以预定义的方式与 Windows 10 集成已打包的桌面应用程序。

例如，使用扩展创建一个防火墙例外，使你的应用程序文件类型的默认应用程序或开始菜单磁贴指向打包后的应用版本。 若要使用扩展，只需将某些 XML 添加到应用的程序包清单文件。 不需要任何代码。

本主题介绍这些扩展以及使用它们可执行的任务。

## <a name="transition-users-to-your-app"></a>将用户切换到应用

帮助用户切换到打包后的应用。

* [将现有“开始”磁贴和任务栏按钮指向打包后的应用](#point)
* [使打包应用打开文件而不是你的桌面应用程序](#make)
* [将打包应用程序与一组文件类型相关联](#associate)
* [向具有特定文件类型的文件的上下文菜单添加选项](#add)
* [直接使用 URL 打开某些类型的文件](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>将现有“开始”磁贴和任务栏按钮指向打包后的应用

用户可能已将桌面应用程序固定到任务栏或“开始”菜单。 可将这些快捷方式指向打包后的新应用。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration)查找完整的架构参考。

|名称 | 描述 |
|-------|-------------|
|类别 |始终为 ``windows.desktopAppMigration``。
|AumID |打包后的应用的应用程序用户模型 ID。 |
|ShortcutPath |启动桌面版应用的 .lnk 文件的路径。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相关示例

[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>使打包应用打开文件而不是你的桌面应用程序

你可以确保用户默认情况下，针对特定类型的文件而不是打开你的应用的桌面版本打开新打包的应用程序。

为达到该目的，需指定每个想要从中继承文件关联的应用程序的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 可使用此 ID 管理将来版本的应用中的更改。 |
|MigrationProgId |[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) ，描述了应用程序、 组件和要从中继承文件关联的桌面应用程序的版本。|

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相关示例

[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>将打包应用程序与一组文件类型相关联

你可以与文件类型扩展名关联的打包应用程序。 如果用户右键单击某个文件，然后选择**打开方式**选项，你的应用程序会出现在建议列表中。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 可使用此 ID 管理将来版本的应用中的更改。   |
|FileType |应用支持的文件扩展名。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相关示例

[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>向具有特定文件类型的文件的上下文菜单添加选项

在大多数情况下，用户双击即可打开文件。 如果用户右键单击某个文件，将显示多种选项。

可向该菜单添加选项。 这些选项为用户提供与文件进行交互的其他方法，如打印、编辑或预览文件。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 | 始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|Verb |文件资源管理器上下文菜单中显示的名称。 此字符串可使用 ```ms-resource``` 进行本地化。|
|ID |动词命令的唯一 ID。 如果你的应用程序是 UWP 应用，则会传递到你的应用作为其激活事件参数的一部分中，以便它可以相应地处理用户的选择。 如果你的应用程序是完全信任的已打包的应用，该参数将改为接收 （请参阅下一项）。 |
|Parameters |与动词命令关联的实参参数和值的列表。 如果你的应用程序是完全信任的已打包的应用，这些参数传递给该应用程序作为事件参数激活应用程序时。 你可以自定义你的应用程序基于不同激活动词命令的行为。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 如果你的应用程序是 UWP 应用，则无法传递参数。 应用转而接收参数（请参阅上一项）。|
|Extended |指定动词命令仅在用户右键单击文件之前按住 **Shift** 键显示上下文菜单时才显示。 如果未列出该特性，则该特性可选，并且默认为值 **False**（例如，始终显示动词命令）。 为每个动词命令逐个指定此行为（“打开”除外，它始终为 **False**）。|

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相关示例

[使用转换/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>直接使用 URL 打开某些类型的文件

你可以确保用户默认情况下，针对特定类型的文件而不是打开你的应用的桌面版本打开新打包的应用程序。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|UseUrl |指示是否直接从 URL 目标打开文件。 如果你未设置此值，尝试在你的应用程序要使用的 URL 原因系统第一个将该文件下载到本地打开的文件。 |
|Parameters |可选参数。 |
|FileType |相关的文件扩展名。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>执行安装任务

* [为应用创建防火墙例外](#rules)
* [将你的 DLL 文件放到程序包的任意文件夹中](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>为应用创建防火墙例外

如果你的应用程序需要通过端口进行通信，你可以添加到列表的防火墙例外应用程序。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|Category |始终 ``windows.firewallRules``|
|Executable |想要添加到防火墙例外列表的可执行文件名称 |
|Direction |指示规则是传入规则还是传出规则 |
|IPProtocol |通信协议 |
|LocalPortMin |本地端口号范围内较小的端口号。 |
|LocalPortMax |本地端口号范围内最大的端口号。 |
|RemotePortMax |远程端口号范围内较小的端口号。 |
|RemotePortMax |远程端口号范围内最大的端口号。 |
|Profile |网络类型 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>将你的 DLL 文件放到程序包的任意文件夹中

使用扩展标识这些文件夹。 这样，系统就能找到并加载你放在这些文件夹中的文件。 可以将该扩展看作是 _%PATH%_ 环境变量的替代品。

如果你不使用该扩展，系统将按以下顺序进行搜索：进程的程序包依赖关系图、程序包根文件夹、系统目录 (_%SystemRoot%\system32_)。 若要了解详细信息，请参阅 [Windows 应用的搜索顺序](https://msdn.microsoft.com/library/windows/desktop/ms682586.aspx#_search_order_for_windows_store_apps)。

每个程序包只能包含一个这种类型的扩展。 也就是说，你可以将其中一个扩展添加到主程序包，然后向你的每个[可选包和相关的集](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)添加一个扩展。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

在应用清单的包级别声明该扩展。

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|名称 | 描述 |
|-------|-------------|
|类别 |始终为 ``windows.loaderSearchPathOverride``。
|FolderPath | 包含你的 dll 文件的文件夹路径。 指定相对于程序包根文件夹的路径。 你可以在一个扩展中最多指定五个路径。 如果你希望系统搜索程序包根文件夹中的文件，请为这些路径之一使用空字符串。 不要包含重复路径，并确保路径的开头和结尾不包含斜杠或反斜杠。 <br><br> 系统不搜索子文件夹，因此请务必明确列出包含你希望系统加载的 DLL 文件的每个文件夹。|

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>与文件资源管理器集成

帮助用户整理文件并以熟悉的方式与之交互。

* [定义用户选择并同时打开多个文件时，你的应用程序的行为方式](#define)
* [在文件资源管理器内以缩略图显示文件内容](#show)
* [在文件资源管理器的“预览”窗格中显示文件内容](#preview)
* [使用户能够使用文件资源管理器中的“类型”列对文件进行分组](#enable)
* [使文件属性可用于搜索、索引、属性对话框和详细信息窗格](#make-file-properties)
* [使你的云服务中的文件显示在文件资源管理器中](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>定义用户选择并同时打开多个文件时，你的应用程序的行为方式

指定用户同时打开多个文件时，你的应用程序的行为方式。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|MultiSelectModel |请参阅下文 |
|FileType |相关的文件扩展名。 |

**MultSelectModel**

已打包的桌面应用具有与常规桌面应用相同的三个选项。

* ``Player``： 你的应用程序已激活一次。 所有选定的文件传递给你的应用程序作为实参参数。
* ``Single``： 你的应用程序已激活一次为第一个选定的文件。 忽略其他文件。
* ``Document``： 针对每个选定的文件激活一个新的单独实例，你的应用程序。

 可以为不同的文件类型和操作设置不同的首选项。 例如，可能会希望以“文档”** 模式打开*文档*，以“播放机”** 模式打开*图像*。

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

如果用户打开 15 个或更少的文件，**MultiSelectModel** 特性的默认选项为*播放机*。 否则，默认选项“文档”**。 始终将 UWP 应用启动为*播放机*。

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>在文件资源管理器内以缩略图显示文件内容

使用户可在文件图标以中、大或超大尺寸显示时查看文件内容的缩略图。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|Clsid   |应用的类 ID。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>在文件资源管理器的“预览”窗格中显示文件内容

使用户可以在文件资源管理器的“预览”窗格中预览文件的内容。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|Clsid   |应用的类 ID。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>使用户能够使用文件资源管理器中的“类型”列对文件进行分组

可以将文件类型的一个或多个预定义值与**类型**字段相关联。

在文件资源管理器中，用户可以使用该字段对这些文件进行分组。 系统组件也会将此字段用于不同的用途，例如建立索引。

若要详细了解**类型**字段以及可对此字段使用的值，请参阅[使用类型名称](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|value |有效的[类型值](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144136.aspx#kind_hierarchy) |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>使文件属性可用于搜索、索引、属性对话和详细信息窗格

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|Clsid  |应用的类 ID。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>使你的云服务中的文件显示在文件资源管理器中

注册你在应用程序中实现的处理程序。 你还可以添加上下文菜单选项 - 当用户在文件资源管理器中右键单击你的基于云的文件时，将显示这些菜单选项。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.cloudfiles``。
|iconResource |代表你的云文件提供商服务的图标。 该图标显示在文件资源管理器的导航窗格中。  用户选择该图标可显示云服务中的文件。 |
|CustomStateHandler Clsid |实现 CustomStateHandler 的应用程序的类 ID。 系统使用该类 ID 请求云文件的自定义状态和列。 |
|ThumbnailProviderHandler Clsid |实现 ThumbnailProviderHandler 的应用程序的类 ID。 系统使用该类 ID 请求云文件的缩略图图像。 |
|ExtendedPropertyHandler Clsid |实现 ExtendedPropertyHandler 的应用程序的类 ID。  系统使用该类 ID 请求云文件的扩展属性。 |
|谓词 |你的云服务提供的文件在文件资源管理器上下文菜单中显示的名称。 |
|ID |动词命令的唯一 ID。 |

#### <a name="example"></a>示例

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>以不同方式启动你的应用程序

* [通过使用某个协议启动你的应用程序](#protocol)
* [通过使用别名启动应用程序](#alias)
* [用户登录 Windows 时启动可执行文件](#executable)
* [使用户能够到其电脑连接设备时启动你的应用程序](#autoplay)
* [在接收来自 Microsoft Store 的更新后自动重启](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>通过使用某个协议启动你的应用程序

协议关联允许其他程序和系统组件与打包后的应用进行互操作。 当使用某个协议启动打包应用程序时，你可以指定要传递到其激活事件参数，以便它可以执行相应的操作的特定参数。 仅打包后完全受信任的应用支持参数。 UWP 应用不能使用参数。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.protocol``。
|Name |协议的名称。 |
|Parameters |参数和值，以激活应用程序时传递到应用程序作为事件参数的列表。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 |

### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>通过使用别名启动应用程序

用户和其他进程可以使用别名启动你的应用程序无需指定你的应用的完整路径。 可以指定该别名。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|Name |描述 |
|-------|-------------|
|类别 |始终为 ``windows.appExecutionAlias``。
|Executable |调用别名时要启动的可执行文件的相对路径。 |
|Alias |应用的简称。 它必须始终以“.exe”扩展名结尾。 只可以为程序包中每个应用程序指定一个应用执行别名。 如果多个应用都注册了同一个别名，系统会调用最后注册的一个应用，因此请确保选择其他应用不太可能覆盖的独特别名。
|

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  ...
  <uap3:Extension
        Category="windows.appExecutionAlias"
        Executable="exes\launcher.exe"
        EntryPoint="Windows.FullTrustApplication">
      <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Contoso.exe" />
      </uap3:AppExecutionAlias>
  </uap3:Extension>
...
</Package>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>用户登录 Windows 时启动可执行文件

启动任务允许你的应用程序在用户登录时自动运行可执行文件。

> [!NOTE]
> 用户必须启动你的应用程序至少一次才可注册此启动任务。

你的应用程序可以声明多个启动任务。 每个任务独立启动。 所有启动任务都将显示在任务管理器的**启动**选项卡下，其名称在应用的清单和应用的图标中指定。 任务管理器将自动分析任务的启动影响。

用户可以使用任务管理器手动禁用应用的启动任务。 如果用户禁用任务，则无法以编程方式重新启用它。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.startupTask``。|
|Executable |要启动的可执行文件的相对路径。 |
|TaskId |任务的唯一标识符。 使用此标识符，你的应用程序可以在[Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask)类以编程方式启用或禁用启动任务中调用 Api。 |
|Enabled |指示是启用还是禁用任务的首次启动。 启用的任务将在用户下次登录时运行（除非用户禁用它）。 |
|DisplayName |任务管理器中显示的任务名称。 可以使用 ```ms-resource``` 本地化此字符串。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>使用户能够到其电脑连接设备时启动你的应用程序

当用户将设备连接到其电脑时，自动播放可以显示你的应用程序作为一个选项。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.autoPlayHandler``。
|ActionDisplayName |表示用户可以对其连接到电脑的设备执行的操作的字符串（例如：“导入文件”或“播放视频”）。 |
|ProviderDisplayName | 一个字符串，表示你的应用程序或服务 (例如:"Contoso 视频播放器")。 |
|ContentEvent |导致向用户提示你的 ``ActionDisplayName`` 和 ``ProviderDisplayName`` 的内容事件的名称。 当卷设备（如相机内存卡、U 盘或 DVD）插入到电脑时，会引发内容事件。 你可以在[此处](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)找到此类事件的完整列表。  |
|谓词 |谓词设置标识传递给你的应用程序针对所选选项的值。 你可以为自动播放事件指定多个启动操作并且可以使用谓词设置确定用户为你的应用选择的选项。 你可以通过检查传递给应用的启动事件参数的 verb 属性来标识用户选择的选项。 你可以为谓词设置使用任何值（但保留的 open 除外）。 |
|DropTargetHandler |应用程序实现[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)接口的类 ID。 系统会将可移动媒体中的文件传递给你的 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 实现的 [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) 方法。  |
|参数 |你不必为所有内容事件实现 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 接口。 对于任意内容事件，你可以提供命令行参数，而不是实现 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 接口。 对于此类事件，自动播放将使用这些命令行参数启动你的应用程序。 你可以在应用的初始化代码中解析这些参数，确定应用是否由自动播放启动，然后提供自定义实现。 |
|DeviceEvent |导致向用户提示你的 ``ActionDisplayName`` 和 ``ProviderDisplayName`` 的设备事件的名称。 当有设备连接到电脑时，将引发设备事件。 设备事件以字符串 ``WPD`` 开头，你可以在[此处](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)找到设备事件列表。 |
|HWEventHandler |应用程序实现[IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx)接口的类 ID。 |
|InitCmdLine |你要传递给 [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx) 接口的 [Initialize](https://msdn.microsoft.com/en-us/library/windows/desktop/bb775495.aspx) 方法的字符串参数。 |

### <a name="example"></a>示例

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>在接收来自 Microsoft Store 的更新后自动重启

如果用户安装应用更新到它时，你的应用程序已打开，应用程序关闭。

如果你希望该应用程序来重启在更新完成之后，你想要重新启动每个进程中调用[RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx)函数。

你的应用程序中的每个活动窗口接收[WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx)的消息。 到目前为止，你的应用程序可以调用[RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx)函数再次以更新命令行，如有必要。

当你的应用程序中的每个活动窗口收到[WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx)消息时，你的应用程序应保存数据，并关闭。

>[!NOTE]
在应用程序不会处理[WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx)消息的情况下，你的活动窗口还会收到[WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx)消息。

到目前为止，你的应用程序有 30 秒时间来关闭自己的进程或平台强制终止这些进程。

更新完成后，重新启动你的应用程序。

## <a name="work-with-other-applications"></a>与其他应用程序配合使用

与其他应用集成，启动其他进程或共享信息。

* [使应用程序中显示为打印目标应用程序支持打印](#printing)
* [与其他 Windows 应用程序共享字体](#fonts)
* [从通用 Windows 平台 (UWP) 应用启动 Win32 进程](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>使应用程序中显示为打印目标应用程序支持打印

当用户想要从另一个应用程序 （如记事本） 打印数据时，你可以使应用程序中显示为打印目标应用的可用打印目标列表。

你将需要修改你的应用程序，以便在其接收 XML 纸张规范 (XPS) 格式的打印数据。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.appPrinter``。
|DisplayName |希望在应用的打印目标列表中显示的名称。 |
|Parameters |你的应用程序正确处理请求所需的任何参数。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

在[此处](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)查找使用此扩展的示例

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>与其他 Windows 应用程序共享字体

与其他 Windows 应用程序共享自定义字体。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

在[此处](https://review.docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.sharedFonts``。
|File |包含想要共享的字体的文件。 |

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>从通用 Windows 平台 (UWP) 应用启动 Win32 进程

启动以完全信任方式运行的 Win32 进程。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|名称 |描述 |
|-------|-------------|
|类别 |始终为 ``windows.fullTrustProcess``。
|GroupID |标识想要传递给可执行文件的参数集的字符串。 |
|Parameters |想要传递给可执行文件的参数。 |

#### <a name="example"></a>示例

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

此扩展可能会很有用，如果你想要创建在所有设备上运行的通用 Windows 平台用户界面，但希望 Win32 应用程序继续以完全信任方式运行的组件。

只需创建适用于 Win32 应用的 Windows 应用包。 然后，将此扩展添加到 UWP 应用的程序包文件。 此扩展指示想要在 Windows 应用包中启动可执行文件。  如果想要在 UWP 应用和 Win32 应用之间进行通信，可以设置一个或多个[应用服务](../launch-resume/app-services.md)来执行此操作。 可以在[此处](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)阅读关于此方案的详细信息。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
