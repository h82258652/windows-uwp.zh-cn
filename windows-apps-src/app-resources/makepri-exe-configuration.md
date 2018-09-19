---
author: stevewhims
Description: This topic describes the schema of the MakePri.exe XML configuration file.
title: MakePri.exe 配置文件
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 512880b7a7ea955a45697762cbbdb7f74ac70102
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052983"
---
# <a name="makepriexe-configuration-file"></a>MakePri.exe 配置文件

本主题介绍 [MakePri.exe](compile-resources-manually-with-makepri.md) XML 配置文件的架构；也称为 PRI 配置文件。 MakePri.exe 工具具有 [createconfig 命令](makepri-exe-command-options.md#createconfig-command)，可用于创建一个新的初始化 PRI 配置文件。

> [!NOTE]
> 当你查看安装 Windows 软件开发工具包时**适用于托管的 UWP 应用的 Windows SDK**选项安装 MakePri.exe。 安装到路径`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe`（以及中名为其他体系结构的文件夹）。 例如，`C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

PRI 配置文件控制索引哪些资源以及如何索引。 配置 XML 必须符合以下架构。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- `default` 元素指定当运行时上下文不匹配任何候选资源时用于解析资源的上下文（语言、比例、对比度等）。 此上下文在生成时间进行指定，并且不会更改，因此当创建上下文时将资源解析到此上下文。 匹配分数在生成时间进行存储。 必须为每个限定符都指定一个值。 请参阅 [ResourceContext](resource-management-system.md#resourcecontext) 获取有关如何选择资源的详细信息。
- `index` 元素定义对资产完成的离散索引传递。 每个索引传递确定要使用的[特定格式索引器](makepri-exe-format-specific-indexers.md)以及要索引的资源。
- `qualifiers` 元素设置其他资源继承的第一个文件或文件夹的初始限定符。 每个限定符元素必须具有有效的名称和值（请参阅[定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md))。
- `root` 属性是索引传递的物理文件的路径根。 它可以是相对的或绝对的。 如果是相对的，则它追加到你在命令行中提供的项目根。 如果是绝对的，则它直接用作索引传递根。 使用反斜杠或正斜杠都可以。 尾部斜杠会被删除。 索引传递的根确定一个文件夹，所有资源对该文件夹而言都被视为相对的。
- `startIndexAt` 属性是编制索引过程中使用的初始种子文件或文件夹。 它相对于索引传递根。 空值假设索引传递根文件夹。

## <a name="default-pri-config-file"></a>默认 PRI 配置文件

当签发 [createconfig 命令](makepri-exe-command-options.md#createconfig-command)时，MakePri.exe 生成此新的初始化 PRI 配置文件。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>打包元素

`packaging` 元素定义 PRI 拆分信息。 针对自动（支持沿特定维度的 `autoResourcePackage`）和手动配置定义 `packaging` 元素的架构。

此示例演示如何使用沿特定维度的 `autoResourcePackage`。

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

此示例演示如何使用手动 `resourcePackage`。

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe 不明确阻止沿任何特定维度生成资源 PRI 文件。 沿一组特定维度的限制在外部由 MakeAppx.exe 或管道中的其他工具进行定义和实现。

MakePri.exe 分析所有 `index` 节点后的 `packaging` 元素以填充所有默认限定符。 MakePri.exe 收集在这些数据结构中分析的信息。

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>resources@isDeploymentMergeable 属性

此属性在 PRI 文件中设置标记，导致

- 部署合并以标识此 PRI 文件可以合并。
- GetFullyQualifiedReference 在设置了该标志但未使用文件来初始化资源管理器的情况下返回错误。

此属性的的默认值为 `true`。 MakePri.exe 仅在你的目标为 Windows 10 时设置 PRI 中的标记。

如果你的目标为 Windows 10，我们建议你在创建资源包时省略 `isDeploymentMergeable`（或将其显式设置为 `true`）。

如果使用 `/dt detailed` 选项运行 `makepri dump`，MakePri.exe 会将 `isDeploymentMergeable` 的值添加到转储文件。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>resources@majorVersion 属性

此属性的默认值为 1。 如果你提供一个显式值，并且还对 MakePri.exe 工具使用弃用的 `/VersionMajor(vma)` 命令行选项，则以配置文件中的值优先。

以下提供了一个示例。

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>resources@targetOsVersion 属性

指示目标操作系统版本。 下表显示了受支持的值；默认值为 6.3.0。

| 值 | 含义 |
| ----- | ------- |
| 10.0.0 | Windows10 |
| 6.3.0（默认值） | Windows 8.1 |
| 6.2.1 | Windows 8 |

以下提供了一个示例。

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**注意** Windows 相对于 PRI 文件后向兼容；但并不总是向前兼容。

如果使用 `/dt detailed` 选项运行 `makepri dump`，MakePri.exe 会将 `targetOsVersion` 的值添加到转储文件。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>验证错误消息

下面提供了一些错误条件及相应的错误消息示例。

| 条件 | 严重性 | 消息 |
| --------- | -------- | ------- |
| 指定除其中一个受支持的值以外的 targetOsVersion。 | 错误 | 无效的配置：指定的 targetOsVersion 无效。 |
| 指定的 targetOsVersion 为“6.2.1”，且存在 `packaging` 元素。 | 错误 | 无效的配置：此 targetOsVersion 不支持“打包”节点。 |
| 在配置中找到多个模式。 例如，指定了 Manual 和 AutoResourcePackage。 | 错误 | 无效的配置：“打包”节点不能具有多个操作模式。 |
| 默认限定符在资源包下列出。 | 错误 | 无效的配置：<Qualifiername>=<QualifierValue> 是默认限定符，不能将其候选项添加到资源包。 |
| AutoResourcePackage 限定符包含多个限定符。 例如 language_scale。 | 错误 | 无效配置: 不支持具有多个限定符的 AutoResourcePackage。 |
| ResourcePackage QualifierSet 包含多个限定符。 例如 language-en-us_scale-100 | 错误 | 无效配置: 不支持具有多个限定符的 QualifierSet。 |
| 发现了重复的资源包名称。 | 错误 | 无效的配置：重复的资源包名称 <rpname>。 |
| 在两个资源包中定义了相同的限定符集。 | 错误 | 无效的配置：找到 QualifierSet“<qualifier tags>”的多个实例。 |
| 找不到为“ResourcePackage”节点列出的 QualifierSet 的候选项。 | 警告 | 无效的配置：找不到 <Resource Package Name> 的候选项。 |
| 找不到在“AutoResourcePackage”节点下列出的限定符的候选项。 | 警告 | 无效的配置：找不到限定符 <qualifier name> 的候选项。 未生成资源包。 |
| 找不到模式。 也就是说，发现了空的 'packaging' 节点。 | 警告 | 无效的配置：未指定打包模式。 |

## <a name="related-topics"></a>相关主题

* [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)
* [MakePri.exe 命令行选项 - createconfig 命令](makepri-exe-command-options.md#createconfig-command)
* [定制语言、比例、高对比度和其他限定符的资源](tailor-resources-lang-scale-contrast.md)
* [资源管理系统 - ResourceContext](resource-management-system.md#resourcecontext)