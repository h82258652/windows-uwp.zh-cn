---
author: TylerMSFT
title: "启动 Windows 设置应用"
description: "了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 ms-settings URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。"
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.sourcegitcommit: 3cf9dd4ab83139a2b4b0f44a36c2e57a92900903
ms.openlocfilehash: e52a4245e8697a68bfc5c5605dc54e5ea510c662

---

# 启动 Windows 设置应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 **ms-settings:** URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。

启动为设置应用是编写隐私感知应用的重要组成部分。 如果你的应用无法访问敏感资源，我们建议为用户提供到该资源的隐私设置的方便链接。 有关详细信息，请参阅[隐私感知应用指南](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## 如何启动“设置”应用


如果隐私设置不允许你的应用访问敏感资源，我们建议在“设置”****应用中提供到该隐私设置的方便链接。 这将使用户更容易更改他们的设置。

若要直接启动到**“设置”**应用，请使用以下示例中所示的 `ms-settings:` URI 方案。

在此示例中，超链接 XAML 控件用于使用 `ms-settings:privacy-microphone` URI 启动麦克风的隐私设置页面。

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

此外，你的应用可以调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以从代码启动**“设置”**应用。

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

此示例介绍了如何使用 `ms-settings:privacy-webcam` URI 启动到相机的隐私设置页面。

![相机隐私设置。](images/privacyawarenesssettingsapp.png)

有关启动 URI 的详细信息，请参阅[启动 URI 的默认应用](launch-default-app.md)。

## ms-settings: URI 方案引用


使用以下 URI 以打开“设置”应用的各个页面。 请注意，受支持的 SKU 列可指示设置页面是存在于 Windows 10 桌面版（家庭版、专业版、企业版和教育版）、Windows 10 移动版中，还是同时存在于这两者中。

| 类别           | 设置页面                          | 受支持的 SKU | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| 主页          | “设置”的登录页面              | 两者           | ms-settings:                              |
| 系统             | 屏幕                                | 两者           | ms-settings:screenrotation                |
|                    | 通知和操作                | 两者           | ms-settings:notifications                 |
|                    | 手机                                  | 仅限移动设备    | ms-settings:phone                         |
|                    | 消息处理                              | 仅限移动设备    | ms-settings:messaging                     |
|                    | 节电模式                          | 带有电池的设备（如平板电脑）上的移动设备和桌面设备    | ms-settings:batterysaver                  |
|                    | 节电模式/节电模式设置 | 带有电池的设备（如平板电脑）上的移动设备和桌面设备 | ms-settings:batterysaver-settings         |
|                    | 节电模式/电池使用            | 带有电池的设备（如平板电脑）上的移动设备和桌面设备    | ms-settings:batterysaver-usagedetails     |
|                    | 电源和睡眠                          | 仅限桌面设备   | ms-settings:powersleep                    |
|                    | 桌面：关于                         | 两者           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | 移动：设备加密              |                |                                           |
|                    | 离线地图                           | 两者           | ms-settings:maps                          |
|                    | 关于                                  | 两者           | ms-settings:about                         |
| 设备            | 默认相机                         | 仅限移动设备    | ms-settings:camera                        |
|                    | 蓝牙                              | 仅限桌面设备   | ms-settings:bluetooth                     |
|                    | 鼠标和触摸板                       | 两者           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | 两者           | ms-settings:nfctransactions               |
| 网络和无线 | WLAN                                  | 两者           | ms-settings:network-wifi                  |
|                    | 飞行模式                          | 两者           | ms-settings:network-airplanemode          |
| 网络和 Internet | 数据使用量                             | 两者           | ms-settings:datausage                     |
|                    | 手机网络和 SIM 卡                         | 两者           | ms-settings:network-cellular              |
|                    | 移动热点                         | 两者           | ms-settings:network-mobilehotspot         |
|                    | 代理                                  | 两者           | ms-settings:network-proxy                 |
| 个性化    | 个性化（类别）             | 两者           | ms-settings:personalization               |
|                    | 背景                             | 仅限桌面设备   | ms-settings:personalization-background    |
|                    | 颜色                                 | 两者           | ms-settings:personalization-colors        |
|                    | 声音                                 | 仅限移动设备    | ms-settings:sounds                        |
|                    | 锁屏界面                            | 两者           | ms-settings:lockscreen                    |
| 帐户           | 你的电子邮件和帐户                | 两者           | ms-settings:emailandaccounts              |
|                    | 工作访问                            | 两者           | ms-settings:workplace                     |
|                    | 同步设置                     | 两者           | ms-settings:sync                          |
| 时间和语言  | 日期和时间                            | 两者           | ms-settings:dateandtime                   |
|                    | 区域和语言                      | 仅限桌面设备   | ms-settings:regionlanguage                |
| 轻松使用     | 讲述人                               | 两者           | ms-settings:easeofaccess-narrator         |
|                    | 放大镜                              | 两者           | ms-settings:easeofaccess-magnifier        |
|                    | 高对比度                          | 两者           | ms-settings:easeofaccess-highcontrast     |
|                    | 隐藏式字幕                        | 两者           | ms-settings:easeofaccess-closedcaptioning |
|                    | 键盘                               | 两者           | ms-settings:easeofaccess-keyboard         |
|                    | 鼠标                                  | 两者           | ms-settings:easeofaccess-mouse            |
|                    | 其他选项                          | 两者           | ms-settings:easeofaccess-otheroptions     |
| 隐私            | 位置                               | 两者           | ms-settings:privacy-location              |
|                    | 相机                                 | 两者           | ms-settings:privacy-webcam                |
|                    | 麦克风                             | 两者           | ms-settings:privacy-microphone            |
|                    | 动作                                 | 两者           | ms-settings:privacy-motion                |
|                    | 语音、墨迹书写和键入                | 两者           | ms-settings:privacy-speechtyping          |
|                    | 帐户信息                           | 两者           | ms-settings:privacy-accountinfo           |
|                    | 联系人                               | 两者           | ms-settings:privacy-contacts              |
|                    | 日历                               | 两者           | ms-settings:privacy-calendar              |
|                    | 呼叫历史记录                           | 两者           | ms-settings:privacy-callhistory           |
|                    | 电子邮件                                  | 两者           | ms-settings:privacy-email                 |
|                    | 消息处理                              | 两者           | ms-settings:privacy-messaging             |
|                    | 无线电收发器                                 | 两者           | ms-settings:privacy-radios                |
|                    | 后台应用                        | 两者           | ms-settings:privacy-backgroundapps        |
|                    | 其他设备                          | 两者           | ms-settings:privacy-customdevices         |
|                    | 反馈和诊断                 | 两者           | ms-settings:privacy-feedback              |
| 更新和安全  | 对于开发人员                         | 两者           | ms-settings:developers                    |
 



<!--HONumber=Jun16_HO5-->


