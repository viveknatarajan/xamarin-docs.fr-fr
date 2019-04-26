---
title: Nouveautés d’App Store iOS 11
description: Ce document explore les modifications apportées à l’App Store dans iOS 11. Il aborde l’icône du magasin d’une application, promues achats dans l’application, page du produit repensé, communication avec le client et les versions en plusieurs phases.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 022d6b5c3f85863352dd1343752e934240b357aa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61229653"
---
# <a name="app-store-changes-in-ios-11"></a>Nouveautés d’App Store iOS 11

L’App Store iOS a eu une conception totalement nouvelle, qui non seulement permet aux utilisateurs de naviguer efficacement dans le magasin, mais vous permet également, en tant que développeur, pour promouvoir votre application aux utilisateurs. Ces promotions incluent les mises à jour pour les achats dans l’application et les mises à jour de la page du produit. iOS 11 ajoute également des mises à jour concernant comment communiquer avec les utilisateurs, l’ajout de l’icône de votre application et comment publier votre application pour le grand public.

![Nouvelle disposition de magasin de l’application](app-store-changes-images/image3.jpg)

Le magasin d’applications repensé comprend les sections suivantes :

- **Aujourd'hui** – cet onglet inclut les applications qui sont un choix « le mot du rédacteur » ou application proposée. Pour promouvoir votre application ici renseignez les informations sur le [promouvoir](https://developer.apple.com//contact/app-store/promote/) page.
- **Jeux** – toutes les applications qui est défini sur le **jeu** catégorie dans iTunes Connect peut être trouvée sous cet onglet.
- **Applications** – cet onglet fonctionnalités de toutes les autres applications. Les utilisateurs peuvent parcourir par les applications proposées, des catégories, payé/free supérieur.
- **Mises à jour** – cette application affiche les mises à jour à vos applications. Même si vous publiez une application via [en plusieurs phases de mise en production](#Phased_Release), les utilisateurs peuvent toujours accéder à cet onglet et téléchargez la dernière version.
- **Recherche** – cet onglet permet aux utilisateurs de recherche pour votre application.

## <a name="store-icon"></a>Icône de Store

Icônes de Store (ou d’icônes marketing) ne soient plus gérés dans iTunes Connect et à la place le doivent figurer sous un [catalogue](~/ios/app-fundamentals/images-icons/app-icons.md) dans votre binaire d’application, similaire à des icônes d’application. Une icône de 1 024 x 1 024 magasin au format PNG doit être incluse dans un catalogue de ressources pour l’envoi réussi d’applications iOS 11.

Réduction de l’application permet de s’assurer que ce catalogue de composants supplémentaires n’augmente pas la taille de l’application.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>Achats dans l’application promu dans l’App Store

Apple a effectué les achats dans l’application pour qu’il soit plus détectable dans l’App Store. Vous pouvez maintenant ajouter jusqu'à 20 _Store application promues_ achats dans l’application qui se trouve désormais dans la page applications, sur la page de produit de votre application, ou en effectuant une recherche.

Pour que vos achats dans l’application affichés dans l’App Store, vous devez inclure les données suivantes :

- **Image** – vous devez fournir une image spécialement conçue pour l’icône qui décrit ce que fait l’achat dans l’application. Cette image doit être différente de l’icône d’application et ne peut pas être une capture d’écran.
- **Nom** – le nom peut être uniquement un maximum de 30 caractères.
- **Description** – la description peut comporter uniquement les 45 caractères au maximum.

Les promotions d’achat dans l’application sont soumis à une évaluation de magasin de l’application avant de pouvoir être publié.

Pour rendre vos achats dans l’application disponibles pour promouvoir, ouvrez votre application et accédez à **fonctionnalités > Achats dans l’application**. Accédez à la **App Store Promotion (facultatif)** section et ajouter une image de 1 024 x 1 024 et **enregistrer**, comme illustré dans l’image suivante :

![Section de Promotion App Store dans iTune Connect](app-store-changes-images/image4.png)

Vous devez également ajouter le `ShouldAddStorePayment` méthode à la `SKPaymentTransactionObserver` protocole dans votre application.

Pour plus d’informations sur les offres d’achat dans l’application, consultez d’Apple [promotion de vos achats de dans l’application](https://developer.apple.com/app-store/promoting-in-app-purchases/) page.

## <a name="redesigned-product-page"></a>Page de produit repensé

Les modifications suivantes ont été apportées à la page de produit :

- Titres sont maintenant définies sur un maximum de 30 caractères
- Un sous-titre a été ajouté.
    - Il doit être un bref résumé qui complète le titre.
    - Sous-titre doit avoir un maximum de 30 caractères
- Aperçus d’application
    - Vous pouvez désormais avoir trois vidéos ou des captures d’écran.
    - Vidéos la lecture automatique lorsqu’un utilisateur consulte la page.
    - Pour plus d’informations, consultez d’Apple [aperçu de l’application](https://developer.apple.com/app-store/app-previews/) page.
- Texte promotionnel
    - Cette nouvelle fonctionnalité fournit 170 caractères de texte, ce qui vous permet de décrire les informations relatives à votre application change fréquemment.
    - Vous pouvez mettre à jour à tout moment sans avoir à envoyer une nouvelle version de l’application.

## <a name="customer-communication"></a>Communication avec le client

En 10.3, Apple lancé une nouvelle façon pour les développeurs de communiquer directement avec les utilisateurs de l’application grâce à la possibilité de répondre aux révisions. Vous pouvez accéder à cette nouvelle fonctionnalité dans iTunes se connecter en accédant à **application > activité > des appréciations et des**, comme illustré dans l’image suivante :

![Boîte de dialogue affichant la zone pour entrer la réponse pour insérer un commentaire](app-store-changes-images/image5.png)

Il existe quelques éléments à connaître lors de la réponse aux utilisateurs :

- Vous pouvez seulement répondre qu’une seule fois, mais les deux parties peuvent modifier leurs commentaires autant qu’ils le souhaitent.
- Les utilisateurs recevoir une notification lorsque vous réagissez à un commentaire.
- Une nouvelle prise en charge client rôle a été créé dans iTunes se connecter. Les utilisateurs disposant de ce rôle ou un rôle d’administrateur peuvent répondre aux commentaires.

Pour plus d’informations, consultez d’Apple [répondre aux révisions](https://developer.apple.com/app-store/responding-to-reviews/) page.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Mise en production en plusieurs phases

Avec iOS 11, Apple a implémenté l’option des versions échelonnées des mises à jour à votre application. Vous pouvez activer des versions en plusieurs phases à la mise à jour de votre application à être progressivement aux clients, en garantissant que la demande ne surcharge pas votre environnement de production.

Les versions en plusieurs phases sont activées dans iTunes Connect. Cliquez sur votre application dans la barre latérale, faites défiler jusqu'à la **par phases Release pour les mises à jour automatiques** section en bas, puis sélectionnez **mise à jour de la mise en production à l’aide période de 7 jours par phases version**:

![Affichage de l’option en plusieurs phases de mise en production pour les mises à jour automatiques](app-store-changes-images/image6.png)

Votre mise à jour est disponible immédiatement dans l’onglet mises à jour de l’App Store. Les versions en plusieurs phases sont uniquement disponibles pour les utilisateurs qui ont des téléchargements automatiques sélectionnés.


## <a name="related-links"></a>Liens associés

- [Quelles sont les nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [La mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
