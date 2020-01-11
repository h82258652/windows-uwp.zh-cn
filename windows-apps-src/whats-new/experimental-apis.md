---
title: 实验性 API
description: 了解实验性 API
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 实验性, api
ms.localizationpriority: medium
ms.openlocfilehash: 542e007d07d490c2f18077e646f7598bfd2587c3
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684912"
---
# <a name="experimental-apis"></a>实验性 API

实验性 API 正处于设计的早期阶段，可能会随着所有者纳入反馈而发生变化，并添加对其他场景的支持。

这些 API 通过 [Windows 预览体验成员 SDK](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) 进行外部测试，以便开发人员可以在它们成为官方平台的一部分之前进行试用并提供反馈。 它们在进行外部测试时，无法承诺稳定性或承诺使用量。

## <a name="consuming-experimental-apis"></a>使用实验性 API
IntelliSense 会告知你 API 是否为实验性 API。 在使用实验性 API 时也会收到编译器警告，例如“...仅用于评估目的，在未来的更新中可能会更改或删除”。

这些警告有助于防止你在生产项目中创建对实验性 API 的依赖项。 对于实验性项目，可以忽略或禁用这些警告。

默认情况下，这些 API 在运行时被禁用，调用它们会导致运行时异常。 这是另一个安全措施，有助于防止使用实验性 API 的应用产生无意依赖项以及分布过于广泛。

若要启用这些 API 进行实验，可使用目标设备上的 [Windows 设备门户 (WDP)](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal) 功能插件，以启用对应于想要调用的 API 的功能。

特定实验性 API 的文档由拥有它的团队自主决定。

## <a name="providing-feedback"></a>提供反馈

如果已试用实验性 API，并且想要提供反馈，请使用 [Windows 反馈中心](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub)中的“开发人员平台”类别  。