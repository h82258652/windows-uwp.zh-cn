---
author: msatranjr
title: 适用于旁加载 UWP 应用的中转 Windows 运行时组件
description: 本文讨论 windows 10，这允许触摸友好的.NET 应用使用对主要关键业务操作负责的现有代码支持面向企业的功能。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: 3228cd80e7a9e8efb5dca1ec3a2d469e40a52c8a
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6838225"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>适用于旁加载 UWP 应用的中转 Windows 运行时组件

本文讨论 windows 10，这允许触摸友好的.NET 应用使用对主要关键业务操作负责的现有代码支持的面向企业的功能。

## <a name="introduction"></a>介绍

>**注意**可能为[Visual Studio 2015 & 2017年](https://aka.ms/brokeredsample)下载本文附带的示例代码。 用于生成中转 Windows 运行时组件的 Microsoft Visual Studio 模板可以在此处下载：[面向 Windows 10 通用 Windows 应用的 Visual Studio 2015 模板](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

Windows 包含了新功能称为*旁加载应用程序的中转 Windows 运行时组件*。 我们使用术语 IPC（进程间通信）来描述在单个进程（桌面组件）中运行现有桌面软件资源的同时，在 UWP 应用中与此代码交互的能力。 这对于企业开发人员来说是熟悉的模型，因为数据库应用程序以及在 Windows 中使用 NT 服务的应用程序共享相似的多进程体系结构。

应用的旁加载是此功能的关键组件。
企业特定的应用程序在一般消费者 Microsoft Store 中并没有一席之地，并且企业对安全、隐私、分发、设置和服务有非常具体的要求。 因此，旁加载模型既是此功能的使用者的要求，也是关键的实现详细信息。

以数据为中心的应用程序是此应用程序体系结构的主要目标。 根据预想，已存在的（例如在 SQL Server 中）现有业务规则将是桌面组件的公共部分。 这当然不是桌面组件可提供的唯一功能类型，但此功能的大部分需求都与现有数据和业务逻辑相关。

最后，鉴于 .NET 运行时和 C\# 语言在企业开发中的巨大渗透，开发此功能时强调同时针对 UWP 应用和桌面组件端使用 .NET。 尽管有其他语言和运行时可用于 UWP 应用，但附带的示例仅演示 C\#，并且仅限于 .NET 运行时。

## <a name="application-components"></a>应用程序组件

>**注意**此功能将专门用于.NET。 客户端应用和桌面组件都必须使用 .NET 进行授权。

**应用程序模型**

此功能围绕称作 MVVM (Model View View-Model) 的一般应用程序体系结构而生成。 照此，假设该“模型”完全存在于桌面组件内部。 因此显而易见的是，桌面组件将为“无外设”（即，不包含 UI）组件。 该视图将完全包含在旁加载的企业应用程序中。 尽管不要求此应用程序利用“视图模型”构造生成，但预计此模型的使用将很常见。

**桌面组件**

此功能中的桌面组件是作为此功能的一部分引入的新应用程序类型。 此桌面组件仅可采用 C\# 编写，并且必须面向适用于 Windows 10 的 .NET 4.6 或更高版本。 由于进程间通信格式包含 UWP 类型和类，因此该项目类型是面向 UWP 的 CLR 之间的混合类型，同时允许桌面组件调用 .NET 运行时类库的所有部分。 稍后将详细描述对 Visual Studio 项目的影响。 此混合配制允许在桌面组件上生成的应用程序间封送 UWP 类型，同时允许在桌面组件实现内部调用桌面 CLR 代码。

**合约**

旁加载应用程序和桌面组件之间的合约根据 UWP 类型系统进行描述。 这涉及到要声明可表示 UWP 的一个或多个 C\# 类。 有关使用 C\# 创建 Windows 运行时类的具体要求，请参阅 MSDN 主题[使用 C\# 和 Visual Basic 创建 Windows 运行时组件](https://msdn.microsoft.com/library/br230301.aspx)

>**注意**桌面组件和旁加载应用程序在此时间之间的 Windows 运行时组件合约中不支持枚举。

**旁加载应用程序**

旁加载应用程序在每个方面都是正常的 UWP 应用，除了一个方面：它是旁加载的，而不是通过 Microsoft Store 安装。 大部分安装机制都相同：清单和应用程序打包很相似（稍后介绍有关清单的一项额外说明）。 启用旁加载后，一个简单的 PowerShell 脚本便可安装必要的证书和应用程序本身。 旁加载应用程序通过 Visual Studio 中的“项目/应用商店”菜单所含的 WACK 认证测试是正常的最佳做法。

>**注意**旁加载可以处于打开状态中设置-&gt;更新和安全&gt;适用于开发人员。

要注意的一个要点是，作为 Windows 10 一部分提供的应用代理机制仅限于 32 位。 桌面组件必须为 32 位。
旁加载应用程序可以为 64 位（假设已同时注册 64 位和 32 位代理），但这并不是典型情况。 使用正常“中性”配置和“首选 32 位”默认值在 C\# 中生成旁加载应用程序自然可创建 32 位旁加载应用程序。

**服务器实例和 AppDomains**

每个旁加载应用程序都接收它自己的应用代理服务器实例（称为“多实例”）。 服务器代码在单个 AppDomain 内部运行。 这将允许多个版本的库在独立的实例中运行。 例如，应用程序 A 需要一个组件的 V1.1，而应用程序 B 需要 V2。 通过使 V1.1 和 V2 组件处于独立的服务器目录并将应用程序指向支持所需正确版本的服务器，可明确地分离它们。

服务器代码实现可在多个应用代理服务器实例之间共享，方法是将多个应用程序指向相同的服务器目录。 仍然会有应用代理服务器的多个实例，但它们将运行相同的代码。 单个应用程序中使用的所有实现组件应当存在于相同的路径中。

## <a name="defining-the-contract"></a>定义合约

使用此功能创建应用程序的第一步，是创建旁加载应用程序和桌面组件之间的合约。 必须使用 Windows 运行时类型专门执行此操作。
幸运的是，使用 C\# 类可以轻松地声明它们。 但是，在定义这些对话时，有一些重要的性能注意事项，将在后面的部分中进行介绍。

下面介绍的是定义合约的顺序：

**步骤 1：** 在 Visual Studio 中创建新的类库。 请务必使用类库模板而非 Windows 运行时组件模板创建该项目

接下来，显然要介绍如何实现，但是此部分仅介绍如何定义进程间合约。 附带的示例包括以下类 (EnterpriseServer.cs)，此类的开始形式如下所示：

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

这定义了类“EnterpriseServer”，此类可从旁加载应用程序中进行实例化。 此类可提供 RuntimeClass 中承诺的功能。 RuntimeClass 可用于生成引用 winmd，该引用将包含在旁加载应用程序中。

**步骤 2：** 手动编辑项目文件，将项目的输出类型更改为 Windows 运行时组件

若要在 Visual Studio 中执行此操作，请右键单击新创建的项目并选择“卸载项目”，然后再次右键单击并选择“编辑 EnterpriseServer.csproj”，以打开项目文件（即 XML 文件）进行编辑。

在打开的文件中，搜索 \<OutputType\> 标记并将其值更改为“winmdobj”。

**步骤 3：** 创建生成规则，该规则可创建“引用”Windows 元数据文件（.winmd 文件）。 即，没有任何实现。

**步骤 4：** 创建可创建“实现”Windows 元数据文件的生成规则，即，具有相同的元数据信息，但还包括实现。

按照以下脚本即可完成此操作。 在项目“属性”**** > “生成事件”**** 中，将这些脚本添加到生成后事件命令行。

> **注意** 脚本并不相同，具体取决于你要面向的 Windows 版本 (Windows 10) 以及所使用的 Visual Studio 版本。

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

一次引用**winmd**创建 （文件夹项目的目标文件夹下的"参考"） 中，它是执行 （复制） 为每个使用的旁加载应用程序项目和引用的手。 这将在下一部分中进一步描述。 体现在以上生成规则中的项目结构确保实现和引用**winmd**处于生成层次结构以避免混淆中明确隔离的目录。

## <a name="side-loaded-applications-in-detail"></a>旁加载应用程序的详细信息
如之前所述，旁加载应用程序的生成方式和任何其他 UWP 应用相同，但是有一个额外细节：要声明旁加载的应用程序清单中 RuntimeClass 的可用性。 这使应用程序只需编写新内容便可访问桌面组件中的功能。 <Extension> 部分中的新清单项描述了桌面组件中实现的 RuntimeClass 和关于它的位置的信息。 应用程序清单中的这些声明内容同样适用于面向 Windows 10 的应用。 例如：

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

类别为 inProcessServer，因为 outOfProcessServer 类别中有多个项均不适用于此应用程序配置。 请注意，<Path>组件必须始终包含 clrhost.dll (但是这次是**不**强制，且指定不同的值将以未定义的方式失败)。

<ActivatableClass> 部分与应用包中 Windows 运行时组件首选的真正的进程内 RuntimeClass 相同。 <ActivatableClassAttribute> 是新元素，并且属性 Name="DesktopApplicationPath" 和 Type="string" 是强制且不变的。 “值”属性指向桌面组件的实现 winmd 所在的位置（下一部分将介绍有关于这一点的更多详细信息）。 桌面组件首选的每个 RuntimeClass 都应具有自己的 <ActivatableClass> 元素树。 ActivatableClassId 必须匹配 RuntimeClass 的完全命名空间限定名称。

如“定义合约”部分中所提到的，必须对桌面组件的引用 winmd 进行项目引用。 Visual Studio 项目系统通常使用相同的名称创建一个两级目录结构。 在本例中，它是 EnterpriseIPCApplication\\EnterpriseIPCApplication。 引用**winmd**手动复制到此二级目录，然后项目引用对话框用于 （单击**浏览...** 按钮) 定位和引用此**winmd**。 在此之后，桌面组件的顶级命名空间（例如 Fabrikam）应当作为顶级节点出现在该项目的“引用”部分中。

>**注意**非常重要，若要使用**引用 winmd**旁加载应用程序中。 如果你意外延续**实现 winmd**到旁加载应用目录并引用它，你将可能收到与"无法找到 IStringable"相关的错误。 这是一个确保登录的错误**winmd**已引用。 IPC 服务器应用中 （在下一节中详细介绍） 周密的生成后规则介绍这些两个**winmd**到单独的目录。

环境变量（尤其是 %ProgramFiles%）可以在 <ActivatableClassAttribute Value="path"> 中使用。如前所述，应用代理仅支持 32 位，因此，如果应用程序在 64 位 OS 上运行，%ProgramFiles% 将解析为 C:\\Program Files (x86)。

## <a name="desktop-ipc-server-detail"></a>桌面 IPC 服务器详细信息

前两个部分介绍了类的声明和运送引用**winmd**机制到旁加载应用程序项目。 桌面组件中大部分剩余工作都涉及到实现。 由于桌面组件的全部意义在于能够调用桌面代码（通常用于重新利用现有代码资产），必须以特殊方式配置该项目。
正常情况下，使用 .NET 的 Visual Studio 项目使用两个“配置文件”中的一个。
一个用于桌面（“.NetFramework”），另一个面向 CLR 的 UWP 应用部分（“.NetCore”）。 此功能中的桌面组件是这两者之间的混合。 因此，引用部分经过非常周密的构造以融合这两个配置文件。

正常的 UWP 应用项目不包含显式项目引用，因为已隐式包含整个 Windows 运行时 API 图面。
正常情况下，仅进行其他项目间引用。 但是，桌面组件项目有一组非常特殊的引用。 它起初是“Classic Desktop\\Class Library”项目，因此是桌面项目。 因此显式引用 Windows 运行时 API (通过引用**winmd**文件) 都必须进行。 添加适当的引用，如下所示。

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

以上引用是对此混合服务器的正确操作至关重要的引用的周密组合。 该协议旨在打开 .csproj 文件（在如何编辑项目 OutputType 中有所说明），并根据需要添加这些引用。

正确配置引用后，下一个任务是实现服务器的功能。 请参阅 MSDN 主题[的 Windows 运行时组件 （使用 C\ # /vb/c + + 和 XAML 的 UWP 应用） 互操作性最佳做法](https://msdn.microsoft.com/library/windows/apps/hh750311.aspx)。
该任务是创建一个 Windows 运行时组件 dll，可调用桌面代码作为其实现的一部分。 附带的样本包括在 Windows 运行时中使用的主要模式：

-   方法调用

-   桌面组件的 Windows 运行时事件源

-   Windows 运行时异步操作

-   返回基本类型的数组

**安装**

若要安装该应用，将复制实现**winmd**到关联的旁加载应用程序清单中指定的正确目录： <ActivatableClassAttribute>Value ="path"。 还需复制任何相关联的支持文件和代理/存根 dll（下面介绍了后者的详细信息）。 未能复制实现**winmd**到服务器目录位置将导致所有旁加载的 RuntimeClass 上的新应用程序的调用引发"没有注册类"错误。 安装代理/存根失败（或注册失败）将导致所有调用失败，且无返回值。 后者的错误通常是**不**与可见异常相关联。
如果由于此配置错误而观察到异常，它们可能指的是“无效的强制转换”。

**服务器实现注意事项**

桌面 Windows 运行时服务器可以视为基于“工作人员”或“任务”。 每个对服务器的调用都在非 UI 线程上运行，而且所有代码必须能感知多线程且安全。 旁加载应用程序的哪一部分在调用服务器的功能同样重要。 始终避免从任何旁加载应用程序中的 UI 线程调用长期运行的代码至关重要。 完成此操作主要有两种方法：

1.  如果从 UI 线程调用服务器功能，请始终在服务器的公用表面区域和实现中使用异步模式。

2.  从旁加载应用程序中的后台线程调用服务器的功能。

**服务器中的 Windows 运行时异步**

鉴于应用程序模型的跨进程属性，对服务器的调用相比专门在进程内运行的代码有更多的开销。 正常情况下，调用返回内存内值的简单属性是安全的，因为它将足够快速地执行，无需担心阻止 UI 线程。 但是，任何涉及到所有种类的 I/O（这包括所有文件处理和数据库检索）的调用可能阻止调用 UI 线程，并且导致应用程序由于无响应而终止。 此外，由于性能原因，不建议在此应用程序体系结构中进行对象的属性调用。
下一部分将对此做更深入的介绍。

正常情况下，正确实现的服务器将实现通过 Windows 运行时异步模式从 UI 线程直接进行的调用。 这可以通过遵循此模式来实现。 首先，声明（仍然来自附带的示例）：

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

这声明了返回整数的 Windows 运行时异步操作。
异步操作的实现通常采用以下形式：

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**注意** 通常要在编写实现的同时等待一些其他潜在长期运行的操作。 如果是这样，**Task.Run**代码需要声明：

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

此异步方法的客户端可等待此操作，和任何其他 Windows 运行时异步操作一样。

**从应用程序后台线程调用服务器功能**

由于通常客户端和服务器将由相同的组织编写，可采用以下编程做法，即所有对服务器的调用将由旁加载应用程序中的后台线程进行。 可从后台线程进行从服务器收集一批或多批数据的直接调用。 当完全检索到结果时，应用程序进程中常驻内存的一批数据通常可直接从 UI 线程检索。 C\# 对象本身在后台线程和 UI 线程之间非常敏捷，因此对这种调用模式尤其有用。

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>创建和部署 Windows 运行时代理

由于 IPC 方法涉及到在两个进程之间封送 Windows 运行时接口，因此必须使用全局注册的 Windows 运行时代理和存根。

**在 Visual Studio 中创建代理**

[在 Windows 运行时组件中引发事件](https://msdn.microsoft.com/library/windows/apps/dn169426.aspx)的主题介绍了用于创建并注册代理和存根，以在常规 UWP 应用包内部使用的过程。
本文中介绍的步骤比下面介绍的过程更复杂，因为它涉及到在应用程序包内部注册代理/存根（与全局注册相对）。

**步骤 1：** 使用适用于桌面组件项目的解决方案，在 Visual Studio 中创建代理/存根项目。

**“解决方案”&gt;“添加”&gt;项目”&gt;“Visual C++”&gt;“Win32 控制台” 选择 DLL 选项。**

对于以下步骤，假设服务器组件被称为**MyWinRTComponent**。

**步骤 3：** 删除所有来自该项目的 CPP/H 文件。

**步骤 4:** 上一节"定义合约"包含运行**winmdidl.exe**、**midl.exe**、**mdmerge.exe**，等的生成后命令。 从该生成后命令的 MIDL 步骤得出的输出之一将生成以下 4 个重要输出：

a) Dlldata.c

b) 头文件（例如 MyWinRTComponent.h）

c) \*\_i.c 文件（例如 MyWinRTComponent\_i.c）

d) \*\_p.c 文件（例如 MyWinRTComponent\_p.c）

**步骤 5：** 将这四个生成文件添加到“MyWinRTProxy”项目。

**步骤 6:** 将 def 文件添加到"MyWinRTProxy"项目 **(项目 > 添加新项 > 代码 > 模块定义文件**) 和内容更新为：

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**步骤 7：** 打开“MyWinRTProxy”项目的属性：

**“配置属性”&gt;“常规”&gt;“目标名称”：**

MyWinRTComponent.Proxies

**“C/C++”&gt;“预处理器定义”&gt;“添加”**

“WIN32;\_WINDOWS;REGISTER\_PROXY\_DLL”

**“C/C++”&gt;“预编译标头”：选择“不使用预编译标头”**

**“链接器”&gt;“常规”&gt;“忽略导入库”：选择“是”**

**“链接器”&gt;“输入”&gt;“其他依赖项”：添加 rpcrt4.lib;runtimeobject.lib**

**“链接器”&gt;“Windows 元数据”&gt;“生成 Windows 元数据”：选择“否”**

**步骤 8：** 生成“MyWinRTProxy”项目。

**部署代理**

必须全局注册该代理。 执行此操作的最简单方法是让你的安装进程对代理 dll 调用 DllRegisterServer。 请注意，由于此功能仅支持针对 x86 生成的服务器（即，无 64 位支持），因此最简单的配置是使用 32 位服务器、32 位代理和 32 位旁加载应用程序。 该代理通常位于与实现**winmd**并行适用于桌面组件。

必须执行一个额外配置步骤。 为了使旁加载进程加载和执行该代理，该目录必须为 ALL_APPLICATION_PACKAGES 标记为“读取/执行”。 这是通过**icacls.exe**命令行工具。 应在目录中执行此命令的实现**winmd**和代理/存根 dll 所在：

*icacls。 /T /grant \*S-1-15-2-1:RX*

## <a name="patterns-and-performance"></a>模式和性能

周密监视跨进程传输的性能非常重要。 跨进程调用的成本至少是进程内调用的两倍。 跨进程创建“闲聊”对话或执行大对象（例如位图图像）的重复传输可能导致意外且不良的应用程序性能。

以下是要考虑的事项的非穷尽列表：

-   应当始终避免从应用程序的 UI 线程对服务器的同步方法调用。 从应用程序中的后台线程调用该方法，然后使用 CoreWindowDispatcher 将结果带到 UI 线程上（如果有必要）。

-   从应用程序 UI 线程调用异步操作是安全的，但是请考虑下述性能问题。

-   结果的批量传输降低了跨进程闲聊。 这通常通过使用 Windows 运行时数组构造来执行。

-   返回*列表<T>* 其中*T*是来自异步操作或属性提取的对象，将导致许多跨进程闲聊。 例如，假设你返回*列表&lt;用户&gt;* 对象。 每次迭代传递都将是一次跨进程调用。 每个*用户*返回的对象都由代理和每个调用方法或对该单个对象的属性会导致跨进程调用。 因此"无辜"*列表&lt;用户&gt;* 对象的*计数*大将导致大量的慢速调用。 数组中内容结构的批量传输可产生更好的性能。 例如：

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

然后返回*PersonStruct\ [\]* 而不是*列表&lt;PersonObject&gt;*。
这将在一次跨进程“跳跃”中获取所有数据。

和所有性能注意事项一样，测量和测试至关重要。 理想情况下，应当将遥测插入到各种操作中以确定它们所需的时间。 请务必在一系列测量： 例如，多长时间未实际需要使用所有*人*对象的旁加载应用程序中为特定的查询？

另一项技术是变量加载测试。 这可以通过将性能测试挂钩放入将变量延迟加载引入服务器处理的应用程序来完成。 这可以模拟各种加载和应用程序对不同服务器性能的反应。
此示例演示如何使用正确的异步技术将时间延迟放入代码中。 要注入的延迟的确切量和要放入人工加载中的随机化的范围，将根据应用程序设计和应用程序将在其中运行的预期环境而变化。

## <a name="development-process"></a>开发进程

当你对服务器作出更改时，有必要确保之前运行的任何实例都不再继续运行。 COM 最终会清理该进程，但在按照削减计时器清理之前，该进程仍对迭代开发有效。 因此，终止以前运行的实例是开发期间的常用步骤。 这要求开发人员一直跟踪用于托管服务器的 dllhost 实例。

可使用任务管理器或其他第三方应用查找和终止服务器进程。 命令行工具**TaskList.exe**也包含在内，并具有灵活的语法，例如：

  
 | **命令** | **操作** |
 | ------------| ---------- |
 | Tasklist | 按照创建时间的大致顺序列出所有运行的进程，最近创建的进程靠近底部位置。 |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | 列出关于所有 dllhost.exe 实例的信息。 /M 开关可列出已加载的模块。 |
 | tasklist /FI "PID eq 12564" /M | 如果你知道 dllhost.exe 的 PID，可以使用该选项查询它。 |

代理服务器的模块列表应该列出*clrhost.dll*在其加载模块列表中。

## <a name="resources"></a>资源

-   [适用于 Windows 10 和 VS 2015 的中转 WinRT 组件项目模板](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

-   [NorthwindRT 中转 WinRT 组件示例](http://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [提供可靠且值得信任的 Microsoft Store 应用](http://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [应用合约和扩展（Microsoft Store 应用）](https://msdn.microsoft.com/library/windows/apps/hh464906.aspx)

-   [如何在 Windows 10 中旁加载应用](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#GroupPolicy)

-   [将 UWP 应用部署到企业](http://go.microsoft.com/fwlink/p/?LinkID=264770)

