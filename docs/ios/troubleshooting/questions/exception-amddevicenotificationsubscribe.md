---
title: Erreur System.Exception AMDeviceNotificationSubscribe retournée...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4fb0712366422e8810a2db60d40c3b85d9f4cd82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119252"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>Erreur System.Exception AMDeviceNotificationSubscribe retournée...

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.


## <a name="fix"></a>Correctif

1.  Arrêter le `usbmuxd` traiter afin qu’il redémarre le système :

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Si le problème persiste, redémarrez l’ordinateur Mac.

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

Ce message peut s’afficher dans une boîte de dialogue d’erreur lorsque vous démarrez Visual Studio pour Mac, ou dans le `mtbserver.log` fichier dans l’application hôte de Build Xamarin.iOS (**hôte de Build Xamarin.iOS > Afficher le journal hôte Build**).

Notez qu’il s’agit d’un problème rare. Si Visual Studio rencontre des problèmes pour vous connecter à l’hôte de build Mac, il existe d’autres erreurs qui sont plus susceptibles d’apparaître dans le `mtbserver.log` fichier.

### <a name="errors-in-systemlog"></a>Erreurs dans system.log

Erreur de certains cas les deux messages peuvent également s’afficher à plusieurs reprises dans `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Informations supplémentaires

Une estimation à la cause de l’erreur est que les services système responsables de la consignation des informations sur les appareils et de simulateurs iOS peuvent dans de rares cas OS X permet d’entrer un état inattendu. Xamarin ne peut pas interagir correctement avec les services système dans cet état. Redémarrage de l’ordinateur redémarre les services système et résout le problème.

Basé sur les erreurs de `system.log` il semble que ce problème peut être lié à Bonjour (`mDNSResponder`). Modification entre les différents réseaux Wi-Fi semble augmenter les chances d’atteindre le problème.

## <a name="references"></a>Références

*   [Bogue 11789 - MonoTouch.MobileDevice.MobileDeviceException : AMDeviceNotificationSubscribe retournée : 0xe8000063 [NORESPONSE résolus]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
