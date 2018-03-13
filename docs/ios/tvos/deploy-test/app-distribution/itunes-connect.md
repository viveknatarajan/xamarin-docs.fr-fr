---
title: Configurez votre application tvOS dans iTunes Connect
description: "Cet article fournit un guide supplémentaire à configurer votre application dans iTunes Connect pour les configurations spécifiques tvOS iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b6fad9eadbff272f86f9e426e3f6eb5d48847127
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Configurez votre application tvOS dans iTunes Connect

_Cet article fournit un guide supplémentaire à configurer votre application dans iTunes Connect pour les configurations spécifiques tvOS iOS._


Outre les configurations et le paramètre dont vous aurez besoin pour créer en suivant l’iOS [configurer votre application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide, ce document décrit les configurations spécifiques qui seront requis pour libérer un Xamarin.tvOS application dans le magasin d’applications Apple TV.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Ajout d’une Version Release tvOS

Si vous créez une nouvelle application à être publié sur l’App Store d’Apple TV ou en ajoutant la prise en charge Apple TV à une application iOS existante, vous devez avoir créé un iTunes Connect et qu’il est configuré à l’aide de l’e/s suivants Guide spécifique :

- [Création d’un enregistrement iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Gestion des vidéos et des captures d’écran de l’application](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Gestion du nom, de la description, des nouveautés, des mots clés et des URL](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Conservation des informations générales](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Si vous le souhaitez, vous pouvez avoir également besoin :

- [Gestion des informations relatives à Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Gestion des informations relatives aux achats dans l’application](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Toutes les étapes ci-dessus s’est terminées, ouvrez iTunes Connect enregistrement de votre application et les sélectionner pour ajouter la prise en charge de tvOS à l’aide de la barre latérale gauche :

[![](itunes-connect-images/connect01.png "Ajouter la prise en charge de tvOS à l’aide de la barre latérale gauche")](itunes-connect-images/connect01.png#lightbox)

Les écrans des informations spécifiques de tvOS sera ensuite disponibles pour l’enregistrement de connexion d’iTunes donné :

[![](itunes-connect-images/connect02.png "L’écran des informations spécifiques de tvOS")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS informations de Version

Dans le volet gauche, sélectionnez **1.0 Préparation pour l’envoi de** sous la section tvOS application :

[![](itunes-connect-images/connect03.png "tvOS informations de Version")](itunes-connect-images/connect03.png#lightbox)

Dans cet écran, fournissez les informations suivantes :

- Le requis captures d’écran, Description, mots clés et les URL.
- Informations générales application telles que le numéro de Version, les droits d’auteur et âge.
- Achats dans l’application facultatif.
- Prise en charge de facultatif Game Center avec ajouterons et des résultats.
- Les informations de révision d’application telles que les contacts, les comptes de démonstration et les Notes nécessaires.

Une fois que vous avez entré les informations requises, cliquez sur le **enregistrer** bouton dans le coin supérieur droit de l’écran pour enregistrer vos modifications :

[![](itunes-connect-images/connect04.png "tvOS prêt à envoyer des informations de Version")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Préparation de la soumettre pour révision

Lorsque vous êtes enfin prêt à envoyer votre application Xamarin.tvOS vers le magasin d’applications Apple TV pour révision, revenez à iTunes de l’application enregistrement de connexion, puis cliquez sur le **soumettre pour révision** situé dans l’angle supérieur droit de l’écran :

[![](itunes-connect-images/connect05.png "Soumettre à des fins de révision")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a donné à la vue d’ensemble du paramètre tvOS spécifiques requis dans iTunes Connect pour publier une application de tvOS sur l’App Store d’Apple TV.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
