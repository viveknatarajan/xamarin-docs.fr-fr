---
title: Prise en main de macOS Mojave
description: Ce document décrit comment la jusqu'à build macOS applications Mojave avec Xamarin.Mac. Il explique comment télécharger le 10 Xcode et mettre à jour de Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067298"
---
# <a name="getting-started-with-macos-mojave"></a>Prise en main de macOS Mojave

![Preview](~/media/shared/preview.png)

> [!WARNING]
> MacOS de Xamarin prise en charge Mojave est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, s’il n’est pas complet et peut changer.
> Utilisez-le pour seulement l’expérimentation.

> [!NOTE]
> Pour plus d’informations, consultez la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour la Xamarin version préliminaire.

Ce document décrit comment la jusqu'à build macOS applications Mojave avec Xamarin.Mac. Il explique comment télécharger le 10 Xcode et mettre à jour de Visual Studio pour Mac.

## <a name="download-and-install"></a>Téléchargez et installez

1. **Installez la dernière version bêta de Xcode 10** : les développeurs d’inscrit l’Apple peuvent télécharger et installer la dernière version de 10 Xcode à partir de la [portail des développeurs Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > Le Kit de développement logiciel Mojave macOS est distribué avec Xcode 10.

2. **Exécuter Xcode 10** – 10 de Xcode exécuter avant la mise à jour et en cours d’exécution Visual Studio pour Mac ; il installe des outils qui nécessite de Xamarin.

3. **Mettre à jour de Visual Studio pour Mac** : suivez les instructions de la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour installer la version préliminaire de Xamarin.

4. _(facultatif)_  **Installer la dernière version de mise à jour bêta macOS Mojave sur votre Mac** : permet de tester des applications Xamarin.Mac qui utilisent récemment introduits macOS Mojave API, inscrit peut de développeurs Apple [télécharger](https://developer.apple.com/download/) et installer le dernière macOS Mojave developer bêta.

   > [!TIP]
   > Même si votre application n’utilise pas de n’importe quel nouveau macOS Mojave API, veillez à générer avec macOS Mojave Kit de développement logiciel (installé avec Xcode 10) et de le tester pour vous assurer que tout fonctionne comme prévu. Si une application n’appelle pas les nouvelles API, vous pouvez le recompiler avec le Kit de développement logiciel Mojave macOS et testez-le sans mettre à niveau le système d’exploitation de votre Mac.

   > [!IMPORTANT]
   > Avant la mise à niveau de votre Mac à macOS Mojave pour générer et tester des applications Xamarin.Mac qui appellent le nouveau macOS Mojave API :
   > - Lecture [notes de publication d’Apple](https://developer.apple.com/download/) pour la mise à jour du système d’exploitation.
   > - Lecture la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour la Xamarin version préliminaire. Notez que cette première version d’évaluation n’inclut pas de liaisons pour le nouveau macOS Mojave AppKit APIs (par exemple, le Mode foncé).

## <a name="related-links"></a>Liens connexes

- [Télécharger Xcode 10](https://developer.apple.com/download/)
- Aperçu de Xamarin [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/)
