---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: 本部分介绍了如何在通用 Windows 平台 (UWP) 应用之间共享数据，包括如何使用“共享”合约、复制和粘贴以及拖放。
title: 应用到应用的通信
ms.date: 02/12/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2de9d5ae04c526c7c35fda4a7aef713814b4fbc2
ms.sourcegitcommit: 366c96d16e9a330bd4fbd9e1e19983f890f72642
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2020
ms.locfileid: "77258334"
---
# <a name="app-to-app-communication"></a>应用到应用的通信

本部分介绍了如何在通用 Windows 平台 (UWP) 应用之间共享数据，包括如何使用“共享”合约、复制和粘贴、拖放，以及应用服务。

“共享”合约是用户可以在应用之间快速交换数据的一种方式。 例如，用户可能希望使用社交网络应用与其好友共享网页，或者将链接保存在笔记应用中以供日后参考。 如果你的应用需要为处于另一个应用的上下文中的用户快速完成内容接收，则可以考虑使用“共享”合约。

应用可以通过两种方式支持“共享”功能。 首先，应用可以是[提供用户要共享的内容的源应用](share-data.md)。 其次，应用可以是[用户选择作为共享内容目标的目标应用](receive-data.md)。 一个应用也可以既是源应用，也是目标应用。 如果你希望你的应用作为源应用共享内容，则需要确定你的应用可以提供的数据格式。

除了“共享”合约外，应用还可以集成用于传输数据的经典技术，如[拖放](../design/input/drag-and-drop.md)或[复制和粘贴](copy-and-paste.md)。 除了在 UWP 应用之间通信外，这些方法还支持在桌面应用程序之间共享。

UWP 应用还可以创建为其他 UWP 应用提供功能的[应用服务](../launch-resume/how-to-create-and-consume-an-app-service.md)。 应用服务作为后台任务在主机应用中运行，并可向其他应用提供其服务。 例如，应用服务可能会提供其他应用可能使用的条形码扫描仪服务。

## <a name="in-this-section"></a>本部分内容

| 主题 | 说明 |
|-------|-------------|
| [共享数据](share-data.md) | 本文将说明如何在 UWP 应用中支持“共享”合约。 “共享”合约是一种在应用之间快速共享文本、链接、照片和视频等数据的简便方法。 例如，用户可能希望使用社交网络应用与其好友共享网页，或者将链接保存在笔记应用中以供日后参考。 |
| [接收数据](receive-data.md) | 本文介绍如何接收使用“共享”合约从另一个应用共享的 UWP 应用中的内容。 此“共享”合约允许在用户调用“共享”时，将你的应用表示为一个选项。 |
| [复制和粘贴](copy-and-paste.md) | 本文介绍如何支持在 UWP 应用中通过使用剪贴板进行复制和粘贴。 复制和粘贴是在应用之间或在应用内交换数据的传统方法，并且在一定程度上，几乎每个应用都可以支持剪贴板操作。 |
| [拖放](../design/input/drag-and-drop.md) | 本文说明如何将拖放操作添加到你的通用 UWP 应用中。 拖放是一种与内容（例如图像和文件）交互的经典且自然的方法。 实现后，拖放会在所有方向上无缝运行，包括应用到应用、应用到桌面和桌面到应用。 |
| [创建和使用应用服务](../launch-resume/how-to-create-and-consume-an-app-service.md) | 本文介绍如何在 UWP 应用中创建应用服务，以便向其他 UWP 应用提供服务。  |

## <a name="see-also"></a>另请参阅

- [开发 UWP 应用](https://docs.microsoft.com/windows/uwp/develop/)
