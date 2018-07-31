---
title: Frameworks incorporés dans Xamarin.iOS
description: Ce document décrit comment partager du code avec des frameworks incorporés dans une application Xamarin.iOS. Cela est possible avec l’outil mtouch ou des références natives.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: cce5356fd1d3d9a5cf16370a4843c3541b00a7c0
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351432"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Frameworks incorporés dans Xamarin.iOS

_Ce document décrit comment les développeurs d’applications peuvent incorporer des infrastructures d’utilisateur dans leurs applications._

Avec iOS Apple 8.0 permettait de créer une structure incorporée afin de partager du code entre les extensions d’application et de l’application principale dans Xcode.

Xamarin.iOS 9.0 prend en charge pour la consommation de ces frameworks incorporés (créées avec Xcode) dans les applications Xamarin.iOS. *Il sera **pas** possible de créer des frameworks incorporés à partir de n’importe quel type de projets Xamarin.iOS, uniquement consommer des infrastructures (Objective-C) natifs existantes.*

Il existe deux manières de consommer des infrastructures dans Xamarin.iOS :

- Passer de l’infrastructure à l’outil mtouch, en ajoutant le code suivant aux arguments mtouch supplémentaires dans le projet **Build iOS** options :

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

Dans les versions futures de Visual Studio pour Mac et les outils Xamarin pour Visual Studio, il sera possible consommer des infrastructures à partir de l’IDE (sans avoir à modifier manuellement les fichiers projet).

Vous trouverez quelques exemples de projets sur [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Limitations

- Frameworks incorporés sont uniquement pris en charge dans [unifiés](~/cross-platform/macios/unified/index.md) projets.
- Frameworks incorporés sont uniquement pris en charge dans les projets avec une cible de déploiement d’au moins iOS 8.0.
- Si une extension nécessite une infrastructure incorporée, l’application de conteneur doit également avoir une référence à l’infrastructure, sinon que le framework ne sera pas inclus dans le bundle d’applications.

## <a name="the-mono-runtime"></a>Le runtime Mono

En interne, Xamarin.iOS tire parti de cette fonctionnalité pour lier avec le runtime Mono en tant qu’infrastructure, au lieu de lier le runtime Mono statiquement dans chaque extension et l’application de conteneur.

Cela est fait automatiquement si l’application de conteneur est une application unifiée, il contient des extensions et le déploiement cible est iOS 8.0 ou version ultérieure.

Applications sans extensions de liens seront toujours avec le runtime Mono statiquement, car il existe une altération de taille pour l’utilisation d’une infrastructure s’il en existe qu’une seule application qui la référencent.

Ce comportement peut être remplacé par le développeur d’application, en ajoutant le code suivant en tant qu’argument mtouch supplémentaires dans des options Build iOS du projet :

- `--mono:static`: Lie statiquement avec le runtime Mono.
- `--mono:framework`: Liens avec le runtime Mono en tant qu’infrastructure.

Un scénario pour la liaison avec le runtime Mono en tant qu’infrastructure même pour les applications sans extensions consiste à réduire la taille de l’exécutable, pour surmonter les restrictions de taille Qu'apple impose sur le fichier exécutable. Pour référence, le runtime Mono ajoute environ 1,7 Mo par architecture (comme de Xamarin.iOS 8.12, cependant son varie entre les versions et même entre les applications). Le framework Mono ajoute environ 2,3 Mo par architecture, ce qui signifie que pour une application unique-architecture sans extensions, la liaison de l’application est jugée avec le runtime Mono résultant comme une infrastructure sera réduire le fichier exécutable par ~1.7MB, mais ajouter une infrastructure ~2.3MB, dans un alltogether ~0.6MB plus grande application.

