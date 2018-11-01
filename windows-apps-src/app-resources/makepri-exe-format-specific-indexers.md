---
author: stevewhims
Description: This topic describes the format-specific indexers used by the MakePri.exe tool to generate its index of resources.
title: MakePri.exe 特定格式索引器
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: 439a69da400caaa9ae509a121f2aa7336853d2ca
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876662"
---
# <a name="makepriexe-format-specific-indexers"></a>MakePri.exe 特定格式索引器

本主题介绍 [MakePri.exe](compile-resources-manually-with-makepri.md) 工具生成其资源索引时所使用的特定格式索引器。

> [!NOTE]
> 当你选择**适用于 UWP 管理应用的 Windows SDK**选项安装 Windows 软件开发工具包时，会安装 MakePri.exe。 安装到路径`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe`（以及中名为其他体系结构的文件夹）。 例如，`C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

MakePri.exe 通常与 `new`、`versioned` 或 `resourcepack` 命令一起使用。 请参阅 [MakePri.exe 命令行选项](makepri-exe-command-options.md)。 在这些情况下，它索引源文件以生成资源索引。 MakePri.exe 使用各种单独的索引器读取不同的源资源文件或资源容器。 最简单的索引器是文件夹索引器，它索引文件夹的内容，如 `.jpg` 或 `.png` 图像。

通过指定 [MakePri.exe 配置文件](makepri-exe-configuration.md)的 `<index>` 元素中的 `<indexer-config>` 元素标识特定格式索引器。 `type` 属性标识使用的特定格式索引器。

在索引过程中遇到的资源容器通常将它们的内容编入索引，而不是添加到索引自身中。 例如，文件夹索引器找到的 `.resjson` 文件可能会由 `.resjson` 索引器进一步编制索引，在这种情况下 `.resjson` 文件本身不显示在索引中。 **注意**与该容器关联的索引器的 `<indexer-config>` 元素必须包括在配置文件中才会发生这种情况。

通常情况下，在包含实体（如文件夹或 `.resw` 文件）上找到的限定符应用到其中的所有资源，如文件夹中的文件，或 `.resw` 文件中的字符串。

## <a name="folder"></a>文件夹

文件夹索引器由文件夹的 `type` 属性进行标识。 它索引文件夹的内容，并从文件夹和文件名确定资源限定符。 它的配置元素符合以下架构。

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

`qualifierDelimiter` 属性指定在哪个字符后指定文件名中的限定符，忽略扩展。 默认值为“.”。

## <a name="pri"></a>PRI

PRI 索引器由 PRI 的 `type` 属性进行标识。 它索引 PRI 文件的内容。 你通常在将另一个程序集、DLL、SDK 或类库中包含的资源索引到应用的 PRI 中时使用它。

PRI 文件中包含的所有资源名称、限定符和值都直接在新的 PRI 文件中进行维护。 但顶级资源映射不在最终 PRI 中进行维护。 资源映射合并。

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

PriInfo 索引器由 PRIINFO 的 `type` 属性进行标识。 它索引详细转储文件的内容。 你可以通过使用 `/dt detailed` 选项运行 `makepri dump` 的方式产生详细的转储文件。 索引器的配置元素符合以下架构。

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

此配置元素允许可选属性配置 PriInfo 索引器的行为。 `emitStrings` 和 `emitPaths` 的默认值为 `true`。 如果 `emitStrings` 为 `true`，则 `type` 属性设置为“字符串”的候选资源包括在索引中，否则从索引中排除。 如果“emitPaths”为 `true`，则 `type` 属性设置为“路径”的候选资源包括在索引中，否则从索引中排除。

下面提供了包括字符串资源类型但跳过路径资源类型的配置示例。

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

要编入索引，转储文件必须以扩展 `.pri.xml` 结束，并且必须符合以下架构。

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri.exe 支持转储类型“基本”、“详细”、“架构”和“摘要”。 要配置 MakePri.exe 以发出 PriInfo 索引器可以读取的转储类型，则在使用 `dump` 命令时包括“详细 /DumpType”。

MakePri.exe 跳过 `.pri.xml` 文件的多个元素。 这些元素在索引过程中进行计算，或在 MakePri.exe 配置文件中指定。 转储文件中包含的资源名称、限定符和值都直接在新的 PRI 文件中进行维护。 但是，顶级资源映射并不保留在最终 PRI 中。 资源映射合并为索引的一部分。

这是来自转储文件的有效的字符串类型资源的示例。

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

这是来自转储文件的具有两个候选项的有效的路径类型资源的示例。

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

ResFiles 索引器由 RESFILES 的 `type` 属性进行标识。 它索引 `.resfiles` 文件的内容。 它的配置元素符合以下架构。

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resfiles` 文件是包含一个文件路径简单列表的文本文件。 `.resfiles` 文件可以包含“//”评论。 以下提供了一个示例。

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

ResJSON 索引器由 RESJSON 的 `type` 属性进行标识。 它索引 `.resjson` 文件的内容，这是字符串资源文件。 它的配置元素符合以下架构。

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resjson` 文件包含 JSON 文本（请参阅 [JavaScript 对象表示法 (JSON) 的应用程序/json 媒体类型](http://www.ietf.org/rfc/rfc4627.txt)）。 文件必须包含一个具有分层属性的简单 JSON 对象。 每个属性必须是另外一个 JSON 对象或字符串值。

名称以下划线（“_”）开头的 JSON 属性不编译到最终 PRI 文件中，但保留在日志文件中。

文件还可以包含“//”评论，该评论在分析过程中被忽略。

`initialPath` 属性通过将此初始路径追加在资源名称前面的方式将所有资源放在此初始路径下。 这通常在为类库资源编制索引时会用到。 默认值为空。

## <a name="resw"></a>ResW

ResW 索引器由 RESW 的 `type` 属性进行标识。 它索引 `.resw` 文件的内容，这是字符串资源文件。 它的配置元素符合以下架构。

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resw` 文件是符合以下方案的 XML 文件。

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

`convertDotsToSlashes` 属性将在资源名称（数据元素名称属性）中找到的所有点（“.”）字符转换为正斜杠“/”，除非点字符位于“[”和“]”之间。

`initialPath` 属性通过将此初始路径追加在资源名称前面的方式将所有资源放在此初始路径下。 这通常在为类库资源编制索引时用到。 默认值为空值。

## <a name="related-topics"></a>相关主题

* [使用 MakePri.exe 手动编译资源](compile-resources-manually-with-makepri.md)
* [MakePri.exe 命令行选项](makepri-exe-command-options.md)
* [MakePri.exe 配置文件](makepri-exe-configuration.md)
* [JavaScript 对象表示法 (JSON) 的应用程序/json 媒体类型](http://www.ietf.org/rfc/rfc4627.txt)