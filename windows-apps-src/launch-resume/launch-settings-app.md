---
author: TylerMSFT
title: "启动 Windows 设置应用"
description: "了解如何从你的应用启动 Windows 设置应用。 本主题介绍了 ms-settings URI 方案。 使用此 URI 方案将 Windows 设置应用启动到特定设置页面。"
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>启动 Windows 设置应用

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

使用以下 URI 以打开“设置”应用的各个页面。

> 请注意，设置页面是否可用因 Windows SKU 而异。 并非 Windows 10 桌面版中所有可用的设置在 Windows 10 移动版上也都可用，反之亦然。 备注列中还记录了一些使页面可用所必须满足的附加要求。

<table border="1">
 <tr>
  <th>类别</th>
  <th>设置页面</th>
  <th>URI</th>
  <th>备注</th>
 </tr>
 <tr>
  <td rowspan="6">帐户</td>
  <td>访问工作或学校帐户</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>电子邮件和应用帐户</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>家人和其他人</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>登录选项</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>同步设置</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>你的信息</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">应用</td>
  <td>应用和功能</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>网站应用</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>默认应用</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>应用和功能</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">设备</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>音频和语音</td>
  <td>ms-settings:holographic-audio</td>
  <td>仅在安装混合现实门户应用后可用（在 Windows 应用商店中提供）</td>
 </tr>
 <tr>
  <td>自动播放</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>触摸板</td>
  <td>ms-settings:devices-touchpad</td>
  <td>仅在触摸板硬件存在时可用</td>
 </tr>
 <tr>
  <td>笔和 Windows Ink</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>打印机和扫描仪</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>键入</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>滚轮</td>
  <td>ms-settings:wheel</td>
  <td>仅在配对拨盘时可用</td>
 </tr>
 <tr>
  <td>默认相机</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>蓝牙</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>已连接的设备</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>鼠标和触摸板</td>
  <td>ms-settings:mousetouchpad</td>
  <td>触摸板设置仅在具有触摸板的设备上可用</td>
 </tr>
 <tr>
  <td rowspan="7">轻松使用</td>
  <td>讲述人</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>放大镜</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>高对比度</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>隐藏式字幕</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>键盘</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>鼠标</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>其他选项</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>附加</td>
  <td>附加</td>
  <td>ms-settings:extras</td>
  <td>仅在安装了“设置应用”后可用（例如，通过第三方安装）</td>
 </tr>
 <tr>
  <td rowspan="4">游戏</td>
  <td>广播</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>游戏栏</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>游戏 DVR</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>游戏模式</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>主页</td>
  <td>“设置”的登录页面</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">网络和 Internet</td>
  <td>以太网</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>VPN</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>拨号</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>仅在启用 DirectAccess 后可用</td>
 </tr>
 <tr>
  <td>WLAN 呼叫</td>
  <td>ms-settings:network-wificalling</td>
  <td>仅在启用 WLAN 呼叫后可用</td>
 </tr>
 <tr>
  <td>数据使用量</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>手机网络和 SIM 卡</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>移动热点</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>代理</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>状态</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>管理已知网络</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">网络和无线</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>WLAN</td>
  <td>ms-settings:network-wifi</td>
  <td>仅在设备具有 WLAN 适配器时可用</td>
 </tr>
 <tr>
  <td>飞行模式</td>
  <td>ms-settings:network-airplanemode</td>
  <td>在 Windows 8.x 上使用 ms-settings:proximity</td>
 </tr>
 <tr>
  <td rowspan="10">个性化</td>
  <td>“开始”菜单</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>主题</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>概览</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>导航栏</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>个性化（类别）</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>背景</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>颜色</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>声音</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>锁屏界面</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>任务栏</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">隐私</td>
  <td>应用诊断</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>通知</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>任务</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>常规</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>外部设备应用</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>广告 ID</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>电话呼叫</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>位置</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>相机</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>麦克风</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>动作</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>语音、墨迹书写和键入</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>帐户信息</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>联系人</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>日历</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>呼叫历史记录</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>电子邮件</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>消息</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>无线电收发器</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>后台应用</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>其他设备</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>反馈和诊断</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>帐户</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>团队会议</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>团队设备管理</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>会话清理</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>欢迎屏幕</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">系统</td>
    <td>共享体验</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>显示</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>多任务</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>投影到这台电脑</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>平板电脑模式</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>任务栏</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>电话</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>显示</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>通知和操作</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>电话</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>消息</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>节电模式</td>
  <td>ms-settings:batterysaver</td>
  <td>仅在具有电池的设备（如平板电脑）上可用</td>
 </tr>
 <tr>
  <td>电池使用</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>仅在具有电池的设备（如平板电脑）上可用</td>
 </tr>
 <tr>
  <td>电源和睡眠</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>关于</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>存储</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>存储感知</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>加密</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>离线地图</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">时间和语言</td>
  <td>日期和时间</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>区域和语言</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>语音语言</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>拼音键盘</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>在安装了 Microsoft 拼音输入法编辑器的情况下可用</td>
 </tr>
 <tr>
     <td>五笔输入模式</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>在安装了 Microsoft 五笔输入法编辑器的情况下可用</td>
 </tr>
 <tr>
  <td rowspan="13">更新和安全</td>
  <td>Windows Hello 设置</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>备份</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>查找我的设备</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 预览体验计划</td>
  <td>ms-settings:windowsinsider</td>
  <td>仅在 WIP 中注册用户后才存在</td>
 </tr>
 <tr>
  <td>Windows 更新</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 更新</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 更新</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 更新</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 更新</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>激活</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>恢复</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>疑难解答</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>适用于开发人员</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">用户帐户</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>设备必须支持 Windows Anywhere</td>
 </tr>
 <tr>
  <td>预配</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>仅在企业部署了预配包后可用</td>
 </tr>
</table><br/>  
