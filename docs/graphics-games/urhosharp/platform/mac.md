---
title: Prise en charge UrhoSharp Mac
description: Le programme d’installation spécifique de Mac et les fonctionnalités UrhoSharp.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 4f439d8f848e84b0e775a56204a1834b55c7ad10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="urhosharp-mac-support"></a>Prise en charge UrhoSharp Mac

_Fonctionnalités et configuration du Mac spécifique_

Bien que Urho est une bibliothèque de classes portable et permet la même API à utiliser sur la plateforme différents pour la logique du jeu, vous devez initialiser Urho dans votre pilote plateforme et dans certains cas, vous souhaitez tirer parti des fonctionnalités spécifiques de plateforme .

Dans les pages suivantes, supposons que `MyGame` est une sous-classe de la `Application` classe.

## <a name="macos"></a>macOS

**Architectures prises en charge :** x86/x86-64 pour 32 bits et 64 bits.

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet de console, référence Urho NuGet et assurez-vous que vous pouvez localiser les ressources (les répertoires contenant le répertoire de données).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Exemple

[Exemple complet](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


