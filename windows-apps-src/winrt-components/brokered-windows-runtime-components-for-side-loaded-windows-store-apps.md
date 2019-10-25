---
title: 用于加载边的 UWP 应用的中转 Windows 运行时组件
description: 本白皮书讨论了 Windows 10 支持的企业级功能，使触摸友好的 .NET 应用程序可以使用负责关键业务关键操作的现有代码。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690347"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>用于加载边的 UWP 应用的中转 Windows 运行时组件

本文介绍 Windows 10 支持的面向企业的功能，该功能允许触摸友好的 .NET 应用使用负责关键业务关键操作的现有代码。

## <a name="introduction"></a>介绍

>**请注意** 本文附带的示例代码可能会下载给 [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample)。 可在此处下载用于构建中转 Windows 运行时组件的 Microsoft Visual Studio 模板：[面向适用于 Windows 10 的通用 Windows 应用的 Visual Studio 2015 模板](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows 包含一项新功能 *，称为中转应用程序 Windows 运行时组件*。 我们使用术语 "IPC （进程间通信）" 来说明在一个进程（桌面组件）中运行现有桌面软件资产的功能，同时与 UWP 应用中的此代码交互。 这对于企业开发人员来说是一种熟悉的模型，因为在 Windows 中利用 NT 服务的数据库应用程序和应用程序共享类似的多进程体系结构。

应用程序的边加载是此功能的关键组件。
企业特定的应用程序在普通消费者 Microsoft Store 中没有任何地方，公司对安全、隐私、分发、设置和维护方面的要求非常具体。 因此，两侧加载模型既是使用此功能的用户的要求，也是关键实现细节。

以数据为中心的应用程序是此应用程序体系结构的关键目标。 Ensconced，现有业务规则（例如 SQL Server）将是桌面组件的常见组成部分。 这并不是可以由桌面组件提供的唯一功能类型，但对此功能的需求很大的一部分与现有的数据和业务逻辑相关。

最后，考虑到 .NET 运行时和企业开发中的 C\# 语言的巨大渗透，开发了此功能，其中重点介绍了如何为 UWP 应用和桌面组件端使用 .NET。 尽管 UWP 应用有其他可用的语言和运行时，附带的示例只说明 C\#，并仅限于 .NET 运行时。

## <a name="application-components"></a>应用程序组件

>**请注意** 此功能仅适用于 .net。 必须使用 .NET 创作客户端应用和桌面组件。

**应用程序模型**

此功能围绕一般的应用程序体系结构构建，称为 MVVM （模型视图视图-模型）。 因此，假设 "模型" 完全存放在桌面组件中。 因此，应该立即认为，桌面组件将是 "无外设" （即，不包含 UI）。 视图将完全包含在加载的 "企业应用程序" 中。 尽管不要求使用 "查看模型" 构造构建此应用程序，但我们认为此模式的使用很常见。

**桌面组件**

此功能中的桌面组件是作为此功能的一部分引入的一种新应用程序类型。 此桌面组件只能用 C 语言编写\# 并且必须面向 .NET 4.6 或更高版本的 Windows 10。 项目类型是 CLR 目标 UWP 之间的混合，因为进程间通信格式包含 UWP 类型和类，而允许桌面组件调用 .NET 运行时类库的所有部分。 稍后将详细介绍对 Visual Studio 项目的影响。 此混合配置允许在桌面组件上构建的应用程序之间封送 UWP 类型，同时允许在桌面组件实现内调用桌面 CLR 代码。

**合同期**

在 "UWP 类型系统" 中描述了 "侧加载" 应用程序和桌面组件之间的协定。 这涉及声明一个或多个可以表示 UWP 的 C\# 类。 有关使用 C\#创建 Windows 运行时类的特定要求，请参阅 MSDN 主题[在 C 中创建 Windows 运行时组件\# 和 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) 。

>**请注意**，此时在桌面组件和边装应用程序之间的 Windows 运行时组件协定中不支持  枚举。

**端加载的应用程序**

两侧加载的应用程序在每个方面都是普通的 UWP 应用程序，但其中一种应用程序是端加载的，而不是通过 Microsoft Store 安装。 大多数安装机制都是相同的：清单和应用程序打包类似（稍后将详细介绍其中的一个清单）。 启用边载后，简单的 PowerShell 脚本就可以安装所需的证书和应用程序本身。 这是一种正常的最佳做法，即端加载的应用程序将包含在 Visual Studio 中的项目/存储菜单中的 WACK 认证测试

>**注意**可以在 "设置-&gt; 更新" & 安全&gt; 开发人员的 "设置" 中打开边载。

需要注意的一个重要一点是，作为 Windows 10 的一部分提供的 App Broker 机制仅限32位。 桌面组件必须是32位。
端加载的应用程序可以是64位（前提是已注册64位和32位代理），但这种情况不正常。 使用正常的 "非特定" 配置和 "首选32位" 默认情况下，在 C\# 中构建加载的应用程序，默认情况下会创建32位端加载的应用程序。

**服务器实例化和 Appdomain**

每个已加载的应用程序都接收其自己的应用代理服务器实例（称为 "多实例"）。 服务器代码在单个 AppDomain 中运行。 这允许在不同的实例中运行多个版本的库。 例如，应用程序 A 需要组件的1.1 版，而应用程序 B 需要 V2。 这些组件通过以下方式进行完全分离：将1.1 和 V2 组件置于单独的服务器目录中，并将应用程序指向支持所需的正确版本的服务器。

服务器代码实现可通过将多个应用程序指向同一服务器目录，在多个应用程序代理服务器实例之间共享。 应用代理服务器仍会有多个实例，但它们将运行相同的代码。 单个应用程序中使用的所有实现组件都应位于同一路径中。

## <a name="defining-the-contract"></a>定义协定

使用此功能创建应用程序的第一步是在加载的应用程序和桌面组件之间创建协定。 必须使用 Windows 运行时类型来完成此操作。
幸运的是，这些都可以使用 C\# 类进行声明。 但在定义这些会话时，有一些重要的性能注意事项，如后面的部分中所述。

定义协定的顺序如下所示：

**步骤1：** 在 Visual Studio 中创建新的类库。 请确保使用**类库模板而**不是**Windows 运行时组件**模板来创建项目。

下面是一个实现，但此部分仅涵盖进程间协定的定义。 随附的示例包括以下类（EnterpriseServer.cs），其开始形状如下所示：

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

这会定义一个类 "EnterpriseServer"，该类可从侧加载的应用程序实例化。 此类提供 RuntimeClass 中承诺的功能。 可以使用 RuntimeClass 来生成将包含在加载端应用程序中的引用 winmd。

**步骤2：** 手动编辑项目文件，以将项目的输出类型更改为**Windows 运行时组件**。

若要在 Visual Studio 中执行此操作，请右键单击新创建的项目并选择 "卸载项目"，然后再次右键单击并选择 "编辑 EnterpriseServer"，以打开项目文件（XML 文件）进行编辑。

在打开的文件中，搜索 \<OutputType\> 标记，并将其值更改为 "winmdobj"。

**步骤3：** 创建用于创建 "引用" Windows 元数据文件（winmd 文件）的生成规则。 即没有实现。

**步骤4：** 创建一个生成规则，用于创建 "实现" Windows 元数据文件，即具有相同的元数据信息，但还包括实现。

这将通过以下脚本来完成。 将脚本添加到生成后事件命令行中的 "项目**属性**" > **生成事件**"。

> **请注意**，根据你所面向的 windows 版本（windows 10）和正在使用的 Visual Studio 版本，该脚本会有所不同。

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

一旦创建了引用**winmd** （在项目的目标文件夹下的 "引用" 文件夹中），就会将它手动传送（复制）到每个使用的加载端应用程序项目，并对其进行引用。 下一节将对此进行进一步介绍。 上述生成规则中所述的项目结构确保了实现和引用**winmd**在生成层次结构中以明显隔离的目录，以避免混淆。

## <a name="side-loaded-applications-in-detail"></a>并行加载的应用程序
正如前文所述，侧加载的应用程序的构建方式与其他任何 UWP 应用一样，但还有一个更详细的信息：在 RuntimeClass 的应用程序清单中声明的可用性。 这样，应用程序只需编写新的即可访问桌面组件中的功能。 <Extension> 部分中的新清单条目描述了在桌面组件中实现的 RuntimeClass，以及其所在位置的信息。 应用程序清单中的这些声明内容对于面向 Windows 10 的应用程序是相同的。 例如：

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

类别为 inProcessServer，因为 outOfProcessServer 类别中存在一些不适用于此应用程序配置的条目。 请注意，<Path> 组件必须始终包含 clrhost （但这**不**是强制的，并且指定不同的值将失败）。

<ActivatableClass> 部分与应用包中 Windows 运行时组件首选的进程内 RuntimeClass 相同。 <ActivatableClassAttribute> 是新元素，且属性 Name = "DesktopApplicationPath" 和 Type = "string" 是必需的，并且是固定的。 值特性指向桌面组件实现 winmd 所在的位置（在下一节中对此进行了更详细的说明）。 桌面组件首选的每个 RuntimeClass 都应有自己的 <ActivatableClass> 元素树。 ActivatableClassId 必须与 RuntimeClass 的完全限定命名空间的名称相匹配。

如 "定义协定" 一节中所述，必须对桌面组件的引用 winmd 进行项目引用。 Visual Studio 项目系统通常会创建一个具有相同名称的两个级别的目录结构。 在示例中，它是 EnterpriseIPCApplication\\EnterpriseIPCApplication。 将引用**winmd**手动复制到第二个级别目录，然后使用 "项目引用" 对话框（单击 "**浏览"。** 按钮）查找并引用此**winmd**。 此后，桌面组件（例如 Fabrikam）的顶级命名空间应显示为项目的 "引用" 部分中的顶级节点。

>**注意**在加载的应用程序中使用**引用 winmd**非常重要。 如果你无意中将**实现 winmd**接管到已加载的应用程序目录并对其进行引用，则可能会收到与 "找不到 IStringable" 相关的错误。 这是一种确保引用了错误的**winmd**的标志。 IPC 服务器应用中的后期生成规则（在下一节中详细介绍）将这两个**winmd**分解为单独的目录。

环境变量（特别是% ProgramFiles%）可在 <ActivatableClassAttribute Value="path"> 中使用。如前文所述，App Broker 仅支持32位，因此，如果应用程序在64位操作系统上运行，则% ProgramFiles% 将解析为 C：\\Program Files （x86）。

## <a name="desktop-ipc-server-detail"></a>桌面 IPC 服务器详细信息

前面两部分介绍类的声明，以及将引用**winmd**传输到加载端应用程序项目的机制。 桌面组件中的剩余工作量涉及到实现。 由于桌面组件的整个点都可以调用桌面代码（通常是为了重新利用现有的代码资产），因此必须以特殊方式配置项目。
通常，使用 .NET 的 Visual Studio 项目使用两个 "配置文件" 中的一个。
一种用于桌面（"".Netframework "），另一个针对 CLR 的 UWP 应用程序部分（" "。NetCore "）。 此功能中的桌面组件是这两者之间的混合。 因此，引用部分会非常仔细地构造，以混合使用这两个配置文件。

普通 UWP 应用项目不包含显式项目引用，因为整个 Windows 运行时 API 图面都是隐式包括的。
通常情况下，仅创建其他项目间引用。 但是，桌面组件项目具有一组特别的引用。 它以 "经典桌面\\类库" 项目开始，因此是桌面项目。 因此必须进行显式引用 Windows 运行时 API （通过引用**winmd**文件）。 添加正确的引用，如下所示。

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

上述参考是 eferences 的一种仔细混合，对此混合服务器的正确操作非常重要。 协议是打开 .csproj 文件（如如何编辑项目 OutputType 中所述），并根据需要添加这些引用。

正确配置引用后，下一任务是实现服务器的功能。 请参阅主题使用 [Windows 运行时组件实现互操作性的最佳做法（使用 C\#C++ /VB/和 XAML 的 UWP 应用）](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))。
任务是创建一个可在其实现中调用桌面代码的 Windows 运行时组件 dll。 随附的示例包括 Windows 运行时中使用的主要模式：

-   方法调用

-   通过桌面组件 Windows 运行时事件源

-   Windows 运行时异步操作

-   返回基本类型的数组

**安装**

若要安装应用，请将实现**winmd**复制到关联的侧加载应用程序清单中指定的正确目录： <ActivatableClassAttribute>的值 = "path"。 同时复制任何关联的支持文件和代理/存根（stub） dll （如下所述）。 未能将实现**winmd**复制到服务器目录位置会导致所有已加载的应用程序在 RuntimeClass 上调用 new，并引发 "未注册类" 错误。 无法安装代理/存根（或无法注册）将导致所有调用都失败，且没有返回值。 此后一错误通常**不**与可见异常相关联。
如果由于此配置错误而发现异常，则它们可能会引用 "无效强制转换"。

**服务器实现注意事项**

可以将桌面 Windows 运行时服务器视为基于 "worker" 或 "task"。 对服务器的每次调用都在非 UI 线程上运行，并且所有代码都必须是多线程感知和安全的。 端加载的应用程序的哪个部分调用服务器的功能也很重要。 始终避免从加载的应用程序中的任何 UI 线程调用长时间运行的代码是至关重要的。 可以通过两种主要方法完成此操作：

1.  如果从 UI 线程调用服务器功能，请始终在服务器的公共 surface 区和实现中使用异步模式。

2.  从端加载的应用程序中的后台线程调用服务器的功能。

**服务器中的 Windows 运行时异步**

考虑到应用程序模型的跨进程性质，对服务器的调用比独占在进程内运行的代码的开销更高。 通常可以安全地调用返回内存中值的简单属性，因为它的执行速度足以阻止 UI 线程。 但是，任何包含任何排序（包括所有文件处理和数据库检索）的 i/o 的调用都可能会阻止调用 UI 线程，并导致应用程序因无响应而终止。 此外，出于性能方面的考虑，不建议在此应用程序体系结构中对对象执行属性调用。
下一节将更深入地介绍这一点。

正确实现的服务器通常会通过 Windows 运行时异步模式实现直接从 UI 线程发出的调用。 这可以通过遵循此模式实现。 首先，声明（同样，从附带的示例中）：

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

这将声明一个返回整数 Windows 运行时异步操作。
异步操作的实现通常采用以下格式：

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**注意**编写实现时，通常会等待一些其他可能运行时间较长的操作。 如果是这样，则需要声明**任务：运行**代码：

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

此 async 方法的客户端可以像任何其他 Windows 运行时 aysnc 操作一样等待此操作。

**从应用程序后台线程调用服务器功能**

由于通常情况下，客户端和服务器都是由同一组织编写的，因此可以采用编程做法，对服务器的所有调用都将由侧加载的应用程序中的后台线程进行。 从服务器收集一个或多个数据批的直接调用可以从后台线程进行。 完全检索结果时，通常可以从 UI 线程直接检索在应用程序进程内存中的数据批。 在后台线程和 UI 线程之间，C @ no__t_0_ 对象自然非常灵活，因此对于这种调用模式特别有用。\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>创建和部署 Windows 运行时代理

由于 IPC 方法涉及在两个进程之间封送 Windows 运行时接口，因此必须使用一个全局注册的 Windows 运行时代理和存根。

**在 Visual Studio 中创建代理**

在 [Windows 运行时组件中引发事件](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))的主题中介绍了用于创建和注册代理和存根以便在常规 UWP 应用包内使用的过程。
本文中所述的步骤比下述过程更复杂，因为它涉及在应用程序包中注册代理/存根（而不是全局注册）。

**步骤1：** 使用桌面组件项目的解决方案，在 Visual Studio 中创建代理/存根项目：

**解决方案 > 将 > 项目添加 > C++ Visual > Win32 控制台选择 DLL 选项。**

对于下面的步骤，我们假设服务器组件称为**MyWinRTComponent**。

**步骤3：** 从项目中删除所有 CPP/H 文件。

**步骤4：** 前面的 "定义协定" 一节包含运行**winmdidl**、 **midl**、 **mdmerge**等的后期生成命令。 " 此后生成命令的 midl 步骤中的一个输出生成四个重要输出：

a） Dlldata。c

b）标头文件（例如，MyWinRTComponent）

c）\_MyWinRTComponent 文件的 \*（例如，\_

d） \*\_p .c 文件（例如，MyWinRTComponent\_p）

**步骤5：** 将这四个生成的文件添加到 "MyWinRTProxy" 项目。

**步骤6：** 将一个 def 文件添加到 "MyWinRTProxy" 项目 **（project > 将新项添加 > 代码 > 模块定义文件**），并将内容更新为：

库 MyWinRTComponent

导出

DllCanUnloadNow 私有

DllGetClassObject 私有

DllRegisterServer 私有

DllUnregisterServer 私有

**步骤7：** 打开 "MyWinRTProxy" 项目的属性：

**Comfiguration 属性 > 常规 > 目标名称：**

MyWinRTComponent

**> 添加C++ C/> 预处理器定义**

WIN32\_WINDOWS;注册\_PROXY\_DLL "

**C/C++ > 预编译标头：选择 "不使用预编译标头"**

**链接器 > 常规 > 忽略导入库：选择 "是"**

**链接器 > 输入 > 其他依赖项：添加 rpcrt4; runtimeobject.lib**

**链接器 > Windows 元数据 > 生成 Windows 元数据：选择 "否"**

**步骤8：** 生成 "MyWinRTProxy" 项目。

**部署代理**

代理必须是全局注册的。 要执行此操作，最简单的方法是在代理 dll 上让安装过程调用 DllRegisterServer。 请注意，由于此功能仅支持为 x86 （即无64位支持）构建的服务器，因此最简单的配置是使用32位服务器、32位代理和一个32位端加载的应用程序。 代理通常与桌面组件的实现**winmd**一起提供。

必须执行一个额外的配置步骤。 若要加载和执行代理的加载和执行程序，该目录必须标记为 "read/execute" （对于 ALL_APPLICATION_PACKAGES）。 这是通过**icacls**命令行工具来完成的。 此命令应在实现**winmd**和代理/存根 dll 所在的目录中执行：

*icacls./T/grant \*S-1-15-2-1： RX*

## <a name="patterns-and-performance"></a>模式和性能

密切监视跨进程传输的性能非常重要。 跨进程调用的开销至少是进程内调用的两倍。 创建 "多个" 对话跨进程或执行对大对象的重复传输（如位图图像）可能导致意外的和不良的应用程序性能。

下面是要考虑的重要事项列表：

-   应始终避免从应用程序的 UI 线程到服务器的同步方法调用。 从应用程序中的后台线程调用方法，并在必要时使用 CoreWindowDispatcher 将结果返回到 UI 线程。

-   从应用程序 UI 线程调用 async 操作是安全的，但请考虑下面讨论的性能问题。

-   大容量传输的结果减少了跨进程交互成本。 通常使用 Windows 运行时数组构造执行此方法。

-   返回*List<T>* 其中*t*是异步操作或属性提取中的对象，将导致大量的跨进程交互成本。 例如，假设您返回 *&lt;人员&gt;* 对象的列表。 每次迭代都将是一个跨进程调用。 返回的每个*用户*对象由一个代理表示，对该单个对象的方法或属性的每个调用都将导致跨进程调用。 这是一个 "非合法"*列表&lt;用户&gt;* 对象，其中*计数*很大，会导致大量的缓慢调用。 通过大容量传输数组中内容的结构来获得更好的性能结果。 例如：

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

然后返回 * PersonStruct\[\]* 而不是*List&lt;PersonObject&gt;* 。
这会跨进程 "跃点" 获取所有数据

与所有性能注意事项一样，度量和测试非常重要。 理想情况下，应在各种操作中插入遥测数据，以确定所需的时间。 一定要跨一定范围来度量：例如，需要多长时间才能使用已加载的应用程序中特定查询的所有*人员*对象？

另一种方法是可变负载测试。 为此，可以将性能测试挂钩放入应用程序，将延迟加载引入服务器处理中。 这可以模拟各种负载以及应用程序对不同服务器性能的反应。
此示例演示如何使用适当的异步技术将时间延迟放入代码中。 要注入的精确延迟量以及要放入该人工负载的随机化范围将因应用程序设计和应用程序运行时的预期环境而异。

## <a name="development-process"></a>开发过程

对服务器进行更改时，必须确保任何以前运行的实例不再运行。 COM 最终会清理该进程，但断开计时器所花的时间比对迭代开发效率更高。 因此，在开发过程中，取消以前运行的实例是一个普通步骤。 这要求开发人员跟踪承载服务器的 dllhost.exe 实例。

可以使用任务管理器或其他第三方应用来查找和终止服务器进程。 还包括命令行工具 * * TaskList * *，它具有灵活的语法，例如：

  
 | **命令** | **操作** |
 | ------------| ---------- |
 | tasklist | 按创建时间的大致顺序列出所有正在运行的进程，最靠近底部的创建进程。 |
 | tasklist/FI "IMAGENAME eq dllhost.exe"/M | 列出了所有 dllhost.exe 实例的信息。 /M 开关列出了已加载的模块。 |
 | tasklist/FI "PID eq 12564"/M | 如果知道 dllhost.exe，可以使用此选项查询它的 PID。 |

代理服务器的模块列表应在其已加载的模块列表中列出*clrhost* 。

## <a name="resources"></a>资源

-   [适用于 Windows 10 和 VS 2015 的中转 WinRT 组件项目模板](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT 中转 WinRT 组件示例](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [提供可靠且可信 Microsoft Store 应用](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [应用协定和扩展（Windows 应用商店应用）](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [如何在 Windows 10 上旁加载应用](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [将 UWP 应用部署到企业](https://go.microsoft.com/fwlink/p/?LinkID=264770)

