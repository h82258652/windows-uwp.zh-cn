---
author: drewbatgit
ms.assetid: 
description: "本文展示如何利用仅在移动设备上提供的特殊相机 UI 功能。"
title: "移动设备的相机 UI 功能"
translationtype: Human Translation
ms.sourcegitcommit: 77d1709cd42253c229b01df21ae3416e57c1c2ab
ms.openlocfilehash: ec437d7111b1490f52bfc53b3ad2cd06f0c66ef3

---

#移动设备的相机 UI 功能

本文展示如何利用仅在移动设备上提供的特殊相机 UI 功能。 

## 将移动扩展添加到项目 

若要使用这些功能，必须将对用于通用应用平台的 Microsoft 移动扩展 SDK 的引用添加到你的项目中。

**添加对硬件相机按钮支持的移动扩展 SDK 的引用**

1.  在“解决方案资源管理器”****中，右键单击“引用”****，然后选择“添加引用”****。

2.  展开“Windows 通用”****节点，然后选择“扩展”****。

3.  选中“适用于通用应用平台的 Microsoft 移动扩展 SDK”****复选框。

## 隐藏状态栏

移动设备具有向用户提供有关设备的状态信息的 [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 控件。 此控件占用屏幕上的空间，这可能会干扰媒体捕获 UI。 你可以通过调用 [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) 来隐藏状态栏，但你必须从你使用 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 方法以确定 API 是否在其中可用的条件块内执行此调用。 此方法将仅在支持状态栏的移动设备上返回 True。 当应用启动时或当你开始从相机预览时，应该隐藏状态栏。

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

当应用关闭时或者用户退出应用的媒体捕获页面时，可以使该控件再次可见。

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## 使用硬件相机按钮

某些移动设备具有专用硬件相机按钮，相比屏幕控件，一些用户更倾向于使用它们。 若要在按下硬件相机按钮时收到通知，请注册 [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 事件的处理程序。 由于此 API 仅适用于移动设备，必须再次使用 **IsTypePresent** 以确保在尝试访问该 API 之前，它在当前设备上受支持。

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

在 **CameraPressed** 事件的处理程序中，你可以启动照片捕获。

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

当应用关闭或者用户离开应用的媒体捕获页面时，请注销该硬件按钮处理程序。

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

> [!NOTE]
> 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。                                                                                   |

## 相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)








<!--HONumber=Aug16_HO3-->


