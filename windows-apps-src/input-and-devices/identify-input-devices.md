---
author: Karl-Bridge-Microsoft
Description: 标识连接到通用 Windows 平台 (UWP) 设备的输入设备，并标识其功能和属性。
title: 标识输入设备
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
---

# 标识输入设备

标识连接到通用 Windows 平台 (UWP) 设备的输入设备，并标识其功能和属性。

**重要的 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


## <span id="Retrieve_mouse_properties"></span><span id="retrieve_mouse_properties"></span><span id="RETRIEVE_MOUSE_PROPERTIES"></span>检索鼠标属性


[
            **Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空间包含 [**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626) 类，用于检索由一个或多个连接的鼠标公开的属性。 只需创建新的 **MouseCapabilities** 对象并获取感兴趣的属性。

**注意** 此处讨论的属性返回的值基于所有检测到的鼠标：如果至少一个鼠标支持特定功能，则布尔属性返回非零值，数值属性返回任何一个鼠标公开的最大值。

 

以下代码使用一系列 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素来显示各个鼠标属性和值。

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <span id="Retrieve_keyboard_properties"></span><span id="retrieve_keyboard_properties"></span><span id="RETRIEVE_KEYBOARD_PROPERTIES"></span>检索键盘属性


[
            **Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空间包含 [**KeyboardCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225623) 类，用于检索是否已连接键盘。 只需创建新的 **KeyboardCapabilities** 对象并获取 [**KeyboardPresent**](https://msdn.microsoft.com/library/windows/apps/br225625) 属性。

以下代码使用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素来显示键盘属性和值。

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <span id="Retrieve_touch_properties"></span><span id="retrieve_touch_properties"></span><span id="RETRIEVE_TOUCH_PROPERTIES"></span>检索触摸属性


[
            **Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空间包含 [**TouchCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225644) 类，用于检索是否已连接任何触摸数字化器。 只需创建新的 **TouchCapabilities** 对象并获取感兴趣的属性。

**注意** 此处讨论的属性返回的值基于所有检测到的触摸数字化器：如果至少一个数字化器支持特定功能，则布尔属性返回非零值，数值属性返回任何一个数字化器公开的最大值。

 

以下代码使用一系列 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素来显示触摸属性和值。

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <span id="Retrieve_pointer_properties"></span><span id="retrieve_pointer_properties"></span><span id="RETRIEVE_POINTER_PROPERTIES"></span>检索指针属性


[
            **Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空间包含 [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633) 类，用于检索是否所有检测到的设备都支持指针输入（触摸、触摸板、鼠标或笔）。 只需创建新的 **PointerDevice** 对象并获取感兴趣的属性。

**注意** 此处讨论的属性返回的值基于所有检测到的指针设备：如果至少一个设备支持特定功能，则布尔属性返回非零值，数值属性返回任何一个指针设备公开的最大值。

 

以下代码使用一个表格来显示每个指针设备的属性和值。

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <span id="related_topics"></span>相关文章


**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)

**存档示例**
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
 

 






<!--HONumber=May16_HO2-->


