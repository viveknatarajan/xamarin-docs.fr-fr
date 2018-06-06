---
title: Framework cible pour Xamarin.Mac
description: Cet article traite de l’infrastructure cible (bibliothèques de classes de Base) disponible pour Xamarin.Mac et les conséquences de leur utilisation dans votre projet Xamarin.Mac.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 28d312ae10ce736a1720384fe76714910c3ff8f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792500"
---
# <a name="target-framework-for-xamarinmac"></a>Framework cible pour Xamarin.Mac

_Cet article traite de l’infrastructure cible (bibliothèques de classes de Base) disponible pour Xamarin.Mac et les conséquences de leur utilisation dans votre projet Xamarin.Mac._

![Cibler les options de l’infrastructure pour Xamarin.Mac](target-framework-images/select-target.png "Target framework des options pour Xamarin.Mac")

## <a name="background"></a>Présentation

Chaque programme .NET ou une bibliothèque dépend des fonctionnalités fournies par la bibliothèque de classes de Base (BCL). Cette BCL inclut les assemblys qui fournissent les fonctionnalités courantes intégrées à tous les langages .NET tels que mscorlib, système, System.Net.Http et System.Xml.

Au fil des années, vous avez développé plusieurs versions différentes de cette BCL, optimisées pour différents cas d’usage. La BCL « bureau » inclut un ensemble plus riche de bibliothèques qui peuvent être trop haute densité pour les autres cas d’utilisation, tandis que mobile se concentre sur la garantie de Qu'api est sécurisés pour la liaison, ce qui supprime le code non utilisé pour réduire l’encombrement de l’application.

Une de ces différentes infrastructures de cible, les répercussions plus importantes est que tous les assemblys dans un programme donné *doit* cibler des assemblys BCL compatibles. Si cela n’était pas le cas, vous pouvez avoir deux assemblys liés avec différentes versions de la **System.dll** désaccord sur la signature d’un type donné. Une bibliothèque partagée peut soit cible [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), qui est le sous-ensemble commun de l’infrastructure cible ou une infrastructure cible spécifique.

Il existe trois options de cible de .NET Framework pour Xamarin.Mac, chacun avec un compromis et différents avantages :

- **Moderne** (appelé Mobile dans la documentation plus ancienne) – un sous-ensemble très similaire pour les puissances Xamarin.iOS, un réglage précis pour les performances et la taille. Cette infrastructure cible est l’éditeur de liens sécurisé, ces projets peuvent avoir leur occupation finale considérablement réduite en supprimant le code non utilisé.

- **Complète** (appelé XM 4.5 dans la documentation plus ancienne) – un sous-ensemble très similaire à la BCL « bureau », avec quelques suppressions petites. Que le Framework cible est presque identique à net45 (et versions ultérieures), il peut consommer facilement des nombreuses nugets qui ne fournissent pas soit netstandard2 ou de Xamarin.Mac spécifique. Toutefois, en raison de l’utilisation de System.Configuration, il est incompatible avec la liaison.

- **Non pris en charge** (appelé système dans la documentation plus ancienne) – à la place d’un lien vers une BCL fournie par Xamarin.Mac, utilisez le mono système en cours. Cela fournit le jeu est saturé d’assemblys, y compris certains problèmes (System.Drawing par exemple). Cette option existe uniquement a « dernier recours » et il est fortement recommandé d’épuiser les autres options avant de l’utiliser. Comme son nom l’indique, l’utilisation non pris en charge par les canaux de prise en charge officielle.

## <a name="setting-the-target-framework"></a>Définir la version .NET framework cible

Pour modifier le type de cible de .NET Framework pour un projet Xamarin.Mac, procédez comme suit :

1. Ouvrez le projet Xamarin.Mac dans Visual Studio pour Mac.
2. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier projet pour ouvrir la boîte de dialogue **Options du projet**.
3. À partir de la **général** , sélectionnez le type de **Framework cible** qui répond aux besoins de votre application :

  [![À l’aide de la fenêtre des Options de projet pour choisir une infrastructure cible](target-framework-images/select-target-full.png "à l’aide de la fenêtre des Options de projet pour choisir une infrastructure cible")](target-framework-images/select-target-full-large.png#lightbox)

4. Cliquez sur le bouton **OK** pour enregistrer vos changements.

Vous devez **Clean** , puis **reconstruire** votre projet Xamarin.Mac après le passage du type de cible de .NET Framework.

## <a name="summary"></a>Récapitulatif

Cet article a couvert brièvement les différents types d’infrastructure cible (bibliothèques de classes de Base) disponible pour une application Xamarin.Mac et lors de chaque type de framework doit être utilisé.


## <a name="related-links"></a>Liens associés

- [iOS et Mac partage de code](~/cross-platform/macios/index.md)
- [API unifiée](~/cross-platform/macios/unified/index.md)
- [Bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md)
- [Assemblys](~/cross-platform/internals/available-assemblies.md)
- [Mise à jour d’applications Mac existantes](~/cross-platform/macios/unified/updating-mac-apps.md)
