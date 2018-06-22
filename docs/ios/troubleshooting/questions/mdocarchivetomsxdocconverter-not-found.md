---
title: MDocArchiveToMsxDocConverter.exe introuvable du serveur. BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 6d7fe8f48d22c6b5d445fa8356e52f924549f6e0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775674"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe introuvable du serveur. BaseCommand.OnRequest

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.


## <a name="error-message"></a>Message d'erreur

Cette erreur peut apparaître dans le *journal du serveur Mac* dans Visual Studio :

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Il existe 2 problèmes distincts dans ce message :

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Cette erreur est sans conséquence, mais il est également trompeur. Il [sera supprimé](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) dans une version ultérieure.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Cette erreur est le problème réel. Malheureusement, due à un [limitation](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) cette trace de pile d’exception est *incomplète*. Si vous remarquez une trace de pile incomplet comme suit dans le journal du serveur Mac, vous pouvez vérifier le `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` fichier sur l’hôte de build Mac pour rechercher la trace de pile complet.
