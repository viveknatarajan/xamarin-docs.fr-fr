---
title: Mise à jour de votre application pour iOS 11
description: Ce document liens vers des guides pas à pas qui décrivent les nouvelles fonctionnalités disponibles pour les développeurs de Xamarin.iOS avec la version d’iOS 11. Par exemple, App Store de mises à jour de la conception visuelle, modifie et met à jour de l’icône de l’application.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: ea57acbd6165f7b1abd8b9bd69873670c179f411
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787570"
---
# <a name="updating-your-app-to-ios-11"></a>Mise à jour de votre application pour iOS 11

Dans iOS 11, Apple a introduit les mises à jour de l’architecture, des modifications visuelles nouvelle et un processus de connexion iTunes mis à jour. Ce guide explore de chacune de ces modifications, pour vous aider à obtenir votre application Xamarin.iOS mis à jour pour iOS 11.

## <a name="architecture-changesarchitecture-changesmd"></a>[Changements dans l'architecture](architecture-changes.md)

Un des changements plus importants que vous devez connaître avec iOS 11 est la désapprobation de prise en charge 32 bits pour les applications, comme détaillé dans [Apple](https://developer.apple.com/news/?id=06282017b) communiqué de presse.

Ce guide vous guide tout au long de la mise à jour de votre application 64 bits.

## <a name="visual-design-updatesvisual-designmd"></a>[Mises à jour de la conception visuelle](visual-design.md)

Dans iOS 11, Apple a introduit de nouvelles modifications visuelles, y compris les mises à jour de la barre de navigation, barre de recherche et les vues de table. En outre les améliorations ont été apportées pour permettre une plus grande flexibilité sur les marges et le contenu de l’écran complet. Ces modifications sont traitées dans ce guide.

## <a name="app-store-changesapp-store-changesmd"></a>[Changements dans la conception visuelle](app-store-changes.md)

L’App Store iOS a eu une conception totalement nouvelle, non seulement permet aux utilisateurs de naviguer efficacement dans le magasin, mais vous permet également, en tant que développeur, à votre application aux utilisateurs de promouvoir. Promotions incluent les mises à jour des achats dans l’application et les mises à jour de la page du produit. iOS 11 ajoute également les mises à jour concernant la communication avec les utilisateurs, l’ajout de l’icône de l’application et comment déployer votre application au public.

## <a name="app-icon-updates"></a>Icône de l’application des mises à jour

> [!NOTE]
> Icônes de l’application doivent maintenant être remis par un _catalogue_. 

Pour plus d’informations sur l’utilisation de catalogues de l’élément multimédia, reportez-vous à la [App Store icône](~/ios/app-fundamentals/images-icons/app-store-icon.md) guide. Pour vous aider à la migration d’icônes à partir d’un fichier Info.plist à un catalogue, consultez le [migration de Info.plist vers les catalogues Asset](~/ios/app-fundamentals/images-icons/app-icons.md) guide.

L’icône requis dans le catalogue est nommé **App Store** et doit être de 1 024 x 1024 taille. Apple indique que l’icône visible dans l’App Store et qui se trouve dans le catalogue de ressources ne peut pas être transparente ni contenir de canal alpha.

![Emplacement de l’icône Windows store dans le catalogue d’applications.](images/image1.png)

## <a name="related-links"></a>Liens associés

- [Nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
