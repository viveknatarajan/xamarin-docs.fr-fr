---
title: Utilisation des fonctionnalités
description: L’ajout de fonctionnalités à une application nécessite souvent une configuration supplémentaire du provisionnement. Ce guide explique la configuration requise pour l’ensemble des fonctionnalités.
ms.topic: article
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: ffc694e2f513d61cf32e94d8dc3bae17c887521b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-capabilities"></a>Utilisation des fonctionnalités

_L’ajout de fonctionnalités à une application nécessite souvent une configuration plus poussée de l’approvisionnement. Ce guide explique les paramètres requis par les différentes fonctionnalités._

Apple fournit aux développeurs diverses _fonctionnalités_, souvent appelées _services d’application_, qui leur permettent de proposer des applications iOS plus complètes et dotées de capacités étendues. Les développeurs peuvent mieux intégrer les fonctionnalités de la plateforme à leurs applications, par exemple, des transactions monétaires lancées à partir de l’application, des services d’appareil supplémentaires comme Siri, et bien plus encore.
Ces fonctionnalités sont compatibles avec les projets Xamarin.iOS. Voici la liste complète des services :

* App Groups
* Associated Domains
* Data Protection
* Game Center
* HealthKit
* HomeKit
* Configuration d’accessoires sans fil
* iCloud
* In-App Purchase
* Inter-App Audio
* Apple Pay
* Wallet
* Push Notification
* VPN personnel
* Siri
* Maps
* Background Modes
* Keychain Sharing
* Extensions réseau
* Hotspot Configuration
* Multipath
* NFC Tag Reading


Vous pouvez activer ces fonctionnalités à partir de Visual Studio pour Mac, ou manuellement dans le portail des développeurs d’Apple. Certaines fonctionnalités, telles que Wallet, Apple Pay et iCloud, nécessitent une configuration supplémentaire des ID d’application.

Ce guide explique comment activer chacun de ces services d’application pour votre application dans Visual Studio pour Mac ou manuellement par le biais du developer center. Il précise également la configuration supplémentaire requise, s’il y a lieu. 

## <a name="adding-app-services"></a>Ajout de services d’application

Pour utiliser des fonctionnalités, l’application doit avoir un profil de provisionnement valide contenant un ID d’application pour lequel le service approprié est activé. Ce profil de provisionnement peut être créé automatiquement dans Visual Studio pour Mac ou manuellement dans le developer center d’Apple.

Cette section explique comment utiliser le provisionnement automatique de Visual Studio pour Mac ou le Centre de développement pour activer la plupart des fonctionnalités. Certaines fonctionnalités telles que Wallet, iCloud, Apple Pay et App Groups nécessitent une configuration supplémentaire. Celles-ci sont décrites en détail dans les guides correspondants.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

> [!IMPORTANT]
> Toutes les fonctionnalités ne peuvent pas être ajoutées et gérées dans Visual Studio pour Mac. La liste suivante répertorie les fonctionnalités prises en charge :
>
>* HealthKit 
>* HomeKit 
>* VPN personnel 
>* Configuration d’accessoires sans fil 
>* Inter-App Audio 
>* SiriKit 
>* Zone réactive 
>* Extensions réseau 
>* NFC Tag Reading
>* Multipath 
>
>Les fonctionnalités Push Notifications, Game Center, In-App Purchase, Maps, Keychain Sharing, Associated Domains et Data Protection ne sont pas prises en charge. Pour ajouter ces fonctionnalités, utilisez le provisionnement manuel et suivez les étapes décrites dans la section [Developer center](#devcenter).


Les fonctionnalités sont ajoutées au fichier **Entitlements.plist** dans Visual Studio pour Mac. Pour ajouter des fonctionnalités, effectuez les étapes suivantes :

1. Ouvrez le fichier **Info.plist** de votre application iOS et assurez-vous que l’option **Activer la signature automatique** est sélectionnée. Si vous avez besoin d’aide, suivez les étapes décrites dans le guide [Provisionnement automatique](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) :

    ![Option Activer la signature automatique](images/manage-signing.png)

2. Ouvrez le fichier **Entitlements.plist** et sélectionnez la fonctionnalité que vous souhaitez ajouter :

    ![Ajouter des fonctionnalités au fichier entitlements.plist](images/image17.png)

    La sélection d’une fonctionnalité a deux actions :
    * Elle ajoute cette fonctionnalité à votre ID d’application.
    * Elle ajoute la paire clé-valeur du droit dans votre fichier Entitlements.plist.

    Visual Studio pour Mac vous signale la réussite de ces opérations par le message suivant :

    ![Ajouter des fonctionnalités au fichier entitlements.plist](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Comme le provisionnement automatique n’est actuellement pas pris en charge dans Visual Studio 2017, vous devrez utiliser le [Centre de développement](#devcenter) pour créer un ID d’application avec les services d’application adéquats.

-----

<!--
<a name="xcode" />

## Xcode

Xamarin developers can also use Xcode to quickly create a provisioning profile with a suitable App ID. This process, described below, can be used for any app service in the list:

1.  Open Xcode and create a ‘dummy’ project. Give the dummy project the same name as your Xamarin.iOS project. The bundle identifier should be identical to the bundle identifier of your Xamarin.iOS project:

    ![Xcode Create Project](images/image1.png)

2.  Ensure **Automatically manage signing** is selected:

    ![Automatically manage signing selection](images/image2.png)

3.  Once the app has been created, go to the tab named **Capabilities**:

    ![Xcode Capabilities tab](images/image3.png)

4.  Browse to the capability that you wish to add, and move the switch to the **ON** position.
5.  This will create a provisioning profile with an App ID that contains the capability and adds the entitlement to the profile.
6.  In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created in Xcode:

    ![Visual Studio for Mac Project Options](images/image4.png)
-->

<a name="devcenter" />

## <a name="developer-center"></a>Developer Center

Pour accéder au developer center, vous devez effectuer deux étapes : créer un ID d’application, puis créer un profil de provisionnement à l’aide de ce nouvel ID d’application. Ces étapes sont détaillées ci-dessous.

### <a name="creating-an-app-id-with-an-app-service"></a>Création d’un ID d’application avec un service d’application

1.  Accédez au [developer center d’Apple](https://developer.apple.com/account) sur un Mac (l’hôte de build Mac si vous utilisez une machine Windows) et ouvrez une session.
2.  Sélectionnez **Certificates, Identifiers & Profiles** :

    ![Developer center d’Apple](images/image5.png)

3.  Sous **Identifiers**, sélectionnez **App IDs** :

    ![Sélection de l’ID d’application dans le developer center](images/image6.png)

4.  Appuyez sur le bouton **+** dans le coin supérieur droit pour créer un ID d’application.
5.  Entrez une description de l’ID d’application, sélectionnez Explicit App ID et entrez un ID de bundle au format `com.domain.appname`. Cet ID de bundle doit correspondre à l’ID de bundle défini dans votre projet :

    ![Ajout des détails de l’ID d’application](images/image7.png)

6.  Sous **App Services**, sélectionnez le ou les services souhaités dans votre application :

    ![Page de sélection des services d’application](images/image8.png)

7.  Appuyez sur **Continue**.
8.  Confirmez votre ID d’application. Chaque service peut avoir l’un des états suivants : **Enabled**, **Disabled** ou **Configurable**, comme illustré ci-dessous. S’il a l’état **Enabled**, il est prêt à être utilisé dans un profil de provisionnement. S’il a l’état **Configurable**, une configuration supplémentaire est requise pour cette fonctionnalité. Ces étapes supplémentaires seront décrites plus en détail dans des sections ultérieures.

    ![Confirmation de l’ID d’application](images/image9.png)

9.  Cliquez sur **Register**, puis sur **Done**. Le nouvel ID d’application s’affiche normalement dans la liste des ID d’application iOS.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>Création d’un profil de provisionnement

Maintenant, créez un profil de provisionnement qui contient cet ID d’application. Suivez les étapes ci-dessous :

1.  Dans le developer center d’Apple, accédez à **Provisioning Profiles > All** :

    ![Section Provisioning Profiles](images/image10.png)

2.  Appuyez sur le bouton **+** dans le coin supérieur droit pour créer un profil de provisionnement.
3.  Sélectionnez le type de profil de provisionnement souhaité, puis cliquez sur **Continue** :

    ![Sélection du profil de provisionnement](images/image11.png)

4.  Dans la liste déroulante, sélectionnez l’ID d’application que vous avez créé plus haut, puis appuyez sur **Continue** :

    ![Sélection de l’ID d’application](images/image12.png)

5.  Sélectionnez les certificats utilisés pour signer l’application et appuyez sur **Continue** :

    ![Sélection des certificats](images/image13.png)

6.  Sélectionnez les appareils à inclure dans ce profil et appuyez sur **Continue** :

    ![Sélectionner les appareils pour le profil de provisionnement](images/image14.png)

7.  Nommez le profil afin qu’il soit facilement identifiable et appuyez sur **Continue** pour générer le profil :

    ![Nommer le profil de provisionnement](images/image15.png)

8.  Appuyez sur le bouton **Download** pour télécharger le profil, puis double-cliquez sur le fichier dans le Finder pour installer le profil de provisionnement.

9. Si vous utilisez Visual Studio pour Mac, vérifiez que l’option **Activer la signature automatique** est désélectionnée dans le fichier **Info.plist**.

10. Dans Visual Studio pour Mac ou Visual Studio, accédez à **Options du projet > Signature du bundle** et définissez votre nouveau profil comme profil de provisionnement :

    ![Options du projet dans Visual Studio pour Mac](images/image16.png)

> [!IMPORTANT]
> Vous devrez peut-être aussi définir des clés de droit dans le fichier Entitlement.plist et des clés de confidentialité dans le fichier Info.plist. Pour plus d’informations sur ces droits, consultez le guide [Utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md).

<a name="nextsteps" />

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez ajouté une fonctionnalité côté serveur, il vous reste quelques étapes à effectuer pour permettre à votre application d’utiliser la fonctionnalité. S’il y a lieu, effectuez les étapes supplémentaires décrites ci-après :

*   Utilisez l’espace de noms de framework dans votre application.
*   Ajoutez les droits requis à votre application. Pour obtenir des informations détaillées sur les droits requis et leur ajout, consultez le guide [Utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md).

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Dépannage

La liste ci-dessous décrit quelques-uns des problèmes les plus courants susceptibles d’entraver le développement d’une application avec un service d’application activé.

-   Assurez-vous que l’ID correct a été créé et enregistré de manière appropriée dans la section **Certificates, Identifiers & Profiles** du portail des développeurs d’Apple.
-   Assurez-vous que le service a été ajouté à l’ID d’application (ou d’extension) et qu’il est configuré pour utiliser l’identificateur App Group/Merchant ID/Container créé précédemment dans la section **Certificates, Identifiers & Profiles** du portail des développeurs d’Apple.
-   Assurez-vous que les profils de provisionnement et les ID d’application ont été installés, et que le fichier d’application **Info.plist** (dans le projet Xamarin) utilise l’un des ID d’application configurés plus haut.
-   Assurez-vous que le service approprié est activé dans le fichier d’application **Entitlements.plist** (dans le projet Xamarin).
-   Assurez-vous que les clés de confidentialité appropriées sont définies dans le fichier Info.plist
-   Assurez-vous que, dans la section **Signature du bundle iOS** de l’application, le paramètre **Droits personnalisés** est défini sur **Entitlements.plist**. Ce n’est _pas_ le paramètre par défaut pour les builds Debug et iOS Simulator.

<a name="summary" />

## <a name="summary"></a>Récapitulatif

Ce guide a présenté les différentes fonctionnalités, ou _services d’application_, puis a expliqué comment les activer dans Visual Studio et dans le developer center d’Apple. Il a également détaillé comment configurer des services plus complexes tels que les services Wallet, iCloud, Apple Pay et App Groups. Enfin, il a indiqué les étapes suivantes à effectuer pour la configuration ainsi que des options de dépannage simples.