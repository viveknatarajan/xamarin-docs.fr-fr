---
title: Incorporation .NET
description: Incorporation .NET permet à votre Code .NET existant (C#, F#, etc.) à être consommés par le code écrit dans d’autres langages de programmation.
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 23233ea8b06e0db580ba99edf2705e3dae5b931f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363446"
---
# <a name="net-embedding"></a>Incorporation .NET

![Preview](~/media/shared/preview.png)

Incorporation .NET permet à votre Code .NET existant (C#, F#, etc.) à être consommés à partir d’autres langages de programmation et dans différents environnements différents.

Cela signifie que si vous avez une bibliothèque .NET que vous souhaitez utiliser à partir de votre application iOS existante, vous pouvez le faire.   Ou, si vous souhaitez lier à une bibliothèque C++ native, vous pouvez le faire également.   Ou utiliser le code .NET à partir de Java.

Incorporation .NET est basé sur le [Embeddinator-4000](https://github.com/mono/Embeddinator-4000) projet open source.

## <a name="environments-and-languages"></a>Langages et environnements

L’outil est conscient de l’environnement, qu'il utilisera, ainsi que la langue dans laquelle elle consommera.   Par exemple, la plateforme iOS n’autorise pas de compilation de (JIT) juste-à-temps, afin de l’incorporation de .NET compiler statiquement votre code .NET en code natif qui peut être utilisé dans iOS.  Autres environnements n’autorisent pas la compilation JIT et dans ces environnements, nous allons opter pour la compilation JIT.

Il prend en charge divers consommateurs de langage, il met en évidence de code .NET en tant que code IDIOMATIQUE dans le langage cible.   Voici la liste des langues prises en charge à l’heure actuelle :

- [**Objective-C** ](objective-c/index.md) – mappage .NET IDIOMATIQUE API Objective-C
- [**Java** ](android/index.md) – mappage .NET IDIOMATIQUE API Java
- [**C** ](get-started/c.md) – mappage .NET à et orienté objet, telles que les API C

Davantage de langues sera proposée ultérieurement.

## <a name="getting-started"></a>Prise en main

Pour commencer, consultez un de nos guides pour chacune des langues prises en charge actuellement :

- [**Objective-C** ](get-started/objective-c/index.md) – couvre macOS et iOS
- [**Java** ](get-started/java/index.md) – couvre macOS et Android
- [**C** ](get-started/c.md) – couvre le langage C sur les plateformes de bureau

## <a name="related-links"></a>Liens associés

- [Embeddinator-4000 sur GitHub](https://github.com/mono/Embeddinator-4000)
