---
title: "Publication sur l’App Store"
description: "Cet article explique comment configurer, générer et publier une application Xamarin.iOS pour la distribuer via l’App Store. Il comprend un guide pas à pas qui explique comment préparer la distribution de votre application, comment utiliser les outils d’Apple pour soumettre votre application à des fins d’évaluation, et enfin, comment publier votre application sur l’App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: dfa3d1f89d813f2e57863e615c701cd78c655ac0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="publishing-to-the-app-store"></a>Publication sur l’App Store

_Cet article explique comment configurer, générer et publier une application Xamarin.iOS pour la distribuer via l’App Store. Il comprend un guide pas à pas qui explique comment préparer la distribution de votre application, comment utiliser les outils d’Apple pour soumettre votre application à des fins d’évaluation, et enfin, comment publier votre application sur l’App Store._

Pour permettre la distribution des applications sur tous les appareils iOS, Apple impose la publication de ces applications via l’ *App Store* , le point d’achat unique des applications iOS. Avec plus de 500 000 applications dans l’App Store, les développeurs de nombreux types d’application ont capitalisé sur le succès massif de ce point de distribution unique. L’App Store est une solution clé en main, qui offre à la fois un système de distribution et un système de paiement aux développeurs d’applications.

Le processus de soumission d’une application sur l’App Store implique les actions suivantes :

1. Création d’un **ID d’application** et sélection de **droits**
2. Création d’un **profil de provisionnement de distribution**
3. Utilisation de ce profil pour générer votre application
4. Soumission de votre application via **iTunes Connect**


Cet article décrit toutes les étapes nécessaires pour provisionner, générer et soumettre une application afin de la distribuer dans l’App Store.

## <a name="before-you-submit-an-application"></a>Avant de soumettre une application

Une fois que vous avez soumis une application pour qu’elle soit publiée sur l’App Store, Apple procède à son évaluation afin de vérifier qu’elle respecte les recommandations de qualité et de contenu d’Apple. Si votre application ne respecte pas ces recommandations, Apple la rejettera. Vous devrez alors corriger la non-conformité citée par Apple, puis soumettre à nouveau votre application.
Ainsi, vous avez de plus grandes chances de réussir l’évaluation effectuée par Apple en vous familiarisant avec ces recommandations et en essayant d’adapter votre application à ces dernières. Les instructions d’Apple sont disponibles sur la page des [recommandations pour le processus d’évaluation dans l’App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html).

Voici quelques éléments à surveiller quand vous soumettez une application :

1. Vérifiez que la description de l’application correspond aux fonctionnalités incluses dans cette dernière.
2. Vérifiez que l’application ne se bloque pas dans des conditions d’utilisation normales. Cela inclut l’utilisation sur tous les appareils iOS pris en charge.


Apple gère également une liste de conseils pour la soumission d’applications à l’App Store. Vous pouvez les lire sur la page décrivant la [distribution d’applications sur l’App Store](https://developer.apple.com/appstore/resources/submission/tips.html).

## <a name="configuring-your-application-in-itunes-connect"></a>Configuration de votre application dans iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) est une suite d’outils web qui vous permettent, entre autres choses, de gérer vos applications iOS sur l’App Store. Vous devez préparer correctement votre application Xamarin.iOS dans iTunes Connect avant de pouvoir la soumettre pour évaluation à Apple, et ensuite la mettre en vente ou la proposer gratuitement dans l’App Store.

Effectuez ce qui suit :

1. Vérifiez que les contrats appropriés sont en place et à jour dans la section **Agreements, Tax, and Banking (Contrats, taxes et informations bancaires)** d’iTunes Connect pour pouvoir publier une application iOS gratuite ou payante.
2. Créez un **iTunes Connect Record (Enregistrement iTunes Connect)** pour l’application, et spécifiez son **Display Name (Nom d’affichage)** (indiqué dans l’App Store).
3. Sélectionnez un **Sale Price (Prix de vente)** ou indiquez que l’application est disponible gratuitement.
5. Fournissez une **Description** claire et succincte de l’application, notamment ses fonctionnalités et avantages pour l’utilisateur final.
6. Fournissez les **Categories (Catégories)**, **Sub Categories (Sous-catégories)** et **Keywords (Mots clés)** qui permettent à l’utilisateur de trouver votre application dans l’App Store.
7. Fournissez les URL de **Contact** et de **Support** sur votre site web (condition imposée par Apple).
8. Définissez le **Rating (Classification)** de votre application pour les paramètres de contrôle parental dans l’App Store.
9. Configurez les technologies facultatives de l’App Store, par exemple **Game Center** et **In-App Purchase (Achat dans l’application)**.

Pour plus d’informations, consultez [Configuration d’une application dans iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dans notre documentation.

## <a name="preparing-for-app-store-distribution"></a>Préparation à la distribution via l’App Store

Pour publier une application sur l’App Store, vous devez d’abord la générer dans le cadre d’une distribution, ce qui implique de nombreuses étapes. Les sections suivantes décrivent tous les éléments à préparer pour la publication d’une application Xamarin.iOS. Ainsi, vous pourrez générer votre application et la soumettre à l’App Store pour qu’elle soit évaluée et, le cas échéant, publiée.

### <a name="provisioning-for-application-services"></a>Provisionnement des services d’application

Apple propose une sélection de services d’application spéciaux, également appelés droits, que vous pouvez activer pour votre application iOS quand vous créez un ID unique pour cette dernière. Que vous utilisiez des droits personnalisés ou non, vous devez toujours créer un ID unique pour votre application Xamarin.iOS afin de pouvoir la publier sur l’App Store.

La création d’un ID d’application et, éventuellement, la sélection de droits impliquent l’exécution des étapes suivantes via le portail web de provisionnement iOS d’Apple :

1. Dans la section **Certificates, Identifiers & Profiles (Certificats, identificateurs et profils)**, sélectionnez **Identifiers (Identificateurs)** > **App ID (ID d’application)**.
2. Cliquez sur le bouton **+**, puis indiquez un **Name (Nom)** et un **Bundle ID (ID de bundle)** pour la nouvelle application.
3. Faites défiler l’affichage vers le bas de l’écran, puis sélectionnez les **services d’application** nécessaires pour votre application Xamarin.iOS.
4. Cliquez sur le bouton **Continue** et suivez les instructions affichées à l’écran pour créer l’ID d’application.

En plus de la sélection et de la configuration des services d’application nécessaires durant la définition de votre ID d’application, vous devez également configurer l’ID d’application et les droits de votre projet Xamarin.iOS en modifiant les fichiers `Info.plist` et `Entitlements.plist`.

Effectuez ce qui suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Info.plist` pour l’ouvrir et le modifier.
2. Dans la section **iOS Application Target (Cible d’application iOS)**, entrez le nom de votre application et le **Bundle Identifier (Identificateur de bundle)** que vous avez créé quand vous avez défini l’ID de l’application.
3. Enregistrez les changements dans le fichier `Info.plist`.
4. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Entitlements.plist` pour l’ouvrir et le modifier.
5. Sélectionnez et configurez les droits nécessaires pour votre application Xamarin.iOS afin qu’ils correspondent à la configuration que vous avez effectuée ci-dessus quand vous avez défini l’ID de l’application.
6. Enregistrez les changements dans le fichier `Entitlements.plist`.

Pour obtenir des instructions détaillées, consultez [Provisionnement des services d’application](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dans notre documentation.

### <a name="setting-the-store-icons"></a>Définition des icônes à afficher dans l’App Store

Vous devez désormais fournir les icônes à afficher dans l’App Store via un catalogue de ressources. Pour ajouter une icône à afficher dans l’App Store, localisez tout d’abord l’image **AppIcon** définie dans le fichier **Assets.xcassets** de votre projet.

L’icône nécessaire dans le catalogue de ressources se nomme **App Store** et doit avoir une taille de **1 024 x 1 024** pixels. Apple indique que l’icône visible dans l’App Store et qui se trouve dans le catalogue de ressources ne peut pas être transparente ni contenir de canal alpha.

Pour plus d’informations sur la définition de l’icône visible dans l’App Store, consultez le guide [Icône de l’App Store](~/ios/app-fundamentals/images-icons/app-store-icon.md).

### <a name="setting-the-apps-icons-and-launch-screens"></a>Définition des icônes d’applications et des écrans de lancement

Pour qu’une application iOS soit acceptée par Apple dans l’App Store, elle doit avoir des icônes et des écrans de lancement appropriés pour tous les appareils iOS sur lesquels elle s’exécute. Les icônes d’applications sont ajoutées à vos projets dans un catalogue de ressources via une image **AppIcon** définie dans le fichier **Assets.xcassets**. Vous ajoutez les écrans de lancement via un Storyboard.

Pour obtenir des instructions détaillées sur la création d’icônes d’application et d’écrans de lancement, consultez les guides [Icône d’application](~/ios/app-fundamentals/images-icons/app-icons.md) et [Écrans de lancement](~/ios/app-fundamentals/images-icons/launch-screens.md).

### <a name="creating-and-installing-a-distribution-profile"></a>Création et installation d’un profil de distribution

iOS utilise des *profils de provisionnement* pour contrôler la façon dont une build d’application particulière peut être déployée. Il s’agit de fichiers qui contiennent des informations sur le certificat utilisé pour signer une application, sur l’*ID de l’application* et sur l’emplacement d’installation de l’application. Pour des tâches de développement et de distribution ad hoc, le profil de provisionnement inclut également la liste des appareils autorisés sur lesquels vous pouvez déployer l’application. Toutefois, pour la distribution dans l’App Store, seules les informations sur les certificats et les ID d’application sont incluses, car l’unique mécanisme de distribution publique est celui de l’App Store.

Le provisionnement implique l’exécution des étapes suivantes à partir du portail web de provisionnement iOS d’Apple :

1.  Sélectionnez **Provisioning (Provisionnement)** > **Distribution**.
2.  Cliquez sur le bouton **+** et sélectionnez le type **App Store** en tant que profil de distribution à créer.
3.  Dans la liste déroulante, sélectionnez l’**ID d’application** pour lequel vous souhaitez créer un profil de distribution.
4.  Sélectionnez un certificat de production (distribution) valide pour signer l’application.
5.  Entrez un **Name (Nom)** pour le nouveau **Distribution Profile (Profil de distribution)**, puis générez le profil.
6.  Sur le Mac, ouvrez Xcode, puis accédez à **Preferences (Préférences) > [sélectionnez votre identifiant Apple]> View Details (Afficher les détails)**. Téléchargez tous les profils disponibles dans Xcode sur le Mac.
7.  Retournez à l’IDE puis, dans les options de **Signature du bundle iOS**, sélectionnez le profil de provisionnement de distribution pour la _configuration de build_ appropriée (**App Store** ou **Mise en production**).

Pour obtenir des instructions détaillées, consultez [Création d’un profil de distribution](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) et [Sélection d’un profil de distribution dans un projet Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="setting-the-build-configuration-for-your-application"></a>Définition de la configuration de build pour votre application

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Effectuez ce qui suit :

1. Cliquez avec le bouton droit sur le **Nom du projet** dans le **Panneau Solutions**, puis sélectionnez **Option** pour modifier ces dernières.
2. Sélectionnez **Build iOS**, puis **Mise en production | iPhone** dans la liste déroulante **Configuration** :

    ![](publishing-to-the-app-store-images/configurevs01.png "Sélectionner AppStore dans la liste déroulante Configuration")

3. Si vous ciblez une version iOS spécifique, vous pouvez la sélectionner dans la liste déroulante **Version du SDK**. Sinon, laissez la valeur par défaut.
4. L’édition des liens réduit la taille globale du fichier distribuable de votre application via la suppression des méthodes, propriétés, classes, etc. inutilisées. Dans la plupart des cas, la valeur par défaut, **Lier les assemblys du SDK uniquement**, doit être conservée. Dans certaines situations, par exemple quand vous utilisez des bibliothèques tierces spécifiques, vous pouvez être obligé de sélectionner la valeur **Ne pas lier** pour empêcher la suppression des éléments nécessaires. Pour plus d’informations, consultez le guide sur les [mécanismes de build iOS](~/ios/deploy-test/ios-build-mechanics.md).
5. Vous devez cocher la case **Optimiser les fichiers image PNG pour iOS** pour réduire davantage la taille du fichier livrable de votre application.
6. Vous ne devez _pas_ activer le débogage, car cela rend la build inutilement volumineuse.
8. Pour iOS 11, vous devez sélectionner l’une des architectures de périphérique qui prend en charge **ARM64**. Pour plus d’informations sur la génération d’applications pour des appareils iOS 64 bits, consultez la section **Activation de builds 64 bits d’applications Xamarin.iOS** dans la documentation traitant des [considérations relatives à la plateforme 32/64 bits](~/cross-platform/macios/32-and-64/index.md).
9. Vous pouvez éventuellement utiliser le compilateur **LLVM**, qui crée du code plus petit et plus rapide, mais la compilation prend plus de temps.
10. En fonction des besoins de votre application, vous pouvez également ajuster le type de **Garbage Collection** utilisé et configurer l’**internationalisation**.
11. Enregistrez les changements apportés à la configuration de build.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Par défaut, quand vous créez une application Xamarin.iOS dans Visual Studio, les _configurations de build_ sont créées automatiquement pour les déploiements de type **Ad Hoc** et **App Store**. Avant d’effectuer la build finale de votre application que vous soumettez à Apple, vous devez apporter quelques modifications à la configuration de base.

Effectuez ce qui suit :

1. Cliquez avec le bouton droit sur le **Nom du projet** dans l’**Explorateur de solutions**, puis sélectionnez **Propriétés** pour modifier ces dernières.
2. Sélectionnez **Build iOS** et **AppStore** (ou **Mise en production** si AppStore n’est pas disponible) dans la liste déroulante **Configuration** :

    ![](publishing-to-the-app-store-images/configurevs01.png "Sélectionner AppStore dans la liste déroulante Configuration")

3. Si vous ciblez une version iOS spécifique, vous pouvez la sélectionner dans la liste déroulante **Version du SDK**. Sinon, laissez la valeur par défaut.
4. L’édition des liens réduit la taille globale du fichier distribuable de votre application via la suppression des méthodes, propriétés, classes, etc. inutilisées. Dans la plupart des cas, la valeur par défaut, **Lier les assemblys du SDK uniquement**, doit être conservée. Dans certaines situations, par exemple quand vous utilisez des bibliothèques tierces spécifiques, vous pouvez être obligé de sélectionner la valeur **Ne pas lier** pour empêcher la suppression des éléments nécessaires. Pour plus d’informations, consultez le guide sur les [mécanismes de build iOS](~/ios/deploy-test/ios-build-mechanics.md).
5. Vous devez cocher la case **Optimiser les fichiers image PNG pour iOS** pour réduire davantage la taille du fichier livrable de votre application.
6. Vous ne devez _pas_ activer le débogage, car cela rend la build inutilement volumineuse.
7. Cliquez sur l’onglet **Avancé** :

    ![](publishing-to-the-app-store-images/configurevs02.png "Onglet Avancé")

8. Si votre application Xamarin.iOS cible iOS 8 et les appareils iOS 64 bits, vous devez sélectionner l’une des architectures d’appareils prenant en charge **ARM64**. Pour plus d’informations sur la génération d’applications pour des appareils iOS 64 bits, consultez la section **Activation de builds 64 bits d’applications Xamarin.iOS** dans la documentation traitant des [considérations relatives à la plateforme 32/64 bits](~/cross-platform/macios/32-and-64/index.md).
9. Vous pouvez éventuellement utiliser le compilateur **LLVM**, qui crée du code plus petit et plus rapide, mais la compilation prend plus de temps.
10. En fonction des besoins de votre application, vous pouvez également ajuster le type de **Garbage Collection** utilisé et configurer l’**internationalisation**.
11. Enregistrez les changements apportés à la configuration de build.

-----

## <a name="building-and-submitting-the-distributable"></a>Génération et soumission du fichier distribuable

Une fois votre application Xamarin.iOS correctement configurée, vous pouvez maintenant effectuer la build de distribution finale que vous allez soumettre à Apple pour qu’elle soit évaluée, et le cas échéant, publiée.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

### <a name="build-your-archive"></a>Générer votre archive

1. Sélectionnez la configuration **Mise en production | Appareil** dans Visual Studio pour Mac :

    ![](publishing-to-the-app-store-images/buildxs01new.png "Sélectionner la configuration Mise en production | Appareil")
1. Dans le menu **Générer**, sélectionnez **Archiver pour publication** :

    ![](publishing-to-the-app-store-images/buildxs02new.png "Sélectionner Archiver pour publication")

1. Une fois l’archive créée, l’affichage **Archives** est présenté :

    ![](publishing-to-the-app-store-images/buildxs03new.png "L’affichage Archives est présenté")


> [!NOTE]
> Remarque : Bien que les anciennes configurations _App Store_ et _Ad hoc_ aient été supprimées de tous les projets de modèle Visual Studio pour Mac, vous pouvez trouver d’anciens projets avec ces configurations. Si tel est le cas, vous pouvez continuer à utiliser la configuration **App Store | Appareil** indiquée à l’étape 1 de la liste ci-dessus.

### <a name="sign-and-distribute-your-app"></a>Signer et distribuer votre application

 Chaque fois que vous générez votre application pour qu’elle soit archivée, l’**affichage Archives** s’ouvre automatiquement, ce qui entraîne l’affichage de tous les projets archivés, regroupés par solution. Par défaut, cet affichage montre uniquement la solution actuelle, ouverte. Pour afficher toutes les solutions ayant des archives, cliquez sur l’option **Afficher toutes les archives**.

 Il est recommandé de conserver les archives déployées auprès des clients (déploiements App Store ou Entreprise) pour que les informations de débogage générées puissent être symbolisées plus tard.

 Pour signer votre application et préparer sa distribution :


1. Sélectionnez **Signer et distribuer**, comme indiqué dans l’illustration ci-dessous :

    ![](publishing-to-the-app-store-images/buildxs04new.png "Sélectionner Signer et distribuer")

1. Cela entraîne l’ouverture de l’Assistant Publication. Sélectionnez le canal de distribution **App Store** pour créer un paquet, puis ouvrez Application Loader :

    ![](publishing-to-the-app-store-images/distribute01.png "Ouvrir Application Loader")

1. Dans l’écran Profil de provisionnement, sélectionnez votre identité de signature et le profil de provisionnement correspondant, ou reconnectez-vous avec une autre identité :

    ![](publishing-to-the-app-store-images/distribute02.png "Sélectionner l’identité de signature et le profil de provisionnement correspondant")

1. Vérifiez les détails du paquet, puis cliquez sur **Publier** pour enregistrer votre paquet `.ipa` :

    ![](publishing-to-the-app-store-images/distribute03.png "Vérifier les détails du paquet")

1. Une fois le paquet `.ipa` enregistré, votre application est prête à être chargée sur iTunes Connect via Application Loader :

    ![](publishing-to-the-app-store-images/distribute04.png "Écran indiquant la réussite de la publication")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le plug-in Xamarin pour Visual Studio ne prend pas en charge le workflow d’archivage pour la publication d’applications iOS sur l’App Store. Ainsi, vous avez chargé un fichier IPA créé via la commande de **génération de fichier IPA ad hoc**, décrite ci-dessous.


## <a name="build-an-ipa"></a>Générer un fichier IPA

 Cette section décrit la génération d’un fichier IPA, opération similaire au workflow de distribution ad hoc ou d’entreprise. Toutefois, il est signé à l’aide du profil de provisionnement App Store créé ci-dessus.

 Effectuez ce qui suit :

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nom du projet Xamarin.iOS, puis sélectionnez **Propriétés** pour modifier ces dernières :

    ![](publishing-to-the-app-store-images/imagevs01.png "Sélectionner les propriétés")
1. Sélectionnez **Signature du bundle iOS**, puis remplacez le profil de provisionnement existant par le profil de provisionnement App Store :

    ![](publishing-to-the-app-store-images/ipa01.png "Sélectionner Signature du bundle iOS, puis remplacer le profil de provisionnement existant par le profil de provisionnement App Store")
1. Sélectionnez **Options IPA iOS**, puis **Ad hoc** dans la liste déroulante **Configuration** (si Ad hoc n’apparaît pas, sélectionnez **Mise en production** à la place) :

    ![](publishing-to-the-app-store-images/imagevs02.png "Sélectionner Ad hoc dans la liste déroulante Configuration")

1. Vous pouvez éventuellement spécifier un **Nom de paquet** pour le fichier IPA. Sinon, il aura le même nom que le projet Xamarin.iOS.
1. Enregistrez les changements apportés aux propriétés du projet.
1. Sélectionnez **Ad hoc** dans la liste déroulante **Configuration de build** de Visual Studio pour Mac :

    ![](publishing-to-the-app-store-images/imagevs05.png "Sélectionner Ad hoc dans la liste déroulante Configuration de build")
1. Générez le projet pour créer le paquet IPA.
1. Le paquet IPA est généré dans le dossier `Bin` > _Appareil iOS_ > `Ad Hoc` :

    ![](publishing-to-the-app-store-images/imagevs06.png "Paquet IPA affiché dans l’Explorateur de fichiers")

-----


## <a name="customizing-the-ipa-location"></a>Personnalisation de l’emplacement du fichier IPA

Une nouvelle propriété **MSBuild** `IpaPackageDir` a été ajoutée pour faciliter la personnalisation de l’emplacement de sortie du fichier `.ipa`. Si `IpaPackageDir` est défini à l’aide d’un emplacement personnalisé, le fichier `.ipa` est stocké dans cet emplacement au lieu du sous-répertoire horodaté par défaut. Cela peut être utile quand vous créez des builds automatisées qui reposent sur un chemin de répertoire spécifique pour fonctionner correctement, par exemple les build d’intégration continue (CI).

Il existe plusieurs façons d’utiliser la nouvelle propriété :

Par exemple, pour envoyer le fichier `.ipa` vers l’ancien répertoire par défaut (comme dans Xamarin.iOS 9.6 et les versions antérieures), vous pouvez affecter à la propriété `IpaPackageDir` la valeur `$(OutputPath)` en adoptant l’une des approches suivantes. Les deux approches sont compatibles avec toutes les builds d’API unifiée Xamarin.iOS, notamment les builds d’IDE et les builds de ligne de commande qui utilisent `xbuild`, `msbuild` ou `mdtool` :

  - La première option consiste à définir la propriété `IpaPackageDir` dans un élément `<PropertyGroup>` d’un fichier **MSBuild**. Par exemple, vous pouvez ajouter le `<PropertyGroup>` suivant au bas du fichier `.csproj` du projet d’application iOS (juste avant la balise de fermeture `</Project>`) :

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - Une meilleure approche consiste à ajouter un élément `<IpaPackageDir>` au bas du `<PropertyGroup>` existant, qui correspond à la configuration utilisée pour générer le fichier `.ipa`. Cette solution est préférable, car elle permet de préparer le projet pour une compatibilité future avec un paramètre planifié de la page de propriétés de projet dans les Options IPA iOS. Si vous utilisez la configuration `Release|iPhone` pour générer le fichier `.ipa`, l’ensemble du groupe de propriétés mis à jour peut ressembler à ce qui suit :

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
      </PropertyGroup>
      ```
Il existe une autre technique pour les builds de ligne de commande `msbuild` ou `xbuild`. Elle consiste à ajouter un argument de ligne de commande `/p:` pour définir la propriété `IpaPackageDir`. Dans ce cas, notez que `msbuild` ne développe pas les expressions `$()` passées sur la ligne de commande, il est donc impossible d’utiliser la syntaxe `$(OutputPath)`. À la place, vous devez fournir un chemin complet. La commande Mono `xbuild` développe les expressions `$()`, mais il est tout de même préférable d’utiliser un chemin complet, car `xbuild` risque d’être déprécié au profit de la [version multiplateforme de `msbuild`](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) dans les versions futures. Voici à quoi peut ressembler un exemple complet de cette approche sur Windows :

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Ou sur Mac :

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Une fois votre build de distribution créée et archivée, vous êtes prêt à soumettre votre application à iTunes Connect.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>Copier automatiquement les bundles .app dans Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Soumission de votre application à Apple

> [!NOTE]
> Remarque : Apple a changé récemment son processus de vérification des applications iOS, et peut rejeter les applications qui contiennent `iTunesMetadata.plist` dans le paquet IPA. Si vous rencontrez l’erreur `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`, la solution de contournement décrite [ici](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) permet en principe de résoudre le problème.

Une fois que vous avez effectué la build de distribution, vous êtes prêt à soumettre votre application iOS à Apple pour qu’elle soit évaluée et, le cas échéant, publiée sur l’App Store.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

 Effectuez ce qui suit :

1. Démarrez **Xcode**.
2. Dans le menu **Window (Fenêtre)**, sélectionnez **Organizer (Organisateur)**.
3. Cliquez sur l’onglet **Archives**, puis sélectionnez l’archive générée ci-dessus :

    ![](publishing-to-the-app-store-images/publishxs01.png "Sélectionner l’archive à soumettre")
4. Cliquez sur le bouton **Validate (Valider)**.
5. Sélectionnez le compte à vérifier, puis cliquez sur le bouton **Choose (Choisir)** :

    ![](publishing-to-the-app-store-images/publishxs02.png "Sélectionner le compte à vérifier")
6. Cliquez sur le bouton **Validate (Valider)** :

    ![](publishing-to-the-app-store-images/publishxs03.png "Boîte de dialogue de récapitulatif du fichier")
7. S’il existe des problèmes liés au bundle, ils sont affichés.
8. Corrigez les problèmes et regénérez l’archive dans Visual Studio pour Mac.
9. Cliquez sur le bouton **Submit (Soumettre)**.
10. Sélectionnez le compte à vérifier, puis cliquez sur le bouton **Choose (Choisir)** :

    ![](publishing-to-the-app-store-images/publishxs04.png "Sélectionner le compte à vérifier")
11. Cliquez sur le bouton **Submit (Soumettre)** :

    ![](publishing-to-the-app-store-images/publishxs05.png "Boîte de dialogue de récapitulatif du fichier")
12. Xcode vous informe à la fin du chargement du fichier sur iTunes Connect.


Le workflow d’archivage dans Visual Studio pour Mac ouvre automatiquement Application Loader, une fois que vous avez enregistré le fichier _.ipa_ :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour soumettre votre application à Apple afin qu’elle soit évaluée, vous devez utiliser Application Loader. Vous devez effectuer ces étapes sur l’hôte de build Mac. Vous pouvez télécharger une copie d’Application Loader à partir d’[ici](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg).

-----

1. Sélectionnez *Deliver Your App (Livrer votre application)*, puis cliquez sur le bouton *Choose (Choisir)* :

    [![](publishing-to-the-app-store-images/publishvs01.png "Sélectionner Livrer votre application")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. Sélectionnez le fichier zip ou IPA créé ci-dessus, puis cliquez sur le bouton **OK**.

3. Application Loader valide le fichier :

    [![](publishing-to-the-app-store-images/publishvs02.png "Écran de validation")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. Cliquez sur le bouton *Suivant* pour permettre la validation de l’application par l’App Store :

    [![](publishing-to-the-app-store-images/publishvs03.png "Validation pour l’App Store")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. Cliquez sur le bouton **Envoyer** pour envoyer l’application à Apple pour évaluation.
6. Application Loader vous informe une fois le chargement du fichier réussi.

## <a name="itunes-connect-status"></a>État dans iTunes Connect

Si vous vous reconnectez à iTunes Connect et si vous sélectionnez votre application dans la liste des applications disponibles, l’état dans iTunes Connect doit être **Waiting for Review (En attente d’évaluation)** (il est parfois indiqué temporairement **Upload Received (Chargement reçu)** pendant le traitement) :

[![](publishing-to-the-app-store-images/image21.png "L’état dans iTunes Connect doit maintenant indiquer que l’évaluation est en attente")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>Récapitulatif

Cet article a présenté un guide pas à pas qui explique comment configurer, générer et soumettre une application pour qu’elle soit publiée sur l’App Store. Tout d’abord, il a décrit les étapes nécessaires pour créer et installer un profil de provisionnement de distribution. Ensuite, il a expliqué comment utiliser Visual Studio et Visual Studio pour Mac pour créer une version de distribution. Enfin, il vous a montré comment utiliser iTunes Connect et l’outil permettant de soumettre une application dans l’App Store.


## <a name="related-links"></a>Liens associés

- [Utilisation d’images](~/ios/app-fundamentals/images-icons/index.md)
- [Guide du workflow de développement d’applications iOS : distribution d’applications](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Conseils pour la soumission à l’App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Motifs courants de rejet d’application](https://developer.apple.com/app-store/review/rejections/)
- [Recommandations pour l’évaluation dans l’App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
