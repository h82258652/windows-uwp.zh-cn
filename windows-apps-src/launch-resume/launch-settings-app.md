---
author: TylerMSFT
title: "启动 Windows 设置应用"
description: "了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 ms-settings URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。"
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e4cc17bf268ddb470c3c64dfe3e471053d8fca55
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-windows-settings-app"></a>启动 Windows 设置应用

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 **ms-settings:** URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。

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

此外，你的应用可以调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以从代码启动**“设置”**应用。 此示例介绍了如何使用 `ms-settings:privacy-webcam` URI 启动到相机的隐私设置页面。

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

上述代码会启动相机的隐私设置页面：

![相机隐私设置。](images/privacyawarenesssettingsapp.png)

有关启动 URI 的详细信息，请参阅[启动 URI 的默认应用](launch-default-app.md)。

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 方案引用

使用以下 URI 以打开“设置”应用的各个页面。 请注意，受支持的 SKU 列可指示设置页面是存在于 Windows 10 桌面版（家庭版、专业版、企业版和教育版）、Windows 10 移动版中，还是同时存在于这两者中。

<table border="1">
    <tr>
        <th>类别</th>
        <th>设置页面</th>
        <th>受支持的 SKU</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>主页</td>
        <td>“设置”的登录页面</td>
        <td>两者</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">系统</td>
        <td>屏幕</td>
        <td>两者</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>通知和操作</td>
        <td>两者</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>手机</td>
        <td>仅限移动设备</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>消息处理</td>
        <td>仅限移动设备</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>节电模式</td>
        <td>两者<br>仅在具有电池（如平板电脑）的设备上可用</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>电池使用</td>
        <td>两者<br>仅在具有电池（如平板电脑）的设备上可用</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>电源和睡眠</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>关于</td>
        <td>两者</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>加密</td>
        <td>两者</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>离线地图</td>
        <td>两者</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">设备</td>
        <td>默认相机</td>
        <td>仅限移动设备</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>蓝牙</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>已连接的设备</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>鼠标和触摸板</td>
        <td>两者<br>触摸板设置仅在具有触摸板的设备上可用</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">网络和无线</td>
        <td>NFC</td>
        <td>两者</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>WLAN</td>
        <td>两者</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>飞行模式</td>
        <td>两者</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">网络和 Internet</td>
        <td>数据使用量</td>
        <td>两者</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>手机网络和 SIM 卡</td>
        <td>两者</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>移动热点</td>
        <td>两者</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>代理</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>状态</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">个性化</td>
        <td>个性化（类别）</td>
        <td>两者</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>背景</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>颜色</td>
        <td>两者</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>声音</td>
        <td>仅限移动设备 </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>锁屏界面</td>
        <td>两者</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">帐户</td>
        <td>访问工作或学校帐户</td>
        <td>两者</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>电子邮件和应用帐户</td>
        <td>两者</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>家人和其他人</td>
        <td>两者</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>登录选项</td>
        <td>两者</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>同步设置</td>
        <td>两者</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>其他人</td>
        <td>两者</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>你的信息</td>
        <td>两者</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">时间和语言</td>
        <td>日期和时间</td>
        <td>两者</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>区域和语言</td>
        <td>仅限桌面设备</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">轻松使用</td>
        <td>讲述人</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>放大镜</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>高对比度 </td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>隐藏式字幕</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>键盘</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>鼠标</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>其他选项</td>
        <td>两者</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">隐私</td>
        <td>位置</td>
        <td>两者</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>相机</td>
        <td>两者</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>麦克风</td>
        <td>两者</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>动作</td>
        <td>两者</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>语音、墨迹书写和键入</td>
        <td>两者</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>帐户信息</td>
        <td>两者</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>联系人</td>
        <td>两者</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>日历</td>
        <td>两者</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>呼叫历史记录</td>
        <td>两者</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>电子邮件</td>
        <td>两者</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>消息处理</td>
        <td>两者</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>无线电收发器</td>
        <td>两者</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>后台应用</td>
        <td>两者</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>其他设备</td>
        <td>两者</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>反馈和诊断</td>
        <td>两者</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>更新和安全</td>
        <td>对于开发人员</td>
        <td>两者</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>

