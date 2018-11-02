---
author: JordanRh1
title: 启用 GPIO、I2C 和 SPI 的用户模式访问
description: 本教程介绍如何对 Windows 10 上的 GPIO、I2C、SPI 和 UART 启用用户模式访问。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, acpi, gpio, i2c, spi, uefi
ms.assetid: 2fbdfc78-3a43-4828-ae55-fd3789da7b34
ms.localizationpriority: medium
ms.openlocfilehash: 09957c19414f586a49a1a2cb9186aa027dc1de07
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5947082"
---
# <a name="enable-usermode-access-to-gpio-i2c-and-spi"></a>启用 GPIO、I2C 和 SPI 的用户模式访问



Windows 10 包含的新 API 可用于直接通过用户模式访问 GPIO、I2C、SPI 和 UART。 开发板（如 Raspberry Pi 2）将公开这些连接的子集，这些连接支持用户使用自定义电路扩展基本计算模块来处理特定应用程序。 通常，只需使用 GPIO 引脚的一个子集和标头上公开的总线，即可与其他关键板载功能共享这些低级别总线。 若要保持系统稳定性，必须通过用户模式应用程序指定可供安全修改的引脚和总线。 

本文档介绍如何在 ACPI 中指定此配置，并提供工具来验证是否正确指定了配置。 

> [!IMPORTANT]
> 本文档适用于 UEFI 和 ACPI 开发人员。 假定略微熟悉 ACPI、ASL 编写和 SpbCx/GpioClx。

Windows 上低级别总线的用户模式访问通过现有 `GpioClx` 和 `SpbCx` 框架实现。 称为 *RhProxy*、在 Windows IoT 核心版和 Windows 企业版上可用的新驱动程序会向用户模式公开 `GpioClx` 和 `SpbCx` 资源。 若要启用这些 API，必须在 ACPI 表（内含应向用户模式公开的每个 GPIO 和 SPB 资源）中声明用于 RhProxy 的设备节点。 本文档演示了编写和验证 ASL。 


## <a name="asl-by-example"></a>ASL 示例

演示 Raspberry Pi 2 上的 RhProxy 设备节点声明。 首先，在 \\_SB 作用域中创建 ACPI 设备声明。  

```cpp
Device(RHPX) 
{ 
    Name(_HID, "MSFT8000") 
    Name(_CID, "MSFT8000") 
    Name(_UID, 1) 
    
```

* _HID – 硬件 ID。将它设置为特定于供应商的硬件 ID。 
* _CID – 兼容 ID。必须是“MSFT8000”。  
* _UID – 唯一 ID。设置为 1。  

接下来，声明应向用户模式公开的每个 GPIO 和 SPB 资源。 资源声明的顺序非常重要，因为使用资源索引将属性与资源关联起来。 如果存在多条公开的 I2C 或 SPI 总线，声明的第一条总线被视为该类型的“默认”总线，并且会是 [Windows.Devices.I2c.I2cController](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2ccontroller.aspx) 和 [Windows.Devices.Spi.SpiController](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spicontroller.aspx) 的 `GetDefaultAsync()` 方法返回的实例。 

### <a name="spi"></a>SPI 

Raspberry Pi 具有两条公开的 SPI 总线。 SPI0 有两条硬件芯片选择线，SPI1 有一条硬件芯片选择线。 每条总线的每条芯片选择线需要一个 SPISerialBus\(\) 资源声明。 以下两个 SPISerialBus 资源声明适用于 SPI0 上的两条芯片选择线。 DeviceSelection 字段包含驱动程序将其解释为硬件芯片选择线标识符的唯一值。 放入 DeviceSelection 字段中的确切值取决于驱动程序解释 ACPI 连接描述符的这一字段的方式。  

```cpp
// Index 0 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE0  - GPIO 8  - Pin 24 
    0,                     // Device selection (CE0) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

// Index 1 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE1  - GPIO 7  - Pin 26 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

软件如何知道这两个资源应该与同一总线关联？ 总线友好名称和资源索引之间的映射在 DSD 中指定：  

```cpp
Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }}, 
```

这将创建名为“SPI0”、具有两条芯片选择线的总线 – 资源索引 0 和 1。 若要声明 SPI 总线的功能，还需要更多属性。  

```cpp
Package(2) { "SPI0-MinClockInHz", 7629 }, 
Package(2) { "SPI0-MaxClockInHz", 125000000 },
```

**MinClockInHz** 和 **MaxClockInHz** 属性指定控制器所支持的最小和最大时钟速度。 API 会阻止用户指定超出此范围的值。 会将时钟速度传递给连接描述符（ACPI 章节 6.4.3.8.2.2）的 _SPE 字段中的 SPB 驱动程序。  

```cpp
Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }}, 
```

**SupportedDataBitLengths** 属性列出控制器所支持的数据位长度。 可以在用逗号分隔的列表中指定多个值。 API 会阻止用户指定不在此列表范围内的值。 会将数据位长度传递给连接描述符（ACPI 章节 6.4.3.8.2.2）的 _LEN 字段中的 SPB 驱动程序。  

你可以将这些资源声明视为“模板”。 某些字段在系统启动时是固定的，而其他字段在运行时是动态指定的。 SPISerialBus 描述符的以下字段是固定的： 

* DeviceSelection 
* DeviceSelectionPolarity 
* WireMode 
* SlaveMode 
* ResourceSource 

以下字段是由用户在运行时指定的值的占位符： 

* DataBitLength 
* ConnectionSpeed 
* ClockPolarity 
* ClockPhase 

由于 SPI1 仅包含一条芯片选择线，因此单个 `SPISerialBus()` 资源如下声明： 

```cpp
// Index 2 

SPISerialBus(              // SCKL - GPIO 21 - Pin 40 
                           // MOSI - GPIO 20 - Pin 38 
                           // MISO - GPIO 19 - Pin 35 
                           // CE1  - GPIO 17 - Pin 11 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

随附友好名称声明（这是必需的）在 DSD 中指定，并引用此资源声明的索引。 

```cpp
Package(2) { "bus-SPI-SPI1", Package() { 2 }}, 
```

这会创建名为“SPI1”的总线，并将其与资源索引 2 关联。  

#### <a name="spi-driver-requirements"></a>SPI 驱动程序要求 

* 必须使用 `SpbCx` 或与 SpbCx 兼容 
* 必须已通过 [MITT SPI 测试](https://msdn.microsoft.com/library/windows/hardware/dn919873.aspx)
* 必须支持 4Mhz 时钟速度 
* 必须支持 8 位数据长度 
* 必须支持所有 SPI 模式：0、1、2、3 

### <a name="i2c"></a>I2C 

接下来，声明 I2C 资源。 Raspberry Pi 公开了引脚 3 和 5 上的一条 I2C 总线。 

```cpp
// Index 3 
I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1) 
    0xFFFF,                // SlaveAddress: placeholder 
    ,                      // SlaveMode: default to ControllerInitiated 
    0,                     // ConnectionSpeed: placeholder 
    ,                      // Addressing Mode: placeholder 
    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name 
    , 
    , 
    )                      // VendorData 

```

随附友好名称声明（这是必需的）在 DSD 中指定： 

```cpp
Package(2) { "bus-I2C-I2C1", Package() { 3 }}, 
```

这将声明友好名称为“I2C1”、引用资源索引 3（它是以上声明的 I2CSerialBus() 资源的索引）的 I2C 总线。 

I2CSerialBus\(\) 描述符的以下字段是固定的： 

* SlaveMode 
* ResourceSource 

以下字段是由用户在运行时指定的值的占位符。 

* SlaveAddress 
* ConnectionSpeed 
* AddressingMode 

#### <a name="i2c-driver-requirements"></a>I2C 驱动程序要求 

* 必须使用 SpbCx 或与 SpbCx 兼容 
* 必须已通过 [MITT I2C 测试](https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx) 
* 必须支持 7 位寻址 
* 必须支持 100kHz 时钟速度 
* 必须支持 400kHz 时钟速度 

### <a name="gpio"></a>GPIO 

接下来，声明向用户模式公开的所有 GPIO 引脚。 提供以下用于确定要公开的引脚的指南： 

* 声明已公开标头上的所有引脚。 
* 声明连接到有用板载功能（如按钮和 LED）的引脚。 
* 不要声明为系统功能保留的引脚，也不要声明未连接任何内容的引脚。 

以下 ASL 块声明两个引脚 – GPIO4 和 GPIO5。 为了简明起见，此处未显示其他引脚。 附录 C 包含可以用于生成 GPIO 资源的 PowerShell 脚本示例。 

```cpp
// Index 4 – GPIO 4 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 4 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 4 } 

// Index 6 – GPIO 5 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 5 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 5 } 
```

声明 GPIO 引脚时，必须遵守以下要求： 

* 仅支持内存映射的 GPIO 控制器。 通过 I2C/SPI 交互的 GPIO 控制器不受支持。 如果控制器驱动程序在 [CLIENT_CONTROLLER_BASIC_INFORMATION](https://msdn.microsoft.com/library/windows/hardware/hh439358.aspx) 结构中设置 [MemoryMappedController](https://msdn.microsoft.com/library/windows/hardware/hh439449.aspx) 标志来响应 [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx) 回调，则它是内存映射的控制器。 
* 每个引脚都需要 GpioIO 和 GpioInt 资源。 GpioInt 资源必须紧跟着 GpioIO 资源，并且必须引用相同的引脚编号。 
* 必须按升序引脚编号对 GPIO 资源进行排序。 
* 每个 GpioIO 和 GpioInt 资源都必须恰好包含引脚列表中的一个引脚编号。 
* 这两个描述符的 ShareType 字段必须为“Shared” 
* GpioInt 描述符的 EdgeLevel 字段必须为“Edge” 
* GpioInt 描述符的 ActiveLevel 字段必须为ActiveBoth 
* PinConfig 字段 
  * 在 GpioIO 和 GpioInt 描述符中必须相同 
  * 必须是 PullUp、PullDown 或 PullNone 之中的一个。 它不能是 PullDefault。
  * 拉配置必须匹配引脚的通电状态。 将引脚从通电状态置于指定的拉模式不得改变引脚的状态。 例如，如果数据表指定引脚出现了一个上拉，请将PinConfig指定为PullUp。  

启动期间，固件、UEFI 和驱动程序初始化代码不应改变处于通电状态的引脚的状态。 只有用户清楚附加到引脚的内容，从而知道哪些状态转换是安全的。 必须记录每个引脚的通电状态，以便用户可以设计与引脚正确交互的硬件。 启动期间，引脚不得意外改变状态。  

#### <a name="supported-drive-modes"></a>支持的驱动器模式 

如果你的 GPIO 控制器除了支持内置的上拉和下拉电阻器之外，还支持高阻抗输入和 CMOS 输出，则必须使用可选 SupportedDriveModes 属性进行指定。 

```cpp
Package (2) { “GPIO-SupportedDriveModes”, 0xf }, 
```

SupportedDriveModes 属性指示哪些驱动器模式受 GPIO 控制器支持。 在上述示例中，以下所有驱动器模式都受支持。 属性是以下值的位掩码： 

| 标志值 | 驱动器模式 | 描述 |
|------------|------------|-------------|
| 0x1        | InputHighImpedance | 引脚支持高阻抗输入，对应于 ACPI 中的“PullNone”值。 |
| 0x2        | InputPullUp | 引脚支持内置的上拉电阻器，对应于 ACPI 中的“PullUp”值。 |
| 0x4        | InputPullDown | 引脚支持内置的下拉电阻器，对应于 ACPI 中的“PullDown”值。 |
| 0x8        | OutputCmos | 引脚支持生成很强的高电平和很强的低电平（相对于开漏）。 |

几乎所有 GPIO 控制器都支持 InputHighImpedance 和 OutputCmos。 如果未指定 SupportedDriveModes 属性，则这是默认设置。 

如果 GPIO 信号在到达公开的标头之前经过电平位移器，请声明受 SOC 支持的驱动器模式，即使在外部标头上观察不到驱动器模式也是如此。 例如，如果引脚经过使引脚表现为具有电阻式上拉的开漏的双向电平位移器，你将永远不会在公开的标头上观察到高阻抗状态，即使该引脚配置为高阻抗输入也是如此。 你仍应声明引脚支持高阻抗输入。 

#### <a name="pin-numbering"></a>引脚编号 

Windows 支持两种引脚编号方案： 

* 顺序引脚编号 - 用户看到诸如 0、1、2 ... 数字，一直到公开的引脚数。 0 是 ASL 中声明的第一个 GpioIo 资源，1 是 ASL 中声明的第二个 GpioIo 资源，依此类推。 
* 本机引脚编号 – 用户看到 GpioIo 描述符中指定的引脚编号，例如 4、5、12、13 …。  

```cpp
Package (2) { “GPIO-UseDescriptorPinNumbers”, 1 }, 
```

**UseDescriptorPinNumbers** 属性会告诉 Windows 使用本机引脚编号而不是顺序引脚编号。 如果未指定 UseDescriptorPinNumbers 属性或其值为零，Windows 将默认使用顺序引脚编号。 

如果使用本机引脚编号，还必须指定 **PinCount** 属性。 

```cpp
Package (2) { “GPIO-PinCount”, 54 }, 
```

**PinCount** 属性应与通过 `GpioClx` 驱动程序的 [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx) 回调中的 **TotalPins** 属性返回的值相匹配。 

选择与你的开发板的现有发布文档最兼容的编号方案。 例如，Raspberry Pi 使用本机引脚编号，因为许多现有的引出线图使用 BCM2835 引脚编号。 MinnowBoardMax 使用顺序引脚编号（因为有几个现有的引出线图），并且顺序引脚编号简化了开发人员体验（因为超过 200 个引脚中只公开了 10 个引脚）。 决定使用顺序引脚编号还是使用本机引脚编号应以减少开发人员混淆为目标。 

#### <a name="gpio-driver-requirements"></a>GPIO 驱动程序要求 

* 必须使用 `GpioClx`
* 必须是 SOC 上的内存映射 
* 必须使用模拟 ActiveBoth 中断处理 

### <a name="uart"></a>UART 

如果你的 UART 驱动程序使用 `SerCx` 或 `SerCx2`，则可以使用 rhproxy 向用户模式公开驱动程序。 创建类型为 `GUID_DEVINTERFACE_COMPORT` 的设备接口的 UART 驱动程序无需使用 rhproxy。 收件箱 `Serial.sys` 驱动程序是这些情况之一。

若要向用户模式公开 `SerCx` 样式的 UART，请按照如下方法声明 `UARTSerialBus` 资源。

```cpp
// Index 2 
UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2 
    115200,                // InitialBaudRate: in bits ber second 
    ,                      // BitsPerByte: default to 8 bits 
    ,                      // StopBits: Defaults to one bit 
    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled 
    ,                      // IsBigEndian: default to LittleEndian 
    ,                      // Parity: Defaults to no parity 
    ,                      // FlowControl: Defaults to no flow control 
    32,                    // ReceiveBufferSize 
    32,                    // TransmitBufferSize 
    "\\_SB.URT2",          // ResourceSource: UART bus controller name 
    , 
    , 
    , 
    )
```

仅 ResourceSource 字段是固定的，而所有其他字段是由用户在运行时指定的值的占位符。 

随附友好名称声明如下所示： 

```cpp
Package(2) { "bus-UART-UART2", Package() { 2 }}, 
```

这会将友好名称“UART2”分配给控制器，该名称是用户将用于从用户模式访问总线的标识符。  

## <a name="runtime-pin-muxing"></a>运行时引脚复用 

引脚复用可以将同一物理引脚用于不同功能。 多个不同的芯片上外设（例如 I2C 控制器、SPI 控制器和 GPIO 控制器）可以路由到 SOC 上的同一物理引脚。 复用块控制哪项功能在任何给定时间在引脚上处于活动状态。 通常，固件负责在启动时建立功能分配，此分配通过启动会话保持静态。 运行时引脚复用允许在运行时重新配置引脚功能分配。 支持用户在运行时选择引脚的功能可加速开发（方法是支持用户快速重新配置开发板的引脚），同时使硬件可以支持范围更广的应用程序（相较于静态配置而言）。 

无需编写任何其他代码，用户就可以使用对 GPIO、I2C、SPI 和 UART 的复用支持。 当用户使用 [OpenPin\(\)](https://msdn.microsoft.com/library/dn960157.aspx) 或 [FromIdAsync\(\)](https://msdn.microsoft.com/windows.devices.i2c.i2cdevice.fromidasync) 打开 GPIO 或总线时，基础物理引脚会自动复用为请求的功能。 如果其他功能已在使用该引脚，OpenPin\(\) 或 FromIdAsync\(\) 调用将失败。 当用户通过释放 [GpioPin](https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.gpiopin.aspx)、[I2cDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2cdevice.aspx)、[SpiDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spidevice.aspx) 或 [SerialDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.serialdevice.aspx) 对象关闭设备时，会释放引脚，从而允许它们稍后供其他功能打开。 

Windows 在 [GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439515.aspx)、[SpbCx](https://msdn.microsoft.com/library/windows/hardware/hh406203.aspx) 和 [SerCx](https://msdn.microsoft.com/library/windows/hardware/dn265349.aspx) 框架中包含对引脚复用的内置支持。 当访问 GPIO 引脚或总线时，这些框架协同工作，以自动将引脚切换到正确的功能。 仲裁对引脚的访问，以防止在多个客户端之间发生冲突。 除了此内置支持，适用于引脚复用的接口和协议是通用的，可以进行扩展以支持其他设备和方案。 

本文档首先介绍引脚复用所涉及的基础接口和协议，然后介绍如何将对引脚复用的支持添加到 GpioClx、SpbCx 和 SerCx 控制器驱动程序。 

### <a name="pin-muxing-architecture"></a>引脚复用体系结构 

本部分介绍引脚复用所涉及的基础接口和协议。 支持 GpioClx/SpbCx/SerCx 驱动程序的引脚复用不一定非要了解基础协议的知识。 有关如何支持 GpioCls/SpbCx/SerCx 驱动程序的引脚复用的详细信息，请参阅[在 GpioClx 客户端驱动程序中实现引脚复用支持](#supporting-muxing-support-in-gpioclx-client-drivers)和[在 SpbCx 和 SerCx 控制器驱动程序中使用复用支持](#supporting-muxing-in-spbcx-and-sercx-controller-drivers)。 

通过多个组件的协作实现引脚复用。 

* 引脚复用服务器 – 这些是控制引脚复用控制块的驱动程序。 引脚复用服务器通过请求保留复用资源（通过 *IRP_MJ_CREATE* 请求）和请求切换引脚的功能（通过 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 请求），从客户端接收引脚复用请求。 引脚复用服务器通常是 GPIO 驱动程序，因为复用块有时是 GPIO 块的一部分。 即使复用块是单独的外设，GPIO 驱动程序也是用于放置复用功能的逻辑位置。 
* 引脚复用客户端 – 这些是使用引脚复用的驱动程序。 引脚复用客户端从 ACPI 固件接收引脚复用资源。 引脚复用资源是一种连接资源，受资源中心管理。 引脚复用客户端通过打开资源的句柄来保留引脚复用资源。 若要使硬件更改生效，客户端必须通过发送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 请求来提交配置。 客户端通过关闭句柄释放引脚复用资源，其中点复用配置会还原为其默认状态。 
* ACPI 固件 – 指定具有 `MsftFunctionConfig()` 资源的复用配置。 MsftFunctionConfig 资源表示的管脚中具有客户端所需的复用配置。 MsftFunctionConfig 资源包含功能编号、拉配置和管脚编号列表。 MsftFunctionConfig 资源提供给管脚复用客户端作为硬件资源，驱动程序在其 PrepareHardware 回调中接收这些资源（类似于 GPIO 和 SPB 连接资源）。 客户端接收可用于打开资源句柄的资源中心 ID。 

> 必须将 `/MsftInternal` 命令行开关传递到 `asl.exe`，才能编译包含 `MsftFunctionConfig()` 描述符的 ASL 文件，因为 ACPI 工作委员会当前正在审查这些描述符。 例如： `asl.exe /MsftInternal dsdt.asl`

引脚复用中涉及的操作顺序如下所示。 

![引脚复用客户端服务器交互](images/usermode-access-diagram-1.png)

1.  客户端在其 [EvtDevicePrepareHardware\(\)](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx) 回调中从 ACPI 固件接收 MsftFunctionConfig 资源。
2.  客户端使用资源中心帮助程序函数 `RESOURCE_HUB_CREATE_PATH_FROM_ID()` 从资源 ID 创建路径，然后打开路径句柄（使用 [ZwCreateFile\(\)](https://msdn.microsoft.com/library/windows/hardware/ff566424.aspx)、[IoGetDeviceObjectPointer\(\)](https://msdn.microsoft.com/library/windows/hardware/ff549198.aspx) 或 [WdfIoTargetOpen\(\)](https://msdn.microsoft.com/library/windows/hardware/ff548634.aspx)）。
3.  服务器使用资源中心帮助程序函数 `RESOURCE_HUB_ID_FROM_FILE_NAME()` 从文件路径提取资源中心 ID，然后查询资源中心以获取资源描述符。
4.  服务器为描述符中的每个引脚执行共享仲裁，然后完成 IRP_MJ_CREATE 请求。
5.  客户端在收到的句柄上发出 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 请求。
6.  为响应 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*，服务器通过使每个引脚上的特定功能处于活动状态来执行硬件复用操作。
7.  客户端将继续执行取决于所请求的引脚复用配置的操作。
8.  当客户端不再需要复用引脚时，它会关闭句柄。
9.  为响应要关闭的句柄，服务器会将引脚恢复回其初始状态。

### <a name="protocol-description-for-pin-muxing-clients"></a>引脚复用客户端的协议描述

本部分介绍了客户端如何使用引脚复用功能。 这不适用于 `SerCx` 和 `SpbCx` 控制器驱动程序，因为框架会代表控制器驱动程序实现此协议。

####    <a name="parsing-resources"></a>解析资源

WDF 驱动程序在其 [EvtDevicePrepareHardware\(\)](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx) 例程中接收 `MsftFunctionConfig()` 资源。 MsftFunctionConfig 资源可以由以下字段来标识：

```cpp
CM_PARTIAL_RESOURCE_DESCRIPTOR::Type = CmResourceTypeConnection
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Class = CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Type = CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG
```

`EvtDevicePrepareHardware()` 例程可能会提取 MsftFunctionConfig 资源，如下所示：

```cpp
EVT_WDF_DEVICE_PREPARE_HARDWARE evtDevicePrepareHardware;

_Use_decl_annotations_
NTSTATUS
evtDevicePrepareHardware (
    WDFDEVICE WdfDevice,
    WDFCMRESLIST ResourcesTranslated
    )
{
    PAGED_CODE();

    LARGE_INTEGER connectionId;
    ULONG functionConfigCount = 0;

    const ULONG resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);
    for (ULONG index = 0; index < resourceCount; ++index) {
        const CM_PARTIAL_RESOURCE_DESCRIPTOR* resDescPtr =
            WdfCmResourceListGetDescriptor(ResourcesTranslated, index);

        switch (resDescPtr->Type) {
        case CmResourceTypeConnection:
            switch (resDescPtr->u.Connection.Class) {
            case CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG:
                switch (resDescPtr->u.Connection.Type) {
                case CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG:                    
                    switch (functionConfigCount) {
                    case 0:
                        // save the connection ID
                        connectionId.LowPart = resDescPtr->u.Connection.IdLowPart;
                        connectionId.HighPart = resDescPtr->u.Connection.IdHighPart;
                        break;
                    } // switch (functionConfigCount)
                    ++functionConfigCount;
                    break; // CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG

                } // switch (resDescPtr->u.Connection.Type)
                break; // CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
            } // switch (resDescPtr->u.Connection.Class)
            break;
        } // switch
    } // for (resource list)

    if (functionConfigCount < 1) {
        return STATUS_INVALID_DEVICE_CONFIGURATION;
    }
    // TODO: save connectionId in the device context for later use

    return STATUS_SUCCESS;
}
```

####    <a name="reserving-and-committing-resources"></a>保留和提交资源

当客户端想要复用管脚时，它会保留并提交 MsftFunctionConfig 资源。 以下示例演示客户端会如何保留并提交 MsftFunctionConfig 资源。

```cpp
_IRQL_requires_max_(PASSIVE_LEVEL)
NTSTATUS AcquireFunctionConfigResource (
    WDFDEVICE WdfDevice,
    LARGE_INTEGER ConnectionId,
    _Out_ WDFIOTARGET* ResourceHandlePtr
    )
{
    PAGED_CODE();

    //
    // Form the resource path from the connection ID
    //
    DECLARE_UNICODE_STRING_SIZE(resourcePath, RESOURCE_HUB_PATH_CHARS);
    NTSTATUS status = RESOURCE_HUB_CREATE_PATH_FROM_ID(
            &resourcePath,
            ConnectionId.LowPart,
            ConnectionId.HighPart);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    
    //
    // Create a WDFIOTARGET
    //
    WDFIOTARGET resourceHandle;
    status = WdfIoTargetCreate(WdfDevice, WDF_NO_ATTRIBUTES, &resourceHandle);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Reserve the resource by opening a WDFIOTARGET to the resource
    //
    WDF_IO_TARGET_OPEN_PARAMS openParams;
    WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(
        &openParams,
        &resourcePath,
        FILE_GENERIC_READ | FILE_GENERIC_WRITE);

    status = WdfIoTargetOpen(resourceHandle, &openParams);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    //
    // Commit the resource
    //
    status = WdfIoTargetSendIoctlSynchronously(
            resourceHandle,
            WDF_NO_HANDLE,      // WdfRequest
            IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS,
            nullptr,            // InputBuffer
            nullptr,            // OutputBuffer
            nullptr,            // RequestOptions
            nullptr);           // BytesReturned
    
    if (!NT_SUCCESS(status)) {
        WdfIoTargetClose(resourceHandle);
        return status;
    }

    //
    // Pins were successfully muxed, return the handle to the caller
    //
    *ResourceHandlePtr = resourceHandle;
    return STATUS_SUCCESS;
}
```

驱动程序应该在它的其中一个上下文区域中存储 WDFIOTARGET，以便它可以稍后进行关闭。 当驱动程序准备释放复用配置时，它应该通过调用 [WdfObjectDelete\(\)](https://msdn.microsoft.com/library/windows/hardware/ff548734.aspx) 或 [WdfIoTargetClose\(\)](https://msdn.microsoft.com/library/windows/hardware/ff548586.aspx) 来关闭资源句柄（如果你打算重新使用 WDFIOTARGET）。

```cpp
    WdfObjectDelete(resourceHandle);
```

当客户端关闭其资源句柄时，引脚将复用回其初始状态，现在可以由其他客户端获取。

### <a name="protocol-description-for-pin-muxing-servers"></a>引脚复用服务器的协议描述

本部分介绍引脚复用服务器如何向客户端公开其功能。 这不适用于 `GpioClx` 微型端口驱动程序，因为框架会代表客户端驱动程序实现此协议。 有关如何在 `GpioClx` 客户端驱动程序中支持引脚复用的详细信息，请参阅[在 GpioClx 客户端驱动程序中实现复用支持](#supporting-muxing-support-in-gpioclx-client-drivers)。

####    <a name="handling-irpmjcreate-requests"></a>处理 IRP_MJ_CREATE 请求

当客户端想要保留引脚复用资源时，它们会打开资源句柄。 引脚复用服务器通过资源中心的重分析操作来接收 *IRP_MJ_CREATE* 请求。 *IRP_MJ_CREATE* 请求的尾随路径组件包含资源中心 ID，该 ID 是十六进制格式的 64 位整数。 服务器应使用 reshub.h 中的 `RESOURCE_HUB_ID_FROM_FILE_NAME()` 从文件名中提取资源中心 ID，然后向资源中心发送 *IOCTL_RH_QUERY_CONNECTION_PROPERTIES* 以获取 `MsftFunctionConfig()` 描述符。

服务器应该验证描述符，然后从描述符中提取共享模式和引脚列表。 接下来，应该对引脚执行共享仲裁，在完成请求之前，如果成功，将该引脚标记为保留。

如果对引脚列表中的每个引脚成功执行共享仲裁，则共享仲裁总体成功。 应按如下方式对每个引脚进行仲裁：

*   如果尚未保留引脚，则共享仲裁成功。
*   如果引脚已保留为独占，则共享仲裁失败。
*   在引脚已保留为共享时，
  * 如果传入的请求是共享的，则共享仲裁成功。
  * 如果传入的请求是独占的，则共享仲裁失败。

如果共享仲裁失败，应使用 *STATUS_GPIO_INCOMPATIBLE_CONNECT_MODE* 完成请求。 如果共享仲裁成功，应使用 *STATUS_SUCCESS* 完成请求。

请注意，应从 MsftFunctionConfig 描述符而不是从 [IrpSp-&gt;Parameters.Create.ShareAccess](https://msdn.microsoft.com/library/windows/hardware/ff548630.aspx) 中获取传入请求的共享模式。

####    <a name="handling-ioctlgpiocommitfunctionconfigpins-requests"></a>处理 IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 请求

在客户端通过打开句柄成功保留了 MsftFunctionConfig 资源后，它可以发送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 以请求服务器执行实际的硬件复用操作。 当服务器接收 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 时，对于引脚列表中的每个引脚，它应该 

*   将在 PNP_FUNCTION_CONFIG_DESCRIPTOR 结构的 PinConfiguration 成员中指定的拉模式设置到硬件。
*   将引脚复用到由 PNP_FUNCTION_CONFIG_DESCRIPTOR 结构的 FunctionNumber 成员指定的功能。

然后服务器应使用 *STATUS_SUCCESS* 完成请求。

FunctionNumber 的含义由服务器定义，据了解，MsftFunctionConfig 描述符是运用了服务器如何解释此字段这一方面的知识撰写的。

请记住，当关闭该句柄时，服务器必须将引脚恢复为收到 IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 时引脚所使用的配置，因此服务器在修改引脚之前，可能需要保存这些引脚的状态。

####    <a name="handling-irpmjclose-requests"></a>处理 IRP_MJ_CLOSE 请求

当客户端不再需要复用资源时，它会关闭其句柄。 当服务器接收 *IRP_MJ_CLOSE* 请求时，它应该将引脚恢复为收到 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 时引脚所处的状态。 如果客户端从未发送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*，则无需执行任何操作。 接下来，服务器应该在执行共享仲裁后将引脚标记为可用，然后使用 *STATUS_SUCCESS* 完成请求。 请确保正确同步 *IRP_MJ_CLOSE* 处理与 *IRP_MJ_CREATE* 处理。

### <a name="authoring-guidelines-for-acpi-tables"></a>ACPI 表的编写指南

本部分介绍如何将复用资源提供给客户端驱动程序。 请注意，你需要 Microsoft ASL 编译器版本 14327 或更高版本来编译包含 `MsftFunctionConfig()` 资源的表。 `MsftFunctionConfig()` 资源提供给引脚复用客户端作为硬件资源。 `MsftFunctionConfig()` 应将资源提供给需要更改引脚复用的驱动程序，这些驱动程序通常是 SPB 和串行控制器驱动程序；但不应将资源提供给 SPB 和串行外设驱动程序，因为该控制器驱动程序将处理复用配置。
`MsftFunctionConfig()` ACPI 宏按如下方式定义：

```cpp
  MsftFunctionConfig(Shared/Exclusive
                PinPullConfig,
                FunctionNumber,
                ResourceSource,
                ResourceSourceIndex,
                ResourceConsumer/ResourceProducer,
                VendorData) { Pin List }

```

* 共享/独占 – 如果独占，则每次只有一个客户端可以获取此引脚。 如果共享，则多个共享客户端可以获取该资源。 始终将它设置为独占，因为允许多个未协调的客户端访问可变资源可能导致数据竞争，从而导致不可预知的结果。 
* PinPullConfig – 其中之一 
  * PullDefault – 使用 SOC 定义的通电默认拉配置 
  * PullUp – 启用上拉电阻器 
  * PullDown – 启用下拉电阻器 
  * PullNone – 禁用所有拉电阻器 
* FunctionNumber – 要编程进复用的功能编号。 
* ResourceSource – 引脚复用服务器的 ACPI 命名空间路径 
* ResourceSourceIndex – 将它设置为 0 
* ResourceConsumer/ResourceProducer – 将它设置为 ResourceConsumer 
* VendorData – 可选二进制数据，它的含义由引脚复用服务器进行定义。 通常它应该保留为空。
* 引脚列表 – 配置所应用到的引脚编号的逗号分隔列表。 当引脚复用服务器为 GpioClx 驱动程序时，这些是 GPIO 引脚编号，并且所具有的含义与 GpioIo 描述符中的引脚编号相同。 

以下示例演示一个管脚复用服务器可能会如何将 MsftFunctionConfig() 资源提供给 I2C 控制器驱动程序。 

```cpp
Device(I2C1) 
{ 
    Name(_HID, "BCM2841") 
    Name(_CID, "BCMI2C") 
    Name(_UID, 0x1) 
    Method(_STA) 
    { 
        Return(0xf) 
    } 
    Method(_CRS, 0x0, NotSerialized) 
    { 
        Name(RBUF, ResourceTemplate() 
        { 
            Memory32Fixed(ReadWrite, 0x3F804000, 0x20) 
            Interrupt(ResourceConsumer, Level, ActiveHigh, Shared) { 0x55 } 
            MsftFunctionConfig(Exclusive, PullUp, 4, "\\_SB.GPI0", 0, ResourceConsumer, ) { 2, 3 } 
        }) 
        Return(RBUF) 
    } 
} 
```

除了控制器驱动程序通常所需的内存和中断资源，还要指定 `MsftFunctionConfig()` 资源。 此资源允许 I2C 控制器驱动程序将引脚 2 和 3（由 \\_SB.GPIO0 处的设备节点进行管理）置于已启用上拉电阻器的功能 4 中。 

## <a name="supporting-muxing-support-in-gpioclx-client-drivers"></a>在 GpioClx 客户端驱动器中支持复用支持 

`GpioClx` 具有对引脚复用的内置支持。 GpioClx 微型端口驱动程序（也称为“GpioClx 客户端驱动程序”），驱动 GPIO 控制器硬件。 从 Windows 10 内部版本 14327 开始，GpioClx 微型端口驱动程序可以通过实现两个新的 DDI 添加对引脚复用的支持： 

* CLIENT_ConnectFunctionConfigPins – 由 `GpioClx` 调用以命令微型端口驱动程序应用指定的复用配置。 
* CLIENT_DisconnectFunctionConfigPins – 由 `GpioClx` 调用以命令微型端口驱动程序恢复复用配置。 

有关这些例程的描述，请参阅 [GpioClx 事件回调函数](https://msdn.microsoft.com/library/windows/hardware/hh439464.aspx)。

除了这两个新的 DDI，应针对引脚复用兼容性审核现有 DDI： 

* CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt – CLIENT_ConnectIoPins 由 GpioClx 调用以命令微型端口驱动程序配置一组用于 GPIO 输入或输出的引脚。 GPIO 与 MsftFunctionConfig 是互斥的，这意味着永远不会同时为 GPIO 和 MsftFunctionConfig 连接管脚。 由于引脚的默认功能不需要成为 GPIO，因此调用 ConnectIoPins 时，引脚未必不会复用为 GPIO。 需要调用 ConnectIoPins，才可以执行使引脚可供 GPIO IO 使用的所有必要操作，包括复用操作。 *CLIENT_ConnectInterrupt* 应具备类似行为，因为中断可以视为 GPIO 输入的一种特殊情况。 
* CLIENT_DisconnectIoPins/CLIENT_DisconnectInterrupt – 这些例程应该将引脚返回到调用 CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt 时引脚所处的状态，除非指定了 PreserveConfiguration 标志。 除了将引脚的方向恢复为其默认状态之外，微型端口还应该将每个引脚的复用状态恢复为调用 _Connect 例程时引脚所处的状态。 

例如，假设引脚的默认复用配置为 UART，则也可以将引脚用作 GPIO。 调用 CLIENT_ConnectIoPins 以连接用于 GPIO 的引脚时，它应该将该引脚复用为 GPIO；在 CLIENT_DisconnectIoPins 中，它应该将该引脚复用回 UART。 一般情况下，_Disconnect 例程应撤消 _Connect 例程执行的操作。 

## <a name="supporting-muxing-in-spbcx-and-sercx-controller-drivers"></a>在 SpbCx 和 SerCx 控制器驱动程序中支持复用 

从 Windows 10 内部版本 14327 开始，`SpbCx` 和 `SerCx` 框架包含对引脚复用的内置支持，这允许在无需更改 `SpbCx` 和 `SerCx` 控制器驱动程序自身的任何代码的情况下，就可以使这些控制器驱动程序成为引脚复用客户端。 通过扩展，连接到支持复用的 SpbCx/SerCx 控制器驱动程序的任何 SpbCx/SerCx 外设驱动程序将触发引脚复用活动。 

下图显示了每个组件之间的依存关系。 如你所见，引脚复用将依存关系从 SerCx 和 SpbCx 控制器驱动程序引入了 GPIO 驱动程序，它通常负责复用。 

![引脚复用依存关系](images/usermode-access-diagram-2.png)

在设备初始化期间，`SpbCx` 和 `SerCx` 框架会解析作为硬件资源提供给设备的所有 `MsftFunctionConfig()` 资源。 然后 SpbCx/SerCx 按需获取和释放引脚复用资源。

`SpbCx` 仅在调用客户端驱动程序的 [EvtSpbTargetConnect\(\)](https://msdn.microsoft.com/library/windows/hardware/hh450818.aspx) 回调之前，在其 *IRP_MJ_CREATE* 处理程序中应用引脚复用配置。 如果无法应用复用配置，将不会调用控制器驱动程序的 `EvtSpbTargetConnect()` 回调。 因此，SPB 控制器驱动程序可能会假设在调用 `EvtSpbTargetConnect()` 时，引脚会复用为 SPB 功能。

`SpbCx` 仅在调用控制器驱动程序的 [EvtSpbTargetDisconnect\(\)](https://msdn.microsoft.com/library/windows/hardware/hh450820.aspx) 回调之后，在其 *IRP_MJ_CLOSE* 处理程序中恢复引脚复用配置。 结果是，每当外设驱动程序打开 SPB 控制器驱动程序的句柄时，引脚就会复用为 SPB 功能；当外设驱动程序关闭其句柄时，会复用回引脚。

`SerCx` 类似行为。 `SerCx` 仅在调用控制器驱动程序的 [EvtSerCx2FileOpen\(\)](https://msdn.microsoft.com/library/windows/hardware/dn265209.aspx) 回调之前，在其 *IRP_MJ_CREATE* 处理程序中获取所有 `MsftFunctionConfig()` 资源；仅在调用控制器驱动程序的 [EvtSerCx2FileClose](https://msdn.microsoft.com/library/windows/hardware/dn265208.aspx) 回调之后，在其 IRP_MJ_CLOSE 处理程序中释放所有资源。

适用于 `SerCx` 和 `SpbCx` 控制器驱动程序的动态引脚复用的含义就是：它们必须能够容忍在某些时候从 SPB/UART 功能复用回引脚。 控制器驱动程序需要假设：在调用 `EvtSpbTargetConnect()` 或 `EvtSerCx2FileOpen()` 之前，不会复用引脚。 在以下回调期间，引脚不必复用为 SPB/UART 功能。 以下列表虽然不完整，但呈现了控制器驱动程序所实现的最常用 PNP 例程。

* DriverEntry 
* EvtDriverDeviceAdd 
* EvtDevicePrepareHardware/EvtDeviceReleaseHardware 
* EvtDeviceD0Entry/EvtDeviceD0Exit 

## <a name="verification"></a>验证 

当你准备好测试 rhproxy 时，使用下面的分步过程将很有帮助。

1. 验证是否每个 `SpbCx`、`GpioClx` 和 `SerCx` 控制器都正确加载和运行
1. 验证系统中是否存在 `rhproxy`。 有些 Windows 版本没有此功能。
1. 编译并加载使用的 rhproxy 节点 `ACPITABL.dat`
1. 验证 `rhproxy` 设备节点是否存在
1. 验证 `rhproxy` 是否加载和启动
1. 验证是否向用户模式开放了预期设备
1. 验证你能否从命令行与每台设备进行交互
1. 验证你能否从 UWP 应用与每台设备进行交互
1. 运行 HLK 测试

### <a name="verify-controller-drivers"></a>验证控制器驱动程序

由于 rhproxy 将系统上的其他设备向用户模式公开，因此它仅在这些设备运行时工作。 第一步是验证这些设备（你希望公开的 I2C、SPI、GPIO 控制器）是否已经运行。

在命令提示符处，运行
```
devcon status *
```

查看输出并验证所有关注的设备是否都已启动。 如果设备出现问题代码，你需要找出设备未加载的原因。 所有设备均应在平台初始启动期间已启用。 `SpbCx`、`GpioClx` 或 `SerCx` 控制器驱动程序的疑难解答已超出本文的介绍范围。

### <a name="verify-that-rhproxy-is-present-on-the-system"></a>验证系统中是否存在 rhproxy

验证系统中是否存在 `rhproxy` 服务。

```
reg query HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\rhproxy
```

如果此注册表项不存在，那么你的系统上就没有 rhproxy。 所有 IoT 核心版的内部版本和 Windows 企业版版本 15063 及更高版本均有 Rhproxy。

### <a name="compile-and-load-asl-with-acpitabldat"></a>使用 ACPITABL.dat 编译和加载 ASL 

现在你已创作了 rhproxy ASL 节点，应该编译并加载它了。 你可以将 rhproxy 节点编译为可附加到系统 ACPI 表的独立 AML 文件。 或者，如果你有权访问系统的 ACPI 源，则可以将 rhproxy 节点直接插入平台的 ACPI 表中。 但是，在初始启动时，使用 `ACPITABL.dat` 可能更方便。

1. 创建名为“yourboard.asl”的文件，然后将 RHPX 设备节点放入 DefinitionBlock 中： 
```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
        ...
        }
    }
}
```
2.  在以下位置下载 [WDK](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk) 并找到 `asl.exe` `C:\Program Files (x86)\Windows Kits\10\Tools\x64\ACPIVerify`
3.  运行以下命令以生成 ACPITABL.dat：
```
asl.exe yourboard.asl
```
4.  将生成的 ACPITABL.dat 文件复制到在测系统上的 c:\windows\system32。
5.  在在测系统上打开 testsigning：
```
bcdedit /set testsigning on
```
6.  重启在测系统： 系统会将 ACPITABL.dat 中定义的 ACPI 表附加到系统固件表。

### <a name="verify-that-the-rhproxy-device-node-exists"></a>验证 rhproxy 设备节点是否存在

运行以下命令以枚举 rhproxy 设备节点。
```
devcon status *msft8000
```
Devcon 的输出应指示设备存在。 如果设备节点未显示，表示 ACPI 表未成功添加到系统。

### <a name="verify-that-rhproxy-is-loading-and-starting"></a>验证 rhproxy 是否加载和启动

检查 rhproxy 的状态：
```
devcon status *msft8000
```
如果输出指示 rhproxy 已启动，则 rhproxy 已成功加载并启动。 如果你看到问题代码，则需要进行调查。 一些常见的问题代码有：
* 问题 51 - `CM_PROB_WAITING_ON_DEPENDENCY` - 系统未启动 rhproxy，因为它的依赖项之一加载失败。 这表明传递到 rhproxy 的资源指向无效的 ACPI 节点，或目标设备未启动。 首先，请仔细检查是否所有设备都成功运行（请参阅上方的“验证控制器驱动程序”）。 然后，仔细检查 ASL 并确保所有资源路径（例如，`\_SB.I2C1`）是正确的并指向 DSDT 中的有效节点。
* 问题 10 - `CM_PROB_FAILED_START` - Rhproxy 启动失败，很可能是由于资源解析问题。 仔细检查 ASL 以及 DSD 中的资源索引，并验证 GPIO 资源是否按照引脚编号的升序顺序指定。

### <a name="verify-that-the-expected-devices-are-exposed-to-usermode"></a>验证是否向用户模式开放了预期设备

现在 rhproxy 已经运行，它应该已经创建了可由用户模式访问的设备接口。 我们将使用几种命令行工具来枚举设备，并查看它们是否显示。

克隆[https://github.com/ms-iot/samples](https://github.com/ms-iot/samples)存储库并生成`GpioTestTool`， `I2cTestTool`， `SpiTestTool`，并且`Mincomm`示例。 将工具复制到正在测试的设备，并使用以下命令枚举设备。
```
I2cTestTool.exe -list
SpiTestTool.exe -list
GpioTestTool.exe -list
MinComm.exe -list
```
你应该看到列出了你的设备和友好名称。 如果你看不到正确的设备和友好名称，请仔细检查你的 ASL。

### <a name="verify-each-device-on-the-command-line"></a>验证命令行中的每个设备

下一步是使用命令行工具打开设备并与之交互。

I2CTestTool 示例：
```
I2cTestTool.exe 0x55 I2C1
> write {1 2 3}
> read 3
> writeread {1 2 3} 3
```

SpiTestTool 示例：
```
SpiTestTool.exe -n SPI1
> write {1 2 3}
> read 3
```

GpioTestTool 示例：
```
GpioTestTool.exe 12
> setdrivemode output
> write 0
> write 1
> setdrivemode input
> read
> interrupt on
> interrupt off
```

MinComm (serial) 示例。 运行之前将 Rx 连接到 Tx：
```
MinComm "\\?\ACPI#FSCL0007#3#{86e0d1e0-8089-11d0-9ce4-08003e301f73}\0000000000000006"
(type characters and see them echoed back)
```

### <a name="verify-each-device-from-a-uwp-app"></a>从 UWP 应用验证每台设备

使用下面的示例验证从 UWP 工作的设备。

| 示例 | 链接 |
|------|------|
| IoT-GPIO | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-GPIO |
| IoT-I2C | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-I2C | 
| IoT-SPI | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-SPI |
| CustomSerialDeviceAccess | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomSerialDeviceAccess |

### <a name="run-the-hlk-tests"></a>运行 HLK 测试

下载 [Hardware Lab Kit (HLK)](https://docs.microsoft.com/windows-hardware/test/hlk/windows-hardware-lab-kit)。 提供以下测试：
 * [GPIO WinRT 功能和压力测试](https://docs.microsoft.com/windows-hardware/test/hlk/testref/f1fc0922-1186-48bd-bfcd-c7385a2f6f96)
 * [I2C WinRT 写入测试（需要 EEPROM）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2ab0df1b-3369-4aaf-a4d5-d157cb7bf578)
 * [I2C WinRT 读取测试（需要 EEPROM）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/ca91c2d2-4615-4a1b-928e-587ab2b69b04)
 * [I2C WinRT 不存在的从属地址测试](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2746ad72-fe5c-4412-8231-f7ed53d95e71)
 * [I2C WinRT 高级功能测试（需要嵌入 LPC1768）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/a60f5a94-12b2-4905-8416-e9774f539f1d)
 * [SPI WinRT 时钟频率验证测试（需要嵌入 LPC1768）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/50cf9ccc-bbd3-4514-979f-b0499cb18ed8)
 * [SPI WinRT IO 传输测试（需要嵌入 LPC1768）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/00c892e8-c226-4c71-9c2a-68349fed7113)
 * [SPI WinRT 步幅验证测试](https://docs.microsoft.com/windows-hardware/test/hlk/testref/20c6b079-62f7-4067-953f-e252bd271938)
 * [SPI WinRT 传输间隔检测测试（需要嵌入 LPC1768）](https://docs.microsoft.com/windows-hardware/test/hlk/testref/6da79d04-940b-4c49-8f00-333bf0cfbb19)

当在 HLK 管理器中选择 rhproxy 设备节点时，将自动选择适用的测试。

在 HLK 管理器中，选择“资源中心代理设备”：

![HLK 管理器屏幕截图](images/usermode-hlk-1.png)

随后单击“测试”选项卡，然后依次选择 I2C WinRT、Gpio WinRT 和 Spi WinRT 测试。

![HLK 管理器屏幕截图](images/usermode-hlk-2.png)

单击“运行所选项”。 通过右键单击某个测试，然后单击“测试描述”可以获得有关每个测试的进一步文档。

## <a name="resources"></a>资源

| 目标 | 链接 |
|-------------|------|
| ACPI 5.0 规范 | http://acpi.info/spec.htm |
| Asl.exe（Microsoft ASL 编译器） | https://msdn.microsoft.com/library/windows/hardware/dn551195.aspx |
| Windows.Devices.Gpio  | https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.aspx | 
| Windows.Devices.I2c | https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.aspx |
| Windows.Devices.Spi | https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.aspx |
| Windows.Devices.SerialCommunication | https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.aspx |
| 测试授权和执行框架 (TAEF) | https://msdn.microsoft.com/library/windows/hardware/hh439725.aspx |
| SpbCx | https://msdn.microsoft.com/library/windows/hardware/hh450906.aspx |
| GpioClx   | https://msdn.microsoft.com/library/windows/hardware/hh439508.aspx |
| SerCx | https://msdn.microsoft.com/library/windows/hardware/ff546939.aspx |
| MITT I2C 测试 | https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx |
| GpioTestTool | https://developer.microsoft.com/windows/iot/samples/GPIOTestTool |
| I2cTestTool   | https://developer.microsoft.com/windows/iot/samples/I2cTestTool | 
| SpiTestTool | https://developer.microsoft.com/windows/iot/samples/spitesttool |
| MinComm (Serial) |    https://github.com/ms-iot/samples/tree/develop/MinComm |
| Hardware Lab Kit (HLK) | https://msdn.microsoft.com/library/windows/hardware/dn930814.aspx |

## <a name="apendix"></a>附录

### <a name="appendix-a---raspberry-pi-asl-listing"></a>附录 A - Raspberry Pi ASL 一览

标头引出线：https://developer.microsoft.com/windows/iot/samples/PinMappingsRPi2

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{

    Scope (\_SB)
    {
        //
        // RHProxy Device Node to enable WinRT API
        //
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE0  - GPIO 8  - Pin 24
                    0,                     // Device selection (CE0)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 1
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE1  - GPIO 7  - Pin 26
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 2
                SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                                           // MOSI - GPIO 20 - Pin 38
                                           // MISO - GPIO 19 - Pin 35
                                           // CE1  - GPIO 17 - Pin 11
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data
                // Index 3
                I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
                    0xFFFF,                // SlaveAddress: placeholder
                    ,                      // SlaveMode: default to ControllerInitiated
                    0,                     // ConnectionSpeed: placeholder
                    ,                      // Addressing Mode: placeholder
                    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
                    ,
                    ,
                    )                      // VendorData

                // Index 4 - GPIO 4 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 4 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 4 }
                // Index 6 - GPIO 5 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 5 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 5 }
                // Index 8 - GPIO 6 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 6 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 6 }
                // Index 10 - GPIO 12 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 12 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 12 }
                // Index 12 - GPIO 13 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 13 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 13 }
                // Index 14 - GPIO 16 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 16 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 16 }
                // Index 16 - GPIO 18 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 18 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 18 }
                // Index 18 - GPIO 22 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 22 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 22 }
                // Index 20 - GPIO 23 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 23 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 23 }
                // Index 22 - GPIO 24 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 24 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 24 }
                // Index 24 - GPIO 25 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 25 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 25 }
                // Index 26 - GPIO 26 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 26 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 26 }
                // Index 28 - GPIO 27 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 27 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 27 }
                // Index 30 - GPIO 35 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 35 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 35 }
                // Index 32 - GPIO 47 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 47 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 47 }
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // Reference http://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md
                    // SPI 0
                    Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},                       // Index 0 & 1
                    Package(2) { "SPI0-MinClockInHz", 7629 },                               // 7629 Hz
                    Package(2) { "SPI0-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // SPI 1
                    Package(2) { "bus-SPI-SPI1", Package() { 2 }},                          // Index 2
                    Package(2) { "SPI1-MinClockInHz", 30518 },                              // 30518 Hz
                    Package(2) { "SPI1-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI1-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // I2C1
                    Package(2) { "bus-I2C-I2C1", Package() { 3 }},
                    // GPIO Pin Count and supported drive modes
                    Package (2) { "GPIO-PinCount", 54 },
                    Package (2) { "GPIO-UseDescriptorPinNumbers", 1 },
                    Package (2) { "GPIO-SupportedDriveModes", 0xf },                        // InputHighImpedance, InputPullUp, InputPullDown, OutputCmos
                }
            })
        }
    }
}

```

### <a name="appendix-b---minnowboardmax-asl-listing"></a>附录 B - MinnowBoardMax ASL 一览

标头引出线：https://developer.microsoft.com/windows/iot/samples/PinMappingsMBM

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate() 
            {  
                // Index 0 
                SPISerialBus(            // Pin 5, 7, 9 , 11 of JP1 for SIO_SPI
                    1,                     // Device selection
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    8,                     // databit len
                    ControllerInitiated,   // slave mode
                    8000000,               // Connection speed
                    ClockPolarityLow,      // Clock polarity
                    ClockPhaseSecond,      // clock phase
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                    ResourceConsumer,      // Resource usage
                    JSPI,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      // Vendor Data  
    
                // Index 1     
                I2CSerialBus(            // Pin 13, 15 of JP1, for SIO_I2C5 (signal)
                    0xFF,                  // SlaveAddress: bus address
                    ,                      // SlaveMode: default to ControllerInitiated
                    400000,                // ConnectionSpeed: in Hz
                    ,                      // Addressing Mode: default to 7 bit
                    "\\_SB.I2C6",          // ResourceSource: I2C bus controller name (For MinnowBoard Max, hardware I2C5(0-based) is reported as ACPI I2C6(1-based))
                    ,
                    ,
                    JI2C,                  // Descriptor Name: creates name for offset of resource descriptor
                    )                      // VendorData
    
                // Index 2
                UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    ,                      // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT2",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR2,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      
    
                // Index 3
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {0}  // Pin 21 of JP1 (GPIO_S5[00])
                // Index 4
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {0} 
    
                // Index 5
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {1}  // Pin 23 of JP1 (GPIO_S5[01])
                // Index 6
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {1}
    
                // Index 7
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {2}  // Pin 25 of JP1 (GPIO_S5[02])
                // Index 8
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {2} 
    
                // Index 9
                UARTSerialBus(           // Pin 6, 8, 10, 12 of JP1, for SIO_UART1
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    FlowControlHardware,   // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT1",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR1,              // DescriptorName: creates name for offset of resource descriptor
                    )  
    
                // Index 10
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {62}  // Pin 14 of JP1 (GPIO_SC[62])
                // Index 11
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {62} 

                // Index 12
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {63}  // Pin 16 of JP1 (GPIO_SC[63])
                // Index 13
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {63} 
    
                // Index 14
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {65}  // Pin 18 of JP1 (GPIO_SC[65])
                // Index 15
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {65} 
    
                // Index 16
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {64}  // Pin 20 of JP1 (GPIO_SC[64])
                // Index 17
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {64} 
    
                // Index 18
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {94}  // Pin 22 of JP1 (GPIO_SC[94])
                // Index 19
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {94} 
    
                // Index 20
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {95}  // Pin 24 of JP1 (GPIO_SC[95])
                // Index 21
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {95} 
    
                // Index 22
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {54}  // Pin 26 of JP1 (GPIO_SC[54])
                // Index 23
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {54}
            })
    
            Name(_DSD, Package() 
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package() 
                {
                    // SPI Mapping
                    Package(2) { "bus-SPI-SPI0", Package() { 0 }},

                    Package(2) { "SPI0-MinClockInHz", 100000 },
                    Package(2) { "SPI0-MaxClockInHz", 15000000 },
                    // SupportedDataBitLengths takes a list of support data bit length
                    // Example : Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8, 7, 16 }},
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32 }},
                     // I2C Mapping
                    Package(2) { "bus-I2C-I2C5", Package() { 1 }},
                    // UART Mapping
                    Package(2) { "bus-UART-UART2", Package() { 2 }},
                    Package(2) { "bus-UART-UART1", Package() { 9 }},
                }
            })
        }
    }
}
```

### <a name="appendix-c---sample-powershell-script-to-generate-gpio-resources"></a>附录 C - 生成 GPIO 资源的示例 PowerShell 脚本

以下脚本可用于生成适用于 Raspberry Pi 的 GPIO 资源声明：

```
$pins = @(
    @{PinNumber=4;PullConfig='PullUp'},
    @{PinNumber=5;PullConfig='PullUp'},
    @{PinNumber=6;PullConfig='PullUp'},
    @{PinNumber=12;PullConfig='PullDown'},
    @{PinNumber=13;PullConfig='PullDown'},
    @{PinNumber=16;PullConfig='PullDown'},
    @{PinNumber=18;PullConfig='PullDown'},
    @{PinNumber=22;PullConfig='PullDown'},
    @{PinNumber=23;PullConfig='PullDown'},
    @{PinNumber=24;PullConfig='PullDown'},
    @{PinNumber=25;PullConfig='PullDown'},
    @{PinNumber=26;PullConfig='PullDown'},
    @{PinNumber=27;PullConfig='PullDown'},
    @{PinNumber=35;PullConfig='PullUp'},
    @{PinNumber=47;PullConfig='PullUp'})
    
# generate the resources
$FIRST_RESOURCE_INDEX = 4
$resourceIndex = $FIRST_RESOURCE_INDEX
$pins | % {
    $a = @"
// Index $resourceIndex - GPIO $($_.PinNumber) - $($_.Name)
GpioIO(Shared, $($_.PullConfig), , , , "\\_SB.GPI0", , , , ) { $($_.PinNumber) }
GpioInt(Edge, ActiveBoth, Shared, $($_.PullConfig), 0, "\\_SB.GPI0",) { $($_.PinNumber) }
"@    
    Write-Host $a
    $resourceIndex += 2;
}
```
