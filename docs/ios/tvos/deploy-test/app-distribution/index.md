---
title: "Vue d’ensemble de la distribution d’applications"
description: "Ce document donne une vue d’ensemble des techniques de distribution qui sont disponibles pour l’application de Xamarin.tvOS et sert de pointeur vers des documents plus détaillées sur la rubrique."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 3e96e98f90c7f4c849a9f679b2de819ccaabfec0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="app-distribution-overview"></a>Vue d’ensemble de la distribution d’applications

_Ce document donne une vue d’ensemble des techniques de distribution qui sont disponibles pour l’application de Xamarin.tvOS et sert de pointeur vers des documents plus détaillées sur la rubrique._


Une fois que votre application Xamarin.tvOS a été développée, l’étape suivante du cycle de vie de développement logiciel est à distribuer votre application aux utilisateurs, comme indiqué dans la section en surbrillance le schéma ci-dessous :


[![La vue d’ensemble du cycle de vie de développement logiciel](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple fournit les méthodes suivantes pour distribuer une application de tvOS, qui sont pris en charge par Xamarin.tvOS :

1. [**App Store**](#Apple-TV-App-Store-Distribution)
2. [**Interne (Entreprise)**](#In-House-Distribution) 
2. [**Ad hoc**](#Ad_Hoc_Distribution) 

Dans tous ces scénarios, les applications doivent être provisionnées à l’aide du *profil de provisionnement* approprié. Les profils de provisionnement sont des fichiers qui contiennent les informations sur le code de signature, ainsi que l’identité de l’application et le mécanisme de distribution prévu. Pour la distribution hors App Store, ils contiennent également des informations relatives aux appareils sur lesquels l’application peut être déployée.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Distribution de l’App Store d’Apple TV

Il s’agit de la méthode principale que les applications de tvOS sont distribuées aux consommateurs sur les appareils Apple TV. Toutes les applications sont soumises à l’App Store d’Apple TV doivent être approuvées par Apple.

Les applications sont soumises à l’App Store via un portail appelé *iTunes Connect*. Veuillez consulter notre [configurer votre application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide pour plus d’informations sur les rubriques suivantes :

- La gestion des contrats, taxes et bancaire.
- La création d’un iTunes enregistrement de se connecter.
- Gestion des vidéos d’application et des captures d’écran.
- Nom de l’application, Description, quelles sont les nouveautés, de la gestion des mots clés et les URL.
- Gérer les informations de l’application de général.
- Informations de mise à jour Game Center.
- Gérer les informations de révision de l’application.
- Gestion des informations de tarification.
- Informations de mise à jour dans l’application fournisseur.
- Affichage des révisions d’Application.

Alors que ne pas spécifiquement écrits pour tvOS, ce document fournit des informations sur la façon de configurer et utiliser ce portail pour préparer votre application pour la publication dans l’App Store d’Apple TV. Grand nombre des mêmes étapes sont requises si vous configurez une application iOS ou tvOS.

Une fois que vous avez effectué toutes les étapes ci-dessus, consultez notre [configurer votre application tvOS dans iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) pour ajouter les informations de spécifique de l’application de tvOS qui seront nécessaires.

Il est important de noter que seuls les développeurs membres du **programme Developer d’Apple** ont accès à iTunes Connect. Les membres du programme **Developer Enterprise d’Apple** n’y ont pas accès.

Si vous rencontrez des problèmes de soumission de votre application Xamarin.tvOS vers le magasin d’applications Apple TV, veuillez consulter notre [dépannage](~/ios/tvos/troubleshooting.md) guide. Il contient plusieurs problèmes connus que vous pouvez rencontrer et comment les résoudre dans les Xamarin.tvOS.

Pour plus d’informations, visitez le [publication sur l’App Store d’Apple TV](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) guide.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Distribution en interne

Parfois appelée *Distribution d’entreprise*, la distribution en interne permet aux membres du **programme Developer Enterprise d’Apple** de distribuer des applications en interne à d’autres membres de la même organisation. La distribution en interne présente l’avantage de ne pas nécessiter une évaluation pour l’App Store et de ne pas limiter le nombre d’appareils sur lesquels une application peut être installée. Toutefois, il est important de noter que les membres du **programme Developer Enterprise d’Apple** n’ont **pas** accès à iTunes Connect, et que le licencié est donc responsable de la distribution de l’application.

Pour plus d’informations sur l’obtention de la configuration et la distribution de votre application en interne, reportez-vous à la [guide de Distribution interne](~/ios/deploy-test/app-distribution/in-house-distribution.md). Ce document est spécifique à iOS, mais les mêmes techniques sont utilisés pour les applications de tvOS.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Distribution ad hoc

Xamarin.tvOS applications peuvent être testé par utilisateur via une distribution ad hoc, qui est disponible sur les deux le **Apple Developer Program**et le **Apple Developer Enterprise Program**et permet aux appareils d’Apple TV jusqu'à 100 à tester. Le meilleur des cas utiliser pour la distribution ad hoc sont la distribution au sein de votre société quand iTunes Connect n’est pas une option.

Pour plus d’informations sur l’obtention de la configuration et la distribution de votre application en interne, reportez-vous à la [guide de Distribution Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Là encore, ce document est spécifique à iOS, mais les mêmes techniques sont utilisées pour les applications de tvOS.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble des mécanismes de distribution qui sont disponibles pour les applications Xamarin.tvOS. Il a introduit le magasin d’applications Apple TV, Ad Hoc et en interne déploiement et fourni des liens vers des informations plus détaillées.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
