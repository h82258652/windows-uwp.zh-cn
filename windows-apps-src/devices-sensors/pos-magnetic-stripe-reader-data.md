---
author: eliotcowley
title: 获取并了解磁条数据
description: 了解如何获取和解释磁条中的数据。
ms.author: elcowle
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10，uwp，点的服务、 pos、 磁条阅读器
ms.localizationpriority: medium
ms.openlocfilehash: a130243fb5a77277e4e8a326316cd30bab2cd96e
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698918"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>获取并了解磁条数据

一旦你已设置好磁条阅读器应用程序使用[服务点入门](pos-basics.md)中概述的步骤中，便可以开始从其获取数据。

## <a name="subscribe-to-datareceived-events"></a>订阅 * DataReceived 事件

每当认识到了轻扫的卡读卡器，它将引发三个事件之一：

* [AamvaCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)： 刷电机车辆卡时发生。
* [BankCardDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)： 扫是银行卡时发生。
* [VendorSpecificDataReceived 事件](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived)： 扫特定于供应商的卡片时发生。

你的应用程序只需订阅受磁条阅读器的事件。 你可以看到的[MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
)支持哪些类型的卡。

以下代码演示了三个订阅 ***DataReceived**事件：

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

[ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)和*参数*对象，其类型将会有所不同，具体取决于该事件，将传递的事件处理程序：

* **AamvaCardDataReceived**事件： [MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived**事件： [MagneticStripeReaderBankCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived**事件： [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>获取数据

对于**AamvaCardDataReceived**和**BankCardDataReceived**事件中，你可以获取的一些数据直接从*参数*对象。 以下示例演示获取几个属性，并将它们分配给成员变量：

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

但是，某些数据，包括所有数据从**VendorSpecificDataReceived**事件，必须通过**报告**对象，它是*实参*形参的属性。 这是类型[MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)。

可以使用[CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype)属性来确定哪种类型的卡片具有扫，并使用，以通知如何解释[Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)、 [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)、 [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)，以及[Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)中的数据。

从每个轨的数据表示为[MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)对象。 从此类，你可以获取以下类型的数据：

* [数据](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data)： 原始或解码数据。
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata)： 自由数据。 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata)： 加密的数据。

以下代码段获取报告和跟踪数据，然后再检查卡片类型：

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

* [磁条读取器](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)