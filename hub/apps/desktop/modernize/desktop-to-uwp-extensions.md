---
Description: 可以使用扩展以预定义方式将打包的桌面应用与 Windows 10 集成。
title: 将打包的桌面应用程序与 Windows 10 和 UWP (桌面桥) 集成
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: c47eba0e0f5969e978cde5575cf8ab05e589350e
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682482"
---
# <a name="integrate-your-packaged-desktop-app-with-windows-10-and-uwp"></a>将打包桌面应用与 Windows 10 和 UWP 集成

如果将[桌面应用程序打包到 .msix 容器中](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), 则可以使用[应用程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)中的预定义扩展, 通过扩展将打包桌面应用程序与 Windows 10 集成。

例如, 使用扩展创建防火墙例外, 将应用程序设置为文件类型的默认应用程序, 或将开始磁贴指向应用的打包版本。 若要使用扩展，只需将某些 XML 添加到应用的程序包清单文件。 不需要任何代码。

本文介绍了这些扩展以及使用这些扩展可以执行的任务。

> [!NOTE]
> 本文中所述的功能要求你为桌面应用程序创建 Windows 应用包。 如果尚未执行此操作, 请参阅[打包桌面应用程序](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。

## <a name="transition-users-to-your-app"></a>将用户切换到应用

帮助用户切换到打包后的应用。

* [将现有的开始磁贴和任务栏按钮指向打包的应用](#point)
* [使打包的应用程序打开文件而不是桌面应用](#make)
* [将打包应用程序与一组文件类型相关联](#associate)
* [向具有特定文件类型的文件的上下文菜单添加选项](#add)
* [使用 URL 直接打开特定类型的文件](#open)

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
|Category |始终为 ``windows.desktopAppMigration``。
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

[具有过渡/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>使打包的应用程序打开文件而不是桌面应用

您可以确保默认情况下, 用户打开新的打包应用程序以用于特定类型的文件, 而不是打开您的应用程序的桌面版本。

为达到该目的，需指定每个想要从中继承文件关联的应用程序的[编程标识符 (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids)。

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids)。 此 ID 可用于管理将来版本的应用中的更改。 |
|MigrationProgId |[编程标识符 (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) , 用于描述要从中继承文件关联的桌面应用程序的应用程序、组件和版本。|

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

[具有过渡/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>将打包应用程序与一组文件类型相关联

可以将打包应用程序与文件类型扩展名关联。 如果用户右键单击文件, 然后选择 "**打开方式**" 选项, 则应用程序将显示在建议列表中。

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 此 ID 内部用于生成与文件类型关联相关联的经过哈希处理的[编程标识符 (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids)。 此 ID 可用于管理将来版本的应用中的更改。   |
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

[具有过渡/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

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
|Category | 始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
|Verb |文件资源管理器上下文菜单中显示的名称。 此字符串可使用 ```ms-resource``` 进行本地化。|
|Id |动词命令的唯一 ID。 如果你的应用程序是 UWP 应用, 则会将其作为激活事件参数的一部分传递给你的应用, 以便它能够相应地处理用户的选择。 如果你的应用程序是完全信任的打包应用程序, 则它将改为接收参数 (请参阅下一项目符号)。 |
|Parameters |与动词命令关联的实参参数和值的列表。 如果你的应用程序是完全信任的打包应用程序, 则当激活应用程序时, 这些参数将作为事件参数传递给应用程序。 您可以根据不同的激活谓词自定义应用程序的行为。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 如果应用程序是 UWP 应用程序, 则不能传递参数。 应用转而接收参数（请参阅上一项）。|
|扩展 |指定动词命令仅在用户右键单击文件之前按住 **Shift** 键显示上下文菜单时才显示。 如果未列出该特性，则该特性可选，并且默认为值 **False**（例如，始终显示动词命令）。 为每个动词命令逐个指定此行为（“打开”除外，它始终为 **False**）。|

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

[具有过渡/迁移/卸载的 WPF 图片查看器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>直接使用 URL 打开某些类型的文件

您可以确保默认情况下, 用户打开新的打包应用程序以用于特定类型的文件, 而不是打开您的应用程序的桌面版本。

#### <a name="xml-namespaces"></a>XML 命名空间

* http:\//schemas.microsoft.com/appx/manifest/uap/windows10
* http:\//schemas.microsoft.com/appx/manifest/uap/windows10/3 "

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
|UseUrl |指示是否直接从 URL 目标打开文件。 如果未设置此值, 则通过使用 URL 打开文件的应用程序会导致系统首先在本地下载文件。 |
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
* [将 DLL 文件放到包的任何文件夹中](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>为应用创建防火墙例外

如果你的应用程序需要通过端口进行通信, 则可以将应用程序添加到防火墙例外列表中。

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
|Category |始终``windows.firewallRules``|
|可执行文件 |想要添加到防火墙例外列表的可执行文件名称 |
|Direction |指示规则是传入规则还是传出规则 |
|IPProtocol |通信协议 |
|LocalPortMin |本地端口号范围内较小的端口号。 |
|LocalPortMax |本地端口号范围内最大的端口号。 |
|RemotePortMax |远程端口号范围内较小的端口号。 |
|RemotePortMax |远程端口号范围内最大的端口号。 |
|配置文件 |网络类型 |

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

如果你不使用该扩展，系统将按以下顺序进行搜索：进程的程序包依赖关系图、程序包根文件夹、系统目录 ( _%SystemRoot%\system32_)。 若要了解详细信息，请参阅 [Windows 应用的搜索顺序](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order)。

每个程序包只能包含一个这种类型的扩展。 也就是说，你可以将其中一个扩展添加到主程序包，然后向你的每个[可选包和相关的集](/windows/msix/package/optional-packages)添加一个扩展。

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
|Category |始终为 ``windows.loaderSearchPathOverride``。
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

* [定义当用户同时选择和打开多个文件时应用程序的行为](#define)
* [在文件资源管理器中显示缩略图中的文件内容](#show)
* [在文件资源管理器的预览窗格中显示文件内容](#preview)
* [使用文件资源管理器中的 Kind 列允许用户对文件进行分组](#enable)
* [使文件属性可用于搜索、索引、属性对话框和详细信息窗格](#make-file-properties)
* [为文件类型指定上下文菜单处理程序](#context-menu)
* [使云服务中的文件显示在文件资源管理器中](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>定义当用户同时选择和打开多个文件时应用程序的行为

指定当用户同时打开多个文件时应用程序的行为。

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
|MultiSelectModel |请参阅下文 |
|FileType |相关的文件扩展名。 |

**MultSelectModel**

已打包的桌面应用具有与常规桌面应用相同的三个选项。

* ``Player``：你的应用程序已激活一次。 所有选定的文件都作为参数参数传递给你的应用程序。
* ``Single``：对于第一个选定的文件, 将激活一次应用程序。 忽略其他文件。
* ``Document``：将为每个选定文件激活单独的应用程序的新实例。

 可以为不同的文件类型和操作设置不同的首选项。 例如，可能会希望以“文档”  模式打开*文档*，以“播放机”  模式打开*图像*。

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

如果用户打开 15 个或更少的文件，**MultiSelectModel** 特性的默认选项为*播放机*。 否则，默认选项“文档”  。 始终将 UWP 应用启动为*播放机*。

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
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

若要详细了解**类型**字段以及可对此字段使用的值，请参阅[使用类型名称](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names)。

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
|FileType |相关的文件扩展名。 |
|value |有效的[类型值](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) |

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

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>使文件属性可用于搜索、索引、属性对话框和详细信息窗格

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
|Category |始终为 ``windows.fileTypeAssociation``。
|名称 |应用的唯一 ID。 |
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

<a id="context-menu" />

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>为文件类型指定上下文菜单处理程序

如果桌面应用程序定义了[上下文菜单处理程序](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers), 请使用此扩展插件来注册 menu 处理程序。

#### <a name="xml-namespaces"></a>XML 命名空间

* http://schemas.microsoft.com/appx/manifest/foundation/windows10
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/4

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

在此处找到完整的架构参考: [com: ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver)和[desktop4: FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)。

#### <a name="instructions"></a>说明

若要注册上下文菜单处理程序, 请遵循这些说明。

1. 在桌面应用程序中, 通过实现[IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand)或[IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate)接口实现[上下文菜单处理程序](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers)。 有关示例, 请参阅[ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb)代码示例。 请确保为每个实现对象定义类 GUID。 例如, 以下代码定义[IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand)的实现的类 ID。

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. 在包清单中, 指定使用上下文菜单处理程序实现的类 ID 注册 COM 代理服务器的[com: ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver)应用程序扩展。

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. 在包清单中, 指定用于注册上下文菜单处理程序实现的[desktop4: FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)应用程序扩展。

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>示例

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
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
|Category |始终为 ``windows.cloudfiles``。
|iconResource |代表你的云文件提供商服务的图标。 该图标显示在文件资源管理器的导航窗格中。  用户选择该图标可显示云服务中的文件。 |
|CustomStateHandler Clsid |实现 CustomStateHandler 的应用程序的类 ID。 系统使用该类 ID 请求云文件的自定义状态和列。 |
|ThumbnailProviderHandler Clsid |实现 ThumbnailProviderHandler 的应用程序的类 ID。 系统使用该类 ID 请求云文件的缩略图图像。 |
|ExtendedPropertyHandler Clsid |实现 ExtendedPropertyHandler 的应用程序的类 ID。  系统使用该类 ID 请求云文件的扩展属性。 |
|Verb |你的云服务提供的文件在文件资源管理器上下文菜单中显示的名称。 |
|Id |动词命令的唯一 ID。 |

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

## <a name="start-your-application-in-different-ways"></a>以不同的方式启动应用程序

* [使用协议启动应用程序](#protocol)
* [使用别名启动应用程序](#alias)
* [用户登录到 Windows 时启动可执行文件](#executable)
* [使用户能够在将设备连接到其 PC 时启动你的应用程序](#autoplay)
* [从 Microsoft Store 收到更新后自动重新启动](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>使用协议启动应用程序

协议关联允许其他程序和系统组件与打包后的应用进行互操作。 使用协议启动打包应用程序时, 可指定要传递给其激活事件参数的特定参数, 使其能够相应地进行处理。 仅打包后完全受信任的应用支持参数。 UWP 应用不能使用参数。

#### <a name="xml-namespace"></a>XML 命名空间

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>该扩展的元素和特性

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
|名称 |协议的名称。 |
|Parameters |激活应用程序时作为事件参数传递给应用程序的参数和值的列表。 如果变量可包含文件路径，请用引号将参数值括起来。 这将避免路径包含空格的情况下出现的任何问题。 |

### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>使用别名启动应用程序

用户和其他进程可以使用别名来启动你的应用程序, 而无需指定应用程序的完整路径。 可以指定该别名。

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

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.appExecutionAlias``。
|可执行文件 |调用别名时要启动的可执行文件的相对路径。 |
|别名 |应用的简称。 它必须始终以“.exe”扩展名结尾。 只可以为程序包中每个应用程序指定一个应用执行别名。 如果多个应用都注册了同一个别名，系统会调用最后注册的一个应用，因此请确保选择其他应用不太可能覆盖的独特别名。
|

#### <a name="example"></a>示例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
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
 
...
</Package>
```

在[此处](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)查找完整的架构参考。

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>用户登录 Windows 时启动可执行文件

启动任务使应用程序能够在用户登录时自动运行可执行文件。

> [!NOTE]
> 用户至少必须启动一次应用程序才能注册此启动任务。

您的应用程序可以声明多个启动任务。 每个任务独立启动。 所有启动任务都将显示在任务管理器的**启动**选项卡下，其名称在应用的清单和应用的图标中指定。 任务管理器将自动分析任务的启动影响。

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
|Category |始终为 ``windows.startupTask``。|
|可执行文件 |要启动的可执行文件的相对路径。 |
|TaskId |任务的唯一标识符。 使用此标识符, 你的应用程序可以调用[windows.applicationmodel.resources.core. StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask)类中的 api 以编程方式启用或禁用启动任务。 |
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

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>使用户能够在将设备连接到其 PC 时启动你的应用程序

当用户将设备连接到 PC 时, 自动播放可以提供应用程序。

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
|Category |始终为 ``windows.autoPlayHandler``。
|ActionDisplayName |一个字符串, 该字符串表示用户可对其连接到 PC 的设备执行的操作 (例如:"导入文件", 或 "播放视频"。 |
|ProviderDisplayName | 表示应用程序或服务的字符串 (例如:"Contoso 视频播放器")。 |
|ContentEvent |导致向用户提示你的 ``ActionDisplayName`` 和 ``ProviderDisplayName`` 的内容事件的名称。 当卷设备（如相机内存卡、U 盘或 DVD）插入到电脑时，会引发内容事件。 你可以在[此处](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)找到此类事件的完整列表。  |
|Verb |谓词设置标识为所选选项传递到应用程序的值。 你可以为自动播放事件指定多个启动操作并且可以使用谓词设置确定用户为你的应用选择的选项。 你可以通过检查传递给应用的启动事件参数的 verb 属性来标识用户选择的选项。 你可以为谓词设置使用任何值（但保留的 open 除外）。 |
|DropTargetHandler |实现[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)接口的应用程序的类 ID。 系统会将可移动媒体中的文件传递给你的 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 实现的 [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) 方法。  |
|Parameters |你不必为所有内容事件实现 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 接口。 对于任意内容事件，你可以提供命令行参数，而不是实现 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 接口。 对于这些事件, 自动播放将使用这些命令行参数来启动应用程序。 你可以在应用的初始化代码中解析这些参数，确定应用是否由自动播放启动，然后提供自定义实现。 |
|DeviceEvent |导致向用户提示你的 ``ActionDisplayName`` 和 ``ProviderDisplayName`` 的设备事件的名称。 当有设备连接到电脑时，将引发设备事件。 设备事件以字符串 ``WPD`` 开头，你可以在[此处](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)找到设备事件列表。 |
|HWEventHandler |实现[IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler)接口的应用程序的类 ID。 |
|InitCmdLine |你要传递给 [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) 接口的 [Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) 方法的字符串参数。 |

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

如果在用户安装更新时应用程序处于打开状态, 则应用程序将关闭。

如果希望在更新完成后重新启动该应用程序, 请在要重新启动的每个进程中调用[RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart)函数。

应用程序中的每个活动窗口都会收到一条[WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession)消息。 此时, 应用程序可以再次调用[RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart)函数以更新命令行 (如有必要)。

当应用程序中的每个活动窗口收到[WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession)消息时, 应用程序应保存数据并关闭。

>[!NOTE]
活动窗口还会在应用程序不处理[WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession)消息时收到[WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close)消息。

此时, 你的应用程序将有30秒的时间关闭其自己的进程, 否则平台会将它们强制终止。

更新完成后, 应用程序将重新启动。

## <a name="work-with-other-applications"></a>与其他应用程序配合使用

与其他应用集成，启动其他进程或共享信息。

* [使应用程序在支持打印的应用程序中显示为打印目标](#printing)
* [与其他 Windows 应用程序共享字体](#fonts)
* [从通用 Windows 平台 (UWP) 应用启动 Win32 进程](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>使应用程序在支持打印的应用程序中显示为打印目标

当用户想要从其他应用程序 (如记事本) 打印数据时, 可以将应用程序作为打印目标显示在应用程序的可用打印目标列表中。

您必须修改应用程序, 以便它接收 XML 纸张规范 (XPS) 格式的打印数据。

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
|Category |始终为 ``windows.appPrinter``。
|DisplayName |希望在应用的打印目标列表中显示的名称。 |
|Parameters |应用程序正确处理请求所需的任何参数。 |

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

在[此处](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)查找完整的架构参考。

|名称 |描述 |
|-------|-------------|
|Category |始终为 ``windows.sharedFonts``。
|文件 |包含想要共享的字体的文件。 |

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

如果要创建在所有设备上运行的通用 Windows 平台用户界面, 但希望 Win32 应用程序的组件继续以完全信任模式运行, 则此扩展可能会很有用。

只需为 Win32 应用创建 Windows 应用包即可。 然后，将此扩展添加到 UWP 应用的程序包文件。 此扩展指示你要在 Windows 应用包中启动可执行文件。  如果想要在 UWP 应用和 Win32 应用之间进行通信，可以设置一个或多个[应用服务](/windows/uwp/launch-resume/app-services.md)来执行此操作。 可以在[此处](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)阅读关于此方案的详细信息。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
