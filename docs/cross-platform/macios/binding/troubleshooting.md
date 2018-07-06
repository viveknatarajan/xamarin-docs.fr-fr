---
title: Résolution des problèmes de liaison
description: Ce guide décrit la procédure à suivre si vous rencontrez des difficultés pour la liaison d’une bibliothèque Objective-C. En particulier, il traite des liaisons manquants, les exceptions d’argument lors du passage de null pour une liaison et signalement des bogues.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854797"
---
# <a name="binding-troubleshooting"></a>Résolution des problèmes de liaison

Quelques conseils pour la résolution des problèmes de liaisons à macOS (anciennement appelé OS X) API dans Xamarin.Mac.

## <a name="missing-bindings"></a>Absence de liaisons

Bien que Xamarin.Mac couvre une grande partie de l’API d’Apple, parfois, vous devrez peut-être appeler des API Apple n’ayant pas d’une liaison encore. Dans d’autres cas, vous devez appeler tiers C/Objective-C qu’elle dépasse le cadre des liaisons Xamarin.Mac.

Si vous êtes confronté à une API d’Apple, la première étape consiste à informer Xamarin que vous ayez accès à une section de l’API que nous n’avons pas la couverture pour encore. [Signaler un bogue](#reporting-bugs) noter l’API manquante. Nous utilisons des rapports à partir de clients à hiérarchiser les API que nous allons travailler. En outre, si vous avez une licence d’entreprise ou d’entreprise, et ce manque d’une liaison bloque votre progression, suivez également les instructions de [prise en charge](http://xamarin.com/support) pour remettre un ticket. Nous ne pouvons pas promettre une liaison, mais dans certains cas nous pouvons contourner vous un travail.

Une fois que vous avertir Xamarin (le cas échéant) de votre liaison manquant, l’étape suivante consiste à prendre en compte la liaison vous-même. Nous avons un guide complet [ici](~/cross-platform/macios/binding/overview.md) et de la documentation officielle [ici](http://brendanzagaeski.appspot.com/xamarin/0002.html) pour l’habillage des liaisons Objective-C à la main. Si vous appelez une API C, vous pouvez utiliser le mécanisme P/Invoke de #, la documentation est [ici](http://www.mono-project.com/docs/advanced/pinvoke/).

Si vous décidez de travailler sur la liaison, vous-même, sachez que les erreurs dans la liaison peuvent produire toutes sortes de pannes intéressantes dans le runtime natif. En particulier, soyez très prudent que votre signature en C# correspond à la signature native dans le nombre d’arguments et la taille de chaque argument. Cela peut corrompre la mémoire et/ou de la pile et vous pourrez se bloquer immédiatement ou à un moment donné arbitraire dans le futur ou corrompre des données.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Exceptions d’argument lors du passage de null à une liaison

Bien que Xamarin fonctionne pour fournir une haute qualité et testées et liaisons pour les API d’Apple, parfois des erreurs et bon dans les bogues. Problème de loin le plus courant que vous pouvez rencontrer est une API lever `ArgumentNullException` lorsque vous passez null lors de l’API sous-jacente accepte `nil`. Les fichiers d’en-tête natif souvent la définition de l’API ne fournissent pas suffisamment d’informations sur lequel API acceptent nil et qui se bloque si vous le passez dans.

Si vous rencontrez un cas où vous en passant `null` lève un `ArgumentNullException` mais vous pensez qu’il doit fonctionner, suivez ces étapes :

1. Vérifiez la documentation d’Apple ou les exemples pour voir si vous pouvez trouver la preuve qu’il accepte `nil`. Si vous êtes familiarisé avec Objective-C, vous pouvez écrire un petit programme de test pour le vérifier.
2. [Signaler un bogue](#reporting-bugs).
3. Vous pouvez contourner le bogue Si vous pouvez éviter d’appeler l’API avec `null`, une vérification de valeur null simple autour des appels peut être une simple solution de contournement.
4. Toutefois, certaines API exigent passant null pour désactiver ou de désactiver certaines fonctionnalités. Dans ce cas, vous pouvez contourner le problème en affichant l’Explorateur d’assembly (consultez [recherche le membre c# pour un sélecteur donné](~/mac/app-fundamentals/mac-apis.md#finding_selector)), copie de la liaison et la suppression de la vérification de valeur null. Veillez à entrer un bogue (étape 2) si vous le faites, votre liaison copié ne recevoir des mises à jour et correctifs que nous prenons dans Xamarin.Mac, et cela doit être considéré comme une solution de contournement à court terme.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Signalement des bogues

Vos commentaires sont importants pour nous. Si vous rencontrez des problèmes avec Xamarin.Mac :

- Consultez les [forums Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Recherchez dans le [dépôt des problèmes](https://github.com/xamarin/xamarin-macios/issues) 
- Avant de devenir des problèmes GitHub, les problèmes Xamarin étaient suivis sur [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Recherchez-y des problèmes correspondants.
- Si vous ne trouvez pas de problème correspondant, ouvrez un nouveau problème dans le [dépôt de problèmes GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Les problèmes GitHub sont tous publics. Il n’est pas possible de masquer des commentaires ou des pièces jointes. 

Dans la mesure du possible, essayez d’inclure :

- Un exemple simple reproduisant le problème. Si vous pouvez en fournir un, c’est **inestimable**. 
- La trace de pile complète du plantage.
- Le code C# se rapportant au plantage. 

## <a name="related-links"></a>Liens connexes

- [Cours de l’Université de Xamarin : Génération d’une bibliothèque de liaisons Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University cours : Générer une bibliothèque de liaisons Objective-C avec Sharpie objectif](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
