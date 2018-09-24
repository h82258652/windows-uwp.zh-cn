---
title: 编译 XDK Xbox Live API 源
author: KevinAsgari
description: 了解如何编译 Xbox 开发人员工具包 (XDK) 附带的 Xbox Live API 源。
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 301d646b421e77044f613244ccab987f1cb40dc6
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4149167"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>编译 Xbox 开发人员工具包 (XDK) Xbox Live API 源

Xbox 开发人员工具包 (XDK) 包括用于生成 Microsoft.Xbox.Services.dll (XSAPI) 的源。 开发人员可以遵循以下说明更新其项目，以使用 DLL 的本地生成。

如果出现以下情况，你可能要自行生成 XSAPI：
1. 希望调试错误以了解错误代码的来源。
1. 如果我们在分发 QFE 之前，为你提供了可修复问题的源代码修补程序。

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>要自行编译 XDK C++ XSAPI 项目

<ol>
  <li> 获取 Microsoft.Xbox.Services 源。 若要执行此操作，所有文件都提取"%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip"到"C:\Program Files (x86)"，或者你的可写入文件夹可以克隆源<a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 如果你的项目引用预生成 DLL，则需要删除此引用</li>
    <ul>
      <li> 对于 Visual Studio 2012：在 Visual Studio 中选择“项目 ->引用...”。 如果已将 Xbox 服务 API 列为引用，请选择它，然后单击“删除引用”。 单击“确定”并保存项目文件。</li>
      <li> 对于 Visual Studio 2015 或 2017：在 Visual Studio 中 选择“项目”->“添加引用…”。 如果 Xbox 服务 API 为选中状态，请取消选中。 单击“确定”并保存项目文件。</li>
    </ul>
  <li> 如果正在使用 XDK 生成，请在 Visual Studio 中依次选择“文件”->“添加”->“现有项目…”， 将以下两个项目添加到应用程序的解决方案中。 vcxproj 文件将位于将源提取到的文件夹中。</li>
对于 Visual Studio 2017： <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
对于 Visual Studio 2015： <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
对于 Visual Studio 2012： <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> 通过选择“项目”->“引用...”，然后选择“添加引用”，将源项目添加为引用。 在“解决方案”->“项目”下，勾选上述两个项目的条目，然后单击“确定”。</li>
    <li> 通过依次单击“视图”->“其他 Windows”->“属性管理器”，将属性文件添加到项目中，右键单击项目，选择“添加现有属性表”，最后，在 SDK 源根目录中选择 xsapi.staticlib.props 文件。  如果生成系统不支持属性文件，则必须手动添加预处理器定义和库，如 %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props 中所示</li>
    <li> 通过右键单击项目的“添加”->“现有项”并选择 {SDK source root}\Include\xsapi\services.h 文件，将 services.h 文件添加到应用源中</li>
    <li> 确保两个应用程序项目和 Xbox 服务项目的“输出文件夹”相同。 可在 Visual Studio 项目的“属性”->“配置属性”->“常规”->“输出目录”中查找此设置。</li>
    <li> 重新生成 Visual Studio 解决方案</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>要自行编译 XDK WinRT XSAPI 项目

<ol>
  <li> 获取 Microsoft.Xbox.Services 源。 若要执行此操作，所有文件都提取"%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip"到"C:\Program Files (x86)"，或者你的可写入文件夹可以克隆源<a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 如果你的项目引用预生成 DLL，则需要删除此引用</li>
    <ul>
      <li> 对于 Visual Studio 2012：在 Visual Studio 中选择“项目 ->引用...”。 如果已将 Xbox 服务 API 列为引用，请选择它，然后单击“删除引用”。 单击“确定”并保存项目文件。</li>
      <li> 对于 Visual Studio 2015 或 2017：在 Visual Studio 中 选择“项目”->“添加引用…”。 如果 Xbox 服务 API 为选中状态，请取消选中。 单击“确定”并保存项目文件。</li>
    </ul>
  <li> 如果正在使用 XDK 生成，请在 Visual Studio 中依次选择“文件”->“添加”->“现有项目…”， 将以下两个项目添加到应用程序的解决方案中。 Vcxproj 文件将位于将源提取到的文件夹中。  对于 Visual Studio 2015，项目会自动将升级为 VS2015 格式。</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> 在 Visual Studio 中添加引用：</li>
    <ul>
      <li> 对于 Visual Studio 2012：在 Visual Studio 中选择“项目 ->引用...”，然后选择“添加引用”。 在“解决方案”->“项目”下，将上述两个项目的条目选中，然后单击“确定”。</li>
      <li> 对于 Visual Studio 2015 或 2017：在 Visual Studio 中 选择“项目”->“添加引用…”。 在“解决方案”下，将上述两个项目的条目选中，然后单击“确定”。</li>
    </ul>
  <li> 确保两个应用程序项目和 Xbox 服务项目的“输出文件夹”相同。 可在 Visual Studio 项目的“属性”->“配置属性”->“常规”->“输出目录”中查找此设置。</li>
  <li> 重新生成 Visual Studio 解决方案</li>
</ol>
