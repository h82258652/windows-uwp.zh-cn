---
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: This section describes how to modify your PlayReady web app to support the changes made from the previous Windows 8.1 version to the Windows 10 version.
title: PlayReady 加密媒体扩展
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b673122d707e152d24c49d3bacf71ed52cdb0ae5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256812"
---
# <a name="playready-encrypted-media-extension"></a>PlayReady 加密媒体扩展



This section describes how to modify your PlayReady web app to support the changes made from the previous Windows 8.1 version to the Windows 10 version.

通过在 Internet Explorer 中使用 PlayReady 媒体元素，开发人员可以在强制执行内容提供商定义的访问规则的同时，创建能够向用户提供 PlayReady 内容的 Web 应用。 本部分介绍如何仅使用 HTML5 和 JavaScript 将 PlayReady 媒体元素添加到现有 Web 应用。

## <a name="whats-new-in-playready-encrypted-media-extension"></a>PlayReady 加密媒体扩展中的新增功能

This section provides a list of changes made to the PlayReady Encrypted Media Extension (EME) to enable PlayReady content protection on Windows 10.

The following list describes the new features and changes made to PlayReady Encrypted Media Extension for Windows 10:

-   添加了硬件数字版权管理 (DRM)。

    基于硬件的内容保护支持可以在多个设备平台上安全播放高清 (HD) 和超高清 (UHD) 内容。 利用硬件安全保护密钥材料（包括私钥、内容密钥和任何其他用于派生或解锁上述密钥的密钥材料）以及解密的压缩和未压缩视频示例。

-   可主动获取非永久性许可证。
-   在一条消息中可获取多个许可证。

    You can either use a PlayReady object with multiple key identifiers (KeyIDs) as in Windows 8.1, or use [content decryption model data (CDMData)](https://docs.microsoft.com/previous-versions/windows/apps/dn457361(v=ieb.10)?redirectedfrom=MSDN) with multiple KeyIDs.

    > [!NOTE]
    > In Windows 10, multiple key identifiers are supported under &lt;KeyID&gt; in CDMData.

-   添加了实时到期支持，或有限持续时间许可证 (LDL)。

    提供在许可证上设置实时过期时间的功能。

-   添加了 HDCP 类型 1（版本 2.2）策略支持。
-   Miracast 现在已隐式为输出。
-   添加了安全停止。

    安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。

-   添加了音频和视频许可证分离。

    单独的轨道可避免视频被解码为音频；实现更可靠的内容保护。 新兴标准要求对音轨和视频轨使用单独的密钥。

-   添加了 MaxResDecode。

    添加此功能是为了在即使具有功能更强的密钥（但不是许可证）的情况下，也将内容播放限制在最大分辨率以内。 它支持使用单个密钥对多个媒体流大小进行编码的情况。

## <a name="encrypted-media-extension-support-in-playready"></a>PlayReady 中的加密媒体扩展支持

本部分介绍 PlayReady 支持的 W3C 加密媒体扩展的版本。

适用于 Web 应用的 PlayReady 当前已绑定到 [2013 年 5 月 10 日的 W3C 加密媒体扩展 (EME) 草稿](https://www.w3.org/TR/2013/WD-encrypted-media-20130510/)。 这项支持将在将来版本的 Windows 中更改为更新的 EME 规范。

## <a name="use-hardware-drm"></a>使用硬件 DRM

本部分介绍 Web 应用可如何使用 PlayReady 硬件 DRM，以及在受保护内容不支持硬件 DRM 时，如何禁用它。

若要使用 PlayReady 硬件 DRM，JavaScript Web 应用应结合使用 **isTypeSupported** EME 方法和 `com.microsoft.playready.hardware` 的主要系统标识符，以查询来自浏览器的 PlayReady 硬件 DRM 支持。

硬件 DRM 有时不支持某些内容。 硬件 DRM 从不支持 Cocktail 内容；如果想要播放 Cocktail 内容，必须选择退出硬件 DRM。 一些硬件 DRM 支持 HEVC，而一些则不支持；如果希望播放 HEVC 内容，但硬件 DRM 不支持该内容，你也可能想要选择退出。

> [!NOTE]
> 若要确定 HEVC 内容是否受支持，请在实例化 `com.microsoft.playready` 后，使用 [**PlayReadyStatics.CheckSupportedHardware**](https://docs.microsoft.com/uwp/api/windows.media.protection.playready.playreadystatics.checksupportedhardware) 方法。

## <a name="add-secure-stop-to-your-web-app"></a>向 Web 应用添加安全停止

本部分介绍如何向 Web 应用添加安全停止。

安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。 此功能可确保你的媒体流服务能够准确地执行和报告给定帐户在不同设备上的使用限制。

发送安全停止质询的主要方案有两种：

-   在到达内容末尾或用户中途停止媒体演示文稿，因而媒体演示文稿停止时。
-   在上一个会话意外结束时（如因系统或应用崩溃）。 在启动或关机时，应用将查询任何未解决的安全停止会话，并将发送与任何其他媒体播放分离的质询。

以下过程将介绍如何为各种方案设置安全停止。

设置演示文稿正常结束的安全停止：

1.  在开始播放前，先注册 **onEnded** 事件。
2.  **onEnded** 事件处理程序需要从视频/音频元素对象中调用 `removeAttribute("src")`，以将源设置为 **NULL**，它可以触发媒体基础以关闭拓扑、销毁解密器以及设置停止状态。
3.  可以启动处理程序中的安全停止 CDM 会话，向服务器发送安全停止质询，以通知播放在此时已停止，但此操作也可以稍后执行。

在用户导航离开页面或者关闭选项卡或浏览器时设置安全停止：

-   无需应用操作即可记录停止状态；将为你记录此状态。

为自定义页面控件或用户操作（如自定义导航按钮或在完成当前演示文稿之前启动新的演示文稿）设置安全停止：

-   在发生自定义用户操作时，应用需要将源设置为 **NULL**，它可以触发媒体基础以关闭拓扑、销毁解密器以及设置停止状态。

以下示例演示如何在 Web 应用中使用安全停止：

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Receive and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> The secure stop data’s `<SessionID>B64 encoded session ID</SessionID>` in the sample above can be an asterisk (\*), which is a wild card for all the secure stop sessions recorded. That is, the **SessionID** tag can be a specific session, or a wild card (\*) to select all the secure stop sessions.

## <a name="programming-considerations-for-encrypted-media-extension"></a>加密媒体扩展的编程注意事项

This section lists the programming considerations that you should take into account when creating your PlayReady-enabled web app for Windows 10.

应用创建的 **MSMediaKeys** 和 **MSMediaKeySession** 对象必须保持活动状态，直到你的应用关闭。 确保这些对象保持活动状态的一种方法是将它们作为全局变量分配（如果声明为函数内的本地变量，这些变量将超出范围并遵循垃圾回收）。 For example, the following sample assigns the variables *g\_msMediaKeys* and *g\_mediaKeySession* as global variables, which are then assigned to the **MSMediaKeys** and **MSMediaKeySession** objects in the function.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

有关详细信息，请参阅[示例应用程序](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738)。

## <a name="see-also"></a>另请参阅
- [PlayReady DRM](playready-client-sdk.md)




