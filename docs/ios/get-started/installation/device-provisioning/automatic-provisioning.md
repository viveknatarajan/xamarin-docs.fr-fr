---
title: Provisionnement automatique pour Xamarin.iOS
description: Après l’installation de Xamarin.iOS, l’étape suivante dans le développement iOS consiste à provisionner votre appareil iOS. Ce guide décrit l’utilisation de la signature automatique pour demander des certificats et profils de développement.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: 323174b4a37a12828a32acb398fef63cd9b849e3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785815"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Provisionnement automatique pour Xamarin.iOS

_Une fois Xamarin.iOS installé avec succès, l’étape suivante du développement iOS consiste à configurer l’appareil iOS. Ce guide décrit l’utilisation de la signature automatique pour demander des certificats et profils de développement._

## <a name="requirements"></a>Configuration requise

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

- Visual Studio pour Mac 7.3 ou version ultérieure
- Xcode 9 ou supérieure

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 Version 15.7 (ou supérieure)

Vous devez également avoir un appairage avec un hôte de build Mac qui a les éléments suivants :

- Xcode 9 ou supérieure

-----

## <a name="enabling-automatic-signing"></a>Activation de la signature automatique

Avant de commencer le processus de signature automatique, vous devez vérifier qu’un ID Apple est ajouté dans Visual Studio, comme décrit dans le guide de [gestion des comptes Apple](~/cross-platform/macios/apple-account-management.md). Une fois que vous avez ajouté un ID Apple, vous pouvez utiliser toute _équipe_ associée. Ainsi, des certificats, des profils et d’autres ID peuvent être créés par rapport à l’équipe. L’ID d’équipe est également utilisé pour créer un préfixe pour un ID d’application à inclure dans le profil de provisionnement. Apple peut ainsi vérifier que vous êtes bien qui vous prétendez être.

> [!IMPORTANT]
> Avant de commencer, veillez à vous connecter à [iTunes Connect](https://itunesconnect.apple.com/) ou à [appleid.apple.com](https://appleid.apple.com) pour vérifier que vous avez accepté les dernières stratégies de compte Apple. Si vous y êtes invité, effectuez les étapes requises pour accepter tout nouveau contrat du compte provenant d’Apple. Si vous n’acceptez pas l’accord sur la confidentialité datant de mai 2018, vous obtiendrez l’alerte suivante lorsque vous essayerez de provisionner votre appareil :
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Pour signer automatiquement votre application pour un déploiement sur un appareil iOS, effectuez les opérations suivantes :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Ouvrez votre projet iOS dans Visual Studio pour Mac.

2. Ouvrez le fichier **Info.plist**.

3. Dans la section **Signature**, sélectionnez **Attribution automatique** :

    ![Liste déroulante de sélection de l’équipe](automatic-provisioning-images/image2.png)

4. Sélectionnez votre équipe dans la liste déroulante **Équipe**.

6. Au bout de quelques secondes, un certificat de signature et un profil d’attribution seront créés :

    ![certificat et profil correctement créés](automatic-provisioning-images/image5.png)

    Si la signature automatique échoue, le **bloc de signature automatique** indique la raison de l’erreur.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Appairez Visual Studio 2017 avec un Mac comme décrit dans le guide [Appairer avec un Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

2. Ouvrez les options de provisionnement en sélectionnant **Projet > Propriétés de provisionnement...**

3. Sélectionnez le schéma **Provisionnement automatique** :

    ![Sélection du schéma Automatique](automatic-provisioning-images/prov4.png)

4. Sélectionnez votre équipe dans la zone de liste modifiable **Équipe** pour démarrer le processus de signature automatique.

    ![Sélection de l’équipe](automatic-provisioning-images/prov3.png)

4. Le processus de signature automatique démarre. Visual Studio essaie ensuite de générer un ID d’application, un profil de provisionnement et une identité de signature pour utiliser ces artefacts pour la signature. Vous pouvez voir le processus de génération dans la sortie de génération :

    ![Sortie de génération montrant la génération d’artefacts](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Déclenchement du provisionnement automatique

Quand la signature automatique est activée, Visual Studio pour Mac met à jour ces artefacts si nécessaire dans les situations suivantes :

* Un appareil iOS est branché à votre mac
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
