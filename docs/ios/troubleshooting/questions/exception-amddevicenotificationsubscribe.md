---
title: System.Exception AMDeviceNotificationSubscribe retourné...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 257e0c14de3c23825b6abe6601c25438db81c58e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776480"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe retourné...

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.


## <a name="fix"></a>Correctif

1.  Tuer le `usbmuxd` traiter afin qu’il redémarre le système :

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Si cela ne résout pas le problème, redémarrez l’ordinateur Mac.

## <a name="error-message"></a>Message d'erreur

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

Ce message peut s’afficher dans une boîte de dialogue d’erreur lorsque vous démarrez Visual Studio pour Mac ou dans le `mtbserver.log` fichier dans l’application Xamarin.iOS Build Host (**Xamarin.iOS Build Host > Afficher le journal de génération hôte**).

Notez qu’il s’agit d’un problème courant. Si Visual Studio rencontre des problèmes de connexion à l’hôte de build Mac, il existe d’autres erreurs qui sont plus susceptibles d’apparaître dans le `mtbserver.log` fichier.

### <a name="errors-in-systemlog"></a>Erreurs dans System.log

Dans une erreur de cas, les deux messages peuvent également s’afficher à plusieurs reprises dans `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Informations supplémentaires

Une estimation à la cause de l’erreur est que les services système responsables de la création de rapports les informations de périphérique et le simulateur iOS peuvent dans de rares cas OS X entrer dans un état inattendu. Xamarin ne peut pas interagir correctement avec les services système dans cet état. Redémarrage de l’ordinateur redémarre les services système et a résolu le problème.

Basé sur les erreurs de `system.log` il semble que ce problème peut être lié aux Bonjour (`mDNSResponder`). La modification entre différents réseaux Wi-Fi semble augmenter les chances d’atteindre le problème.

## <a name="references"></a>Références

*   [Bogue 11789 - MonoTouch.MobileDevice.MobileDeviceException : Retourné AMDeviceNotificationSubscribe : 0xe8000063 [NORESPONSE résolu]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
