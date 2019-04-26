---
title: Configurer votre application tvOS dans iTunes Connect
description: Cet article fournit un guide supplémentaire à configurer votre application dans iTunes Connect pour les configurations spécifiques tvOS d’iOS.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3f4ef00cfe990de2d5afd461d7a110d32bc4a236
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413238"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Configurer votre application tvOS dans iTunes Connect

_Cet article fournit un guide supplémentaire à configurer votre application dans iTunes Connect pour les configurations spécifiques tvOS d’iOS._


Outre les configurations et de paramètre que vous devrez apporter en suivant l’iOS [configurer votre application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) guide, ce document couvre les configurations spécifiques qui seront exigées pour libérer un Xamarin.tvOS application dans le Store d’application Apple TV.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Ajout d’un Version Release tvOS

Si vous créez une nouvelle application doit être publié le Store d’application Apple TV, ou ajoutez la prise en charge de l’Apple TV à une application iOS existante, vous devez avoir créé un iTunes Connect et configuré à l’aide de l’iOS suivantes guides spécifiques :

- [Création d’un enregistrement iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Gestion des vidéos et des captures d’écran de l’application](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Gestion du nom, de la description, des nouveautés, des mots clés et des URL](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Gestion des informations générales](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Si vous le souhaitez, vous pouvez avoir également besoin :

- [Gestion des informations relatives à Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Gestion des informations relatives aux achats dans l’application](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Toutes les étapes ci-dessus terminées, ouvrez l’enregistrement iTunes Connect et les sélectionner pour ajouter la prise en charge de tvOS à l’aide de la barre latérale gauche de votre application :

[![](itunes-connect-images/connect01.png "Ajouter la prise en charge de tvOS à l’aide de la barre latérale gauche")](itunes-connect-images/connect01.png#lightbox)

Les écrans des informations spécifiques de tvOS sera ensuite disponibles pour la donnée enregistrement iTunes Connect :

[![](itunes-connect-images/connect02.png "L’écran des informations spécifiques de tvOS")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>Informations de Version de tvOS

Dans la barre latérale de gauche, sélectionnez **1.0 Préparation pour la soumission** sous la section application tvOS :

[![](itunes-connect-images/connect03.png "Informations de Version de tvOS")](itunes-connect-images/connect03.png#lightbox)

Dans cet écran, fournissez les informations suivantes :

- Le requis captures d’écran, Description, mots-clés et URL.
- Informations de l’application de générales telles que le numéro de Version, le Copyright et âge.
- Achats dans l’application facultatifs.
- Prise en charge de facultatif Game Center avec les classements et les primes.
- Évaluation de l’application des informations telles que les contacts, les comptes de démonstration et les Notes requises.

Une fois que vous avez entré les informations requises, cliquez sur le **enregistrer** bouton dans le coin supérieur droit de l’écran pour enregistrer vos modifications :

[![](itunes-connect-images/connect04.png "tvOS prêt pour l’envoi des informations de Version")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Préparation de la soumettre pour examen

Lorsque vous êtes enfin prêt à soumettre votre application Xamarin.tvOS vers le Store d’application Apple TV pour révision, revenez à enregistrement iTunes l’application Connect et cliquez sur le **soumettre pour examen** situé dans l’angle supérieur droit de l’écran :

[![](itunes-connect-images/connect05.png "Soumettre pour examen")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a fourni la vue d’ensemble du paramètre tvOS spécifiques requis dans iTunes Connect pour publier une application de tvOS dans le Store d’application Apple TV.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
