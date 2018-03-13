---
title: "Modifications du magasin d’applications"
description: "Explorer les nouvelles fonctionnalités d’iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 78528c750c6350d113b34a07d166a03773119a8b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="app-store-changes"></a>Modifications du magasin d’applications

_Explorer les nouvelles fonctionnalités d’iOS 11_

L’App Store iOS a eu une conception totalement nouvelle, non seulement permet aux utilisateurs de naviguer efficacement dans le magasin, mais vous permet également, en tant que développeur, à votre application aux utilisateurs de promouvoir. Promotions incluent les mises à jour des achats dans l’application et les mises à jour de la page du produit. iOS 11 ajoute également les mises à jour concernant la communication avec les utilisateurs, l’ajout de l’icône de l’application et comment déployer votre application au public.

![Nouvelle disposition de magasin d’application](app-store-changes-images/image3.jpg)

Le magasin d’applications repensé comporte les sections suivantes :

- **Aujourd'hui** – cet onglet comprend les applications qui sont un choix de l’éditeur « » ou proposées d’application. Pour promouvoir votre application ici renseignez les informations sur le [promouvoir](https://developer.apple.com//contact/app-store/promote/) page.
- **Jeux** – toutes les applications qui est définie sur le **jeu** catégorie dans iTunes Connect peut être trouvée sous cet onglet.
- **Applications** : cet onglet comprend toutes les autres applications. Les utilisateurs peuvent parcourir par les applications proposées, des catégories, payé/libre supérieur.
- **Les mises à jour** – cette application affiche les mises à jour à vos applications. Même si vous relâchez une application via [en plusieurs phases de mise en production](#Phased_Release), les utilisateurs peuvent toujours accéder à cet onglet et télécharger la dernière version.
- **Recherche** – cet onglet permet aux utilisateurs de rechercher pour votre application.

## <a name="store-icon"></a>Icône de magasin

Icônes de magasin (ou marketing) ne soient plus gérés dans iTunes Connect et à la place doit être contenue en tant qu’un [catalogue](~/ios/app-fundamentals/images-icons/app-icons.md) dans votre binaire d’application, semblable aux icônes de l’application. Une icône de 1024 x 1024 magasin au format PNG doit être incluse dans un catalogue pour l’envoi réussi des applications iOS 11.

Réduction de l’application permet de s’assurer que ce catalogue supplémentaires n’augmente pas la taille de l’application.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>Achats dans l’application promu dans l’App Store

Apple a effectué les achats dans l’application pour qu’il soit plus détectable dans l’App Store. Vous pouvez maintenant ajouter jusqu'à 20 _App Store promues_ achats dans l’application qui se trouve désormais dans la page applications, sur la page de produit de votre application, ou en effectuant une recherche.

Pour que vos achats dans l’application affichés dans l’App Store, vous devez inclure les données suivantes :

- **Image** – vous devez fournir une image spécialement conçue pour l’icône qui décrit ce que fait l’achat dans l’application. Cette image doit être différente de l’icône d’application et ne peut pas être une capture d’écran.
- **Nom** – le nom peut être uniquement un maximum de 30 caractères.
- **Description** – la description peut comporter uniquement les 45 caractères au maximum.

Les promotions dans l’application achat sont soumis à une révision de magasin d’application avant de pouvoir être publiée.

Pour rendre vos achats dans l’application disponibles pour promouvoir, ouvrez votre application et accédez à **fonctionnalités > Achats dans l’application**. Accédez à la **App Store Promotion (facultatif)** section et ajouter une image de 1024 x 1024 et **enregistrer**, comme illustré dans l’image suivante :

![Section Promotion App Store iTune Connect](app-store-changes-images/image4.png)

Vous devez également ajouter le `ShouldAddStorePayment` méthode à la `SKPaymentTransactionObserver` protocole dans votre application.

Pour plus d’informations sur les offres d’achat de dans l’application, consultez Apple [promouvoir les achats dans l’application votre](https://developer.apple.com/app-store/promoting-in-app-purchases/) page.

## <a name="redesigned-product-page"></a>Page produit repensé

Les modifications suivantes ont été apportées à la page du produit :

- Titres sont maintenant définies pour un maximum de 30 caractères
- Un sous-titre a été ajouté.
    - Il doit être une brève description qui complète le titre.
    - Sous-titre doit avoir un maximum de 30 caractères
- Aperçus d’application
    - Vous pouvez désormais avoir trois vidéos ou des captures d’écran.
    - Exécution automatique de vidéos lorsqu’un utilisateur accède à la page.
    - Pour plus d’informations, consultez Apple [aperçu de l’application](https://developer.apple.com/app-store/app-previews/) page.
- Texte promotionnel
    - Cette fonctionnalité offre 170 caractères de texte, ce qui vous permet de décrire les informations évolutifs sur votre application.
    - Il peut être mis à jour à tout moment sans avoir à envoyer une nouvelle version de l’application.

## <a name="customer-communication"></a>Communication du client

En 10.3, Apple, a lancé une nouvelle façon pour les développeurs de communiquer directement avec les utilisateurs d’application grâce à la possibilité de répondre aux révisions. Vous pouvez accéder à cette nouvelle fonctionnalité dans iTunes se connecter en accédant à **application > activité > et analyses**, comme illustré dans l’image suivante :

![Boîte de dialogue affichant la zone pour entrer des commentaires de réponse](app-store-changes-images/image5.png)

Il existe quelques éléments à connaître lors de la réponse pour les utilisateurs :

- Vous pouvez répondre uniquement une seule fois, mais les deux parties peuvent modifier leurs commentaires autant qu’ils le souhaitent.
- Les utilisateurs obtiennent une notification lorsque vous répondez à un commentaire.
- Une nouvelle prise en charge client rôle a été créé dans iTunes se connecter. Les utilisateurs avec ce rôle ou un rôle d’administrateur puissent répondre aux commentaires.

Pour plus d’informations, consultez Apple [répond aux révisions](https://developer.apple.com/app-store/responding-to-reviews/) page.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Mise en production en plusieurs phases

IOS 11, Apple a implémenté l’option des versions en plusieurs phases des mises à jour à votre application. Vous pouvez activer en plusieurs phases des versions autoriser votre application mise à jour à libérer progressivement aux clients, s’assurer que la demande ne surcharge pas votre environnement de production.

Les versions en plusieurs phases sont activées dans iTunes Connect. Cliquez sur votre application dans la barre latérale, faites défiler vers le **en plusieurs phases de la version de mises à jour automatiques** section en bas, puis sélectionnez **mise à jour de la mise en production sur 7 jours période à l’aide en plusieurs phases de mise en production**:

![Affichage de l’option en plusieurs phases de mise en production pour les mises à jour automatiques](app-store-changes-images/image6.png)

Votre mise à jour est disponible immédiatement dans l’onglet mises à jour de l’App Store. Les versions en plusieurs phases sont uniquement disponibles pour les utilisateurs qui ont les téléchargements automatiques sélectionnés.


## <a name="related-links"></a>Liens associés

- [Nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
