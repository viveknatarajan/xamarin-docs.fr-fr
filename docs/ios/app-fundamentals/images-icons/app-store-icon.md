---
title: Icônes d’application Store dans Xamarin.iOS
description: Ce document décrit comment utiliser des catalogues de composants pour gérer une icône de l’App Store pour une application Xamarin.iOS. Auparavant, les icônes de l’App Store ont été gérés avec iTunes Connect.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/26/2017
ms.openlocfilehash: 53e25ae9f4650254f2aaaa03dc8727fae674c9f0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105881"
---
# <a name="app-store-icons-in-xamarinios"></a>Icônes d’application Store dans Xamarin.iOS

Avant de Xcode 9 toutes les icônes d’application Store ont été ajoutées via iTunes Connect. Toutefois, cela n’est plus le cas. Icônes de l’App Store doivent maintenant être inclus dans le cadre de votre ensemble de projet et ajoutées au sein d’un catalogue de ressources. Les applications qui ne contiennent pas d’une icône de l’App Store sont rejetées par Apple.

L’icône d’application Store est la face de votre application aux utilisateurs, elle doit donc être facile à mémoriser et afficher correctement à une petite taille. Les icônes mémorables sont nettes, simples et immédiatement reconnaissables.

Apple suggère de suivre les recommandations suivantes pour concevoir des icônes d’application :

- Créez une icône appropriée pour votre application.
- Créez une icône simple et cohérente avec la conception de votre application.
- Évitez d’utiliser des mots dans votre icône.
- Pensez de manière globale : une seule icône d’application est utilisée dans tous les App Store du monde.

Une image de 1 024 x 1 024 pixels est nécessaire pour l’icône d’application affichée dans l’App Store.  Apple indique que l’icône visible dans l’App Store et qui se trouve dans le catalogue de ressources ne peut pas être transparente ni contenir de canal alpha.

Pour plus d’informations, consultez d’Apple [Human Interface Guidelines pour iOS](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Ajout d’une icône de l’App Store

Vous devez désormais fournir les icônes à afficher dans l’App Store via un catalogue de ressources. 

Pour ajouter une icône de l’App Store les opérations suivantes :

1. Recherchez le **AppIcon** image définie le **Assets.xcassets** fichier de votre projet. 
    - Tous les nouveaux projets doivent être accompagnés un un **Assets.xcassets** fichier qui contient un ensemble d’images AppIcon.
    - Pour ajouter un nouveau catalogue d’actifs, cliquez sur votre projet, puis sélectionnez **Ajouter > nouveau fichier > catalogue de composants**.
    - Pour ajouter un nouvel ensemble d’images une icône application, cliquez sur dans la zone de jeu d’icônes, puis sélectionnez **icônes d’application & Images de lancement > nouvelle icône d’application**:
    
    ![Ajouter la nouvelle option de jeu d’image](app-store-icon-images/image1.png)

2. Faites défiler vers le **App Store** icône dans la liste :

    ![Icône de l’App Store](app-store-icon-images/image2.png)

3. Cliquez sur l’icône et recherchez votre image de 1 024 x 1 024 pixels. Enregistrer le catalogue de composants.




## <a name="related-links"></a>Liens associés

- [La gestion des icônes avec les catalogues de composants](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
