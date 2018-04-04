---
title: Icône de l’App Store
description: Cet article explique comment, y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS pour être utilisée comme icône App Store.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: f8d993ccb23817e237b9cef8074b881f3ea4b3a2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="app-store-icon"></a>Icône de l’App Store

_Cet article explique comment, y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS pour être utilisée comme icône App Store._

Avant de Xcode 9 toutes les icônes de magasin d’applications ont été ajoutés via iTunes Connect. Toutefois, cela n’est plus le cas. Icônes de magasin de l’application doivent maintenant être inclus dans le cadre de votre offre groupée du projet et ajoutées au sein d’un catalogue. Les applications qui ne contiennent pas d’une icône de l’App Store seront rejetées par Apple.

L’icône de magasin d’application est l’image de votre application pour les utilisateurs, il doit donc être facile à mémoriser et affichage bien avec une petite taille. Les icônes mémorables sont nettes, simples et immédiatement reconnaissables.

Apple suggère de suivre les recommandations suivantes pour concevoir des icônes d’application :

- Créez une icône appropriée pour votre application.
- Créez une icône simple et cohérente avec la conception de votre application.
- Évitez d’utiliser des mots dans votre icône.
- Pensez de manière globale : une seule icône d’application est utilisée dans tous les App Store du monde.

Une image de 1 024 x 1 024 pixels est nécessaire pour l’icône d’application affichée dans l’App Store.  Apple indique que l’icône visible dans l’App Store et qui se trouve dans le catalogue de ressources ne peut pas être transparente ni contenir de canal alpha.

Pour plus d’informations, consultez Apple [iOS indications de l’Interface humaine](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Ajout d’une icône de l’App Store

Vous devez désormais fournir les icônes à afficher dans l’App Store via un catalogue de ressources. 

Pour ajouter une icône de l’App Store comme suit :

1. Recherchez le **AppIcon** image définie le **Assets.xcassets** fichier de votre projet. 
    - Tous les nouveaux projets doivent être accompagnée d’un un **Assets.xcassets** fichier qui contient un ensemble d’images AppIcon.
    - Pour ajouter un nouveau catalogue, cliquez sur votre projet, puis sélectionnez **Ajouter > nouveau fichier > catalogue**.
    - Pour ajouter un nouvel ensemble d’images une icône application, cliquez dans la zone de jeu d’icônes et sélectionnez **icônes de l’application et lancer les Images > nouvelle icône d’application**:
    
    ![Ajouter la nouvelle option de jeu d’image](app-store-icon-images/image1.png)

2. Faites défiler vers le **App Store** icône dans la liste :

    ![Icône de l’App Store](app-store-icon-images/image2.png)

3. Cliquez sur l’icône et recherchez votre image de pixel de 1024 x 1024. Enregistrez le catalogue.




## <a name="related-links"></a>Liens associés

- [Gestion des icônes avec des catalogues d’actif](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
