---
title: Structures incorporées
description: Ce document décrit comment les développeurs d’applications peuvent incorporer des infrastructures d’utilisateur dans leurs applications.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f223d8ef6e89cc44822b8a831dbba3cf71d727c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="embedded-frameworks"></a>Structures incorporées

_Ce document décrit comment les développeurs d’applications peuvent incorporer des infrastructures d’utilisateur dans leurs applications._

Avec iOS 8.0 Apple rend possible de créer une structure incorporée pour partager du code entre les extensions d’application et de l’application principale dans Xcode.

Xamarin.iOS 9.0 ajoute la prise en charge pour l’utilisation de ces infrastructures incorporés (créées avec Xcode) dans les applications Xamarin.iOS. *Il sera **pas** être possible de créer des structures incorporées à partir de n’importe quel type de Xamarin.iOS projets uniquement utiliser les infrastructures (Objective-C) natifs existantes.*

Il existe deux façons de consommer les infrastructures dans Xamarin.iOS :

- Passer de l’infrastructure à l’outil mtouch, en ajoutant le code suivant pour les arguments mtouch supplémentaires dans le fichier **Build iOS** options :

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Cela doit être définie pour chaque configuration de projet.

- Ajouter des références natives dans le menu contextuel

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Avec le bouton droit sur le projet et Parcourir pour ajouter des références natives

![](embedded-frameworks-images/xam-native-refs.png "Sélectionnez Ajouter des références natives dans Visual Studio pour Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Avec le bouton droit sur le projet et Parcourir pour ajouter des références natives

![](embedded-frameworks-images/vs-native-refs.png "Sélectionnez Ajouter des références natives dans Visual Studio")

-----

  Cela fonctionne pour toutes les configurations.

Dans les futures versions de Visual Studio pour Mac et des outils Xamarin pour Visual Studio, il sera possible de consommer des infrastructures à partir de l’IDE (sans avoir à modifier manuellement les fichiers de projet).

Quelques exemples de projets se trouvent sur [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Limitations

- Structures incorporées sont uniquement pris en charge dans [unifié](~/cross-platform/macios/unified/index.md) projets.
- Structures incorporées sont uniquement pris en charge dans les projets avec une cible de déploiement d’au moins iOS 8.0.
- Si une extension nécessite une infrastructure incorporée, l’application conteneur doit également avoir une référence à l’infrastructure, sinon que l’infrastructure de ne pas être inclus dans le lot d’applications.

## <a name="the-mono-runtime"></a>Le runtime Mono

En interne, Xamarin.iOS tire parti de cette fonctionnalité pour lier avec le runtime Mono en tant qu’infrastructure, au lieu de la liaison statique de l’exécution Mono dans chaque extension et l’application conteneur.

Cela est fait automatiquement si l’application conteneur est une application unifiée, il contient des extensions et le déploiement de la cible est iOS 8.0 ou version ultérieure.

Applications sans extensions seront toujours lier avec le runtime Mono statiquement, car une baisse de taille pour l’utilisation d’une infrastructure s’il en existe qu’une seule application y faire référence.

Ce comportement peut être substitué par le développeur de l’application, en ajoutant le code suivant comme argument mtouch supplémentaires dans les e/s du projet options de Build :

- `--mono:static`: Lie statiquement avec le runtime Mono.
- `--mono:framework`: Les liens avec le runtime Mono en tant qu’infrastructure.

Un scénario de liaison avec le runtime Mono en tant qu’infrastructure même pour les applications sans extensions est de réduire la taille exécutable, pour contourner les restrictions de taille Qu'apple applique sur le fichier exécutable. Pour référence, le runtime Mono ajoute environ 1,7 Mo par architecture (comme de Xamarin.iOS 8.12, cependant son varie entre les versions et même entre des applications). L’infrastructure Mono ajoute environ 2,3 Mo par architecture, ce qui signifie que pour une application de l’architecture unique sans les extensions, qui effectue la liaison de l’application avec le runtime Mono résultant comme une infrastructure sera réduire l’exécutable par ~1.7MB, mais ajouter une infrastructure de ~2.3MB, dans un alltogether ~0.6MB plus grande application.

