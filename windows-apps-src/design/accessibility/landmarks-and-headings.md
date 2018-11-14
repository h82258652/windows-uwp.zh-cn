---
author: Xansky
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: 标志和标题
label: Landmarks and Headings
template: detail.hbs
ms.author: mhopkins
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 250ed555e6fcf7dc40d31d89a40fa7a96295aacf
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6468874"
---
# <a name="landmarks-and-headings"></a>标志和标题

用户界面通常以直观高效的方式进行组织，从而允许视力正常的用户快速浏览自己感兴趣的内容，无需放慢速度来阅读*全部*内容。 屏幕阅读器用户需要这种相同的浏览能力。 标志和标题定义帮助辅助技术 (AT) 用户更高效导航的用户界面的各个部分。 将内容标记为标志和标题可为屏幕阅读器用户提供与视力正常的用户所采用方式类似的浏览内容的选项。

[ARIA 标志](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page)、[ARIA 标题](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html) 和 [HTML 标题](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html)的概念数年来一直用在 Web 内容中，以允许屏幕阅读器用户更快地浏览。 网页利用标志和标题通过允许 AT 用户快速到达大区块（标志）和较小区块（标题）来使其内容更易于使用。 具体来说，屏幕阅读器具有允许用户在标志之间跳转和在标题之间跳转（下一个/上一个或特定标题级别）的命令。 由于同样原因，请务必考虑你的应用中的标志和标题。

标志启用要分组到各个类别（如搜索、导航、主内容等）中的内容。 分组后，AT 用户可以在各个组之间快速导航。 这种快速导航允许用户跳过潜在的大量内容，这些内容在以前可能必须按项目进行导航，从而产生糟糕的体验。 

例如，当使用标签面板时，将此视为导航标志。 当使用搜索编辑框时，将此视为搜索标志并考虑将你的主内容设置为主内容标志。 无论是在标志内还是在标志外，请考虑将子元素设置为具有逻辑标题级别的标题。 

请考虑使用 Windows 设置应用中的**轻松访问**页面。 

![Windows 设置应用中的“轻松访问”页面](images/EaseOfAccessSettings.png)  

搜索标志内有一个环绕的搜索编辑框。 左侧的导航元素环绕在导航标志内，右侧的主内容环绕在主内容标志内。 更进一步说明，在导航标志内，有一个名为**轻松访问**的主组，其为标题级别 1。 在此之下是子选项**视觉**、**听觉**，依此类推。 这些具有标题级别 2。 设置标题针对设置主要主题继续在主内容内进行，**显示** 作为标题级别 1 而且子组（如**使一切变大**作为标题级别 2。 

设置应用将可供访问，无需标志和标题，但会变得更易于使用它们。 屏幕阅读器用户可以快速轻松地到达其所需的组（标志），然后也快速到达子组（标题）。 

使用 [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) 将 UI 元素设置为所需的[标志类型](https://msdn.microsoft.com/library/windows/desktop/mt759299)。 此标志 UI 元素将封装对于该标志来说有意义的所有其他 UI 元素。 

使用 [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) 专门命名该标志。 如果你选择预定义的标志类型，如主或导航，这些名称将用于标志名称。 但是，如果将标志类型设置为自定义，你必须通过此属性专门命名标志。 你还可以使用此属性覆盖非自定义标志类型中的默认名称。 

使用 [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) 将 UI 元素设置为从 *Level1* 到 *Level9* 内某特定级别的标题。

