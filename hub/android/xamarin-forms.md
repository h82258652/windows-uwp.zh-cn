---
title: 使用 Xamarin 创建简单的 Android 应用
description: 如何开始编写带有 Xamarin 的 Android 应用程序
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、xaml、教程
ms.date: 04/28/2020
ms.openlocfilehash: a1426bfef9863227c1ac110bc295536786695df7
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255191"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>使用 Xamarin 进行 Android 开发入门

本指南将帮助你开始在 Windows 上使用 Xamarin. Forms 来创建适用于 Android 设备的跨平台应用。

在本文中，你将使用 Xamarin 和 Visual Studio 2019 创建一个简单的 Android 应用。

## <a name="requirements"></a>要求

若要使用本教程，你将需要以下各项：

- Windows 10
- [Visual Studio 2019：社区版、专业版或企业版](https://visualstudio.microsoft.com/downloads/)（请参阅备注）
- Visual Studio 2019 的 "采用 .NET 的移动开发" 工作负荷

> [!NOTE]
> 本指南适用于 Visual Studio 2017 或2019。 如果使用的是 Visual Studio 2017，某些说明可能不正确，因为 Visual Studio 的两个版本之间的 UI 差别。

你还可以使用 Android 手机或配置的模拟器来运行应用。 请参阅[在 Android 设备或模拟器上测试](emulator.md)。

## <a name="create-a-new-xamarinforms-project"></a>创建新的 Xamarin. Forms 项目

启动 Visual Studio。 单击 "文件" > 新建 > 项目 "创建新项目。

在 "新建项目" 对话框中，选择 "**移动应用（Xamarin）** " 模板，然后单击 "**下一步**"。

将项目命名为**TimeChangerForms** ，然后单击 "**创建**"。

在 "新建跨平台应用" 对话框中，选择 "**空白**"。 在 "平台" 部分中，选中 " **Android** " 并取消选中所有其他框。 单击" **确定**"。

Xamarin 将创建包含两个项目的新解决方案： **TimeChangerForms**和**TimeChangerForms** 。

## <a name="create-a-ui-with-xaml"></a>使用 XAML 创建 UI

展开 " **TimeChangerForms** " 项目并打开 " **MainPage**"。 此文件中的 XAML 定义打开 TimeChanger 时用户将看到的第一个屏幕。

TimeChanger 的 UI 非常简单。 它显示当前时间，并具有可按一小时的增量调整时间的按钮。 它使用垂直 StackLayout 对齐按钮上方的时间，并使用水平 StackLayout 并排排列这些按钮。 通过将垂直 StackLayout 的**HorizontalOptions**和**VerticalOptions**设置为 **"CenterAndExpand"**，内容在屏幕上居中。

将 MainPage 的内容替换为以下代码。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

此时，UI 已经完成。 不过，TimeChangerForms 不会生成，因为在 XAML 中引用**UpButton_Clicked**和**DownButton_Clicked**方法，但在任何位置都未定义。 即使应用程序已运行，当前时间也不会显示。 在下一部分中，你将修复这些错误并向你的 UI 添加功能。

## <a name="add-logic-code-with-c"></a>用 C 添加逻辑代码#

在解决方案资源管理器中，右键单击 MainPage，然后单击 "**查看代码**"。 此文件包含将向 UI 添加功能的隐藏代码。

### <a name="set-the-current-time"></a>设置当前时间

此文件中的代码可以使用控件的**x：Name**特性的值引用在 XAML 中声明的控件。 在这种情况下，将调用`time`显示当前时间的标签。

UI 控件必须在主线程上更新。 从另一个线程所做的更改可能会在屏幕上显示的控件时无法正确更新。 因为无法保证此代码始终在主线程上运行，所以请使用**BeginInvokeOnMainThread**方法来确保所有更新正确显示。 下面是完整的 UpdateTimeLabel 方法。

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>每秒更新一次当前时间

此时，TimeChangerForms 启动后，当前时间最多可精确到一秒钟。 必须定期更新标签以保持准确的时间。 **计时器**对象会定期调用回调方法，该方法使用当前时间更新标签。

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>添加 HourOffset

向上和向下按钮按一小时的增量调整时间。 添加**HourOffset**属性以跟踪当前调整。

```csharp
public int HourOffset { get; private set; }
```

现在更新 UpdateTimeLabel 方法，以了解 HourOffset 属性。

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>添加按钮单击事件处理程序

所有的向上和向下按钮都需要增加或减少 HourOffset 属性，然后调用 UpdateTimeLabel。

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

完成后，MainPage.xaml.cs 应如下所示：

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>运行应用

若要运行应用，请按**F5**或单击 "调试" > "开始调试"。 根据你的[调试器的配置](emulator.md)方式，你的应用将在设备上或在仿真程序中启动。

## <a name="related-links"></a>相关链接

- [在 Android 设备或仿真程序上测试](emulator.md)。

- [使用 Xamarin 创建 Android 示例应用](xamarin-android.md)
