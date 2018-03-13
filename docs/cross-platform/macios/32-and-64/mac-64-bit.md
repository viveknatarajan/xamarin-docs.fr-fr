---
title: "Mise à jour des applications Xamarin.Mac unifiée à 64 bits"
description: "Ce guide décrit comment mettre à jour vos applications Xamarin.Mac cible 64 bits"
ms.topic: article
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: b53ef7157ffc393759b7e808655ce64b4744dab5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Mise à jour des applications Xamarin.Mac unifiée à 64 bits

À compter de janvier 2018, Apple nécessite que les nouveaux [les soumissions aux magasins d’applications Mac ciblent 64 bits](https://developer.apple.com/news/?id=06282017a). Applications déjà disponibles sur le Mac App Store doivent être mis à jour pour des cibles 64 bits en juin 2018.

Le **fichier** > **nouveau** modèle de projet Xamarin.Mac crée des applications 64 bits par défaut, toutes les applications récemment créées sont donc déjà compatible 64 bits et ne nécessitent pas de modifications.

## <a name="targeting-64-bit"></a>Ciblage de 64 bits

1. Ouvrez le **Options du projet** fenêtre pour que vous utilisez votre application Xamarin.Mac :

   ![Le menu contextuel du projet](mac-64-bit-images/1-contextual_menu-vsmac.png "le menu contextuel du projet")

2. Sélectionnez **Mac générer** et **prise en charge des architectures** à **x86\_64**:

   [![Définir les architectures prises en charge pour x86_64](mac-64-bit-images/2-project_options-vsmac.png "x86_64 affectant les architectures prises en charge")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Si votre application comporte des dépendances externes telles que des références natives ou des projets de la liaison, les mettre à jour pour des cibles 64 bits.

### <a name="errors"></a>Erreurs

La première fois que vous générez ou exécutez votre application avec prise en charge de 64 bits, vous pouvez rencontrer des erreurs de lien à partir de problèmes clang ou de l’exécution. Ces erreurs peuvent se produire si le tiers dépendances, par exemple, les références natives dans votre Xamarin.Mac ou des projets de liaisons ou chargé manuellement des infrastructures à l’échelle du système, n’ont pas été mis à jour à 64 bits.

> [!TIP]
> Conversion de votre projet à 64 bits constitue un changement majeur et peut découvrir indirectement les erreurs de programmation différents. En particulier, il peut modifier la taille et l’alignement des structures de données, ce qui affecte les signatures de p/invoke et le code natif lié dans votre projet. Pour passer en revue les avertissements de build donnés et tester votre application informées par la suite pour intercepter les problèmes potentiels.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Exemple d’erreur résultant d’une dépendance tierce liées dynamiquement qui ne cible pas 64 bits :

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Cette erreur peut être suivie lors de l’exécution par `dlopen` retour `IntPtr.Zero` au lieu d’un handle attendu.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Exemple d’erreur résultant d’une dépendance tierce liée de manière statique qui ne cible pas 64 bits :

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Pour générer et exécuter avec succès, mettre à jour ces dépendances à 64 bits et recompiler votre application.

