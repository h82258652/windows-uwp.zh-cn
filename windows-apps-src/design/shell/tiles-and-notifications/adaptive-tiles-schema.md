---
author: andrewleader
Description: Here are the elements and attributes you use to create adaptive tiles.
title: 自适应磁贴架构和模板
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30a0e3056f8b7be2ed9d033e2da57795aec6946f
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4688807"
---
# <a name="adaptive-tile-templates-schema-and-guidance"></a>自适应磁贴模板：架构和指南

 

以下是用于创建自适应磁贴的元素和属性。 有关说明和示例，请参阅[创建自适应磁贴](create-adaptive-tiles.md)。

## <a name="tile-element"></a>磁贴元素


``` xml
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <a name="visual-element"></a>可视元素


``` xml
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## <a name="binding-element"></a>绑定元素


``` xml
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## <a name="image-element"></a>图像元素


``` xml
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## <a name="text-element"></a>文本元素


``` xml
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

textStyle 值： caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## <a name="group-element"></a>组元素


``` xml
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <a name="subgroup-element"></a>子组元素


``` xml
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## <a name="related-topics"></a>相关主题


* [创建自适应磁贴](create-adaptive-tiles.md)
 

 




