---
title: Environnement
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>Environnement

*L’environnement d’exécution* correspond à l’ensemble de variables d’environnement qui influencent l’exécution du programme. Les variables d’environnement peuvent être définies temporairement dans les propriétés du projet ou définitivement en spécifiant des arguments supplémentaires à l’outil d’empaquetage mtouch.

## <a name="temporary-environment-variables"></a>Variables d’environnement temporaires

Les variables d’environnement temporaires sont définies dans la fenêtre **Propriétés**/**Options** de la section **Exécuter > Général**. Ces variables d’environnement sont uniquement en vigueur lorsque l’application est démarrée à l’aide de Visual Studio pour Mac, si l’application est démarrée manuellement en tapant dessus, ces variables d’environnement ne sont pas définies.

## <a name="permanent-environment-variables"></a>Variables d'environnement permanentes

Les variables d’environnement permanentes sont définies en spécifiant des arguments supplémentaires à l’outil d’empaquetage mtouch. Ces variables d’environnement sont compilées dans le fichier exécutable et seront définies même si l’application n’est pas lancée à partir de Xamarin Studio.

## <a name="example"></a>Exemple

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

