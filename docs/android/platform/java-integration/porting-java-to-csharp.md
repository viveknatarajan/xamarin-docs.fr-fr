---
title: Portage de Java enC#
description: Une troisième option pour l’utilisation de Java dans une application Xamarin.Android consiste à déplacer le code source de Java vers C#.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
ms.openlocfilehash: 9beb6d59c9376a404c06af7f0cd1efd985929843
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104269"
---
# <a name="porting-java-to-c"></a>Portage de Java enC#

_Une troisième option pour l’utilisation de Java dans une application Xamarin.Android consiste à déplacer le code source de Java vers C#._

## <a name="overview"></a>Vue d'ensemble

Cette approche peut être un intérêt pour les organisations qui :

-  **Basculez piles technologiques à partir de Java à C#.**
-  **Doit conserver un C# et une version Java du même produit.**
-  **Désireux de bénéficier d’une version de .NET d’une bibliothèque Java populaires.**


Il existe deux façons de portage du code Java vers C#. La première consiste à déplacer le code manuellement. Cela implique des développeurs compétents qui comprennent .NET et Java et êtes familiarisé avec les idiomes appropriés pour chaque langue. Cette approche convient le mieux pour les petites quantités de code, ou pour les organisations qui souhaitent complètement éloigner de Java pour C#.

La seconde méthodologie de portage consiste à essayer et d’automatiser le processus à l’aide d’un convertisseur de code, tel que [netteté](https://github.com/mono/sharpen). [Aiguiser](https://github.com/mono/sharpen) est un convertisseur open source à partir de Versant qui a servi à porter le code pour *db4o* à partir de Java à C#. db4o est une base de données orientée objet Versant développé en Java, puis déplacée vers .NET. À l’aide d’un convertisseur de code peut être judicieux pour les projets qui doivent exister dans les deux langages et qui nécessitent une parité entre les deux.

Un exemple de quand un outil de conversion de code automatisé est logique sont consultables dans la [ngit](https://github.com/mono/ngit) projet.
Ngit est un port du projet Java [jgit](http://eclipse.org/).
Jgit lui-même est une implémentation Java de le [Git](http://git-scm.com/) système de gestion de code source. Pour générer C# code à partir de Java, l’utilisation de programmeurs ngit personnalisé automatisée système pour extraire le code Java jgit, appliquer des correctifs pour prendre en compte le processus de conversion et puis exécuter netteté, ce qui génère le C# code. Ainsi, le projet ngit profiter du travail continu, en cours est effectué sur jgit.

Une quantité non négligeable de travail est souvent impliquées dans un outil de conversion de code automatisé d’amorçage, et cela peut s’avérer être un obstacle à utiliser. Dans de nombreux cas, il peut être plus simple et plus facile à Java de port pour C# manuellement.



## <a name="related-links"></a>Liens associés

- [Netteté, outil de Conversion](https://github.com/mono/sharpen)
