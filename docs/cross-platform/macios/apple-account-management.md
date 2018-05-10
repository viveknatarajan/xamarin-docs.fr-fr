---
title: Gestion des comptes Apple
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 2a37f6644c66ebeb3b10a9fa0467115a21f69e75
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="apple-account-management"></a>Gestion des comptes Apple

L’interface de gestion de compte Apple fournit un moyen d’afficher toutes les équipes de développement associés à un ID Apple. Il vous permet également d’afficher plus de détails sur chaque équipe en affichant une liste de _identités de signature_ et _profils de configuration_ qui sont installés sur votre ordinateur.

L’authentification de votre ID Apple est effectuée sur la ligne de commande avec [fastlane](https://fastlane.tools/). FastLane doit être installé sur votre ordinateur pour pouvoir être authentifié avec succès. Plus d’informations sur fastlane et comment l’installer est détaillée dans le [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) guides.

La boîte de dialogue compte Apple vous permet de procéder comme suit :

* **Créer et gérer des certificats** 
* **Créer et gérer des profils de configuration** 

Pour plus d’informations sur la façon de procéder est décrite dans ce guide.

Vous pouvez également utiliser l’outils de mise en service automatique iOS pour créer et gérer vos identités de signature, les ID d’application et les profils de configuration automatiquement.

Pour plus d’informations sur l’utilisation de ces fonctionnalités, reportez-vous à la [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) guide.
️
## <a name="requirements"></a>Spécifications

Gestion des comptes Apple est disponible dans Visual Studio pour Mac et Visual Studio 2017 (Version 15,7 et versions ultérieures)

Vous devez disposer d’un compte Apple Developer pour utiliser cette fonctionnalité. Plus d’informations sur les comptes de développeur Apple est disponible dans le [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) guide.

- Assurez-vous que vous êtes connecté à internet. Il s’agit, car fastlane communique directement avec le portail des développeurs d’Apple.
- Assurez-vous d’avoir [outils fastlane installés](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Assurez-vous d’avoir les tout derniers outils fastlane de [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Avant de commencer, assurez-vous d’accepter des contrats de licence utilisateur dans le [portail des développeurs](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Ajout d’un compte de développeur Apple

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Pour ouvrir la boîte de dialogue de gestion de compte atteindre **Visual Studio > Préférences > compte de développeur Apple**:

    ![Options de compte de développeur Apple](apple-account-management-images/image1.png)

2. Appuyez sur la **+** bouton pour afficher le symbole dans la boîte de dialogue, comme illustré ci-dessous : 

    ![boîte de dialogue FastLane.](apple-account-management-images/image2.png)

4. Entrez votre ID Apple et le mot de passe et cliquez sur le **connexion** bouton. Cela enregistre vos informations d’identification dans la chaîne de clé sécurisée sur cet ordinateur. [FastLane](~/ios/deploy-test/provisioning/fastlane/index.md) est utilisé pour gérer vos informations d’identification en toute sécurité et de les transmettre au portail des développeurs d’Apple.
 
5. Sélectionnez **toujours autoriser** dans la boîte de dialogue alerte pour autoriser Visual Studio à utiliser vos informations d’identification :

    ![Toujours autoriser la boîte de dialogue alerte](apple-account-management-images/image4.png)

6. Une fois que votre compte a été ajouté avec succès, vous verrez votre ID Apple et aux éventuelles équipes faisant partie de votre ID Apple.

    ![Boîte de dialogue compte de développeur Apple avec les comptes ajoutés](apple-account-management-images/image5.png)

7. Sélectionnez n’importe quel équipe et l’appuyez sur la **afficher les détails...** disproportionnée. Cela affiche une liste de toutes les identités de signature et les profils de configuration qui sont installés sur votre ordinateur :

    ![Détails de l’écran affichant des identités de signature et configuration des profils sur votre ordinateur](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Avant d’ajouter votre ID Apple pour Visual Studio 2017, assurez-vous que votre environnement de développement est [associés à un hôte de build Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Pour ouvrir la fenêtre de gestion de compte, accédez à **Outils > Options > Xamarin > Apple comptes**:

    ![Écran des options de comptes d’Apple](apple-account-management-images/prov1.png)

1. Sélectionnez le **ajouter** bouton et entrer votre ID Apple et le mot de passe :

    ![boîte de dialogue Nom d’utilisateur et mot de passe](apple-account-management-images/prov1a.png)

1. Une fois que votre compte a été ajouté avec succès, vous verrez votre ID Apple et aux éventuelles équipes faisant partie de votre ID Apple.
 
1. Sélectionnez n’importe quel équipe et l’appuyez sur la **afficher les détails...** disproportionnée. Cela affiche une liste de toutes les identités de signature et les profils de configuration qui sont installés sur votre ordinateur :

    ![boîte de dialogue Nom d’utilisateur et mot de passe](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>La gestion des identités de signature et profils de configuration

La boîte de dialogue Détails de l’équipe affiche une liste des identités de signature, organisées par type. Le **état** colonne vous indique si le certificat est : 

* **Valide** : l’identité de signature (le certificat et la clé privée) est installée sur votre ordinateur et il n’a pas expiré.

* **Pas dans le trousseau** : il existe une identité de signature valide sur le serveur d’Apple. Pour l’installer sur votre ordinateur, il doit être exporté à partir d’un autre ordinateur. Impossible de télécharger l’identité de signature à partir du portail des développeurs Apple comme il ne contiendra pas de la clé privée.

* **Il manque une clé privée** – un certificat sans clé privée est installé dans le trousseau.

* **Expiré** – le certificat a expiré. Vous devez le supprimer à partir de votre chaîne de clé.

  ![informations de boîte de dialogue Détails de l’équipe](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Créer une identité de signature

Pour créer une nouvelle identité de signature, sélectionnez le **Create Certificate** bouton de liste déroulante et sélectionnez le type dont vous avez besoin. Si vous avez les autorisations appropriées pour une signature nouvelle identité s’affiche après quelques secondes.

Si une option dans la liste déroulante est grisée et désactivée, cela signifie que vous n’avez pas de l’équipe correcte des autorisations pour créer ce type de certificat.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Créez des options de certificat](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Créez des options de certificat](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Télécharger des profils de configuration

La boîte de dialogue Détails de l’équipe affiche également une liste de tous les profils de configuration connecté à votre compte de développeur. Vous pouvez télécharger tous les profils de configuration sur votre ordinateur local en appuyant sur la **télécharger tous les profils** bouton

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Télécharger la section de profils de configuration](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Télécharger la section de profils de configuration](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>Signature d’offre groupée iOS

Pour plus d’informations sur le déploiement de votre application sur un périphérique, reportez-vous à la [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) guide.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="view-details-dialog-is-empty"></a>Boîte de dialogue Vue détails est vide

Il s’agit d’un problème connu, relatives aux bogues actuellement [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Assurez-vous que vous utilisez la dernière version stable de Visual Studio pour Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Si vous rencontrez des problèmes de connexion dans votre compte, essayez les opérations suivantes :

* Ouvrez l’application trousseau et sous la catégorie sélectionnez *des mots de passe*. Recherchez `deliver.`et supprimer toutes les entrées.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>« Erreur lors de l’ajout du compte. Veuillez vous connecter avec un mot de passe spécifiques de l’application »

Il s’agit, car l’authentification à 2 facteurs est activée sur votre compte. Assurez-vous que vous utilisez la dernière version stable de Visual Studio pour Mac

### <a name="failed-to-create-new-certificate"></a>Impossible de créer un nouveau certificat
« Vous avez atteint la limite pour les certificats de ce type »

![boîte de dialogue certificat limite](apple-account-management-images/image10.png)

Le nombre maximal de certificats autorisés a été généré. Pour résoudre ce problème, accédez à la [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) et révoquer le certificat de Production.

## <a name="known-issues"></a>Problèmes connus

* Par défaut, les profils d’approvisionnement de distribution ciblent l’App Store. Vous devez créer les profils internes ou ad hoc manuellement.
