---
title: Résolution des problèmes de liaison
description: Ce guide décrit comment procéder si vous avez des difficultés à la liaison d’une bibliothèque Objective-C.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: 7ea3e3802ec2e0baf0fe8355a41e806bacabc9ac
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-troubleshooting"></a>Résolution des problèmes de liaison

Quelques conseils de dépannage des liaisons à macOS (anciennement OS X) API dans Xamarin.Mac.

## <a name="missing-bindings"></a>Absence de liaisons

Bien que Xamarin.Mac couvre une grande partie des API d’Apple, parfois, vous devrez peut-être appeler des API de Apple n’ayant pas d’une liaison encore. Dans d’autres cas, vous devez appeler tiers C/Objective-C qu’il sort du cadre de liaisons Xamarin.Mac.

Si vous êtes confronté à une API d’Apple, la première étape consiste à informer Xamarin que vous atteignez une section de l’API que nous n’avons pas couverture pour encore. [Entrer un bogue](#reporting-bugs) noter l’API manquant. Nous utilisons des rapports à partir de clients afin de hiérarchiser lesquelles API que nous travaillons sur Suivant. En outre, si vous avez une licence d’entreprise ou de l’entreprise et ce manque d’une liaison bloque votre progression, également suivre les instructions fournies sur [prise en charge](http://xamarin.com/support) pour remettre un ticket. Nous ne pouvons pas vous assurons une liaison, mais dans certains cas nous pouvons contourner vous un travail.

Une fois que vous notifier Xamarin (le cas échéant) de votre liaison manquant, l’étape suivante consiste à prendre en compte la liaison vous-même. Nous avons un guide complet [ici](~/cross-platform/macios/binding/overview.md) et de la documentation officielle [ici](http://brendanzagaeski.appspot.com/xamarin/0002.html) pour l’habillage Objective-C liaisons manuellement. Si vous appelez une API C, vous pouvez utiliser le mécanisme de P/Invoke de #, documentation est [ici](http://www.mono-project.com/docs/advanced/pinvoke/).

Si vous décidez de travailler sur la liaison vous-même, gardez à l’esprit que les erreurs dans la liaison peuvent générer toutes sortes de pannes intéressantes dans le runtime natif. En particulier, veillez que votre signature en c# correspond à la signature native dans le nombre d’arguments et la taille de chaque argument. Cela peut corrompre la mémoire et/ou de la pile et Impossible de se bloquer immédiatement ou à un moment donné arbitraire dans le futur ou d’endommager les données.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Exceptions d’argument lors du passage de null à une liaison

Bien que Xamarin fonctionne pour fournir une haute qualité et des liaisons pour les API d’Apple, parfois des erreurs et bon dans les bogues. Problème de loin la plus commun susceptible de rencontrer est une API lever `ArgumentNullException` lorsque vous transmettez la valeur null lorsque l’API sous-jacente accepte `nil`. Les fichiers d’en-tête natif souvent la définition de l’API ne fournissent pas de suffisamment d’informations sur lequel API accepte nil, qui se bloque si vous passez dans.

Si vous rencontrez un cas où en passant `null` lève une `ArgumentNullException` , mais vous pensez qu’il doit utiliser, suivez ces étapes :

1. Vérifiez la documentation Apple et/ou les exemples pour voir si vous pouvez trouver la preuve qu’il accepte `nil`. Si vous êtes à l’aise avec Objective-C, vous pouvez écrire un petit programme de test pour le vérifier.
2. [Entrer un bogue](#reporting-bugs).
3. Vous pouvez contourner le bogue Si vous pouvez éviter d’appeler l’API avec `null`, une vérification simple null autour des appels peut être une solution de contournement simple.
4. Toutefois, certaines API requièrent passant null pour activer ou désactiver certaines fonctionnalités. Dans ce cas, vous pouvez contourner le problème en affichant le navigateur de l’assembly (consultez [recherche le membre c# pour un sélecteur indiquée](~/mac/app-fundamentals/mac-apis.md#finding_selector)), copie de la liaison et la suppression de la vérification de valeur null. Vérifiez que dans le fichier d’un bogue (étape 2) si vous procédez ainsi, votre liaison copié ne recevoir des mises à jour et correctifs que nous prenons dans Xamarin.Mac et il doit être considérée comme un travail à court terme autour.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Signalement des bogues

Vos commentaires nous sont précieux. Si vous constatez des problèmes avec Xamarin.Mac :

- Consultez les [forums Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Recherchez dans le [dépôt des problèmes](https://github.com/xamarin/xamarin-macios/issues) 
- Avant de devenir des problèmes GitHub, les problèmes Xamarin étaient suivis sur [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Recherchez-y des problèmes correspondants.
- Si vous ne trouvez pas de problème correspondant, ouvrez un nouveau problème dans le [dépôt de problèmes GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Les problèmes GitHub sont tous publics. Il n’est pas possible de masquer des commentaires ou des pièces jointes. 

Dans la mesure du possible, essayez d’inclure :

- Un exemple simple reproduisant le problème. Si vous pouvez en fournir un, c’est **inestimable**. 
- La trace de pile complète du plantage.
- Le code C# se rapportant au plantage. 
