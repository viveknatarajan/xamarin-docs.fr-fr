---
title: Résolution d’une erreur PathTooLongException
description: Cet article explique comment résoudre une PathTooLongException qui peut-être se produire lors de la génération d’une application.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f50ca3e738cb781f9c80e83f58f2e0fa1fa8e113
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Résolution d’une erreur PathTooLongException

## <a name="cause"></a>Cause

Les noms de chemin d’accès généré dans un projet Xamarin.Android peuvent être assez longues.
Par exemple, Impossible de générer un chemin d’accès à ce qui suit lors d’une génération :

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Sur Windows (où la longueur maximale d’un chemin d’accès est [260 caractères](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), un **PathTooLongException** pu être produite lors de la génération du projet si un chemin d’accès généré dépasse la longueur maximale. 

## <a name="fix"></a>Correctif

À partir de Xamarin.Android 8.0, le `UseShortFileNames` propriété MSBuild permet de contourner cette erreur. Lorsque cette propriété a la valeur `True` (la valeur par défaut est `False`), le processus de génération utilise les noms de chemin d’accès plus courts pour réduire le risque de produire un **PathTooLongException**.
Par exemple, lorsque `UseShortFileNames` a la valeur `True`, le chemin d’accès ci-dessus est réduit au chemin d’accès qui est semblable au suivant :

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

Pour définir cette propriété, ajoutez la propriété MSBuild suivante au projet **.csproj** fichier :

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Pour plus d’informations sur la définition des propriétés de build, consultez [processus de génération](~/android/deploy-test/building-apps/build-process.md).
