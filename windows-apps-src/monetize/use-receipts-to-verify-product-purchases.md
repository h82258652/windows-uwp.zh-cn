---
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: 每个成功购买产品的 Microsoft Store 交易都可以选择返回交易收据。
title: 使用收据验证产品购买
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, 应用内购买, IAP, 收据, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: a26d98de58c954f1bec588b335483de08404862b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259226"
---
# <a name="use-receipts-to-verify-product-purchases"></a>使用收据验证产品购买

每个成功购买产品的 Microsoft Store 交易都可以选择返回交易收据。 此收据向客户提供所列出的产品和货币成本的信息。

有权访问此信息可支持以下方案：你的应用需要验证用户是否购买了你的应用，或者是否已从 Microsoft Store 进行了加载项（也称为应用内产品或 IAP）购买。 例如，请设想一个提供下载内容的游戏。 如果购买了该游戏内容的用户要在其他设备上玩这个游戏，则需要验证该用户是否已经拥有此项内容。 操作方法如下。

> [!IMPORTANT]
> 此文章将介绍如何使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空间的成员来获取和验证应用内购买的收据。 如果你使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) 命名空间进行应用内购买（在 Windows 10 版本 1607 中引入，可在 Visual Studio 中用于面向 **Windows 10 周年纪念版（10.0；版本 14393）** 或更高版本的项目），此命名空间不会提供 API 用于获取应用内购买的购买收据。 但是，你可以使用 Microsoft Store 集合 API 中的 REST 方法获取购买交易的数据。 有关详细信息，请参阅[应用内购买的收据](in-app-purchases-and-trials.md#receipts)。

## <a name="requesting-a-receipt"></a>索要收据


**Windows.ApplicationModel.Store** 命名空间支持多个方法来获取收据：

* 当通过使用 [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 或 [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)（或此方法的其他重载之一）进行购买时，返回值将包含收据。
* 可以调用 [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) 方法来检索应用的当前收据信息以及应用中的任何加载项。

应用收据如下所示。

> [!NOTE]
> 此示例的格式有助于提高 XML 的可读性。 实际应用收据元素之间不包含空格。

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

产品收据如下所示。

> [!NOTE]
> 此示例的格式有助于提高 XML 的可读性。 实际产品收据元素之间不包含空格。

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

你可以使用任一收据示例来测试自己的验证代码。 有关收据内容的详细信息，请参阅[元素和属性说明](#receipt-descriptions)。

## <a name="validating-a-receipt"></a>验证收据

若要验证收据的真实性，需要后端系统（Web 服务或类似系统）使用公钥证书检查收据的签名。 若要获取此证书，请使用 URL ```https://go.microsoft.com/fwlink/p/?linkid=246509&cid=CertificateId```，其中 ```CertificateId``` 是收据中的 **CertificateId** 值。

以下是该验证过程的一个示例。 此代码在 .NET Framework 控制台应用程序中运行，该应用程序包含对 **System.Security** 程序集的引用。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>收据的元素和属性说明

本部分介绍收据的元素和属性。

### <a name="receipt-element"></a>Receipt 元素

此文件的根元素是 **Receipt** 元素，其中包含应用和应用内购买的信息。 此元素包含以下子元素。

|  元素  |  必需  |  数量  |  描述   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    无        |  0 或 1  |  包含当前应用的购买信息。            |
|  [ProductReceipt](#productreceipt)  |     无       |  0 或更多    |   包含有关当前应用的应用内购买的信息。     |
|  签名  |      “是”      |  1   |   此元素是一种标准 [XML-DSIG 构造](https://www.w3.org/TR/xmldsig-core/)。 它包含 **SignatureValue** 元素（其中包含可用于验证收据的签名）和 **SignedInfo** 元素。      |

**Receipt** 具有以下必属性。

|  属性  |  描述   |
|-------------|-------------------|
|  **版本**  |    收据的版本号。            |
|  **CertificateId**  |     用于对收据进行签名的证书指纹。          |
|  **ReceiptDate**  |    收据签名和下载的日期。           |  
|  **ReceiptDeviceId**  |   标识用于请求此收据的设备。         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>AppReceipt 元素

此元素包含当前应用的购买信息。

**AppReceipt** 具有以下属性。

|  属性  |  描述   |
|-------------|-------------------|
|  **Id**  |    标识购买。           |
|  **AppId**  |     操作系统用于该应用的程序包系列名称值。           |
|  **LicenseType**  |    **完整**，如果用户购买了完整版本的应用。 **试用**，如果用户下载了应用的试用版。           |  
|  **PurchaseDate**  |    获得应用的日期。          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>ProductReceipt 元素

此元素包含有关当前应用的应用内购买信息。

**ProductReceipt** 具有以下属性。

|  属性  |  描述   |
|-------------|-------------------|
|  **Id**  |    标识购买。           |
|  **AppId**  |     标识应用，用户通过该应用进行购买。           |
|  **ProductId**  |     标识购买的产品。           |
|  **ProductType**  |    确定产品类型。 当前仅支持值 **Durable**。          |  
|  **PurchaseDate**  |    购买发生的日期。          |  |

 

 
