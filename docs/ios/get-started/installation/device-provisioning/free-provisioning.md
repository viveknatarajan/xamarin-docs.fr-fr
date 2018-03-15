---
title: Provisionnement libre
description: "Avec la version d’Apple Xcode 7, un important changement a eu lieu pour tous les développeurs iOS et Mac : le provisionnement libre."
ms.topic: article
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4e93696f8eef44030ffacbdbaa8ebcd860a402f6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="free-provisioning"></a>Provisionnement libre

_Avec la version d’Apple Xcode 7, un important changement a eu lieu pour tous les développeurs iOS et Mac : l’approvisionnement gratuit._

Le provisionnement libre permet aux développeurs de déployer leur application Xamarin.iOS sur leur appareil iOS **sans** faire partie d’un **programme pour développeurs Apple**. Cet avantage est énorme pour les développeurs, car des tests sur un appareil offrent de nombreux atouts par rapport à des tests sur un simulateur, notamment, mais sans s’y limiter, en termes de mémoire, stockage et connectivité réseau.

Le provisionnement sans compte de développeur Apple doit être effectué via Xcode, ce qui permet de créer une *identité de signature* (contenant un certificat de développeur et une clé privée) ainsi qu’un *profil de provisionnement* (contenant un ID d’application explicite et l’UDID de votre appareil iOS connecté).

## <a name="requirements"></a>Configuration requise

Pour tirer parti du déploiement de vos applications Xamarin.iOS sur un appareil avec le provisionnement libre, vous devez utiliser Xcode 7 ou supérieur.

**L’ID Apple utilisé ne doit pas être connecté à un programme pour développeurs Apple.**

L’ID de bundle utilisé dans votre application doit être unique et ne peut pas avoir déjà été utilisé dans une autre application. Tout ID de bundle utilisé avec le provisionnement libre ne peut PAS être réutilisé. Si vous avez déjà distribué une application, vous ne pouvez pas la provisionner avec le provisionnement libre. 

Reportez-vous aux [guides de distribution d’applications](~/ios/deploy-test/app-distribution/index.md) pour plus d’informations.

Si votre application utilise des services d’application, vous devez créer un profil de provisionnement comme indiqué dans le guide de [provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md#appservices). D’autres limitations sont indiquées dans la [section correspondante](#limitations) ci-dessous.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> Lancement de votre application

Pour utiliser le provisionnement libre pour déployer une application sur un appareil, vous allez utiliser Xcode pour créer l’identité de signature et les profils de provisionnement, vous allez utiliser Visual Studio pour Mac ou Visual Studio pour choisir le bon profil avec lequel signer votre application. Suivez la procédure pas à pas ci-dessous pour cela :

1. Si vous n’avez pas d’ID Apple, créez-en un sur [appleid.apple.com](https://appleid.apple.com/account).
2. Ouvrez Xcode et accédez à **Xcode > Préférences**.
3. Sous **comptes**, utilisez le bouton **+** pour ajouter votre ID Apple existant. Celui-ci doit ressembler à la capture d’écran suivante :

  [![](free-provisioning-images/launchapp1.png "Comptes de préférences Xcode")](free-provisioning-images/launchapp1.png#lightbox)

4. Branchez l’appareil iOS sur lequel vous voulez effectuer le déploiement et créez un projet iOS vide avec affichage unique dans Xcode. Affectez à la liste déroulante **Équipe** la valeur de l’ID Apple que vous venez d’ajouter. Son format doit être similaire à `your name (Personal Team - your Apple ID)` :

  [![](free-provisioning-images/launchapp2.png "Créer l’identité de signature")](free-provisioning-images/launchapp2.png#lightbox)

5. Sous la section **Général > Identité**, assurez-vous que l’ID de bundle correspond _exactement_ à l’ID de bundle de votre application Xamarin.iOS et vérifiez que la cible de déploiement correspond ou est inférieure à votre appareil iOS connecté. Cette étape est extrêmement importante, car Xcode crée uniquement un profil de provisionnement avec un ID d’application explicite :

  [![](free-provisioning-images/launchapp5.png "Créer un profil de provisionnement avec un ID d’application explicite")](free-provisioning-images/launchapp5.png#lightbox)

6. Dans la section Signature, sélectionnez **Gérer automatiquement la signature** et sélectionnez votre équipe dans la liste déroulante :

  [![](free-provisioning-images/launchapp6.png "Sélectionner Gérer automatiquement la signature et sélectionner votre équipe dans la liste déroulante")](free-provisioning-images/launchapp6.png#lightbox)

7. L’étape précédente génère automatiquement un profil de provisionnement et une identité de signature pour vous. Cliquez sur l’icône d’information située en regard du profil de provisionnement pour l’afficher :

  [![](free-provisioning-images/launchapp7.png "Afficher le profil de provisionnement")](free-provisioning-images/launchapp7.png#lightbox)

8. Pour tester dans Xcode, déployez l’application vide sur votre appareil en cliquant sur le bouton d’exécution.

9. Revenez à votre IDE, avec le même appareil branché, puis cliquez avec le bouton droit sur le nom de votre projet Xamarin.iOS pour ouvrir la boîte de dialogue **Options du projet**. Accédez à la section Signature du bundle iOS et définissez explicitement votre identité de signature et votre profil de provisionnement :

  [![](free-provisioning-images/launchapp8.png "Sélectionner l’identité de signature et le profil de provisionnement")](free-provisioning-images/launchapp8.png#lightbox)

Si votre identité de signature ou le profil de provisionnement approprié n’apparaissent pas dans votre IDE, vous devrez peut-être le redémarrer.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />Limites

Apple a imposé un certain nombre de limites à l’utilisation du provisionnement libre pour exécuter votre application sur un appareil iOS, en veillant à ce que vous puissiez uniquement déployer sur *votre* appareil. Ces limites sont répertoriées dans cette section.

L’accès à iTunes Connect est également limité et par conséquent, des services tels que la publication sur l’App Store et TestFlight ne sont pas disponibles pour les développeurs qui provisionnent librement leurs applications. Un compte de développeur Apple (Entreprise ou Personnel) est exigé pour distribuer par le biais de moyens ad hoc et internes.

Les profils de provisionnement créés de cette façon expirent à l’issue d’une semaine, les identités de signature au bout d’un an. De plus, les profils de provisionnement sont uniquement créés avec des ID d’application explicites ; vous devez donc suivre les instructions [ci-dessus](#launching) pour chaque application que vous souhaitez installer.

Le provisionnement de la plupart des services d’application n’est également pas possible avec le provisionnement libre. Cela concerne :

- Apple Pay
- Game Center
- iCloud
- Achats dans l’application
- Notifications Push
- Wallet (ancien Passbook)

La liste complète est fournie par Apple dans son guide des [fonctionnalités prises en charge](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1). Pour provisionner votre application afin de l’utiliser avec des services d’application, consultez les guides d’[utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/index.md).


## <a name="summary"></a>Récapitulatif

Ce guide a exploré les avantages et les limites de l’utilisation du provisionnement libre pour installer des applications sur un appareil iOS. Il a également abordé, étape par étape, l’utilisation du provisionnement libre pour installer une application Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [Provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md)
- [Provisionnement des services d’application](~/ios/get-started/installation/device-provisioning/index.md#appservices)
