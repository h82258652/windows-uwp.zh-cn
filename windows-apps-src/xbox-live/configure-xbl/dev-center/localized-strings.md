---
title: 本地化字符串
description: 了解如何将本地化字符串在合作伙伴中心
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 游戏、 uwp、 windows 10，Xbox，本地化字符串，合作伙伴中心
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656592"
---
# <a name="configuring-localized-strings-in-partner-center"></a>在合作伙伴中心配置本地化字符串

可以使用此页面将所有 Xbox Live 配置本地化为你的游戏支持的所有语言。 在所有后续 Xbox Live 页面中创建的所有服务配置都将添加到你要下载的文件中。

可以使用[合作伙伴中心](https://partner.microsoft.com/dashboard)配置与您的游戏相关联的所有语言的本地化的字符串。 通过执行以下操作添加配置：

1. 导航至作品中的**本地化字符串**部分（位于**服务** > **Xbox Live** > **本地化字符串**下）。
2. 单击**下载**按钮会将 localization.xml 文件下载到本地计算机上。

![合作伙伴中心中的已本地化的字符串配置页的屏幕截图](../../images/dev-center/localized-strings/localized-strings-1.png)

3. 可以通过复制添加本地化的字符串 <Value locale="en-US">迷宫播放</Value> 标记和区域设置的值更改为所选的语言和本地化的字符串的值。 要避免错误的开发人员显示区域设置中，您必须具有至少一个值标记。

![编辑本地化字符串](../../images/dev-center/localized-strings/localized-strings.gif)

4. 添加所有本地化字符串后，可以通过拖动或浏览本地计算机来上传文件。

![上传 localization.xml 文件的按钮的图像](../../images/dev-center/localized-strings/localized-strings-2.png)

请注意，上传 localization.xml 文件时可能会出现以下错误：

| 错误 | 原因 |
|---------------------------|-------------|
| 失败的 XSD 验证：命名空间中的元素 LocalizedString http://config.mgt.xboxlive.com/schema/localization/1不能包含文本。 应使用的元素的列表：命名空间中的 ' value'http://config.mgt.xboxlive.com/schema/localization/1 | XML 文档格式不正确时会发生此情况 |
| 本地化字符串缺少开发人员显示区域设置的项目 | 本地化字符串缺少其区域设置与开发人员显示区域设置不匹配的项目时，会发生这种情况 |
| 失败的 XSD 验证：区域设置属性无效-值是根据其数据类型无效 http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString-模式约束失败。 | 发生这种情况时缺少中的区域设置值的已本地化的字符串。 <Value> tag|
