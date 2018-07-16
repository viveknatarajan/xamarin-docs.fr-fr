---
title: Bien démarrer avec iOS 12, 12 de tvOS et watchOS 5
description: Ce document décrit comment préparer le jusqu'à build 12 applications iOS et tvOS 12 avec Xamarin. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 70f67f934c2503e6f6fa0d3bae1f37bcc1f6f0a4
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030650"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Bien démarrer avec iOS 12, 12 de tvOS et watchOS 5

![Preview](~/media/shared/preview.png)

> [!WARNING]
> 12, tvOS de Xamarin iOS 12, et prise en charge watchOS 5 est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas complet, et peut changer. Utilisez-le pour l’expérimentation uniquement.

> [!NOTE]
> Pour plus d’informations, consultez la préversion Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Ce document décrit comment préparer le jusqu'à build iOS 12, 12 de tvOS et watchOS 5 des applications avec Xamarin. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.

## <a name="download-and-install"></a>Téléchargez et installez

1. **Installer la dernière version bêta de Xcode 10** – les développeurs d’Apple inscrits peuvent télécharger et installer la dernière version du 10 Xcode à partir de la [portail des développeurs Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > Le 12 d’iOS, tvOS 12 et kits SDK watchOS 5 est distribués avec Xcode 10.

2. **Exécuter Xcode 10** – 10 de Xcode exécuter avant la mise à jour et d’exécuter Visual Studio pour Mac ou Visual Studio 2017 ; il installe des outils qui nécessite de Xamarin.

3. **Mettre à jour de Visual Studio pour Mac et Visual Studio 2017** : suivez les instructions de la [release blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/) pour installer la version préliminaire de Xamarin.

4. _(facultatif)_  **Installer la dernière version de bêta iOS sur vos appareils iOS** : appareil de test d’applications qu’utilisation nouvellement introduites iOS 12, tvOS 12 ou watchOS 5 API, les développeurs Apple inscrits peuvent [télécharger](https://developer.apple.com/download) et installer la dernière iOS 12, tvOS 12 ou versions bêta de développeur watchOS 5 sur leurs appareils.

   > [!TIP]
   > Même si votre application n’utilise pas de n’importe quel nouveau iOS 12, tvOS 12 ou watchOS 5 API, veillez à le générer avec le 12 d’iOS, tvOS 12 ou watchOS 5 Kit de développement logiciel (installé avec Xcode 10) et de test pour vous assurer que tout fonctionne comme prévu. Si une application n’appelle pas les nouvelles API, vous pouvez le recompiler avec le 12 d’iOS, tvOS 12 ou watchOS 5 SDK et le tester sur les appareils qui n’ont pas encore été mis à niveau vers les nouveaux systèmes d’exploitation.

   > [!IMPORTANT]
   > Avant la mise à niveau de vos appareils iOS 12, tvOS 12 ou watchOS 5 pour tester des applications Xamarin qui appellent 12 de la nouvelle iOS, tvOS 12 ou watchOS 5 API :
   > - Lecture [notes de publication d’Apple](https://developer.apple.com/download/) pour la mise à jour du système d’exploitation.
   > - Lire l’aperçu Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

## <a name="related-links"></a>Liens connexes

- [Télécharger Xcode 10](https://developer.apple.com/download/)
- Aperçu de Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
