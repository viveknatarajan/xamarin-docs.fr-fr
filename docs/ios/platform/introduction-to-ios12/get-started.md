---
title: Bien démarrer avec iOS 12, 12 de tvOS et watchOS 5
description: Ce document décrit comment préparer le jusqu'à build iOS 12, 12 de tvOS et watchOS 5 des applications avec Xamarin. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615172"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Bien démarrer avec iOS 12, 12 de tvOS et watchOS 5

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Prise en charge de Xamarin pour iOS 12, tvOS 12 et kits de développement logiciel watchOS 5 distribués avec Xcode 10 est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas fonctionnalité termine et peut changer. Utilisez-le pour l’expérimentation uniquement.

Ce document décrit comment configurer pour créer des applications Xamarin qui appellent les API publiées avec Xcode 10. Il explique comment télécharger Xcode 10 et mettre à jour de Visual Studio pour Mac et Visual Studio 2017.

## <a name="download-and-install"></a>Téléchargez et installez

1. **Installer la dernière version bêta de Xcode 10** – les développeurs d’Apple inscrits peuvent télécharger et installer la dernière version du 10 Xcode à partir de la [portail des développeurs Apple](https://developer.apple.com/download/).

2. **Exécuter Xcode 10** – nécessite d’exécuter Xcode 10 avant la mise à jour et en cours d’exécution Visual Studio pour Mac ou Visual Studio 2017, car elle installe certains des outils que Xamarin.

3. **Mettre à jour de Visual Studio pour Mac et Visual Studio 2017** : suivez les instructions de la [release blog](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) pour installer la version préliminaire de Xamarin.

4. _(facultatif)_  **Installer la dernière version de bêta iOS sur vos appareils iOS** : appareil de test d’applications qui utilisent les API introduites avec les 10 Xcode, inscrit peut de développeurs Apple [télécharger](https://developer.apple.com/download) et installez la dernière version versions bêta de développeur sur leurs appareils.

   > [!TIP]
   > Même si votre application n’utilise pas les nouvelles API, veillez à le générer avec les kits de développement logiciel plus récent Xcode 10 et testez-le pour vous assurer que tout fonctionne comme prévu. Si une application n’appelle pas les nouvelles API, vous pouvez le recompiler avec ces nouveaux kits de développement logiciel et le tester sur les appareils qui n’ont pas encore été mis à niveau vers le nouveau système d’exploitation.
   >
   > Avant la mise à niveau de vos appareils vers le dernier système d’exploitation à partir d’Apple pour tester vos applications Xamarin, veillez à :
   >
   > - Lecture [notes de publication d’Apple](https://developer.apple.com/download/) pour les mises à jour du système d’exploitation.
   > - Lire l’aperçu Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Liens connexes

- [Télécharger Xcode 10](https://developer.apple.com/download/)
- Aperçu de Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
