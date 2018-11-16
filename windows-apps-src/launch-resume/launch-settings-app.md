---
author: TylerMSFT
title: 启动 Windows 设置应用
description: 了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 ms-settings URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 03/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b86298ca671282dea201e3088bc60845231fe007
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7119467"
---
# <a name="launch-the-windows-settings-app"></a>启动 Windows 设置应用


**重要的 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何启动 Windows"设置"应用。 本主题介绍 **ms-settings:** URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。

启动为设置应用是编写隐私感知应用的重要组成部分。 如果你的应用无法访问敏感资源，我们建议为用户提供到该资源的隐私设置的方便链接。 有关详细信息，请参阅[隐私感知应用指南](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## <a name="how-to-launch-the-settings-app"></a>如何启动“设置”应用

若要启动**设置**应用，请使用以下示例中所示的 `ms-settings:` URI 方案。

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

此外，你的应用可以调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法来启动**设置**应用。 此示例演示如何使用 `ms-settings:privacy-webcam` URI 启动到相机的隐私设置页面。

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

上述代码会启动相机的隐私设置页面：

![相机隐私设置。](images/privacyawarenesssettingsapp.png)

有关启动 URI 的详细信息，请参阅[启动 URI 的默认应用](launch-default-app.md)。

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 方案引用

使用以下 URI 以打开“设置”应用的各个页面。

> 请注意，设置页面是否可用因 Windows SKU 而异。 并非 Windows 10 桌面版中所有可用的设置在 Windows 10 移动版上也都可用，反之亦然。 备注列中还记录了使页面可用所必须满足的附加要求。

## <a name="accounts"></a>帐户

|“设置”页面| URI |
|-------------|-----|
| 访问工作或学校帐户 | ms-settings:workplace |
| 电子邮件和应用帐户  | ms-settings:emailandaccounts |
| 家人和其他人 | ms-settings:otherusers |
| 登录选项 | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| 将设置进行同步 | ms-settings:sync |
| Windows Hello 设置 | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| 你的信息 | ms-settings:yourinfo |

## <a name="apps"></a>应用

|“设置”页面| URI |
|-------------|-----|
| 应用和功能 | ms-settings:appsfeatures |
| 应用功能 | ms-settings:appsfeatures-app（应用的重置、管理加载项和可下载内容等操作）|
| 网站应用 | ms-settings:appsforwebsites |
| 默认应用 | ms-settings:defaultapps |
| 管理可选功能 | ms-settings:optionalfeatures |
| 离线地图 | ms-settings:maps |
| 启动应用 | ms-settings:startupapps |
| 视频播放 | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|“设置”页面| URI |
|-------------|-----|
| 权限和历史记录 | ms-settings:cortana-permissions |
| 更多详细信息 | ms-settings:cortana-moredetails |
| 在我的设备上的 Cortana | ms-settings:cortana-notifications |
| 与 Cortana 交谈 | ms-settings:cortana-language |

> [!NOTE] 
> 在桌面上设置本节时将会调用搜索电脑设置为区域中的 Cortana 当前不可用或已禁用 Cortana 的位置。 在此情况下不会列出特定于 Cortana 的页面 (在我的设备，Cortana) 和 cortana 交谈。 

## <a name="devices"></a>设备

|“设置”页面| URI |
|-------------|-----|
| 音频和语音 | ms-settings:holographic-audio（仅在安装混合现实门户应用后可用 - 可通过 Microsoft Store 获取） |
| 自动播放 | ms-settings:autoplay |
| 蓝牙 | ms-settings:bluetooth |
| 已连接的设备 | ms-settings:connecteddevices |
| 默认相机 | ms-settings:camera |
| 鼠标和触摸板 | ms-settings:mousetouchpad（仅具有触摸板的设备可使用触摸板设置） |
| 触控笔和 Windows Ink | ms-settings:pen |
| 打印机和扫描仪 | ms-settings:printers |
| 触摸板 | ms-settings:devices-touchpad（仅在存在触摸板硬件时可用） |
| 键入 | ms-settings:typing |
| USB | ms-settings:usb |
| 滚轮 | ms-settings:wheel（仅在“拨号”配对成功后可用） |
| 你的手机 | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>轻松使用

|“设置”页面| URI |
|-------------|-----|
| 音频 | ms-settings:easeofaccess-audio |
| 隐藏式字幕 | ms-settings:easeofaccess-closedcaptioning |
| 颜色筛选器 | ms-设置： easeofaccess-colorfilter |
| 屏幕 | ms-settings:easeofaccess-display |
| 目视控制 | ms-settings:easeofaccess-eyecontrol |
| 字体 | ms-settings:fonts |
| 高对比度 | ms-settings:easeofaccess-highcontrast |
| 全息头戴显示设备 | ms-settings:holographic-headset（需要全息硬件） |
| 键盘 | ms-settings:easeofaccess-keyboard |
| 放大镜 | ms-settings:easeofaccess-magnifier |
| 鼠标 | ms-settings:easeofaccess-mouse |
| 讲述人 | ms-settings:easeofaccess-narrator |
| 其他选项 | ms-settings:easeofaccess-otheroptions |
| 语音 | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>附加

|“设置”页面| URI |
|-------------|-----|
| 附加 | ms-settings:extras（仅在安装了“设置应用”后可用，例如，通过第三方安装） |

## <a name="gaming"></a>游戏

|“设置”页面| URI |
|-------------|-----|
| 广播 | ms-settings:gaming-broadcasting |
| 游戏栏 | ms-settings:gaming-gamebar |
| 游戏 DVR | ms-settings:gaming-gamedvr |
| 游戏模式 | ms-settings:gaming-gamemode |
| 全屏玩游戏 | ms-settings:quietmomentsgame |
| TruePlay | ms-settings:gaming-trueplay |
| Xbox 网络 | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>主页

|“设置”页面| URI |
|-------------|-----|
| “设置”主页 | ms-settings: |


## <a name="network--internet"></a>网络和 Internet

|“设置”页面| URI |
|-------------|-----|
| 飞行模式 | ms-settings:network-airplanemode（在 Windows 8.x 上使用 ms-settings:proximity） |
| 手机网络和 SIM 卡 | ms-settings:network-cellular |
| 数据使用量 | ms-settings:datausage |
| 拨号 | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess（仅在启用 DirectAccess 后可用） |
| 以太网 | ms-settings:network-ethernet |
| 管理已知网络 | ms-settings:network-wifisettings |
| 移动热点 | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| 代理 | ms-settings:network-proxy |
| 状态 | ms-settings:network-status |
| VPN | ms-settings:network-vpn |
| WLAN | ms-settings:network-wifi（仅当设备具有 WLAN 适配器时可用） |
| WLAN 呼叫 | ms-settings:network-wificalling（仅在启用 WLAN 呼叫后可用） |

## <a name="personalization"></a>个性化

|“设置”页面| URI |
|-------------|-----|
| 背景 | ms-settings:personalization-background |
| 选择哪些文件夹显示在“开始”菜单上 | ms-settings:personalization-start-places |
| 颜色 | ms-settings:personalization-colors |
| 概览 | ms-settings:personalization-glance |
| 锁屏界面 | ms-settings:lockscreen |
| 导航栏 | ms-settings:personalization-navbar |
| 个性化（类别） | ms-settings:personalization |
| “开始”菜单 | ms-settings:personalization-start |
| 任务栏 | ms-settings:taskbar |
| 主题 | ms-settings:themes |

## <a name="phone"></a>电话

|“设置”页面| URI |
|-------------|-----|
| 你的手机 | ms-settings:mobile-devices  |

## <a name="privacy"></a>隐私

|“设置”页面| URI |
|-------------|-----|
| 外部设备应用 | ms-settings:privacy-accessoryapps |
| 帐户信息 | ms-settings:privacy-accountinfo |
| 活动历史记录 | ms-settings:privacy-activityhistory |
| 广告 ID | ms-settings:privacy-advertisingid |
| 应用诊断 | ms-settings:privacy-appdiagnostics |
| 自动文件下载 | ms-settings:privacy-automaticfiledownloads |
| 后台应用 | ms-settings:privacy-backgroundapps |
| 日历 | ms-settings:privacy-calendar |
| 呼叫历史记录 | ms-settings:privacy-callhistory |
| 相机 | ms-settings:privacy-webcam |
| 联系人 | ms-settings:privacy-contacts |
| 文档 | ms-settings:privacy-documents |
| 电子邮件 | ms-settings:privacy-email |
| 眼球跟踪器 | ms-settings:privacy-eyetracker（需要眼球跟踪器硬件） |
| 反馈和诊断 | ms-settings:privacy-feedback |
| 文件系统 | ms-settings:privacy-broadfilesystemaccess |
| 常规 | ms-settings:privacy-general |
| 位置 | ms-settings:privacy-location |
| 消息 | ms-settings:privacy-messaging |
| 麦克风 | ms-settings:privacy-microphone |
| 动作 | ms-settings:privacy-motion |
| 通知 | ms-settings:privacy-notifications |
| 其他设备 | ms-settings:privacy-customdevices |
| 图片 | ms-settings:privacy-pictures |
| 电话呼叫 | ms-settings:privacy-phonecall |
| 无线电收发器 | ms-settings:privacy-radios |
| 语音、墨迹书写和键入 |ms-settings:privacy-speechtyping |
| 任务 | ms-settings:privacy-tasks |
| 视频 | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|“设置”页面| URI |
|-------------|-----|
| 帐户 | ms-settings:surfacehub-accounts |
| 会话清理 | ms-settings:surfacehub-sessioncleanup |
| 团队会议 | ms-settings:surfacehub-calling |
| 团队设备管理 | ms-settings:surfacehub-devicemanagenent |
| 欢迎屏幕 | ms-settings:surfacehub-welcome |

## <a name="system"></a>系统

|“设置”页面| URI |
|-------------|-----|
| 关于 | ms-settings:about |
| 高级显示设置 | ms-settings:display-advanced（仅适用于支持高级显示选项的设备） |
| 节电模式 | ms-settings:batterysaver（仅在具有电池的设备[如平板电脑]上可用） |
| “节电模式”设置 | ms-settings:batterysaver-settings（仅在具有电池的设备[如平板电脑]上可用） |
| 电池使用 | ms-settings:batterysaver-usagedetails（仅在具有电池的设备[如平板电脑]上可用） |
| 屏幕 | ms-settings:display |
| 默认保存位置 | ms-settings:savelocations |
| 屏幕 | ms-settings:screenrotation |
| 复制我的屏幕 | ms-settings:quietmomentspresentation |
| 在这些时间内 | ms-settings:quietmomentsscheduled |
| 加密 | ms-settings:deviceencryption |
| 专注助手 | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| 图形设置 | ms-settings:display-advancedgraphics（仅适用于支持高级图形选项的设备） |
| 消息 | ms-settings:messaging |
| 多任务 | ms-settings:multitasking |
| 夜灯设置 | ms-settings:nightlight |
| 电话 | ms-settings:phone-defaultapps |
| 投影到这台电脑 | ms-settings:project |
| 共享体验 | ms-settings:crossdevice |
| 平板电脑模式 | ms-settings:tabletmode |
| 任务栏 | ms-settings:taskbar |
| 通知和操作 | ms-settings:notifications |
| 远程桌面 | ms-settings:remotedesktop |
| 电话 | ms-settings:phone |
| 电源和睡眠 | ms-settings:powersleep |
| 声音 | ms-settings:sounds |
| 存储 | ms-settings:storagesense |
| 存储感知 | ms-settings:storagepolicies |

## <a name="time-and-language"></a>时间和语言

|“设置”页面| URI |
|-------------|-----|
| 日期和时间 | ms-settings:dateandtime |
| 日本输入法设置 | ms-settings:regionlanguage-jpnime（在安装了 Microsoft 日本输入法编辑器的情况下可用） |
| 拼音输入法设置 | ms-settings:regionlanguage-chsime-pinyin（在安装了 Microsoft 拼音输入法编辑器的情况下可用） |
| 区域和语言 | ms-settings:regionlanguage |
| 语音语言 | ms-settings:speech |
| 五笔输入法设置  | ms-settings:regionlanguage-chsime-wubi（在安装了 Microsoft 五笔输入法编辑器的情况下可用） |

## <a name="update--security"></a>更新和安全

|“设置”页面| URI |
|-------------|-----|
| 激活 | ms-settings:activation |
| 备份 | ms-settings:backup |
| 传递优化 | ms-settings:delivery-optimization |
| 查找我的设备 | ms-settings:findmydevice |
| 适用于开发人员 | ms-settings:developers |
| 恢复 | ms-settings:recovery |
| 疑难解答 | ms-settings:troubleshoot |
| Windows 安全中心 | ms-settings:windowsdefender |
| Windows 预览体验计划 | ms-settings:windowsinsider（仅当用户在 WIP 中注册时显示） |
| Windows 更新 | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows 更新 - 高级选项 | ms-settings:windowsupdate-options |
| Windows 更新 - 重启选项 | ms-settings:windowsupdate-restartoptions |
| Windows 更新 - 查看更新历史记录 | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>用户帐户

|“设置”页面| URI |
|-------------|-----|
| 预配 | ms-settings:workplace-provisioning（仅在企业部署了预配包后可用） |
| 预配 | ms-settings:workplace-provisioning（仅在移动设备和企业部署了预配包后可用） |
| Windows Anywhere | ms-settings:windowsanywhere（设备必须支持 Windows Anywhere） |
