---
title: Provisionnement automatique
description: "Après l’installation de Xamarin.iOS, l’étape suivante dans le développement iOS consiste à provisionner votre appareil iOS. Ce guide décrit l’utilisation de la signature automatique dans Visual Studio pour Mac pour demander des certificats et profils de développement."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 11/17/2017
ms.openlocfilehash: a411c214e35f78ff9d3dd8d4e9122702d66a2156
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="automatic-provisioning"></a>Provisionnement automatique

_Une fois Xamarin.iOS installé avec succès, l’étape suivante du développement iOS consiste à configurer l’appareil iOS. Ce guide décrit l’utilisation de la signature automatique dans Visual Studio pour Mac pour demander des certificats et profils de développement._

## <a name="requirements"></a>Configuration requise

- Visual Studio pour Mac 7.3 ou version ultérieure
- Xcode 9 ou supérieure

> [!IMPORTANT]
>  Ce guide explique comment utiliser Visual Studio pour Mac pour configurer un appareil Apple pour le déploiement et comment déployer une application. Pour connaître les étapes manuelles de ces opérations ou les effectuer avec Visual Studio sur Windows, il est recommandé de suivre les étapes détaillées dans le guide de [provisionnement manuel](~/ios/get-started/installation/device-provisioning/manual-provisioning.md).

## <a name="enabling-automatic-signing"></a>Activation de la signature automatique

Avant de commencer le processus de signature automatique, vous devez vérifier qu’un ID Apple est ajouté dans Visual Studio pour Mac, comme décrit dans le guide de [gestion des comptes Apple](~/cross-platform/macios/apple-account-management.md). Une fois que vous avez ajouté un ID Apple, vous pouvez utiliser toute _équipe_ associée. Ainsi, des certificats, des profils et d’autres ID peuvent être créés par rapport à l’équipe. L’ID d’équipe est également utilisé pour créer un préfixe pour un ID d’application à inclure dans le profil de provisionnement. Apple peut ainsi vérifier que vous êtes bien qui vous prétendez être.

Pour signer automatiquement votre application pour un déploiement sur un appareil iOS, effectuez les opérations suivantes :

1. Ouvrez votre projet iOS dans Visual Studio pour Mac.

2. Ouvrez le fichier **Info.plist**.

3. Dans la section **Signature**, sélectionnez **Attribution automatique** :

    ![Liste déroulante de sélection de l’équipe](automatic-provisioning-images/image2.png)

4. Sélectionnez votre équipe dans la liste déroulante **Équipe**.

6. Au bout de quelques secondes, un certificat de signature et un profil d’attribution seront créés :

    ![certificat et profil correctement créés](automatic-provisioning-images/image5.png)

    Si la signature automatique échoue, le **bloc de signature automatique** indique la raison de l’erreur.

## <a name="triggering-automatic-provisioning"></a>Déclenchement du provisionnement automatique

Quand la signature automatique est activée, Visual Studio pour Mac met à jour ces artefacts si nécessaire dans les situations suivantes :

* Un appareil iOS est branché sur votre mac
    - La vérification de l’inscription de l’appareil sur le portail des développeurs Apple est automatiquement effectuée. Si l’appareil n’est pas inscrit, il est ajouté et un nouveau profil de provisionnement qui le contient est généré.
* L’ID de bundle de votre application est modifié
    - L’ID de l’application est alors mis à jour. Un nouveau profil de provisionnement qui contient cet ID d’application est créé.
* Une fonctionnalité prise en charge est activée dans le fichier Entitlements.plist.
    - Cette fonctionnalité est ajoutée à l’ID de l’application et un nouveau profil de provisionnement est généré avec l’ID de l’application mis à jour.
    - Certaines fonctionnalités ne sont actuellement pas prises en charge. Pour plus d’informations sur celles qui sont prises en charge, consultez le guide d’[utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/index.md).


## <a name="related-links"></a>Liens associés

- [Provisionnement libre](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribution d’une application](~/ios/deploy-test/app-distribution/index.md)
- [Résolution des problèmes](~/ios/deploy-test/troubleshooting.md)
- [Apple - Guide de distribution d’applications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
