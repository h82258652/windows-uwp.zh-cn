---
title: 获取并了解磁条数据
description: 了解如何获取和解释磁条中的数据。
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10，uwp，点的服务、 pos、 磁条阅读器
ms.localizationpriority: medium
ms.openlocfilehash: 1805213c7c30ccbc67fb96098f11480703589bb4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651602"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>获取并了解磁条数据

一旦设置了应用程序中所述的步骤中你磁条阅读器[开始使用服务点](pos-basics.md)，已准备好开始从其获取数据。

## <a name="subscribe-to-datareceived-events"></a>订阅 * DataReceived 事件

每当读取器可以识别重击的卡，它会引发三个事件之一：

* [AamvaCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived):刷机动车辆卡时发生。
* [BankCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived):刷银行卡时发生。
* [VendorSpecificDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived):实现特定于供应商的卡时发生。

你的应用程序只需订阅事件支持的磁条阅读器。 您可以看到与支持哪些类型的卡[MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
)。

下面的代码演示的三个订阅 ***DataReceived**事件：

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

将传递的事件处理程序[ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)和一个*args*对象，其类型将事件而异：

* **AamvaCardDataReceived**事件：[MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived**事件：[MagneticStripeReaderBankCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived**事件：[MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>获取数据

有关**AamvaCardDataReceived**并**BankCardDataReceived**事件，可以获取直接来自数据的一些*args*对象。 下面的示例演示获取一些属性并将其分配给成员变量：

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

但是，一些数据，包括中的所有数据**VendorSpecificDataReceived**事件，必须通过检索**报表**对象，它是一个属性的*args*参数。 此项的类型[MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)。

可以使用[CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype)属性，找出重击哪些类型的卡，然后使用，以通知如何解释将数据从[Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)， [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)， [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)，并[Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)。

从每个跟踪的数据表示为[MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)对象。 此类可以获取以下类型的数据：

* [数据](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data):原始或已解码的数据。
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata):自定义的数据。 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata):加密的数据。

以下代码片段获取报表和跟踪数据，然后检查卡类型：

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅

* [磁条阅读器](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)