---
title: Provisionnement automatique
description: "Après l’installation de Xamarin.iOS, l’étape suivante dans le développement iOS consiste à provisionner votre appareil iOS. Ce guide décrit l’utilisation de la signature automatique dans Visual Studio pour Mac pour demander des certificats et profils de développement."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/17/2017
ms.openlocfilehash: d7532d052c57ad46caca0cd6d6ce26d0e77dc05f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
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

2. Ouvrez le fichier **Info.plist** :

    ![Fichier Info.plist](automatic-provisioning-images/image1.png)

3. Dans la section **Signature**, sélectionnez votre équipe dans la liste déroulante **Équipe** :

    ![Liste déroulante de sélection de l’équipe](automatic-provisioning-images/image2.png)

4. Cochez la case **Gérer automatiquement la signature**, comme illustré ci-dessous. Le processus de signature automatique démarre. Visual Studio pour Mac essaie de générer un ID d’application, un profil de provisionnement et une identité de signature, puis de définir ces artefacts à utiliser pour la signature. Quand la case est cochée, les contrôles manuels de sélection des identités de signatures sont désactivés.

    ![option de gestion automatique des équipes](automatic-provisioning-images/image3.png)

5. La boîte de dialogue suivante s’affiche, vous informant que le fichier projet va être modifié pour utiliser le certificat et le profil de provisionnement nouvellement générés :

    ![boîte de dialogue qui avertit de la modification du fichier projet](automatic-provisioning-images/image4.png)

6. Après quelques secondes, un profil de certificat de signature et de provisionnement est créé et affiché :

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
