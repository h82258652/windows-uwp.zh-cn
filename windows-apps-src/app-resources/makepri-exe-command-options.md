---
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: MakePri.exe 命令行选项
template: detail.hbs
ms.date: 04/10/2018
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: bc08376dafec8aad9d65ef5acd8d19943d242eed
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8756073"
---
# <a name="makepriexe-command-line-options"></a>MakePri.exe 命令行选项

[MakePri.exe](compile-resources-manually-with-makepri.md) 具有命令集 `createconfig`、`dump`、`new`、`resourcepack` 和 `versioned`。 本主题对命令行选项的使用进行详细介绍。

> [!NOTE]
> 检查安装 Windows 软件开发工具包时**适用于托管的 UWP 应用的 Windows SDK**选项时，MakePri.exe 会安装。 安装到路径`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe`（以及如下所示命名为其他体系结构文件夹）。 例如，`C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

## <a name="getting-help-from-the-command-line"></a>从命令行中获取帮助

你可以运行`MakePri.exe help`或`MakePri.exe /?`以查看你可以使用 MakePri.exe 命令。 你还可以发出`MakePri.exe <command> /?`若要查看有关某个命令，并在极少数情况下的细节，甚至是`MakePri.exe <command> <option>`以查看有关选项的详细信息。

## <a name="makepri-commands"></a>MakePri 命令

```console
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Createconfig 命令

`createconfig` 命令创建一个新的初始化 PRI 配置文件，该文件定义你指定的限定符默认值。 运行 `MakePri.exe createconfig /?` 以查看关于此命令的详细帮助。

```console
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Dump 命令

`dump` 命令输入一个转储文件，该文件包含指定 PRI 文件中的所有资源的列表。 运行 `MakePri.exe dump /?` 以查看关于此命令的详细帮助。

> [!NOTE]
> 无模式资源包是在 PRI 配置文件中使用 *omitSchemaFromResourcePacks* 开关创建的资源包。 若要转储无模式资源包，请使用开关 `/es <main_package_PRI_file>`。 如果不指定主文件，则会看到错误消息“*程序包中的 resources.pri 已损坏，因此加密失败（错误 PRI222：0xdef0000f - 发生未知错误）*”。

```console
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>New 命令

`new` 命令通过按照你的配置文件的指示索引你的项目中的文件的方式创建一个新的 PRI 文件。 运行 `MakePri.exe new /?` 以查看关于此命令的详细帮助。

```console
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>ResourcePack 命令

`resourcepack` 命令通过按照你的配置文件的指示索引你的项目中的文件的方式创建一个新的 PRI 文件。 资源包 PRI 文件仅包含在现有 PRI 文件中已经指定的资源的其他变体。 运行 `MakePri.exe resourcepack /?` 以查看关于此命令的详细帮助。

```console
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Versioned 命令

`versioned` 命令通过按照你的配置文件的指示索引你的项目中的文件的方式创建一个版本化的 PRI 文件。 运行 `MakePri.exe versioned /?` 以查看关于此命令的详细帮助。

```console
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

你使用扩展 DLL 选项 (/ex) 与 `createconfig`、`dump`、`new`、`resourcepack` 和 `versioned` 一起指定资源管理系统环境扩展 DLL 的位置。

## <a name="logging47metadata-file"></a>Logging&#47;metadata 文件

MakePri 可以将资源包特定的信息包括在索引器元数据文件中。 下面是具有资源 PRI 文件 `german.pri` 和 `highresolution.pri` 的 `resources.pri` 的日志文件的示例。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>&#47;IndexFile(if) 选项

你使用索引文件选项 (/if) 与 `dump`、`resourcepack` 和 `versioned` 一起指定输入的 PRI 文件。

对于 `resourcepack` 和 `versioned`，你可以不提供 PRI 文件作为 /IndexFile(if) 的输入参数，而是提供架构文件。

```console
/IndexFile(if) <FILEPATH>
```

**FILEPATH** 是指定输入的 PRI 文件或 PRI 架构文件位置的标记。

## <a name="47indexoptionsio-option"></a>& #47;IndexOptions(io) 选项

使用索引的选项选项 (/ io) 与`new`， `resourcepack`，并`versioned`来指定提供资源索引器的行为的精细的控制的选项。 默认情况下，索引选项处于禁用状态。

```console
/IndexOptions(io) <OPTIONS>
```

**选项**是包含以下选项中的逗号分隔列表。

- + /-HiddenFiles(hf)。 索引 （+） 或忽略 （-） 隐藏的文件和文件夹。
- + /-LinkedFiles(lf)。 索引 （+） 或忽略 （-） 链接的文件和文件夹。

## <a name="47mappingfilemf-option"></a>&#47;MappingFile(mf) 选项

你可以使用映射文件选项 (/mf) 与 `new`、`resourcepack` 和 `versioned` 一起生成映射文件。 [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md) 使用映射文件生成应用包。

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** 是指定映射文件格式的标记。 唯一受支持的有效格式为 `appx`。

```console
/mf appx
```

这是主要映射文件内容的示例。

```console
"ResourceDimensions"                   "language-de-de"
```

这是资源包映射文件内容的示例。

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>输出摘要

如果创建资源包，来自 MakePRI.exe 的输出摘要采用更加详细的窗体。 以下提供了一个示例。

```console
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>&#47;Overwrite(o) 选项

如果未提供覆盖选项 (/o)，且已经存在指定的输出文件，则 MakePri.exe 在覆盖前需要确认。

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;OutputFile(of) 选项

你使用输出文件选项 (/of) 与 `dump`、`new`、`resourcepack` 和 `versioned` 一起指定要生成的 PRI 文件的输出位置和名称。 如果 MakePri.exe 生成多个资源 PRI 文件，它将它们放在目标文件的父文件夹中。 例如，如果你指定了 `/of MyParentFolder\TargetFile.pri`，则 MakePri.exe 在 `ParentFolder` 下生成 `TargetFile.language-en.pri` 和 `TargetFile.scale-100.pri` 以及 `TargetFile.pri`。

下面提供错误条件及相应的错误消息的示例。

| 错误条件 | 错误消息 |
| --------------- | ------------- |
| 输出文件名与配置中的其中一个资源包名称相同。 | 无效的配置：资源包名称 <resource pack name> 不能与输出文件 <outputfilename.pri> 相同。 |

## <a name="reversemaprm-option"></a>/ReverseMap(rm) 选项

你使用反向映射选项 (/rm) 与 `new`、`resourcepack` 和 `versioned` 一起生成 PRI 文件中的反向映射部分，可用于调试。

## <a name="47schemafilesf-option"></a>&#47;SchemaFile(sf) 选项

你使用架构文件选项 (/sf) 与 `new`、`resourcepack` 和 `versioned` 一起在指定位置写入架构文件。

对于 `resourcepack` 和 `versioned`，你可以不提供 PRI 文件作为 /IndexFile(if) 的输入参数，而是提供架构文件。

```console
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** 是指定写入架构文件的位置的标记。

这是架构文件的示例。

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor(vma) 已弃用

主要版本 (/vma) 选项（适用于 `new` 命令）已弃用，使用它将引发此警告消息。

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

若要提供主要版本号，使用配置文件中的 [resources@majorVersion](makepri-exe-configuration.md) 属性。

## <a name="related-topics"></a>相关主题

* [MakePri.exe](compile-resources-manually-with-makepri.md)
