---
title: Mise à jour des applications unifiées Xamarin.Mac vers 64 bits
description: Ce guide décrit comment mettre à jour vos applications Xamarin.Mac pour des cibles 64 bits. Il fournit également des exemples de types d’erreurs qui peuvent survenir lors de cette modification.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 3667f129cb566f71e9afd2969af13fe5b9682882
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109378"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Mise à jour des applications unifiées Xamarin.Mac vers 64 bits

Depuis janvier 2018, Apple exige que les nouveaux [soumissions de Mac App Store ciblent 64 bits](https://developer.apple.com/news/?id=06282017a). Applications déjà disponibles sur le Store d’application Mac doivent être mise à jour cible 64 bits en juin 2018.

Le **fichier** > **New** modèle de projet Xamarin.Mac crée des applications 64 bits par défaut, afin que toutes les applications récemment créées sont déjà compatibles 64 bits et ne nécessitent pas de toutes les modifications.

## <a name="targeting-64-bit"></a>Ciblage de 64 bits

1. Ouvrez le **Options du projet** fenêtre pour que vous êtes à votre application Xamarin.Mac :

   ![Le menu contextuel pour le projet](mac-64-bit-images/1-contextual_menu-vsmac.png "le menu contextuel du projet")

2. Sélectionnez **Build Mac** et définissez **architectures prises en charge** à **x86\_64**:

   [![Définir les architectures prises en charge sur x86_64](mac-64-bit-images/2-project_options-vsmac.png "x86_64 affectant les architectures prises en charge")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Si votre application a des dépendances externes telles que des références natives ou des projets de liaison, mettez-les à jour pour des cibles 64 bits.

### <a name="errors"></a>Erreurs

La première fois que vous générez ou exécutez votre application avec prise en charge 64 bits, vous pouvez rencontrer des erreurs de lien à partir de problèmes clang ou de l’exécution. Ces erreurs peuvent se produire si le tiers dépendances — par exemple, des références natives dans votre Xamarin.Mac ou projets de liaisons ou chargé manuellement les infrastructures de l’échelle du système, n’ont pas été mis à jour pour 64 bits.

> [!TIP]
> Conversion de votre projet vers 64 bits constitue un changement majeur et peut découvrir indirectement des erreurs de programmation différents. En particulier, elle peut changer la taille et l’alignement des structures de données, ce qui affecte les signatures p/invoke et le code natif lié dans votre projet. Pour passer en revue les avertissements de génération étant donnés et tester votre application informées par la suite pour intercepter les problèmes potentiels.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Exemple d’erreur résultant d’une dépendance tierce liées dynamiquement qui ne cible pas 64 bits :

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Cette erreur peut être suivie lors de l’exécution par `dlopen` retournant `IntPtr.Zero` au lieu d’un handle attendu.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Exemple d’erreur résultant d’une dépendance tierce liées de manière statique qui ne cible pas 64 bits :

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Pour générer et exécuter avec succès, mettre à jour ces dépendances pour 64 bits et recompiler votre application.

