---
title: Distribution dans l’App Store
description: Ce document décrit comment distribuer une application Xamarin.iOS sur l’App Store. Il explique comment créer un certificat de distribution, comment créer un profil de provisionnement de distribution, et comment configurer iTunes Connect et soumettre l’application.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/23/2017
ms.openlocfilehash: 14cf2910767e9c205e5ddc8f580020505f54ef46
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "55832026"
---
# <a name="app-store-distribution"></a>Distribution dans l’App Store

Une fois l’application Xamarin.iOS développée, la prochaine étape du cycle de vie du développement du logiciel consiste à la distribuer aux utilisateurs à l’aide de l’App Store d’iTunes. Il s’agit de la manière la plus courante de distribuer des applications. En publiant une application sur l’App Store d’Apple, vous la mettez à la disposition des consommateurs du monde entier.

> [!IMPORTANT]
> Apple [a indiqué](https://developer.apple.com/ios/submit/) qu’à partir de mars 2019 toutes les applications et mises à jour envoyées à l’App Store doivent avoir été créées avec le kit SDK d’iOS 12.1 ou ultérieur, inclus dans Xcode 10.1 ou ultérieur.
> Les applications doivent aussi prendre en charge les tailles d’écran de l’iPhone XS et de l’iPad Pro 12,9".

La distribution d’une application, tout comme son développement, nécessite le provisionnement d’applications à l’aide du *profil de provisionnement* approprié. Les profils de provisionnement sont des fichiers qui contiennent les informations sur le code de signature, ainsi que l’identité de l’application et le mécanisme de distribution prévu. Ils contiennent également des informations relatives aux appareils sur lesquels l’application peut être déployée pour la distribution hors App Store.

> [!IMPORTANT]
> Il est **important** de noter que, pour pouvoir utiliser iTunes Connect et donc publier une application sur l’App Store, vous **devez** faire partie d’un programme pour les développeurs d’Apple, en tant qu’individu ou qu’organisation. Vous ne pouvez pas suivre les étapes de cette page si vous êtes membre d’un programme Developer **Enterprise** d’Apple.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Provisionnement d’une application pour sa distribution via l’App Store

Quelle que soit la manière dont vous comptez publier une application Xamarin.iOS, vous devez créer un *profil de provisionnement de distribution* spécifique. Ce profil permet à l’application d’être publiée avec une signature numérique pour qu’elle puisse être installée sur un appareil iOS. Le profil de provisionnement de distribution, qui est similaire au profil de provisionnement de développement, contient les éléments suivants :

- ID d’application
- Certificat de distribution

Vous pouvez sélectionner le même **ID d’application** et les mêmes **appareils** que ceux que vous avez utilisés pour votre profil de provisionnement de développement. Toutefois, si vous n’en avez pas déjà, vous devez créer un certificat de distribution pour identifier votre organisation quand vous soumettez l’application à l’App Store. Les étapes à suivre pour créer un certificat de distribution sont décrites dans la section ci-dessous.

> [!NOTE]
> Seuls les agents et les administrateurs d’équipe peuvent créer des certificats de distribution et des profils d’approvisionnement.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Création d’un certificat de distribution

1. Accédez à la section *Certificates, Identifiers & Profiles* (Certificats, identificateurs et profils) du Member Center du programme Developer d’Apple.
2. Sous *Certificates*, sélectionnez **Production**.
3. Cliquez sur le bouton **+** pour créer un certificat.
4. Sous le titre *Production*, sélectionnez **App Store and Ad Hoc (App Store et ad hoc)**  :

    [![](images/createcertmanually01.png "Sélectionner App Store et Ad Hoc")](images/createcertmanually01.png#lightbox)
5. Cliquez sur **Continue**, puis suivez les instructions permettant de créer une demande de signature de certificat via Trousseaux d’accès :

    [![](images/createcertmanually02.png "Créer une demande de signature de certificat via l’accès au trousseau")](images/createcertmanually02.png#lightbox)
6. Une fois la demande de signature de certificat créée, cliquez sur **Continue**, puis chargez-la sur le Member Center :

    [![](images/createcertmanually03.png "Charger la demande de signature de certificat sur le Member Center")](images/createcertmanually03.png#lightbox)

7. Cliquez sur **Generate** pour créer le certificat.
8. Pour finir, **téléchargez** le certificat rempli, puis double-cliquez sur le fichier pour l’installer.
9. À ce stade, le certificat doit être installé sur la machine, mais vous devrez peut-être [actualiser vos profils](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) pour vérifier qu’ils sont visibles dans Xcode.

Sinon, il est possible de demander un certificat via la boîte de dialogue Preferences de Xcode. Pour ce faire, suivez les étapes ci-dessous :

1.   Sélectionnez votre équipe, puis cliquez sur **Gérer les certificats…**  : [![](images/selectteam.png "Sélectionner l’équipe et afficher les détails")](images/selectteam.png#lightbox)

2.   Cliquez ensuite sur le bouton **Créer** à côté de **iOS Distribution Certificate (Certificat de distribution iOS)**  : [![](images/selectcert.png "Créer un certificat de distribution iOS")](images/selectcert.png#lightbox)

3.   En fonction de vos privilèges d’équipe, l’identité de signature est générée, comme indiqué ci-dessous. Sinon, vous devez attendre qu’un agent ou un administrateur d’équipe l’approuve : [![](images/generated.png "L’identité de signature est générée et une boîte de dialogue s’affiche")](images/generated.png#lightbox)


<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Création d’un profil de distribution

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Création d’un ID d’application

Comme pour tout profil de provisionnement que vous créez, un ID d’application est nécessaire pour identifier l’application que vous distribuez sur l’appareil de l’utilisateur. Si vous ne l’avez pas déjà créé, suivez les étapes ci-dessous pour en créer un :


1. Dans [Apple Developer Center](https://developer.apple.com/account/overview.action), accédez à la section *Certificates, Identifiers & Profiles* (Certificats, identificateurs et profils). Sélectionnez **App IDs** sous **Identifiers**.
2. Cliquez sur le bouton **+**, puis indiquez un **Name (Nom)** qui identifie l’application sur le portail.
3. Le préfixe d’application doit déjà être défini à l’aide de votre ID d’équipe. Il est impossible de le changer. Sélectionnez un ID d’application explicite ou avec des caractères génériques, puis entrez un ID de bundle au format DNS inversé, par exemple :
    - **Explicite** : com.[DomainName].[AppName]
    - **Avec des caractères génériques** : com.[DomainName].*
4. Sélectionnez les [services d’application](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) nécessaires à l’application.
5. Cliquez sur le bouton **Continue** et suivez les instructions affichées à l’écran pour créer l’ID d’application.


### <a name="creating-a-provisioning-profile"></a>Création d’un profil de provisionnement

Une fois que vous avez les composants nécessaires pour créer un profil de distribution, suivez les étapes ci-dessous pour le créer :

1. Retournez au portail de provisionnement Apple, puis sélectionnez **Provisioning (Provisionnement)** > **Distribution** :

    [![](images/distribute01.png "Sélectionner Provisionnement > Distribution")](images/distribute01.png#lightbox)

2. Cliquez sur le bouton **+** et sélectionnez le type **App Store** en tant que profil de distribution à créer :

    [![](images/distribute02.png "Créer un profil de distribution App Store")](images/distribute02.png#lightbox)

3. Cliquez sur le bouton **Continue**, puis dans la liste déroulante, sélectionnez l’ID d’application pour lequel vous souhaitez créer un profil de distribution :

    [![](images/distribute03.png "Sélectionner l’ID d’application dans la liste déroulante")](images/distribute03.png#lightbox)

4. Cliquez sur le bouton **Continue**, puis sélectionnez le certificat nécessaire pour signer l’application :

    [![](images/distribute04.png "Sélectionner le certificat nécessaire pour signer l’application")](images/distribute04.png#lightbox)

5. Cliquez sur le bouton **Continue**, puis sélectionnez les appareils iOS sur lesquels l’application Xamarin.iOS est autorisée à s’exécuter :

    [![](images/distribute05.png "Sélectionner les appareils iOS sur lesquels l’application est autorisée à s’exécuter")](images/distribute05.png#lightbox)

6. Cliquez sur le bouton **Continue**, puis entrez un **Name (Nom)** pour le nouveau profil de distribution :

    [![](images/distribute06.png "Entrer un nom pour le nouveau profil de distribution")](images/distribute06.png#lightbox)

7. Cliquez sur le bouton **Generate** pour créer le profil et finaliser le processus.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

 Vous devrez peut-être quitter Visual Studio pour Mac et obliger Xcode à actualiser sa liste d’identités de signature et de profils de provisionnement disponibles (en suivant les instructions de la section [Demande d’identités de signature](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)) pour qu’un nouveau profil de distribution soit disponible dans Visual Studio pour Mac.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Vous devrez peut-être quitter Visual Studio et obliger Xcode (sur le Mac de l’hôte de build) à actualiser sa liste d’identités de signature et de profils de provisionnement disponibles (en suivant les instructions de la section [Demande d’identités de signature](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)) pour qu’un nouveau profil de distribution soit disponible dans Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Sélection d’un profil de distribution dans un projet Xamarin.iOS

Une fois que vous êtes prêt à effectuer une build finale d’une application Xamarin.iOS pour la commercialiser dans l’App Store d’iTunes, sélectionnez le profil de distribution créé ci-dessus.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

 Dans Visual Studio pour Mac, effectuez les tâches suivantes :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le nom du projet pour l’ouvrir et le modifier.
2. Sélectionnez **Signature du bundle iOS** et **Mise en production | iPhone** dans la liste déroulante **Configuration** :

    ![](images/releasexs01.png "Sélectionner Mise en production | iPhone dans la liste déroulante Configuration")
3. Dans la plupart des cas, les options **Identité de signature** et **Profil de provisionnement** peuvent garder leurs valeurs par défaut **Automatique**. Visual Studio pour Mac choisit le profil approprié en fonction de l’identificateur de bundle dans Info.plist :

    ![](images/releasexs02.png "Identité de signature et profil de provisionnement avec la valeur par défaut Automatique")
4. Le cas échéant, sélectionnez l’identité de signature et le profil de distribution (créé ci-dessus) dans les listes déroulantes :

    ![](images/releasexs03.png "Sélectionner l’identité de signature et le profil de distribution")
5. Cliquez sur le bouton **OK** pour enregistrer les changements.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Dans Visual Studio, effectuez les tâches suivantes :

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez **Propriétés** pour l’ouvrir et le modifier.
2. Sélectionnez **Signature du bundle iOS** et **Mise en production | iPhone** dans la liste déroulante **Configuration** :

    ![](images/releasevs01.png "Sélectionner Mise en production | iPhone dans la liste déroulante Configuration")
3. Dans la plupart des cas, les options **Identité de signature** et **Profil de provisionnement** peuvent garder leurs valeurs par défaut **Automatique**. Visual Studio choisit le profil approprié en fonction de l’identificateur de bundle dans Info.plist

    ![](images/releasevs02.png "Identité de signature et profil de provisionnement avec la valeur par défaut Automatique")
4. Le cas échéant, sélectionnez l’identité de signature et le profil de distribution (créé ci-dessus) dans les listes déroulantes :

    ![](images/releasevs03.png "Sélectionner l’identité de signature et le profil de distribution")
5. Enregistrez les changements apportés aux propriétés du projet.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Configuration de votre application dans iTunes Connect

Une fois l’application correctement provisionnée, l’étape suivante consiste à la configurer dans [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), une suite d’outils web qui vous permettent, entre autres choses, de gérer vos applications iOS dans l’App Store.

Vous devez préparer correctement votre application Xamarin.iOS dans iTunes Connect avant de pouvoir la soumettre pour évaluation à Apple, et ensuite la mettre en vente ou la proposer gratuitement dans l’App Store.

Pour plus d’informations, consultez [Configuration d’une application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dans notre documentation.

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Soumission d’une application à iTunes Connect

Une fois que l’application a été signée à l’aide du profil de provisionnement de distribution et qu’elle a été créée dans iTunes Connect, son fichier binaire est chargé sur Apple pour évaluation. Si l’évaluation faite par Apple est positive, l’application est disponible dans l’App Store.

Pour plus d’informations sur la publication d’applications sur l’App Store, consultez [Publication sur l’App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Copier automatiquement les bundles .app dans Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Récapitulatif

Cet article a présenté les composants clés de la préparation d’une application Xamarin.iOS pour sa distribution dans l’App Store.

## <a name="related-links"></a>Liens associés

- [Configuration d’une application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publication dans l’App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Distribution en interne](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Distribution ad hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Fichier iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Prise en charge IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
