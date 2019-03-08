---
title: XIM 项目配置
description: 描述如何配置 UWP 应用清单，以使用 Xbox 集成多人游戏 (XIM)。
ms.assetid: 5d0ed7bd-9527-42a3-ae09-8cc9012c1111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox 集成多人游戏, 清单
ms.localizationpriority: medium
ms.openlocfilehash: 926cc4495236fc4762f9b3176a5d6a4a579e2e2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613422"
---
# <a name="xim-project-configuration"></a>XIM 项目配置

## <a name="required-appxmanifest-capability-content"></a>所需 AppXManifest 功能内容

使用 XIM 的应用程序本身需要同时通过 Internet 和本地网络连接并接受来自网络资源的连接。 此外，还需要获得对麦克风设备的访问权限，以支持语音聊天。 因此，该应用应在其 AppXManifest 中声明“internetClientServer”和“privateNetworkClientServer”功能及“microphone”设备功能。 有关每个功能的详细信息，请参阅平台文档。 以下代码段演示了应存在于 Package/Capabilities 节点下的节点，否则连接和聊天将会受阻：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <Capability Name="internetClientServer" />
     <Capability Name="privateNetworkClientServer" />
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="universal-windows-application-xbox-live-multiplayer-invite-protocol-activation-extension"></a>通用 Windows 应用程序 Xbox Live 多人游戏邀请协议激活扩展

Xbox Live 多人游戏应用程序应支持游戏邀请。 本地用户接受的邀请以应用的协议激活形式发出（有关协议激活的详细信息，请参阅平台文档）。 使用 XIM 的应用（除了用于通过带外保留托管的 XIM 网络以外）均会在运行时调用 `xim::extract_protocol_activation_information()` 方法以在处理协议激活中提供帮助，但通用 Windows 平台 (UWP) 应用程序也必须通过 AppXManifest“windows.protocol”扩展，以静态方式声明它们支持此类由系统激活的“ms-xbl-multiplayer”。 此操作可通过如下所示的代码段实现：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" IgnorableNamespaces="uap">
   <Applications>
     <Application ...>
       <Extensions>
         ...
          <uap:Extension Category="windows.protocol">
             <uap:Protocol Name="ms-xbl-multiplayer"/>
          </uap:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

这仅对通用 Windows 应用程序是必需的。 Xbox One 独占资源（“XDK”）应用程序不会显式声明协议激活扩展，因为这些应用程序通过声明其 Xbox Live 游戏 ID 扩展自动注册。

## <a name="xbox-one-exclusive-resource-application-network-manifest-templates"></a>Xbox One 独占资源应用程序网络清单模板

任何使用 XIM 的 Xbox One 独占资源应用程序，都必须确保 AppXManifest 文件的“网络清单”扩展中包含特定套接字描述和模板内容（有关网络清单的详细信息，请参阅平台文档）。 此应用可能还会有其他模板，但必须存在以下代码段，否则将无法移至 XIM 网络：

```xml

 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/2010/manifest" xmlns:mx="http://schemas.microsoft.com/appx/2013/xbox/manifest" IgnorableNamespaces="mx">
   <Applications>
     <Application ...>
       <Extensions>
         ...
         <mx:Extension Category="windows.xbox.networking">
           <mx:XboxNetworkingManifest>
             <mx:SocketDescriptions>
               <mx:SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Initiate" />
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
               <mx:SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
             </mx:SocketDescriptions>
             <mx:SecureDeviceAssociationTemplates>
               <mx:SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
               <mx:SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
             </mx:SecureDeviceAssociationTemplates>
           </mx:XboxNetworkingManifest>
         </mx:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

## <a name="universal-windows-application-uwp-network-manifest-templates"></a>通用 Windows 应用程序 (UWP) 网络清单模板

任何使用 XIM 的通用 Windows 应用程序都必须确保其“网络清单”文件（程序包根目录中的 networkmanifest.xml 文件）包含特定套接字描述和模板内容（有关网络清单的详细信息，请参阅平台文档）。 此应用可能还会有其他模板，但必须存在以下代码段，否则将无法移至 XIM 网络：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <NetworkManifest xmlns="http://schemas.microsoft.com/xbox/2012/networkmanifest">
   <SocketDescriptions>
     <SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Initiate" />
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
     <SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
   </SocketDescriptions>
   <SecureDeviceAssociationTemplates>
     <SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
     <SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
   </SecureDeviceAssociationTemplates>
 </NetworkManifest>
```
