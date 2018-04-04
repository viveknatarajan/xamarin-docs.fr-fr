---
title: Liaison Objective-C
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fef2826f536042dc9be830a4c0dc358658c359d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-objective-c"></a>Liaison Objective-C

Cette section comprend un grand nombre de documents qui couvrent la création de liaisons aux bibliothèques de Objective-C peuvent donc être appelés à partir de c# des applications créées avec Xamarin.iOS ou Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Vue d’ensemble](~/cross-platform/macios/binding/overview.md)

Ce document contient certains éléments internes de la façon dont une liaison s’effectue. Il s’agit d’un document avancé avec des informations techniques.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Liaison de bibliothèques Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

Ce document décrit le processus utilisé pour créer des liaisons c# de l’API de Objective-C et comment les idiomes dans Objective-C sont mappées aux idiomes utilisées dans .NET.
Si vous liez simplement les API C, vous devez utiliser le mécanisme de .NET standard pour ce faire, l’infrastructure de P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Guide de référence de définition de liaison](~/cross-platform/macios/binding/binding-types-reference.md)

Il s’agit du guide de référence qui décrit tous les attributs disponibles pour les auteurs de la liaison pour piloter le processus de génération de liaison.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objectif Sharpie est un outil de ligne de commande permettant de démarrer le premier test d’une liaison. Il fonctionne en analysant les fichiers d’en-tête d’une bibliothèque native pour mapper l’API publique dans le [définition de liaison](~/cross-platform/macios/binding/objective-c-libraries.md) (processus qui peut également être effectué manuellement).

## <a name="ios"></a>iOS

Le [page de liaison iOS](~/ios/platform/binding-objective-c/index.md) revient à ces ressources communes de liaison, en outre les exemples ci-dessous.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Procédure pas à pas : Liaison d’une bibliothèque Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Cet article fournit une procédure pas à pas détaillées de création d’un projet de liaison à l’aide de la source ouverte [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) projet Objective-C comme exemple. La bibliothèque InfColorPicker fournit un contrôleur de vue réutilisables qui permettent à l’utilisateur de sélectionner une couleur en fonction de sa représentation sous forme de TSL, effectuer une sélection de couleur plus conviviaux. Objectif Sharpie permet de faciliter le processus de liaison.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Exemples de liaison](https://github.com/mono/monotouch-bindings)

Une collection de liaisons de tiers qui peuvent être utilisées une référence lors de la création de nouveaux projets de liaison.

## <a name="mac"></a>Mac

Historiquement [liaison de Mac](~/mac/platform/binding.md) a été largement des processus manuels. Il y a actuellement un [aperçu téléchargeable](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) de prise en charge du projet de liaison Mac pour une future version de Visual Studio pour Mac.



## <a name="related-links"></a>Liens associés

- [e/s de liaison](~/ios/platform/binding-objective-c/index.md)
- [Liaison de Mac](~/mac/platform/binding.md)
