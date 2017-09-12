---
author: normesta
Description: "演示如何手动打包用于 Windows 10 的 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）。"
Search.Product: eADQiWindows 10XVcnh
title: "手动打包应用（桌面桥）"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.openlocfilehash: e8a09b6e362662b9bb207117d8a3fcc905da6ef4
ms.sourcegitcommit: ae93435e1f9c010a054f55ed7d6bd2f268223957
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2017
---
# <a name="package-an-app-manually-desktop-bridge"></a>手动打包应用（桌面桥）

本主题演示如何在不使用 Visual Studio 或 Desktop App Converter (DAC) 等工具的情况下打包应用。

<div style="float: left; padding: 10px">
    ![手动流程](images/desktop-to-uwp/manual-flow.png)
</div>

若要手动打包应用，请创建程序包清单文件，然后运行命令行工具生成 Windows 应用包。

如果使用 xcopy 命令安装应用，或者熟悉应用的安装程序对系统进行的更改并且想要更精确地控制进程，请考虑手动打包。

如果不确定安装程序会对系统进行哪些更改，或如果更希望使用自动化工具来生成程序包清单，请考虑任一[这些](desktop-to-uwp-root.md#convert)选项。

## <a name="first-consider-how-youll-distribute-your-app"></a>首先考虑如何分发应用
如果打算将应用发布到 [Windows 应用商店](https://www.microsoft.com/store/apps)中，请先填写[此表单](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)。 Microsoft 将联系你以开始加入过程。 在此过程中，你需要在应用商店中预留一个名称，然后获取对应用打包所需的信息。

## <a name="create-a-package-manifest"></a>创建程序包清单

创建文件，将其命名为 **appxmanifest.xml**，然后向其添加此 XML。

这是一个基本模板，其中包含程序包需要的元素和特性。 我们将在下一步部分中对它们添加值。

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>填写文件的程序包级别元素

在此模板中填写描述程序包的信息。

### <a name="identity-information"></a>标识信息

下面是一个带特性占位符文本的示例 **Identity** 元素。 可将 ``ProcessorArchitecture`` 特性设置为 ``x64`` 或 ``x86``。

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> 如果在 Windows 应用商店中预留了应用名称，则可以使用 Windows 开发人员中心仪表板获取“名称”和“发布者”。 如果打算将应用旁加载到其他系统中，只要选择的发布者名称与用于对应用进行签名的证书上的名称相匹配，就可以提供自己的名称。

### <a name="properties"></a>属性

[属性](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) 元素具有 3 个所需子元素。 下面是一个带元素占位符文本的示例**属性**节点。 **DisplayName** 是在应用商店中预留的应用名称，用于上传到应用商店的应用。

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>资源

下面是一个示例[资源](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources)节点。

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>依存关系

对于桌面桥应用，请始终将 ``Name`` 特性设置为 ``Windows.Desktop``。

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>功能
对于桌面桥应用，必须添加 ``runFullTrust`` 功能。

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>填写应用程序级别元素

在此模板中填写描述应用的信息。

### <a name="application-element"></a>应用程序元素

对于桌面桥应用，应用程序元素的 ``EntryPoint`` 特性始终为 ``Windows.FullTrustApplication``。

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>可视元素

下面是一个示例 [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 节点。

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```

## <a name="optional-add-target-based-unplated-assets"></a>（可选）添加基于目标的未着色资产

基于目标的资源是指显示在以下位置的图标和磁贴：Windows 任务栏、任务视图、ALT+TAB、贴靠助手和“开始”磁贴的右下角。 可在[此处](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets#target-based-assets)了解相关详细信息。

1. 获取正确的 44x44 图像，然后将它们复制到包含图像（即资产）的文件夹。

2. 对于每个 44x44 图像，在相同的文件夹中创建副本，并将 **.targetsize 44_altform unplated** 附加到文件名。 你应该有每个图标的两个副本，每个都以特定方式命名。 例如，完成该过程后，资产文件夹可能包含 **MYAPP_44x44.png** 和 **MYAPP_44x44.targetsize-44_altform unplated.png**。

   > [!NOTE]
   > 在此示例中，**MYAPP_44x44.png** 就是将在 Windows 应用包的 ``Square44x44Logo`` 徽标特性中引用的图标。

3.  在 Windows 应用包中，将制作的每个图标的 ``BackgroundColor`` 设置为透明。

4.  打开 CMD，将目录更改为程序包的根文件夹，然后运行命令 ``makepri createconfig /cf priconfig.xml /dq en-US`` 创建 priconfig.xml 文件。

5.  使用命令 ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml`` 创建 resources.pri 文件。

    例如，应用的命令可能如下所示：``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``。

6.  使用下一步中的说明打包 Windows 应用包以查看结果。

<span id="make-appx" />
## <a name="generate-a-windows-app-package"></a>生成 Windows 应用包

使用 **MakeAppx.exe** 为项目生成 Windows 应用包。 它随 Windows 10 SDK 提供，如果你已安装 Visual Studio，则可通过用于 Visual Studio 版本的开发人员命令提示符轻松访问它。

请参阅[使用 MakeAppx.exe 工具创建应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>运行打包的应用

可运行应用在本地进行测试，无需获得证书并对其进行签名。 只需运行此 PowerShell cmdlet：

```Add-AppxPackage –Register AppxManifest.xml```

若要更新应用的 .exe 或 .dll 文件，请将程序包中的现有文件替换为新文件、增加 AppxManifest.xml 中的版本号，然后再次运行上述命令。

> [!NOTE]
> 打包的应用始终作为交互用户运行，任何安装已打包应用的驱动器都必须格式化为 NTFS 格式。

## <a name="next-steps"></a>后续步骤

**逐步执行代码/查找并解决问题**

请参阅[运行、调试和测试打包的桌面应用（桌面桥）](desktop-to-uwp-debug.md)

**对应用进行签名，然后分发**

请参阅[分发打包的桌面应用（桌面桥）](desktop-to-uwp-distribute.md)

**查找特定问题的答案**

我们的团队会监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
