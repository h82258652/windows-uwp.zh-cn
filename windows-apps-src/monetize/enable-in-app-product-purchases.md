---
author: mcleanbyron
Description: "无论你的应用是否免费，你都可以直接从应用中销售内容、其他应用或新的应用功能（例如解锁游戏的下一关）。 下面我们显示了如何在应用中启用这些产品。"
title: "启用应用内产品购买"
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: in-app offer code sample
ms.sourcegitcommit: bb28828463b14130deede9f7cf796c6e32fcb48b
ms.openlocfilehash: 2e9a011a248e4c7e1d3f06064a7f82e308f07131

---

# 启用应用内产品购买

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

无论你的应用是否免费，你都可以直接从应用中销售内容、其他应用或新的应用功能（例如解锁游戏的下一关）。 下面我们显示了如何在应用中启用这些产品。

> **注意** 应用的试用版不能提供应用内产品。 仅当使用试用版应用的客户购买了完整版应用后，他们才可以购买应用内产品。

## 先决条件

-   可添加供客户购买的功能的 Windows 应用。
-   首次编码和测试新应用内产品时，必须使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 对象而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 对象。 这样，你可以使用对许可证服务器的模拟调用验证许可证逻辑，而不是调用实时服务器。 若要实现此目的，需要在 %userprofile%\\AppData\\local\\packages\\&lt;程序包名称&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中自定义名为“WindowsStoreProxy.xml”的文件。 Microsoft Visual Studio 仿真器会在你首次运行应用时创建此文件，你也可以在运行时加载一个自定义文件。 有关详细信息，请参阅 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)。
-   本主题还参考了[应用商店示例](http://go.microsoft.com/fwlink/p/?LinkID=627610)中提供的代码示例。 若要获得为通用 Windows 平台 (UWP) 应用提供的不同货币化选项的实际体验，此示例是一个不错的选择。

## 步骤 1：为你的应用初始化许可证信息

当应用在执行初始化时，请通过初始化 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 或 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 获取应用的 [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) 对象，从而启用应用内产品的购买。

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## 步骤 2：向应用添加应用内付费内容

针对你希望通过应用内产品提供的每项功能，创建一个付费内容并将它添加到你的应用中。

> **重要提示** 向应用商店提交你的应用之前，必须将你希望呈现给客户的所有应用内产品都添加到应用中。 如果你稍后需要添加新的应用内产品，则必须更新应用并重新提交新版本。

1.  **创建应用内付费内容标记**

    通过标记来标识应用中的每个应用内产品。 此标记是一个字符串，在你的应用和应用商店中定义和使用它来标识特定的应用内产品。 为它提供一个独特（对于你的应用）且有意义的名称，以便你可以在编码时快速标识它所表示的正确功能。 下面是一些名称示例：

    -   “SpaceMissionLevel4”
    
    -   “ContosoCloudSave”
    
    -   “RainbowThemePack”

2.  **在条件块中编码功能**

    必须将与应用内产品关联的每个功能的代码都放在条件块中，从而进行测试以查看客户是否有使用该功能的许可证。

    下面的示例介绍了如何在特定于许可证的条件块中编码名为 **featureName** 的产品功能。 字符串 **featureName** 是在应用中唯一标识此产品的标记，它也用于在应用商店中标识此产品。

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
    {
        // the customer can access this feature
    } 
    else
    {
        // the customer can' t access this feature
    }
    ```

3.  **为此功能添加购买 UI**

    你的应用还必需为客户提供一种方式来购买应用内产品提供的产品或功能。 客户无法以购买完整应用的方式通过应用商店来购买上述内容。

    下面介绍如何测试，以查看客户是否已拥有应用内产品；如果尚未拥有，则显示购买对话框，让用户可以购买该内容。 将注释“显示购买对话框”替换为你的购买对话框自定义代码（如一个包含友好的“购买此应用！”按钮的页面） 。

    ```    CSharp
    void BuyFeature1() 
    {
        if (!licenseInformation.ProductLicenses["featureName"].IsActive)
        {
            try
            {
                // The customer doesn't own this feature, so 
                // show the purchase dialog.
                await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
        
                //Check the license state to determine if the in-app purchase was successful.
            }
            catch (Exception)
            {
                // The in-app purchase was not completed because 
                // an error occurred.
            }
        } 
        else
        {
            // The customer already owns this feature.
        }
    }
    ```

## 步骤 3：更改测试代码以达到最后要求

这是一个简单的步骤：在应用的代码中将对 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 的每个引用都更改为 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)。 你不再需要提供 WindowsStoreProxy.xml 文件，因此请将它从你的应用路径中删除（但是你可能希望保存它，以便在下一步中配置应用内付费内容时进行引用）。

## 步骤 4：在应用商店中配置应用内产品付费内容

在开发人员中心仪表板中为应用内产品定义产品 ID、类型、价格和其他属性。 请确保完全按照测试时在 WindowsStoreProxy.xml 中设置的配置来配置它。 有关详细信息，请参阅 [IAP 提交](https://msdn.microsoft.com/library/windows/apps/mt148551)。

## 备注

如果你对向客户提供可消费应用内产品选项（可购买、用完，然后根据需要再次购买的项目）感兴趣，请参阅[启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)主题。

如果你需要使用收据来验证用户是否进行了应用内购买，请务必查看[使用收据验证产品购买](use-receipts-to-verify-product-purchases.md)。

## 相关主题


* [启用可消费应用内产品购买](enable-consumable-in-app-product-purchases.md)
* [管理应用内产品的大目录](manage-a-large-catalog-of-in-app-products.md)
* [使用收据验证产品购买](use-receipts-to-verify-product-purchases.md)
* [应用商店示例（演示试用版和应用内购买）](http://go.microsoft.com/fwlink/p/?LinkID=627610)







<!--HONumber=Jun16_HO4-->


