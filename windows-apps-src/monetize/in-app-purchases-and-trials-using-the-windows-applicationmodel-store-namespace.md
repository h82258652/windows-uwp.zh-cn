---
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: 了解如何启用 UWP 应用中的应用内购买和试用（定向 Windows10 版本 1607 之前的版本）。
title: 使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用
ms.date: 08/25/2017
ms.topic: article
keywords: uwp, 应用内购买, IAP, 加载项, 试用, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 72f5875721d17bda79842989c1ac22475a06e938
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8327685"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>使用 Windows.ApplicationModel.Store 命名空间的应用内购买和试用

可以使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中的成员，将应用内购买和试用功能添加到通用 Windows 平台 (UWP) 应用，以帮助通过应用盈利。 这些 API 还提供应用的许可证信息访问权限。

本部分中的文章提供有关针对多个常见方案使用 **Windows.ApplicationModel.Store** 命名空间中的成员的深入指南和代码示例。 有关与 UWP 中的应用内购买相关的基本概念概述，请参阅[应用内购买和试用](in-app-purchases-and-trials.md)。 有关演示如何使用 **Windows.ApplicationModel.Store** 命名空间实现试用和应用内购买的完整示例，请参阅[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)。

> [!IMPORTANT]
> **Windows.ApplicationModel.Store** 命名空间不再更新新功能。 如果你的项目针对的是 Visual Studio 中的 **Windows 10 周年纪念版（10.0；版本 14393）** 或更高版本（即，针对 Windows 10 版本 1607 或更高版本），我们建议你使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间。 有关详细信息，请参阅[应用内购买和试用](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)。 在使用[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)的 Windows 桌面应用程序或应用或游戏，在合作伙伴中心中使用的开发沙盒中不受支持**Windows.ApplicationModel.Store**命名空间 （例如，这是这种情况的任何游戏的与 Xbox Live 集成）。 这些产品必须使用 **Windows.Services.Store** 命名空间才能实现应用内购买和试用。

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>开始使用 CurrentApp 和 CurrentAppSimulator 类

**Windows.ApplicationModel.Store** 命名空间的主要入口点是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 类。 此类提供的静态属性和方法可用于获取当前应用及其可用加载项的信息、获取当前应用或其加载项的许可证信息、为当前用户购买应用或加载项以及执行其他任务。

[CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 类从 Microsoft Store 获取其数据，因此必须拥有一个开发人员帐户，而且必须在 Microsoft Store 中发布应用，才能在你的应用中成功使用此类。 在将应用提交到 Microsoft Store 之前，可以使用此类的模拟版本（名为 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx)）测试代码。 测试应用后，并将其提交到 Microsoft Store 之前，必须将 **CurrentAppSimulator** 的实例替换为 **CurrentApp**。 如果应用使用 **CurrentAppSimulator**，则无法通过认证。

当使用 **CurrentAppSimulator** 时，应用许可和应用内产品的初始状态在本地文件中进行介绍，该文件位于名为 WindowsStoreProxy.xml 的开发计算机上。 有关此文件的详细信息，请参阅[将 WindowsStoreProxy.xml 文件与 CurrentAppSimulator 一起使用](#proxy)。

有关可使用 **CurrentApp** 和 **CurrentAppSimulator** 执行的常见任务的详细信息，请参阅以下文章。

| 主题       | 说明                 |
|----------------------------|-----------------------------|
| [排除或限制试用版中的功能](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 如果允许客户在试用期内免费使用你的应用，则可以通过排除或限制试用期内的某些功能，吸引客户升级到完整版应用。 |
| [启用应用内产品购买](enable-in-app-product-purchases.md)      |  无论你的应用是否免费，你都可以直接从应用中销售内容、其他应用或新的应用功能（例如解锁游戏的下一关）。 我们在此处显示了如何在应用中启用这些产品。  |
| [启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)      | 通过应用商店商业平台提供可消费应用内产品（这些项目可以进行购买、使用和再次购买），以便为客户提供强大可靠的购买体验。 这对游戏内货币（金子、金币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。 |
| [管理应用内产品的大目录](manage-a-large-catalog-of-in-app-products.md)      |   如果你的应用提供较大的应用内产品目录，可以选择按照本主题中描述的过程帮助管理你的目录。    |
| [使用收据验证产品购买](use-receipts-to-verify-product-purchases.md)      |   每个促成成功产品购买的 Microsoft Store 交易都可以选择返回交易收据，该收据可向客户提供有关所列产品和货币成本的信息。 有权访问此信息可支持以下方案：你的应用需要验证用户是否购买了你的应用，或者是否已从 Microsoft Store 进行了应用内产品购买。 |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>将 WindowsStoreProxy.xml 文件与 CurrentAppSimulator 一起使用

当使用 **CurrentAppSimulator** 时，应用许可和应用内产品的初始状态在本地文件中进行介绍，该文件位于名为 WindowsStoreProxy.xml 的开发计算机上。 改变应用状态的 **CurrentAppSimulator** 方法（例如，通过购买许可证或处理应用内购买）仅更新内存中 **CurrentAppSimulator** 对象的状态。 WindowsStoreProxy.xml 的内容不会更改。 当应用再次启动时，许可证状态将还原到 WindowsStoreProxy.xml 中所述的内容。

WindowsStoreProxy.xml 文件默认创建在以下位置：%UserProfile%\AppData\Local\Packages\\&lt;应用包文件夹&gt;\LocalState\Microsoft\Windows Store\ApiData。 你可以编辑此文件以定义想要模拟 **CurrentAppSimulator** 属性的场景。

尽管可以修改此文件中的值，但我们建议创建自己的 WindowsStoreProxy.xml 文件（在 Visual Studio 项目的数据文件夹中）以供 **CurrentAppSimulator** 改用。 模拟交易时，调用 [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) 以加载文件。 如果不调用 **ReloadSimulatorAsync** 加载你自己的 WindowsStoreProxy.xml 文件， **CurrentAppSimulator** 将创建/加载（但不是会覆盖）默认 WindowsStoreProxy.xml 文件。

> [!NOTE]
> 请注意，**CurrentAppSimulator** 在 **ReloadSimulatorAsync** 完成之前不会完全初始化。 并且，由于 **ReloadSimulatorAsync** 是异步方法，应格外小心以避免在初始化一个线程时，同时对另一个线程查询 **CurrentAppSimulator** 会出现争用情况。 有一种方法是使用一个标志来指示初始化已完成。 从 Microsoft Store 安装的应用必须使用 **CurrentApp** 而不是 **CurrentAppSimulator**，并且在这种情况下不调用 **ReloadSimulatorAsync**，因此刚刚提到的争用情况不适用。 出于此原因，请设计你的代码，以便它可在这两种情况下（异步和同步）都起作用。


<span id="proxy-examples" />

### <a name="examples"></a>示例

此示例是一个 WindowsStoreProxy.xml 文件（UTF-16 编码），描述了试用模式的应用在 2015 年 1 月 19 日 05:00 (UTC) 过期。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

下一个示例是一个 WindowsStoreProxy.xml 文件（UTF-16 编码），描述了应用已购买，但有一项功能在 2015 年 1 月 19 日 05:00 (UTC) 过期，并具有易耗型应用内购买。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>架构

本部分列出的 XSD 文件定义 WindowsStoreProxy.xml 文件的结构。 若要在使用 WindowsStoreProxy.xml 文件时将此架构应用到 Visual Studio 中的 XML 编辑器，请执行以下操作：

1. 在 Visual Studio 中打开 WindowsStoreProxy.xml 文件。
2. 在**XML**菜单上，单击**创建架构**。 这将基于 XML 文件的内容创建一个临时 WindowsStoreProxy.xsd 文件。
3. 将该 .xsd 文件的内容替换为以下架构。
4. 将文件保存到可以将其应用到多个应用项目的位置。
5. 在 Visual Studio 中切换到 WindowsStoreProxy.xml 文件。
6. 在**XML**菜单上，单击**架构**，然后在列表中找到 WindowsStoreProxy.xsd 文件所在的行。 如果该文件的位置不是你需要的（例如，如果仍然显示临时文件），则单击**添加**。 导航到正确的文件，然后单击**确定**。 你现在应该可以在列表中看到该文件。 确保在该架构的**使用**列中出现复选标记。

一旦完成这些操作，对 WindowsStoreProxy.xml 进行的编辑将遵循该架构。 有关详细信息，请参阅[如何：选择要使用的 XML 架构](http://go.microsoft.com/fwlink/p/?LinkId=403014)。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>元素和属性说明

本部分介绍 WindowsStoreProxy.xml 文件中的元素和属性。

此文件的根元素是 **CurrentApp** 元素，它表示当前应用。 此元素包含以下子元素。

|  元素  |  必需  |  数量  |  描述   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    是        |  1  |  包含应用列表的数据。            |
|  [LicenseInformation](#licenseinformation)  |     是       |   1    |   描述了可用于此应用及其持久型加载项的许可证。     |
|  [ConsumableInformation](#consumableinformation)  |      否      |   0 或 1   |   描述了可用于此应用的易耗型加载项。      |
|  [模拟](#simulation)  |     否       |      0 或 1      |   描述了对各种 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 方法的调用在测试过程如何在应用中工作。    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>ListingInformation 元素

此元素包含应用列表的数据。 **ListingInformation** 是 **CurrentApp** 元素的必需子元素。

**ListingInformation** 包含以下子元素。

|  元素  |  必需  |  数量  |  描述   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    是   |  1   |    提供有关应用的数据。         |
|  [Product](#product-child-of-listinginformation)  |    否  |  0 或更多   |      描述了该应用的加载项。     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>应用元素（ListingInformation 的子元素）

此元素描述了应用的许可证。 **App** 是 [ListingInformation](#listinginformation) 元素的必需子元素。

**App** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  **AppId**  |    是   |  1   |   识别应用商店中应用的 GUID。 这可以是用于测试的任何 GUID。        |
|  **LinkUri**  |    是  |  1   |    应用商店中列表页面的 URI。 这可以是用于测试的任何有效 URI。         |
|  **CurrentMarket**  |    是  |  1   |    客户所在的国家/地区。         |
|  **AgeRating**  |    是  |  1   |     一个整数，表示应用的最低年龄分级。 这是相同的值时，指定在合作伙伴中心提交应用。 由应用商店使用的值是：3、7、12 和 16。 有关这些分级的详细信息，请参阅[年龄分级](../publish/age-ratings.md)。        |
|  [MarketData](#marketdata-child-of-app)  |    是  |  1 或更多      |    包含有关给定国家/地区的应用的信息。 对于在其中列出了应用的每个国家/地区，必须包含 **MarketData** 元素。       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>MarketData 元素（App 的子元素）

此元素提供有关给定国家/地区的应用的信息。 对于在其中列出了应用的每个国家/地区，必须包含 **MarketData** 元素。 **MarketData** 是 [App](#app-child-of-listinginformation) 元素的必需子元素。

**MarketData** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  **Name**  |    是   |  1   |   在此国家/地区中应用的名称。        |
|  **Description**  |    是  |  1   |      在此国家/地区中应用的描述。       |
|  **Price**  |    是  |  1   |     在此国家/地区中应用的价格 。        |
|  **CurrencySymbol**  |    是  |  1   |     在此国家/地区使用的货币符号。        |
|  **CurrencyCode**  |    否  |  0 或 1      |      在此国家/地区使用的货币代码。         |  |

**MarketData** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **xml:lang**  |    是        |     指定市场数据信息适用的国家/地区。          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Product 元素（ListingInformation 的子元素）

此元素描述了应用的加载项。 **Product** 是 [ListingInformation](#listinginformation) 元素的可选子元素，它包含一个或多个 [MarketData](#marketdata-child-of-product) 元素。

**Product** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |    包含应用用来标识加载项的字符串。           |
|  **LicenseDuration**  |    否        |    指示已购买商品后，许可证有效的天数。 由产品购买创建的新许可证的过期日期是购买日期加许可证持续时间。 此属性仅在 **ProductType** 属性是 **Durable** 时使用；对于易耗型加载项，将忽略此属性。           |
|  **ProductType**  |    否        |    包含标识应用内产品的持久性的一个值。 受支持的值有 **Durable**（默认值）和 **Consumable**。 对于持久类型，其他信息由 [LicenseInformation](#licenseinformation) 下的 [Product](#product-child-of-licenseinformation) 元素描述；对于易耗类型，其他信息由 [ConsumableInformation](#consumableinformation) 下的 [Product](#product-child-of-consumableinformation) 元素描述。           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>MarketData 元素（Product 的子元素）

此元素提供有关给定国家/地区的加载项的信息。 对于在其中列出了加载项的每个国家/地区，必须包含 **MarketData** 元素。 **MarketData** 是 [Product](#product-child-of-listinginformation) 元素的必需子元素。

**MarketData** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  **Name**  |    是   |  1   |   在此国家/地区中加载项的名称。        |
|  **Price**  |    是  |  1   |     在此国家/地区中加载项的价格。        |
|  **CurrencySymbol**  |    是  |  1   |     在此国家/地区使用的货币符号。        |
|  **CurrencyCode**  |    否  |  0 或 1      |      在此国家/地区使用的货币代码。         |  
|  **Description**  |    否  |   0 或 1   |      在此国家/地区中加载项的描述。       |
|  **Tag**  |    否  |   0 或 1   |      加载项的[自定义开发人员数据](../publish/enter-add-on-properties.md#custom-developer-data)（也称为标记）。       |
|  **关键字**  |    否  |   0 或 1   |      包含最多 10 个 **Keyword** 元素，这些元素包含加载项的[关键字](../publish/enter-add-on-properties.md#keywords)。       |
|  **ImageUri**  |    否  |   0 或 1   |      加载项列表中[图像的 URI](../publish/create-add-on-store-listings.md#icon)。           |  |

**MarketData** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **xml:lang**  |    是        |     指定市场数据信息适用的国家/地区。          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>LicenseInformation 元素

此元素描述了可用于此应用及其持久型应用内产品的许可证。 **LicenseInformation** 是 **CurrentApp** 元素的必需子元素。

**LicenseInformation** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    是   |  1   |    描述应用的许可证。         |
|  [Product](#product-child-of-licenseinformation)  |    否  |  0 或更多   |      描述了应用中持久型加载项的许可证状态。         |   |

下表显示了如何通过组合 **App** 和 **Product** 元素下的值来模拟某些常见情形。

|  要模拟的条件  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  完全授权  |    true   |  false  |    不存在。 它实际上可能会出现并指定将来的日期，但我们建议你从 XML 文件省略该元素。 如果它存在并指定过去的日期，那么 **IsActive** 将被忽略并为 false。          |
|  在试用期内  |    true  |  true   |      &lt;将来的日期时间&gt; 此元素必须存在，因为 **IsTrial** 为 true。 你可以访问显示当前协调世界时 (UTC) 的网站，了解要设置的日期时间距离现在多久，以便获取想要的剩余试用期。         |
|  试用过期  |    false  |  true   |      &lt;过去的日期时间&gt; 此元素必须存在，因为 **IsTrial** 为 true。 你可以访问显示当前协调世界时 (UTC) 的网站，了解“过去”的 UTC 时间。         |
|  无效  |    false  | false       |     &lt;任何值或省略&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>App 元素（ListingInformation 的子元素）

此元素描述了应用的许可证。 **App** 是 [LicenseInformation](#licenseinformation) 元素的必需子元素。

**App** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  **IsActive**  |    是   |  1   |    描述了此应用的当前许可证状态。 值 **true** 指示许可证有效。**false** 指示许可证无效。 通常此值为 **true**，无论该应用是否具有试用模式。  将此值设置为 **false** 来测试你的应用在具有无效许可证时的行为。           |
|  **IsTrial**  |    是  |  1   |      描述此应用的当前试用状态。 值 **true** 指示应用在试用期内使用；**false** 指示应用不在试用期内，或者因为已购买应用，或者因为试用期已过。         |
|  **ExpirationDate**  |    否  |  0 或 1       |     此应用的过期日期，以协调世界时 (UTC) 表示。 该日期必须表示为：yyyy-mm-ddThh:mm:ss.ssZ。 例如，2015 年 1 月 19 日 05:00 将被指定为 2015-01-19T05:00:00.00Z。 当 **IsTrial** 是 **true** 时，此元素是必需的。 否则，不是必需的。          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Product 元素（LicenseInformation 的子元素）

此元素描述应用中持久型加载项的许可证状态。 **Product** 是 [LicenseInformation](#licenseinformation) 元素的可选子元素。

**Product** 包含以下子元素。

|  元素  |  必需  |  数量  | 描述   |
|-------------|------------|--------|--------|
|  **IsActive**  |    是   |  1     |    描述此加载项的当前许可证状态。 值 **true** 指示可以使用该加载项；**false** 指示不能使用或未购买该加载项           |
|  **ExpirationDate**  |    否   |  0 或 1     |     加载项的过期日期，以协调世界时 (UTC) 表示。 该日期必须表示为：yyyy-mm-ddThh:mm:ss.ssZ。 例如，2015 年 1 月 19 日 05:00 将被指定为 2015-01-19T05:00:00.00Z。 如果该元素存在，该加载项将具有一个过期日期。 如果不存在，该加载项不会到期。  |  

**Product** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |   包含应用用来标识加载项的字符串。            |
|  **OfferID**  |     否       |   包含应用用来标识加载项所属的类别的字符串。 这为大型项目目录提供了支持，如[管理应用内产品的大型目录](manage-a-large-catalog-of-in-app-products.md)所述。           |

<span id="simulation"/>

#### <a name="simulation-element"></a>模拟元素

此元素描述了在测试过程中对各种 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 方法的调用如何在应用中工作。 **Simulation** 是 **CurrentApp** 元素的可选子元素，它包含零个或多个 [DefaultResponse](#defaultresponse) 元素。

**Simulation** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **SimulationMode**  |    否        |      值可以是 **Interactive** 或 **Automatic**。 当此属性设置为 **Automatic** 时，这些方法将自动返回指定的 HRESULT 错误代码。 这可运行自动测试用例时使用。       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>DefaultResponse 元素

此元素描述了 **CurrentAppSimulator** 方法返回的默认错误代码。 **DefaultResponse** 是 [Simulation](#simulation) 元素的可选子元素。

**DefaultResponse** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **MethodName**  |    是        |   将此属性分配给为[架构](#schema)中的 **StoreMethodName** 类型显示的枚举值之一。 每个枚举值表示 **CurrentAppSimulator** 方法，你希望在测试期间为其模拟应用中的错误代码返回值。 例如，值 **RequestAppPurchaseAsync_GetResult** 指示你想要模拟 [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync) 方法的错误代码返回值。            |
|  **HResult**  |     是       |   将此属性分配给为[架构](#schema)中的 **ResponseCodes** 类型显示的枚举值之一。 每个枚举值表示你希望为方法返回的错误代码，该方法已分配给 **DefaultResponse** 元素的 **MethodName** 属性。           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>ConsumableInformation 元素

此元素描述了可用于此应用的易耗型加载项。 **ConsumableInformation** 是 **CurrentApp** 元素的可选子元素，它可以包含零个或多个 [Product](#product-child-of-consumableinformation) 元素。

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Product 元素（ConsumableInformation 的子元素）

此元素描述易耗型加载项。 **Product** 是 [ConsumableInformation](#consumableinformation) 元素的可选子元素。

**Product** 具有以下属性。

|  属性  |  必需  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |   包含应用用来标识易耗型加载项的字符串。            |
|  **TransactionId**  |     是       |   包含在实施过程中应用用于跟踪易耗品购买交易的 GUID（作为字符串）。 请参阅[启用易耗型应用内产品购买](enable-consumable-in-app-product-purchases.md)。            |
|  **状态**  |      是      |  包含应用用于指示易耗品的实施状态的字符串。 值可以是 **Active**、**PurchaseReverted**、**PurchasePending** 或 **ServerError**。             |
|  **OfferID**  |     否       |    包含应用用来标识易耗品所属的类别的字符串。 这为大型项目目录提供了支持，如[管理应用内产品的大型目录](manage-a-large-catalog-of-in-app-products.md)所述。           |
