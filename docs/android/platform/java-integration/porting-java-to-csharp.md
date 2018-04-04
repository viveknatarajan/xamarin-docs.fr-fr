---
title: Portage de Java en c#
description: Une troisième option de l’aide de Java dans une application de Xamarin.Android est porter du code source Java à c#.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: c2d05b101c627dab42dc1343eab2a408d1bd010f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="porting-java-to-c"></a>Portage de Java en c#

_Une troisième option de l’aide de Java dans une application de Xamarin.Android est porter du code source Java à c#._

## <a name="overview"></a>Vue d'ensemble

Cette approche peut être un intérêt pour les organisations qui :

-  **Passez les piles de technologie à partir de Java à c#.**
-  **Doivent maintenir c# et une version Java du même produit.**
-  **Vous souhaitez disposer d’une version de .NET d’une bibliothèque Java populaires.**


Il existe deux façons de déplacer du code Java en c#. La première consiste à déplacer le code manuellement. Cela implique des développeurs expérimentés qui comprennent des .NET et Java et êtes familiarisé avec les idiomes appropriés pour chaque langue. Cette approche la plus logique pour les petites quantités de code, ou pour les organisations qui souhaitent complètement déplacer hors de Java en c#.

La seconde méthodologie portage est et essayez d’automatiser le processus à l’aide d’un convertisseur de code, tel que [l’accentuation](https://github.com/mono/sharpen). [Netteté](https://github.com/mono/sharpen) est un convertisseur open source à partir de Versant qui a servi à déplacer le code pour *db4o* à partir de Java en c#. db4o est une base de données et orienté objet Versant développé en Java, puis déplacée vers .NET. À l’aide d’un convertisseur de code peut convenir pour les projets qui doivent exister dans les deux langages et qui nécessitent une parité entre les deux.

Un exemple de quand un outil de conversion de code automatique est logique peut être consulté dans le [ngit](https://github.com/mono/ngit) projet.
Ngit est un port du projet Java [jgit](http://eclipse.org/).
Jgit lui-même est une implémentation Java du [Git](http://git-scm.com/) système de gestion de code source. Pour générer du code c# à partir de Java, les programmeurs ngit utilisent un système automatisé personnalisé pour extraire le code Java jgit, appliquer des correctifs pour prendre en compte le processus de conversion et puis exécutez l’accentuation, ce qui génère le code c#. Ainsi, le projet ngit profiter du travail effectué sur jgit continu, en cours.

Une quantité de données non triviale de travail est souvent impliquées dans un outil de conversion de code automatisé d’amorçage, et cela peut s’avérer un obstacle à utiliser. Dans de nombreux cas, il peut être plus simple et plus facile à Java de port en c# à la main.



## <a name="related-links"></a>Liens associés

- [Netteté, outil de Conversion](https://github.com/mono/sharpen)
