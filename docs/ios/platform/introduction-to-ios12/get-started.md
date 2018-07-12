---
title: Bien démarrer avec iOS 12 et tvOS 12
description: Ce document décrit comment préparer le jusqu'à build 12 applications iOS et tvOS 12 avec Xamarin. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 887a5cc72b951b2f115cdc6998a6cf6ca7a94df0
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815629"
---
# <a name="getting-started-with-ios-12-and-tvos-12"></a>Bien démarrer avec iOS 12 et tvOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> IOS 12 tvOS 12 prise en charge et de Xamarin est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas complet, et peut changer. Utilisez-le pour l’expérimentation uniquement.

> [!NOTE]
> Pour plus d’informations, consultez le [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour Xamarin la version préliminaire.

Ce document décrit comment préparer le jusqu'à build 12 applications iOS et tvOS 12 avec Xamarin. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.

## <a name="download-and-install"></a>Téléchargez et installez

1. **Installer la dernière version bêta de Xcode 10** – les développeurs d’Apple inscrits peuvent télécharger et installer la dernière version du 10 Xcode à partir de la [portail des développeurs Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > Les kits de développement logiciel tvOS 12 iOS 12 sont distribués avec Xcode 10.

2. **Exécuter Xcode 10** – 10 de Xcode exécuter avant la mise à jour et d’exécuter Visual Studio pour Mac ou Visual Studio 2017 ; il installe des outils qui nécessite de Xamarin.

3. **Mettre à jour de Visual Studio pour Mac et Visual Studio 2017** : suivez les instructions de la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour installer la version préliminaire de Xamarin.

4. _(facultatif)_  **Installer la dernière version de bêta iOS sur vos appareils iOS** : appareil de test d’applications qu’utilisation nouvellement introduites iOS 12 ou tvOS 12 API, les développeurs Apple inscrits peuvent [télécharger](https://developer.apple.com/download) et installer le 12 iOS le plus récent ou tvOS 12 versions bêta de développeur sur leurs appareils iOS et tvOS.

   > [!TIP]
   > Même si votre application n’utilise pas de nouveau iOS 12 ni tvOS 12 API, veillez à le générer avec l’iOS 12 ou tvOS 12 Kit de développement logiciel (installé avec Xcode 10) et de test pour vous assurer que tout fonctionne comme prévu. Si une application n’appelle pas les nouvelles API, vous pouvez le recompiler avec l’iOS 12 ou tvOS 12 SDK et le tester sur les appareils qui n’ont pas encore été mis à niveau vers les nouveaux systèmes d’exploitation.

   > [!IMPORTANT]
   > Avant la mise à niveau de vos appareils iOS 12 ou tvOS 12 pour tester des applications Xamarin qui appellent 12 de la nouvelle iOS ou tvOS 12 API :
   > - Lecture [notes de publication d’Apple](https://developer.apple.com/download/) pour la mise à jour du système d’exploitation.
   > - Lecture la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour Xamarin la version préliminaire.

## <a name="related-links"></a>Liens connexes

- [Télécharger Xcode 10](https://developer.apple.com/download/)
- Aperçu de Xamarin [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/)
