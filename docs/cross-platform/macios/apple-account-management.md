---
title: Gestion des comptes Apple
ms.topic: article
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/05/2017
ms.openlocfilehash: 465ba4822a1004100160703f1607d99199f28a16
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="apple-account-management"></a>Gestion des comptes Apple

L’interface de gestion de compte Apple fournit un moyen d’afficher toutes les équipes de développement associés à un ID Apple. Il vous permet également d’afficher plus de détails sur chaque équipe en affichant une liste de _identités de signature_ et _profils de configuration_ qui sont installés sur votre ordinateur.

L’authentification de votre ID Apple est effectuée sur la ligne de commande avec [fastlane](https://fastlane.tools/). FastLane doit être installé sur votre ordinateur pour pouvoir être authentifié avec succès. Plus d’informations sur fastlane et comment l’installer est détaillée dans le [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) guides.

La boîte de dialogue compte Apple dans Visual Studio pour Mac permet d’effectuer les opérations suivantes :

* **Créer et gérer des certificats** 
* **Créer et gérer des profils de configuration** 

Pour plus d’informations sur la façon de procéder est décrite dans ce guide.

Vous pouvez également utiliser l’outils de signature d’offre groupée iOS pour effectuer les opérations suivantes :

* **Ajouter une nouvelle identité de signature à un profil existant** 
* **Configurer de nouveaux périphériques** 

Pour plus d’informations sur l’utilisation de ces fonctionnalités, reportez-vous à la [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) guide.
️
## <a name="requirements"></a>Configuration requise

Gestion des comptes Apple est disponible dans Visual Studio pour Mac. Il n’est pas disponible actuellement sur Visual Studio pour Windows.

Vous devez disposer d’un compte Apple Developer pour utiliser cette fonctionnalité. Plus d’informations sur les comptes de développeur Apple est disponible dans le [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) guide.

- Assurez-vous que vous êtes connecté à internet. Il s’agit, car fastlane communique directement avec le portail des développeurs d’Apple.
- Assurez-vous d’avoir [outils fastlane installés](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Assurez-vous d’avoir les tout derniers outils fastlane de [https://download.fastlane.tools](https://download.fastlane.tools).
- Avant de commencer, assurez-vous d’accepter des contrats de licence utilisateur dans le [portail des développeurs](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Ajout d’un compte de développeur Apple

1. Pour ouvrir la boîte de dialogue de gestion de compte atteindre **Visual Studio > Préférences > compte de développeur Apple**:

    ![Options de compte de développeur Apple](apple-account-management-images/image1.png)

2. Appuyez sur la  **+**  bouton pour afficher le symbole dans la boîte de dialogue, comme illustré ci-dessous : 

    ![boîte de dialogue FastLane.](apple-account-management-images/image2.png)

4. Entrez votre ID Apple et le mot de passe et cliquez sur le **connexion** bouton. Cela enregistre vos informations d’identification dans la chaîne de clé sécurisée sur cet ordinateur. [FastLane](~/ios/deploy-test/provisioning/fastlane/index.md) est utilisé pour gérer vos informations d’identification en toute sécurité et de les transmettre au portail des développeurs d’Apple.
 
5. Sélectionnez **toujours autoriser** dans la boîte de dialogue alerte pour autoriser Visual Studio à utiliser vos informations d’identification :

    ![](apple-account-management-images/image4.png)

6. Une fois que votre compte a été ajouté avec succès, vous verrez votre ID Apple et aux éventuelles équipes faisant partie de votre ID Apple.

    ![](apple-account-management-images/image5.png)

7. Sélectionnez n’importe quel équipe et l’appuyez sur la **afficher les détails...** disproportionnée. Cela affiche une liste de toutes les identités de signature et les profils de configuration qui sont installés sur votre ordinateur :

    ![](apple-account-management-images/image6.png)


<a name="managing"/>
    


## <a name="managing-signing-identities-and-provisioning-profiles"></a>La gestion des identités de signature et profils de configuration

La boîte de dialogue Détails de l’équipe affiche une liste des identités de signature, organisées par type. Le **état** colonne vous indique si le certificat est : 

* **Valide** : l’identité de signature (le certificat et la clé privée) est installée sur votre ordinateur et il n’a pas expiré.

* **Pas dans le trousseau** : il existe une identité de signature valide sur le serveur d’Apple. Pour l’installer sur votre ordinateur, il doit être exporté à partir d’un autre ordinateur. Impossible de télécharger l’identité de signature à partir du portail des développeurs Apple comme il ne contiendra pas de la clé privée.

* **Il manque une clé privée** – un certificat sans clé privée est installé dans le trousseau.

* **Expiré** – le certificat a expiré. Vous devez le supprimer à partir de votre chaîne de clé.

  ![](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Créer une identité de signature

Pour créer une nouvelle identité de signature, sélectionnez le **créer un nouveau certificat** bouton de liste déroulante et sélectionnez le type dont vous avez besoin. Si vous avez les autorisations appropriées pour une signature nouvelle identité s’affiche après quelques secondes.

Si une option dans la liste déroulante est grisée et désactivée, comme illustré ci-dessous, cela signifie que vous n’avez pas de l’équipe correcte des autorisations pour créer ce type de certificat.

![](apple-account-management-images/image8.png)

## <a name="download-provisioning-profiles"></a>Télécharger des profils de configuration

La boîte de dialogue Détails de l’équipe affiche également une liste de tous les profils de configuration connecté à votre compte de développeur. Vous pouvez télécharger tous les profils de configuration sur votre ordinateur local en appuyant sur la **télécharger tous les profils** bouton

![](apple-account-management-images/image9.png)

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

![](apple-account-management-images/image10.png)

Le nombre maximal de certificats autorisés a été généré. Pour résoudre ce problème, accédez à la [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) et révoquer le certificat de Production.

## <a name="known-issues"></a>Problèmes connus

* Parfois, la boîte de dialogue Détails de l’affichage peut prendre énormément de temps pour extraire les profils et les identités de signature.
* Le focus ne peut pas retournent souvent à Visual Studio pour Mac après avoir entré vos informations, à l’origine de votre compte, ne pas à ajouter. Si c’est le cas, essayez à nouveau le processus.
* Les profils d’approvisionnement créés dans Visual Studio pour Mac ne prennent pas en compte les droits sélectionnés dans vos projets (Entitlements.plist). Cette fonctionnalité sera ajoutée dans les futures versions de l’IDE.
* Par défaut, les profils d’approvisionnement de distribution ciblent l’App Store. Vous devez créer les profils internes ou ad hoc manuellement.
