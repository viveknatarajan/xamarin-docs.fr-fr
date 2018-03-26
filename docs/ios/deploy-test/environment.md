---
title: Environnement
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2018
---
# <a name="environment"></a>Environnement

*L’environnement d’exécution* correspond à l’ensemble de variables d’environnement qui influencent l’exécution du programme. Les variables d’environnement peuvent être définies temporairement dans les propriétés du projet ou définitivement en spécifiant des arguments supplémentaires à l’outil d’empaquetage mtouch.

## <a name="temporary-environment-variables"></a>Variables d’environnement temporaires

Les variables d’environnement temporaires sont définies dans la fenêtre **Propriétés**/**Options** de la section **Exécuter > Général**. Ces variables d’environnement sont uniquement en vigueur lorsque l’application est démarrée à l’aide de Visual Studio pour Mac, si l’application est démarrée manuellement en tapant dessus, ces variables d’environnement ne sont pas définies.

## <a name="permanent-environment-variables"></a>Variables d'environnement permanentes

Les variables d’environnement permanentes sont définies en spécifiant des arguments supplémentaires à l’outil d’empaquetage mtouch. Ces variables d’environnement sont compilées dans le fichier exécutable et seront définies même si l’application n’est pas lancée à partir de Visual Studio pour Mac.

## <a name="example"></a>Exemple

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

