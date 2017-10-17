---
title: Unity for XDK with IL2CPP backend
author: KevinAsgari
description: Add Xbox Live support to Unity for XDK with IL2CPP scripting backend for ID@Xbox and managed partners
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, Unity
ms.openlocfilehash: 33cb39f20bb8b45495a384d8b0206bf07ba43aae
ms.sourcegitcommit: 1eb0a87dd723f47c6dba292bcc716df363ace72c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Add Xbox Live support to Unity for XDK with IL2CPP scripting backend for ID@Xbox and managed partners

## <a name="overview"></a>Overview

Windows Runtime Support for IL2CPP in Unity

With the release of Unity 5.6f3 the engine has included a new feature that enables developers to use Windows Runtime (WinRT) components directly in script by including them in the game project directly. Until 5.6 developers have needed a plugin, or dll to support any platform feature (including Xbox Live SDK) from game script. This new projection layer removes the plugin requirement, and introduces a new and simplified workflow supported only with games that choose the IL2CPP scripting backend.

For more information on how to get started, see the Unity documentation: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Steps

**1) Install Unity**

Install Unity 5.6 or higher, and ensure you have the Xbox One editor extension installed.

**2) Install Visual Studio Tools for Unity version 3.1 and above for IntelliSense support when using WinMDs** For Visual Studio 2015, this can be found at https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  For Visual Studio 2017, the component can be added inside the Visual Studio 2017 installer.

**3) Open a new or existing Unity project**

**4) Switch the platform to Xbox One in the Unity Build Settings menu**

**5) Enable IL2CPP scripting backend in the Unity player settings, and set API compatibility to .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6) 将脚本编译器切换到 Roslyn**

**7) 相应的 Xbox One 系统库将全部自动添加到你的项目中，并且不需要执行额外的步骤来加入平台二进制文件。**

**8) 将新的 C\# 脚本添加并附加到 Unity 对象中。**

For example, click on a Unity object such as the "Main Camera", and click "Add Component" \| "New Script" \| C\# Script \| and name it "XboxLiveScript". 可对任何游戏对象执行此操作。

**9) 在 Visual Studio（已安装 VSTU 3.1+）中打开脚本**

你将看到两个项目，在 VSTU 生成的“Player”项目中打开你的游戏脚本 XboxLiveTest.cs

![](../images/unity/unity-il2cpp-2.png)

This is a special project generated for XDK, and includes references for the winmd files you have placed in your assets.
它还会为你定义“#if ENABLE_WINMD_SUPPORT”，以使 IntelliSense 和语法突出显示功能正常工作。

**10) 将以下 Xbox Live 代码添加到 XboxLiveTest.cs 源文件中**

```cpp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11) 确保你在播放器设置的发布设置中选择了“InternetClient”功能**

![](../images/unity/unity-il2cpp-3.png)

**12) 在 Unity 中生成项目。**
