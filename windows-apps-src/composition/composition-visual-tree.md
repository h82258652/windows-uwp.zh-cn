---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: 合成视觉对象
description: 合成视觉对象组成了合成 API 的所有其他功能均可使用和基于的可视化树结构。 该 API 允许开发人员定义并创建一个或多个可视化对象，其中每个对象表示可视化树中的单个节点。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b1c0b78ca45d98428f38518b337b5889f595c49
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943375"
---
# <a name="composition-visual"></a>合成视觉对象

合成视觉对象组成了合成 API 的所有其他功能均可使用和基于的可视化树结构。 该 API 允许开发人员定义并创建一个或多个可视化对象，其中每个对象表示可视化树中的单个节点。

## <a name="visuals"></a>视觉对象

可视树结构的组成部分包括以下三种视觉类型，以及影响视觉对象内容的多个子类的基本画笔类：

- [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 基对象，大部分属性均位于此处且继承自其他视觉对象。
- [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – 派生自 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)，并添加了创建子视觉对象的功能。
- [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – 派生自 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) 并添加了关联画笔的功能，以便视觉对象可以呈现像素，包括图像、效果或纯色。

可以使用 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) 及其子类（包括 [**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)、[**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 和 [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)）将内容和效果应用于 SpriteVisual。 若要了解有关画笔的详细信息，请参阅 [**CompositionBrush 概述**](https://docs.microsoft.com/windows/uwp/composition/composition-brushes)。

## <a name="the-compositionvisual-sample"></a>CompositionVisual 示例

在此，我们将看一些演示前面列出的三种不同视觉类型的示例代码。 尽管此示例不介绍诸如动画或更复杂的效果等概念，但它包含所有这些系统需要使用的构成要素。 （本文的结尾列出了完整的示例代码）。

在该示例中，提供大量可以在屏幕上单击和拖动的纯色正方形。 在单击某一正方形后它将凸显， 将其旋转 45 度，然后在拖动该正方形时它将变为不透明。

此处显示了大量关于使用 API 的基本概念，其中包括：

- 创建合成器
- 使用 CompositionColorBrush 创建 SpriteVisual
- 剪裁可视对象
- 旋转可视对象
- 设置不透明度
- 更改集合中视觉对象的位置。

## <a name="creating-a-compositor"></a>创建合成器

创建 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 并存储它以用作某一变量中的工厂对象是一项简单的任务。 以下代码段演示了如何创建新的 **Compositor**：

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>创建 SpriteVisual 和 ColorBrush

使用 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 可随时根据需要轻松创建对象，如 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 和 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399)：

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

虽然该代码段只有几行代码，但它演示了一个强大的概念：即 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 对象，此类对象是效果系统的核心。 在创建颜色、图形和效果方面，**SpriteVisual** 具有出色的灵活性和互动性。 **SpriteVisual** 是单一视觉对象类型，该对象可以使用画笔填充 2D 矩形；在本示例中为纯色。

## <a name="clipping-a-visual"></a>剪裁可视对象

[**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 还可以用于创建 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 的剪裁。 下面的例子来自于使用 [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) 剪裁可视对象每一侧的示例：

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

与 API 中的其他对象一样，[**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) 也可以将动画应用到其属性。

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>旋转剪裁

[**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 可以通过旋转进行转换。 请注意，[**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) 同时支持弧度和度数。 其默认采用弧度为单位，不过也可以轻松指定以度数为单位，如以下代码段中所示：

```cs
child.RotationAngleInDegrees = 45.0f;
```

Rotation 仅仅是 API 提供一组转换组件以供简化这些任务的示例之一。 此外，还包括 Offset、Scale、Orientation、RotationAxis 和 4x4 TransformMatrix。

## <a name="setting-opacity"></a>设置不透明度

使用浮点值可轻松设置视觉对象的不透明度。 例如，在该示例中所有正方形的不透明度一开始都为 .8：

```cs
visual.Opacity = 0.8f;
```

与 Rotation 一样，[**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) 属性也进行动画处理。

## <a name="changing-the-visuals-position-in-the-collection"></a>更改集合中视觉对象的位置

借助合成 API，可以采用多种方式更改 [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection) 中视觉对象的位置。 使用 [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove) 可将该对象放置在其他视觉对象的上方、使用 [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow) 可放置在下方、使用 [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop) 可移动到顶部，而使用[**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom) 可移动到底部。

在示例中，如果在 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象上单击，该对象将排列到顶部：

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>完整示例

在完整示例中，上述所有概念可一起用于构造和浏览 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象的一个简单树，以便可以在不使用 XAML、WWA 或 DirectX 的情况下更改不透明度。 此示例显示了如何创建和添加子 **Visual** 对象，以及如何更改属性。

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```
