---
Description: Design your app to provide bidirectional text support (BiDi) so that you can combine script from left-to-right (LTR) and right-to-left (RTL) writing systems, which generally contain different types of alphabets.
title: 针对双向文本设计应用
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 66a158a96fcab5391030f4517b6420ba4585bf04
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7978815"
---
# <a name="design-your-app-for-bidirectional-text"></a>针对双向文本设计应用

设计应用，使其提供双向文本支持（双向），以便组合从右到左 (RTL) 和从左到右 (LTR) 写入系统的脚本，这些系统通常包含不同类型的字母。

从右到左写入系统（如在中东、中亚和南亚及非洲使用的系统）具有独特的设计要求。 这些写入系统要求双向文本支持（双向）。 双向支持是指能够按照从右到左 (RTL) 或从左到右 (LTR) 的顺序输入和显示文本布局。

Windows 共包含 9 种双向语言。
- 两种完全本地化的语言。 阿拉伯语和希伯来语。
- 7 个用于新兴市场的语言界面包。 波斯语、乌尔都语、达里语、中库尔德语、信德语、旁遮普语（巴基斯坦）和维吾尔语。

本主题包括 Windows 双向设计理念，以及展示双向设计注意事项的案例研究。

## <a name="bidi-design-elements"></a>双向设计元素

有 4 个元素会影响 Windows 中的双向设计决策。

- **用户界面 (UI) 镜像**。 用户界面 (UI) 排列允许从右到左的内容按其原始布局显示。 UI 设计需能够打造双向市场的本地体验。
- **用户体验一致性**。 从右到左方向的设计感觉更自然。 UI 元素共享一致的布局方向，并按照用户的预期显示。
- **触控优化**。 与非镜像 UI 中的触控 UI 类似，各元素使用方便，触控交互也很自然。
- **混合文本支持**。 文本方向支持可实现出色的混合文本呈现（双向版本中有英语文本，反之亦然）。

## <a name="feature-design-overview"></a>功能设计概述

Windows 支持 4 种双向设计元素。 我们来看看 Windows 中的一些主要相关功能，并提供一些关于这些功能如何影响应用的内容。

### <a name="navigate-in-the-direction-that-feels-natural"></a>以感觉舒适自然的方向浏览

Windows 调整排字网格的方向，使其从右到左排列，这意味着网格上的第一个磁贴会放置在右上角，最后一个磁贴放置在左下角。 这与书籍、杂志等印刷出版物（其阅读模式始终从右上角开始，向左排列）的 RTL 模式匹配。

![双向“开始”菜单](images/56283_BIDI_01_startscreen_resized.png)
![具有超级按钮的双向“开始”菜单](images/56283_BIDI_02_startscreen_charm_resized.png)

为了保持一致的 UI 排列，磁贴上的内容保留了从右到左的布局，这意味着无论应用 UI 语言如何，应用名称和徽标都位于磁贴的右下角。

#### <a name="bidi-tile"></a>双向磁贴

![双向磁贴](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>英语磁贴

![英语磁贴](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>获取正确读取的磁贴通知

磁贴支持混合文本。 通知区域具有根据通知语言调整文本对齐方式的内置灵活性。  应用发送阿拉伯语、希伯来语或其他双向语言通知时，文本向右对齐。 英语（或其他 LTR）通知到达时，文本向左对齐。

![磁贴通知](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>易于触控的一致 RTL 用户体验

Windows UI 中的每个元素都适合 RTL 方向。 超级按钮和浮出控件位于屏幕的左边缘，这样它们就不会与搜索结果重叠或削弱触控优化。 可用拇指轻松地点击它们。

![双向屏幕截图](images/56286_BIDI_05_search_flyout_resized.png)
![双向屏幕截图](images/56286_BIDI_06_print_flyout_resized.png)

![双向屏幕截图](images/56286_BIDI_07_settings_flyout_resized.png)
![双向屏幕截图](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>任意方向的文本输入

Windows 提供一个干净整洁的屏幕触摸键盘。 对于双向语言，有一个文本方向控件键，这样可以根据需要切换文本输入方向。

![双向语言的触摸键盘](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>以任意语言使用任意应用

以任意语言安装和使用最喜欢的应用。 应用的显示和功能与其在非双向版本 Windows 上的显示和功能相同。 应用内的元素始终放置于可预测的一致位置上。

![显示双向内容的英语应用](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>正确显示圆括号

随着双向圆括号算法 (BPA) 的引入，无论语言或文本对齐属性如何，成对的圆括号始终可以正确显示。

#### <a name="incorrect-parentheses"></a>错误的圆括号

![圆括号错误的双向应用](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>正确的圆括号

![圆括号正确的双向应用](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>版式

Windows 为所有双向语言使用 Segoe UI 字体。 这种字体针对 Windows UI 设计，并且可缩放。

![双向语言的 Segoe UI 字体](images/56290_BIDI_13_start_screen_segoe.png)
![双向语言的 Segoe UI 字体](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>案例研究 #1：双向音乐应用

### <a name="overview"></a>概述

多媒体应用会带来一项非常有趣的设计挑战，因为媒体控件通常应该具有与非双向语言类似的从左到右的布局。

![媒体控件（从左到右）](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![从右到左媒体控件](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>建立 UI 方向性

保持从右到左的 UI 流对于双向市场中的设计一致性十分重要。 在此上下文中添加具有从左到右流向的元素比较困难，这是因为一些导航元素（如“后退”按钮）可能与音频控件中“后退”按钮的方向冲突。

![音乐应用跟踪页面](images/56292_BIDI_16_app_layout_callouts_resized.png)

此音乐应用保留从右到左方向的网格。 这让已按照此方向浏览 Windows UI 的应用用户感觉非常自然。 流是通过以下方式保持的：确保主要元素不但按从右到左的顺序排序而且在节标题中正确对齐，以帮助维护 UI 流。

![音乐应用唱片集页面](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>文本处理

以上屏幕截图中的艺术家个人简介采用左对齐，而其他与艺术家相关的文本片段（如唱片集和曲目名称）保持右对齐。 个人简介字段是一个相当大的文本元素，向右对齐时阅读效果不佳，因为阅读较宽的文本块时难以在行之间跟踪。 通常，对于任何文本元素，只要包含五个或五个以上字词的行超过两行或三行，就要考虑是否会出现类似的对齐异常，其中文本块对齐方式与总体应用方向布局的对齐方式相反。

在整个应用中设置对齐方式看似很简单，但经常暴露出呈现引擎在跨双向字符串放置中性字符时的某些限制和局限。 例如，以下字符串的显示形式可能因对齐方式而异。

| | 英语字符串 (LTR) | 希伯来语字符串 (RTL) |
| -------------- | ------------------- | ------------------- |
| **左对齐** | Hello, World! | בוקר טוב! |
| **右对齐** | !Hello, World | !בוקר טוב |

要确保艺术家信息正确地显示在音乐应用中，开发团队将文本布局属性与对齐分开。 换言之，在许多情况下，艺术家信息可能会显示为右对齐，但字符串布局调整是基于自定义后台处理设置的。 后台处理根据字符串的内容确定最佳方向布局设置。

![音乐应用艺术家页面](images/56292_BIDI_18_app_layout_callouts_resized.png)

例如，如果没有自定义字符串布局处理，则艺术家姓名“The Contoso Band.” 会显示为“.The Contoso Band”。

### <a name="specialized-string-direction-preprocessing"></a>专用字符串方向预处理

应用与服务器取得联系以获取媒体元数据时，会先对每个字符串进行预处理，然后再向用户显示。 在此预处理过程中，应用还会进行转换，使文本方向一致。 为此，应用会检查字符串末尾是否存在中性字符。 此外，如果字符串的文本方向与 Windows 语言设置中的应用方向相反，则会在后面附加（有时在前面追加）Unicode 方向标记。 转换函数如下所示。

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

添加的 Unicode 字符宽度为零，因此不会影响字符串间距。 此代码可能会对性能产生负面影响，因为检测字符串方向需要对整个字符串进行检测，除非遇到非中性字符。 首先还会对要检查其是否为中性的每个字符与几个 Unicode 范围进行比较，因此这是一项重要检查。

## <a name="case-study-2-a-bidi-mail-app"></a>案例研究 #2：双向邮件应用

### <a name="overview"></a>概述

在 UI 布局要求方面，邮件客户端的设计相当简单。 默认对 Windows 中的邮件应用进行镜像。 从文本处理角度来看，邮件应用需要具有更强大的文本显示和组合功能才能适应混合文本场景。

### <a name="establishing-ui-directionality"></a>建立 UI 方向性

对邮件应用的 UI 布局进行镜像。 三个窗格已进行重定向，使文件夹窗格位于屏幕的右边缘，然后其左侧依次是邮件项目列表窗格、电子邮件组合窗格。

![镜像的邮件应用](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

其他项目已进行重定向，使其匹配整个 UI 排列和触控优化。 这些项目包括应用栏及撰写、回复和删除图标。

![通过应用栏镜像的邮件应用](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>文本处理

#### <a name="ui"></a>UI

UI 中的文本对齐方式通常为右对齐。 其中包括文件夹窗格和项目窗格。 项目窗格限于两行文本（地址和标题）。 若要在内容方向与 UI 方向排列相反时保留从右到左的对齐方式，而不引入难以阅读的文本块，这一点非常重要。

#### <a name="text-editing"></a>文本编辑

文本编辑要求能够以从右到左和从左到右的形式进行组合。 此外，必须通过使用富文本等能够保存方向信息的格式保留组合布局。

![邮件应用（从左到右）](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![邮件应用（从右到左）](images/56294_BIDI_22_email_orientation_RtL_resized.png)
