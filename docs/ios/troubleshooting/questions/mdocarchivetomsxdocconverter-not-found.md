---
title: MDocArchiveToMsxDocConverter.exe introuvable rver.BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 0746174857f66843ef9a09429b6286f2efca90d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420535"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe introuvable rver.BaseCommand.OnRequest

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.


## <a name="error-message"></a>Message d'erreur

Cette erreur peut apparaître dans le *journal du serveur Mac* dans Visual Studio :

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Il existe 2 problèmes distincts dans ce message :

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Cette erreur est sans incidence, mais il est également trompeur. Il [sera supprimée](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) dans une version ultérieure.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Cette erreur est le véritable problème. Malheureusement, dû à un [limitation](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) cette trace de pile d’exception est *incomplète*. Si vous remarquez une trace de pile incomplète comme suit dans le journal du serveur Mac, vous pouvez vérifier le `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` fichier sur l’hôte de build Mac pour rechercher la trace de pile complète.
