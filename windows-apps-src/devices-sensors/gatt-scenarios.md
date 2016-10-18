---
author: msatranjr
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: "蓝牙 GATT"
description: "本文提供用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性配置文件 (GATT) API 概述，以及用于三个常见 GATT 方案的示例代码。"
translationtype: Human Translation
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: 508acd449c156fa0f5b14298e4a7700748fc65bb

---
# 蓝牙 GATT

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

本文提供用于通用 Windows 平台 (UWP) 应用的蓝牙通用属性配置文件 (GATT) 的概述，以及用于三个常见 GATT 方案的示例代码：检索蓝牙数据、控制蓝牙 LE 温度计设备，以及控制蓝牙 LE 设备数据的显示。

## 概述

开发人员可使用 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) 命名空间中的 API 访问蓝牙 LE 服务、描述符和特性。 蓝牙 LE 设备通过以下内容的集合公开其功能：

-   主要服务
-   所包含的服务
-   特性
-   描述符

主要服务定义 LE 设备的功能合约，并且包含定义该服务的特性的集合。 这些特性反过来包括描述特性的描述符。

蓝牙 GATT API 公开对象和函数，而不是对原始传输的访问权限。 在驱动程序级别上，使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 将主要服务枚举为蓝牙 LE 设备的子设备节点。

蓝牙 GATT API 还使开发人员可以在能够执行以下任务的情况下使用蓝牙 LE 设备：

-   执行服务/特性/描述符发现
-   读取并写入特性/描述符值
-   为 Characteristic ValueChanged 事件注册回调

蓝牙 GATT API 通过处理常用属性并提供合理的默认值简化了开发，以此来帮助设备管理和配置。 它们为开发人员提供了从应用访问蓝牙 LE 设备功能的方法。

要创建有用的实现，开发人员必须具有应用程序要使用的 GATT 服务和特性的前期知识，并且必须处理特性的特性值，以便由 API 提供的二进制数据在向用户呈现之前转换为有用的数据。 蓝牙 GATT API 仅公开与蓝牙 LE 设备进行通信所需的基本基元。 要解释数据，应用程序配置文件必须经蓝牙 SIG 标准配置文件定义，或经由设备供应商实现的自定义配置文件定义。 配置文件创建应用程序和设备之间的绑定合约，此合约关于交换的数据所代表的内容以及如何解释它。

为方便起见，蓝牙 SIG 将持续提供[公共配置文件](http://go.microsoft.com/fwlink/p/?LinkID=317977)的列表。

## 检索蓝牙数据

在本示例中，应用使用来自蓝牙设备的温度度量，可实现蓝牙 LE 健康温度计服务。 应用指定其希望在新的温度度量可用时收到通知。 通过为“温度计特征值更改”事件注册事件处理程序，应用将在前台运行时收到特征值更改事件通知。

请注意，当应用暂停时，它必须释放所有设备资源；并且当它返回时，它必须重新执行设备枚举和初始化。 如果需要在后台执行设备交互，请查看 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx) 或 [GattCharacteristicNotificationTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.gattcharacteristicnotificationtrigger.aspx)。 通常，DeviceUseTrigger 更适合处理高频事件；反之，GattCharacteristicNotificationTrigger 更适合处理低频率事件。  

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## 控制蓝牙 LE 温度计设备

在本示例中，UWP 应用充当虚拟蓝牙 LE 温度计设备的控制器。 除了 [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603) 配置文件的标准特性外，该设备还声明了允许用户检索以摄氏度或华氏度读取的值的格式特性。 该应用使用可靠的写入事务以确保格式和度量间隔设置为单个值。

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## 控制蓝牙 LE 设备数据的显示

蓝牙 LE 设备可能公开向用户提供的当前电量的电池服务。 电池服务包括可选的 [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742) 描述符，此描述符在解释电量数据时提供一些灵活性。 此方案提供一个应用示例，此应用与此类设备兼容，并且在向用户显示前使用 **PresentationFormats** 属性格式化特性值。

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```






<!--HONumber=Aug16_HO3-->


