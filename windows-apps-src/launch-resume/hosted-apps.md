---
Description: 了解如何生成继承主机应用的可执行文件、入口点和运行时属性的托管应用。
title: 创建托管应用
ms.date: 04/23/2020
ms.topic: article
keywords: windows 10、desktop、package、identity、.MSIX、Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ed4356513e406c7c787ec111d32560ac08d293f1
ms.sourcegitcommit: f26d0b22a70b05679fc7089e11d639ba1a4a23af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82107720"
---
# <a name="create-hosted-apps"></a>创建托管应用

从 Windows 10 版本2004开始，你可以创建*托管应用*。 托管应用与父*主机*应用共享相同的可执行文件和定义，但其外观和行为类似于系统上的单独应用。

托管应用适用于希望组件（如可执行文件或脚本文件）的行为类似于独立 Windows 10 应用的方案，但组件需要主机进程才能执行。 例如，可以将 PowerShell 或 Python 脚本作为托管应用传递，该应用需要安装主机才能运行。 托管应用可以具有其自己的开始磁贴、标识以及与 Windows 10 功能（如后台任务、通知、磁贴和共享目标）的深度集成。

包清单中的几个元素和属性支持托管应用功能，包清单中的这些元素和特性使托管应用可以在主机应用包中使用可执行文件和定义。 当用户运行托管应用程序时，操作系统会自动以托管应用程序的标识启动主机可执行文件。 然后，宿主可以将视觉资产、内容或调用 Api 作为托管应用程序加载。 托管应用获取主机和托管应用之间声明的功能的交集。 这意味着，托管应用无法请求比主机提供的功能更多的功能。

## <a name="define-a-host"></a>定义主机

*宿主*是托管应用程序的主要可执行文件或运行时进程。 目前，唯一受支持的主机是包含*包标识*的桌面应用（.Net 或 c + +/Win32）。 此时不支持将 UWP 应用作为主机。 对于桌面应用程序，有多种方式具有包标识：

* 将程序包标识授予桌面应用最常见的方法是将[其打包到 .msix 包中](https://docs.microsoft.com/windows/msix)。
* 在某些情况下，你也可以选择通过创建[稀疏包](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)来授予包标识。 如果无法采用 .MSIX 打包来部署桌面应用，则此选项很有用。

宿主是在其包清单中通过[**uap10： HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime)扩展声明的。 此扩展具有一个**Id**属性，该属性必须为托管应用程序的包清单所引用的值赋值。 激活托管应用后，主机将以托管应用程序的标识启动，并可以从托管应用程序包加载内容或二进制文件。

下面的示例演示如何在包清单中定义主机。 **Uap10： HostRuntime**扩展在包范围内，因此将声明为[**package**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package)元素的子元素。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

请注意以下有关以下元素的重要详细信息。

| 元素              | 详细信息 |
|----------------------|-------|
| [**uap10:Extension**](https://docs.microsoft.com/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | `windows.hostRuntime`类别声明一个包范围扩展，该扩展定义激活托管应用时要使用的运行时信息。 托管应用将使用扩展中声明的定义运行。 使用在上一示例中声明的主机应用时，托管应用将作为可执行文件**PyScriptEngine**在**mediumIL**信任级别运行。<br/><br/>**可执行文件**、 **uap10： RuntimeBehavior**和**uap10： TrustLevel**属性指定包中主机进程二进制文件的名称，以及托管应用的运行方式。 例如，使用上一示例中的属性的托管应用将作为可执行文件 PyScriptEngine 在 mediumIL 信任级别运行。 |
| [**uap10:HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | **Id**属性声明包中此特定主机应用的唯一标识符。 一个包可以有多个主机应用程序，每个应用程序必须有一个具有唯一**Id**的**uap10： HostRuntime**元素。

## <a name="declare-a-hosted-app"></a>声明托管应用

*托管应用*在*主机*上声明了包依赖关系。 托管应用利用主机 ID （即，主机包中**uap10： HostRuntime**扩展的**id**属性）进行激活，而不是在其自己的包中指定入口点可执行文件。 托管应用通常包含主机可能访问的内容、视觉资产、脚本或二进制文件。 托管应用程序包中的[**y**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)值的目标值应与主机的值相同。

托管应用包可以签名或无符号：

* 已签名的包可能包含可执行文件。 这在具有二进制扩展机制的方案中很有用，此机制使宿主能够在托管应用程序包中加载 DLL 或已注册的组件。
* 未签名的包只能包含不可执行的文件。 当宿主只需要加载映像、资产和内容或脚本文件时，这非常有用。 未签名的包在其`OID` [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素中必须包含一个特殊值，否则不允许注册。 这可以防止未签名的包与签名包的标识冲突，或哄骗签名包的标识。

若要定义托管应用，请在包清单中声明以下项：

* [**Uap10： HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)元素。 这是[依赖项](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies)元素的子元素。
* [**应用程序**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素（适用于应用程序）或[**扩展**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)元素（对于可激活的扩展）的**uap10： HostId**特性。

下面的示例演示了一个未签名的托管应用程序包清单的相关部分。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

请注意以下有关以下元素的重要详细信息。

| 元素              | 详细信息 |
|----------------------|-------|
| [**标识**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | 由于本示例中的托管应用包是无符号的，因此**发布服务器**特性`OID.2.25.311729368913984317654407730594956997722=1`必须包含字符串。 这可确保未签名的包无法欺骗签名包的标识。 |
| [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | **MinVersion**特性必须指定10.0.19041.0 或更高版本的 OS 版本。 |
| [**uap10:HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | 此元素元素声明主机应用包上的依赖关系。 这包括主机包的名称和**发布服务器****以及它所依赖的文件****名称**。 这些值可在主机包的[标识](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素下找到。 |
| [**应用程序**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) | **Uap10： HostId**属性表示主机上的依赖关系。 托管应用包必须为[**应用程序**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)或[**扩展**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)元素声明此特性，而不是常用的**可执行文件**和**入口点**特性。 因此，托管应用从具有相应**HostId**值的主机继承**可执行文件**、**入口点**和运行时属性。<br/><br/>**Uap10： Parameters**特性指定传递给宿主可执行文件的入口点函数的参数。 由于主机需要知道要如何处理这些参数，主机和托管应用之间有隐含的协定。 |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>在运行时注册未签名的托管应用程序包

**Uap10： HostRuntime**扩展的一个优点是，它允许主机在运行时动态生成托管应用包，并使用[**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) API 注册它，而无需对其进行签名。 这样一来，主机就可以为所承载的应用包动态生成内容和清单，然后注册它。

使用[**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)类的以下方法来注册未签名的托管应用程序包。 从 Windows 10 版本2004开始提供这些方法。

* **AddPackageByUriAsync**：使用*Options*参数的**AllowUnsigned**属性注册未签名的 .msix 包。
* **RegisterPackageByUriAsync**：执行松散的包清单文件注册。 如果对包进行了签名，则包含清单的文件夹必须包含[时会文件](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package)和目录。 如果未签名，则必须设置*options*参数的**AllowUnsigned**属性。

### <a name="requirements-for-unsigned-hosted-apps"></a>未签名的托管应用的要求

* 包清单中的[**应用程序**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)或[**扩展**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)元素不能包含激活数据，如**Executable**、 **EntryPoint**或**TrustLevel**属性。 相反，这些元素只能包含表示主机上的依赖项的**uap10： HostId**特性和**uap10： Parameters**特性。
* 包必须是主包。 它不能是捆绑包、框架包、资源或可选包。

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>安装和注册未签名的托管应用程序包的主机的要求

* 主机必须具有[包标识](#define-a-host)。
* 主机必须具有**packageManagement** [限制的功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>示例

有关将自身声明为主机，然后在运行时动态注册托管应用包的功能完整的示例应用，请参阅[托管应用示例](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps)。

### <a name="the-host"></a>主机

主机名为**PyScriptEngine**。 这是用 c # 编写的包装程序，用于运行 python 脚本。 当使用`-Register`参数运行时，脚本引擎将安装包含 python 脚本的托管应用程序。 当用户尝试启动新安装的托管应用时，主机将启动并执行**NumberGuesser** python 脚本。

主机应用的包清单（PyScriptEnginePackage 文件夹中的 appxmanifest.xml 文件）包含一个**uap10： HostRuntime**扩展，该扩展将应用声明为 ID 为**PythonHost**的主机和可执行的**PyScriptEngine**。  

> [!NOTE]
> 在此示例中，包清单的名称为 appxmanifest.xml，它是[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)的一部分。 生成此项目时，它会[生成一个名为 appxmanifest.xml 的清单](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest)并为主机应用生成 .msix 包。

### <a name="the-hosted-app"></a>托管应用

托管应用由 python 脚本和包项目（如包清单）组成。 它不包含任何 PE 文件。

托管应用的包清单（NumberGuesser/Appxmanifest.xml 文件）包含以下项：

* [**标识**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素的**发行者**特性包含无符号包`OID.2.25.311729368913984317654407730594956997722=1`所需的标识符。
* [**应用程序**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**uap10： HostId**属性将**PythonHost**标识为其主机。

### <a name="run-the-sample"></a>运行示例

该示例需要10.0.19041.0 或更高版本的 Windows 10 和 Windows SDK。

1. 将[示例](https://aka.ms/hostedappsample)下载到开发计算机上的文件夹。
2. 在 Visual Studio 中打开 PyScriptEngine 解决方案，并将**PyScriptEnginePackage**项目设置为启动项目。
3. 生成**PyScriptEnginePackage**项目。
4. 在解决方案资源管理器中，右键单击**PyScriptEnginePackage**项目并选择 "**部署**"。
5. 在将示例文件复制到的目录中打开一个命令提示符窗口，并运行以下命令以注册示例**NumberGuesser**应用（托管应用）。 更改`D:\repos\HostedApps`为复制示例文件的路径。

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > 可以在命令`pyscriptengine`行上运行，因为示例中的主机声明了[**AppExecutionAlias**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)。

6. 打开 "**开始**" 菜单，单击 " **NumberGuesser** " 运行托管应用。
