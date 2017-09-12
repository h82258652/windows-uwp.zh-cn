---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: "照明"
description: "Light 对象与 SceneLightingEffect 结合使用来模拟动态照明和反射性。"
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.openlocfilehash: 3f922cae8aa0787f8be6496997df1021dda8e142
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2017
---
# <a name="lighting"></a>照明

[**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) 对象与 [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 结合使用来模拟动态照明和反射性。

可以将光应用于[**视觉对象**](https://msdn.microsoft.com/library/windows/apps/Dn706858)和 XAML [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)。

## <a name="applying-lights-to-xaml-uielements"></a>将光应用于 XAML UIElement

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) 对象用于应用 [**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) 以动态照明 XAML UIElement。 XamlLight 提供了方法以用于面向 UIElement 或其他 XAML 画笔、将光应用于 UIElement 树以及帮助管理 CompositionLight 资源的生存期（根据它们当前是否已使用）。

* 如果使用 XamlLight 面向某个 **Brush**，则光会照亮使用该画笔的任何 UIElement 的位置。
* 如果使用 XamlLight 面向某个 **UIElement**，则光会照亮整个 UIElement 及其子 UIElement。

## <a name="creating-and-using-a-xamllight"></a>创建和使用 XamlLight

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) 是基类，可以用于创建自定义光。

下面是使用附加属性面向特定元素的自定义 XamlLight 的示例：

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        CompositionLight = Window.Current.Compositor.CreateSpotLight();
        CompositionLight.InnerConeColor = Colors.Orange;
        CompositionLight.OuterConeColor = Colors.Yellow;
        CompositionLight.InnerConeAngleInDegrees = 30;
        CompositionLight.OuterConeAngleInDegrees = 45;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

下面是演示上面定义的自定义光的不同可能用法的示例：

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> 仅对于“最低版本”等于 Windows 10 创意者更新或更高版本的应用，才支持如以上示例所示在标记中设置 UIElement.Lights。 对于面向早期版本的应用，必须在代码隐藏中创建光。

## <a name="additional-resources"></a>其他资源

* [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 中的高级 UI 和合成示例。