---
title: 使用 Xamarin 创建简单的 Android 应用
description: 如何开始编写带有 Xamarin 的 Android 应用
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin、教程、xaml
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255201"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>使用 Xamarin 进行 Android 开发入门

本指南将帮助你开始在 Windows 上使用 Xamarin，以创建可在 Android 设备上工作的跨平台应用。

在本文中，你将使用 Xamarin 和 Visual Studio 2019 创建一个简单的 Android 应用。

## <a name="requirements"></a>要求

若要使用本教程，你将需要以下各项：

- Windows 10
- [Visual Studio 2019：社区版、专业版或企业版](https://visualstudio.microsoft.com/downloads/)（请参阅备注）
- Visual Studio 2019 的 "采用 .NET 的移动开发" 工作负荷

> [!NOTE]
> 本指南适用于 Visual Studio 2017 或2019。 如果使用的是 Visual Studio 2017，某些说明可能不正确，因为 Visual Studio 的两个版本之间的 UI 差别。

你还可以使用 Android 手机或配置的模拟器来运行应用。 请参阅[配置 Android 模拟器](emulator.md)。

## <a name="create-a-new-xamarinandroid-project"></a>创建新的 Xamarin.Android 项目

启动 Visual Studio。 选择 "文件" > 新建 > 项目以创建新项目。

在 "新建项目" 对话框中，选择 " **Android 应用（Xamarin）** " 模板，然后单击 "**下一步**"。

将项目命名为**TimeChangerAndroid** ，然后单击 "**创建**"。

在 "新建跨平台应用" 对话框中，选择 "**空白应用**"。 在**最低 Android 版本**中，选择 " **Android 5.0 （棒糖形）**"。 单击" **确定**"。

Xamarin 会创建一个新的解决方案，其中包含一个名为**TimeChangerAndroid**的项目。

## <a name="create-a-ui-with-xaml"></a>使用 XAML 创建 UI

在项目的**Resources\layout**目录中，打开**activity_main .xml**。 此文件中的 XML 定义打开 TimeChanger 时用户将看到的第一个屏幕。

TimeChanger 的 UI 非常简单。 它显示当前时间，并具有按钮，以一小时为增量调整时间。 它使用垂直`LinearLayout`来对齐按钮上方的时间，并使用水平`LinearLayout`来并排排列按钮。 内容在屏幕上居中，方法是将**android：重力**特性设置**center**为垂直`LinearLayout`居中。

将**activity_main**的内容替换为以下代码。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

此时，可以运行**TimeChangerAndroid**并查看已创建的 UI。 在下一部分中，你将向用户界面添加功能，显示当前时间和启用按钮以执行操作。

## <a name="add-logic-code-with-c"></a>用 C 添加逻辑代码#

打开**MainActivity.cs**。 此文件包含将向 UI 添加功能的代码隐藏逻辑。

### <a name="set-the-current-time"></a>设置当前时间

首先，获取一个对的引用`TextView` ，它将显示时间。 使用**FindViewById**搜索具有正确**android： ID**的所有 UI 元素（在上一步的 xml 中设置`"@+id/timeDisplay"`为）。 这是`TextView`将显示当前时间的。

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

UI 控件必须在 UI 线程上更新。 从另一个线程所做的更改可能会在屏幕上显示的控件时无法正确更新。 因为无法保证此代码始终在 UI 线程上运行，所以请使用**RunOnUiThread**方法来确保所有更新正确显示。 下面是完整`UpdateTimeLabel`的方法。

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>每秒更新一次当前时间

此时，TimeChangerAndroid 启动后，当前时间最多可精确到一秒钟。 必须定期更新标签以保持准确的时间。 **计时器**对象会定期调用回调方法，该方法使用当前时间更新标签。

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>创建按钮单击事件处理程序

所有的向上和向下按钮都需要增加或减少 HourOffset 属性，然后调用 UpdateTimeLabel。

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>将 "向上" 和 "向下" 按钮连接到相应的事件处理程序

若要将按钮与相应的事件处理程序相关联，请首先使用 FindViewById 按其 id 查找按钮。 引用 button 对象后，可以向其`Click`事件添加事件处理程序。

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>已完成的 MainActivity.cs 文件

完成后，MainActivity.cs 应如下所示：

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>运行应用程序

若要运行应用，请按**F5**或单击 "调试" > "开始调试"。 根据你的[调试器的配置](emulator.md)方式，你的应用将在设备上或在仿真程序中启动。

## <a name="related-links"></a>相关链接

- [在 Android 设备或仿真程序上测试](emulator.md)。
- [使用 Xamarin 创建 Android 示例应用](xamarin-forms.md)
