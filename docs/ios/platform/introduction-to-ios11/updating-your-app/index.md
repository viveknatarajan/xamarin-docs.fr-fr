---
title: Mise à jour de votre application avec iOS 11
description: Ce document contient des liens vers des guides différents qui décrivent les nouvelles fonctionnalités qui permettent aux développeurs de Xamarin.iOS avec la version d’iOS 11. Par exemple, change de mises à jour de la conception visuelle, App Store, et met à jour de l’icône de l’application.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 3193f27c30080a4335abfe969acb3c8b33516469
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386341"
---
# <a name="updating-your-app-to-ios-11"></a>Mise à jour de votre application avec iOS 11

Dans iOS 11, Apple a introduit les mises à jour de l’architecture, de nouvelles modifications visual et d’un processus de Connect de mise à jour iTunes. Ce guide explore de chacune de ces modifications, qui vous aideront à faire passer votre application Xamarin.iOS mis à jour pour iOS 11.

## <a name="architecture-changesarchitecture-changesmd"></a>[Changements dans l'architecture](architecture-changes.md)

Un des plus grands changements que vous devez connaître avec iOS 11 est la désapprobation de prise en charge 32 bits pour les applications, comme détaillé dans [Apple](https://developer.apple.com/news/?id=06282017b) communiqué de presse.

Ce guide vous guide tout au long de la mise à jour de votre application pour 64 bits.

## <a name="visual-design-updatesvisual-designmd"></a>[Mises à jour de la conception visuelle](visual-design.md)

Dans iOS 11, Apple a introduit de nouvelles modifications visuelles, y compris les mises à jour de la barre de navigation, barre de recherche et les vues de table. En outre des améliorations ont été apportées pour permettre une plus grande flexibilité sur les marges et du contenu de plein écran. Ces modifications sont traitées dans ce guide.

## <a name="app-store-changesapp-store-changesmd"></a>[Changements dans la conception visuelle](app-store-changes.md)

L’App Store iOS a eu une conception totalement nouvelle, qui non seulement permet aux utilisateurs de naviguer efficacement dans le magasin, mais vous permet également, en tant que développeur, pour promouvoir votre application aux utilisateurs. Ces promotions incluent les mises à jour pour les achats dans l’application et les mises à jour de la page du produit. iOS 11 ajoute également des mises à jour concernant comment communiquer avec les utilisateurs, l’ajout de l’icône de votre application et comment publier votre application pour le grand public.

## <a name="app-icon-updates"></a>Mises à jour de l’icône de l’application

> [!NOTE]
> Icônes d’application doivent maintenant être remis par un _catalogue_. 

Pour plus d’informations sur l’utilisation des catalogues de composants, reportez-vous à la [App Store icône](~/ios/app-fundamentals/images-icons/app-store-icon.md) guide. Pour vous aider à migrer les icônes à partir d’un fichier Info.plist pour un catalogue de ressources, consultez le [migration de Info.plist vers des catalogues de composants](~/ios/app-fundamentals/images-icons/app-icons.md) guide.

L’icône nécessaire dans le catalogue de ressources est nommé **App Store** et doit être de 1 024 x 1 024 taille. Apple indique que l’icône visible dans l’App Store et qui se trouve dans le catalogue de ressources ne peut pas être transparente ni contenir de canal alpha.

![Emplacement de l’icône magasin d’applications dans le catalogue de composants.](images/image1.png)

## <a name="related-links"></a>Liens associés

- [Quelles sont les nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [La mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
