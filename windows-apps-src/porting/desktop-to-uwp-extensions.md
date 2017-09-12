---
author: normesta
Description: "可以使用扩展以预定义方式将打包的桌面应用与 Windows 10 集成。"
Search.Product: eADQiWindows 10XVcnh
title: "将应用与 Windows 10 集成（桌面桥）"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: 0c3427a7b49b17fda9a3ba0680e59b134732e9fa
ms.sourcegitcommit: 38ef208ef457ce1857038c9cde3658c884d29b75
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="integrate-your-app-with-windows-10-desktop-bridge"></a>将应用与 Windows 10 集成（桌面桥）

使用扩展以预定义方式将应用与 Windows 10 集成。

例如，使用扩展创建一个防火墙例外，使应用成为某一文件类型的默认应用，或将“开始”磁贴指向打包后的应用版本。 若要使用扩展，只需将某些 XML 添加到应用的程序包清单文件。 不需要任何代码。

本主题介绍这些扩展以及使用它们可执行的任务。

## <a name="transition-users-to-your-app"></a>将用户切换到应用

帮助用户切换到打包后的应用。

* [将现有“开始”磁贴和任务栏按钮指向打包后的应用](#point)
* [使打包后的应用（而非桌面应用）打开文件](#make)
* [将打包的应用与一组文件类型相关联](#associate)
* [向具有特定文件类型的文件的上下文菜单添加选项](#add)
* [直接使用 URL 打开某些类型的文件](#open)

<span id="point" />
### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>将现有“开始”磁贴和任务栏按钮指向打包后的应用

用户可能已将桌面应用程序固定到任务栏或“开始”菜单。 可将这些快捷方式指向打包后的新应用。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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

<span id="make" />
### <a name="make-your-packaged-app-open-files-instead-of-your-desktop-app"></a>使打包后的应用（而非桌面应用）打开文件

可确保用户默认对特定的文件类型打开新打包的应用，而不是打开桌面版应用。

为达到该目的，需指定每个想要从中继承文件关联的应用程序的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 可使用此 ID 管理将来版本的应用中的更改。 |
|MigrationProgId |[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)，描述想要从中继承文件关联的应用程序、组件和桌面应用版本。|

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

<span id="associate" />
### <a name="associate-your-packaged-app-with-a-set-of-file-types"></a>将打包的应用与一组文件类型相关联

可将打包后的应用与文件类型扩展相关联。 如果用户右键单击某个文件，然后选择**打开方式**选项，应用将出现在建议列表中。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
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

<span id="add" />
### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>向具有特定文件类型的文件的上下文菜单添加选项

在大多数情况下，用户双击即可打开文件。 如果用户右键单击某个文件，将显示多种选项。

可向该菜单添加选项。 这些选项为用户提供与文件进行交互的其他方法，如打印、编辑或预览文件。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3


#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category | 始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|Verb |文件资源管理器上下文菜单中显示的名称。 此字符串可使用 ```ms-resource``` 进行本地化。|
|ID |动词命令的唯一 ID。 如果你的应用为 UWP 应用，则会将该动词命令作为应用的激活事件参数的一部分向其传递，以便应用可以相应地处理用户的选择。 如果应用是完全信任的已打包应用，它将改为接收参数（请参阅下一项）。 |
|Parameters |与动词命令关联的实参参数和值的列表。 如果应用是完全信任的已打包应用，激活应用时会将这些参数作为事件参数传递给应用。 可以根据不同的激活动词命令自定义应用的行为。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 如果应用为 UWP 应用，则无法传递参数。 应用转而接收参数（请参阅上一项）。|
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

<span id="open" />
### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>直接使用 URL 打开某些类型的文件

可确保用户默认对特定的文件类型打开新打包的应用，而不是打开桌面版应用。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|UseUrl |指示是否直接从 URL 目标打开文件。 如果未设置此值，应用尝试使用 URL 打开文件的操作将导致系统先在本地下载文件。 |
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

<span id="rules" />
### <a name="create-firewall-exception-for-your-app"></a>为应用创建防火墙例外

如果应用需要通过端口进行通信，可向防火墙例外列表中添加应用。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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

## <a name="integrate-with-file-explorer"></a>与文件资源管理器集成

帮助用户整理文件并以熟悉的方式与之交互。

* [定义用户同时选择并打开多个文件时应用的行为方式](#define)
* [在文件资源管理器内以缩略图显示文件内容](#show)
* [在文件资源管理器的“预览”窗格中显示文件内容](#preview)
* [使用户能够使用文件资源管理器中的“类型”列对文件进行分组](#enable)
* [使文件属性可用于搜索、索引、属性对话框和详细信息窗格](#make-file-properties)

<span id="define" />
### <a name="define-how-your-app-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>定义用户同时选择并打开多个文件时应用的行为方式

指定用户同时打开多个文件时应用的行为方式。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|MultiSelectModel |请参阅下文 |
|FileType |相关的文件扩展名。 |

**MultSelectModel**

已打包的桌面应用具有与常规桌面应用相同的三个选项。

 * ``Player``：应用已激活一次。 将所有所选文件作为实参参数传递给应用。
 * ``Single``：为第一个选定的文件激活一次应用。 忽略其他文件。
 * ``Document``：为每个选定的文件激活应用的一个新的单独实例。

 可以为不同的文件类型和操作设置不同的首选项。 例如，可能会希望以“文档”**模式打开*文档*，以“播放机”**模式打开*图像*。

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

<span id="show" />
### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>在文件资源管理器内以缩略图显示文件内容

使用户可在文件图标以中、大或超大尺寸显示时查看文件内容的缩略图。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]"
            Cutoff="[Cutoff]"
            Treatment="[Treatment]" />
    </FileTypeAssociation>
</Extension>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|Clsid   |应用的类 ID。 |
|Cutoff |不会使用小于该尺寸的缩略图。 请参阅[缩略图缓存和大小调整](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#cache) |
|Treatment |定义缩略图图标外观的[缩略图修饰](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#adornments)。 |

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
              Clsid  ="20000000-0000-0000-0000-000000000001"
              Cutoff="20x20"
              Treatment="Video Sprockets" />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<span id="preview" />
### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>在文件资源管理器的“预览”窗格中显示文件内容

使用户可以在文件资源管理器的“预览”窗格中预览文件的内容。

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
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

<span id="enable" />
### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>使用户能够使用文件资源管理器中的“类型”列对文件进行分组

可以将文件类型的一个或多个预定义值与**类型**字段相关联。

在文件资源管理器中，用户可以使用该字段对这些文件进行分组。 系统组件也会将此字段用于不同的用途，例如建立索引。

若要详细了解**类型**字段以及可对此字段使用的值，请参阅[使用类型名称](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 |
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
<span id="make-file-properties" />
### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>使文件属性可用于搜索、索引、属性对话和详细信息窗格

#### <a name="xml-namespace"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid ]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```
**关键元素和特性说明**

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.fileTypeAssociation``。
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

<span id="start" />
## <a name="start-your-app-in-different-ways"></a>以不同方式启动应用

* [使用协议启动应用](#protocol)
* [使用别名启动应用](#alias)
* [用户登录 Windows 时启动可执行文件](#executable)
* [在接收来自 Windows 应用商店的更新后自动重启](#updates)

<span id="protocol" />
### <a name="start-your-app-by-using-a-protocol"></a>使用协议启动应用

协议关联允许其他程序和系统组件与打包后的应用进行互操作。 使用某个协议启动打包后的应用时，可以指定要传递到其激活事件参数的特定参数，以使该应用做出相应行为。 仅打包后完全受信任的应用支持参数。 UWP 应用不能使用参数。  

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
|Category |始终为 ``windows.protocol``。
|Name |协议的名称。 |
|Parameters |应用激活时要向其传递用作事件参数的参数和值的列表。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 |

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
<span id="alias" />
### <a name="start-your-app-by-using-an-alias"></a>使用别名启动应用

用户和其他进程可以使用别名启动应用，无需指定应用的完整路径。 可以指定该别名。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10


#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.appExecutionAlias``。
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

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.fileTypeAssociation``。
|Name |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 可使用此 ID 管理将来版本的应用中的更改。   |
|FileType |应用支持的文件扩展名。 |

<span id="executable" />
### <a name="start-an-executable-file-when-users-log-into-windows"></a>用户登录 Windows 时启动可执行文件

启动任务允许应用在用户登录时自动运行可执行文件。

> [!NOTE]
> 用户必须至少启动一次应用才可注册此启动任务。

应用可声明多个启动任务。 每个任务独立启动。 所有启动任务都将显示在任务管理器的**启动**选项卡下，其名称在应用的清单和应用的图标中指定。 任务管理器将自动分析任务的启动影响。

用户可以使用任务管理器手动禁用应用的启动任务。 如果用户禁用任务，则无法以编程方式重新启用它。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.startupTask``。|
|Executable |要启动的可执行文件的相对路径。 |
|TaskId |任务的唯一标识符。 应用可以使用此标识符调用 [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 类中的 API，以编程方式启用或禁用启动任务。 |
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

<span id="updates" />
### <a name="restart-automatically-after-receiving-an-update-from-the-windows-store"></a>在接收来自 Windows 应用商店的更新后自动重启

如果在用户安装应用更新时，你的应用已打开，则该应用将关闭。

如果你希望该应用在更新完成后重新启动，请在要重新启动的每个进程中调用 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 函数。

应用中的每个活动窗口都会收到 [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) 消息。 此时，你的应用可在必要时再次调用 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 函数以更新命令行。

当你的应用中的每个活动窗口都收到 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 消息时，应用应保存数据并关闭。

>[!NOTE]
如果该应用未处理 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 消息，你的活动窗口还会收到 [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) 消息。

此时，你的应用有 30 秒时间来关闭自己的进程，或由平台强制终止这些进程。

更新完成后，你的应用将重新启动。

## <a name="work-with-other-applications"></a>与其他应用程序配合使用

与其他应用集成，启动其他进程或共享信息。

* [使应用在支持打印的应用程序中显示为打印目标](#printing)
* [与其他 Windows 应用程序共享字体](#fonts)
* [从通用 Windows 平台 (UWP) 应用启动 Win32 进程](#win32-process)

<span id="printing" />
### <a name="make-your-app-appear-as-the-print-target-in-applications-that-support-printing"></a>使应用在支持打印的应用程序中显示为打印目标

用户想要从其他应用（如记事本）打印数据时，可使应用在可用打印目标的应用列表中显示为打印目标。

必须修改应用，使其接收 XML 纸张规范 (XPS) 格式的打印数据。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.appPrinter``。
|DisplayName |希望在应用的打印目标列表中显示的名称。 |
|Parameters |应用正确处理请求所需的任何参数。 |

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

<span id="fonts" />
### <a name="share-fonts-with-other-windows-applications"></a>与其他 Windows 应用程序共享字体

与其他 Windows 应用程序共享自定义字体。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

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
|Category |始终为 ``windows.sharedFonts``。
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
<span id="win32-process" />
### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>从通用 Windows 平台 (UWP) 应用启动 Win32 进程

启动以完全信任方式运行的 Win32 进程。

#### <a name="xml-namespaces"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此扩展的元素和特性

```XML
<xtension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.fullTrustProcess``。
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
如果想要创建在所有设备上运行的通用 Windows 平台用户界面，但希望 Win32 应用的组件继续以完全信任方式运行，此扩展可能会很有用。

只需为 Win32 应用创建桌面桥程序包。 然后，将此扩展添加到 UWP 应用的程序包文件。 此扩展指示想要在桌面桥程序包中启动可执行文件。  如果想要在 UWP 应用和 Win32 应用之间进行通信，可以设置一个或多个[应用服务](../launch-resume/app-services.md)来执行此操作。 可以在[此处](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)阅读关于此方案的详细信息。

## <a name="next-steps"></a>后续步骤

**查找特定问题的答案**

我们的团队监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
