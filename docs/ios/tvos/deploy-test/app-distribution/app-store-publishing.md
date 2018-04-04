---
title: Publication sur l’App Store d’Apple TV
description: Cet article explique comment configurer, générer et publier une application Xamarin.tvOS pour la distribution via l’App Store d’Apple TV. Il inclut un guide pas à pas qui décrit comment préparer votre application pour la distribution, comment utiliser les outils d’Apple soumettre votre application pour vérifier et, enfin, comment publier votre application sur l’App Store d’Apple TV.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c72b161845f6d8492a47d95e51a80cf0b4f30271
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Publication sur l’App Store d’Apple TV

_Cet article explique comment configurer, générer et publier une application Xamarin.tvOS pour la distribution via l’App Store d’Apple TV. Il inclut un guide pas à pas qui décrit comment préparer votre application pour la distribution, comment utiliser les outils d’Apple soumettre votre application pour vérifier et, enfin, comment publier votre application sur l’App Store d’Apple TV._

Dans l’ordre de distribuer des applications à tous les appareils d’Apple TV, Apple nécessite que les applications à publier via le *App Store d’Apple TV*, qui effectue l’App Store de l’emplacement d’achat unique pour les applications de tvOS. Les développeurs de nombreux types d’applications peuvent en majuscules sur la réussite de gros volumes de ce point de distribution unique. L’App Store d’Apple TV est une solution clé en main, les développeurs d’applications des systèmes de distribution et de paiement.

Le processus de soumission d’une application à l’App Store d’Apple TV comprend :

1. Création d’un *ID d’application* et sélection de *droits*
2. Création d’un *profil de provisionnement de distribution*
3. À l’aide de ce profil pour générer votre application.
4. Soumission de votre application via *iTunes Connect*.


Dans cet article, nous aborderons toutes les étapes nécessaires pour configurer, générer et envoyer une application pour la distribution de l’App Store d’Apple TV.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Avant de soumettre une application

Une fois que vous envoyez une application pour la publication vers le magasin d’applications Apple TV, il passe par un processus de révision par Apple pour vous assurer qu’il répond aux instructions d’Apple pour la qualité et le contenu. Si votre application ne respecte pas ces recommandations, Apple la rejettera. Vous devrez alors corriger la non-conformité citée par Apple, puis soumettre à nouveau votre application.
Ainsi, vous avez de plus grandes chances de réussir l’évaluation effectuée par Apple en vous familiarisant avec ces recommandations et en essayant d’adapter votre application à ces dernières. Les instructions d’Apple sont disponibles au [directives de révision App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html) et [préparer votre application présentation de la nouvelle Apple TV](https://developer.apple.com/tvos/submit/).

Voici quelques éléments à surveiller quand vous soumettez une application :

1. Assurez-vous que la description de l’application correspond à la fonctionnalité incluse dans l’application.
2. Test de l’application ne se bloque en temps normal. Cela inclut l’utilisation sur chaque appareil Apple TV que vous prenez en charge.


Apple conserve également une liste des conseils de soumission TV App Store d’Apple. Vous pouvez les lire sur la page décrivant la [distribution d’applications sur l’App Store](https://developer.apple.com/appstore/resources/submission/tips.html).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Configuration de votre application dans iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) est une suite d’outils basé sur le web, entre autres choses, la gestion de vos applications tvOS sur l’App Store d’Apple TV. Votre application Xamarin.tvOS doit être correctement le programme d’installation et configuré dans iTunes Connect avant qu’il peut être soumise à la révision à Apple et pour finir, libérée pour la vente ou comme une application disponible dans le magasin d’applications Apple TV.

Effectuez ce qui suit :

1. Vérifiez que les contrats appropriés sont en place et à jour dans la section **Agreements, Tax, and Banking (Contrats, taxes et informations bancaires)** d’iTunes Connect pour pouvoir publier une application iOS gratuite ou payante.
2. Créer un nouveau **iTunes Connect enregistrement** pour l’application et spécifier son **nom d’affichage** (comme indiqué dans l’App Store d’Apple TV).
3. Sélectionnez un **Sale Price (Prix de vente)** ou indiquez que l’application est disponible gratuitement.
4. Fournir un **App Store icône** (icône de grande taille) et des captures d’écran de votre application en action, sur les appareils Apple TV il prend en charge. Consultez notre [fonctionne avec des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) guide pour plus de détails.
5. Fournir un clair, concis **Description** de l’application, y compris ses fonctionnalités et avantages pour l’utilisateur final.
6. Fournir **catégories**, **sous-catégories**, et **mots clés** pour aider l’utilisateur à trouver votre application dans l’App Store d’Apple TV.
7. Fournissez les URL de **Contact** et de **Support** sur votre site web (condition imposée par Apple).
8. Valeur de votre application **évaluation**, qui est utilisé par le contrôle parental sur l’App Store d’Apple TV.
9. Configurez les technologies facultatives de l’App Store, par exemple **Game Center** et **In-App Purchase (Achat dans l’application)**.

Pour plus d’informations, consultez notre [configurer votre application tvOS dans iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) documentation.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Préparation à la distribution via l’App Store

Pour publier une application sur l’App Store d’Apple TV, vous devez tout d’abord générer pour la distribution, ce qui implique de nombreuses étapes. Les sections suivantes couvre tous les éléments requis pour préparer une application Xamarin.tvOS pour publication afin qu’il peut être généré et envoyez-le vers le magasin d’applications Apple TV pour révision et mise en production.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Provisionnement des services d’application

Apple fournit une sélection de Services d’Application spéciale, également appelés droits, ce qui peuvent être activés pour votre application tvOS lorsque vous créez un ID unique pour lui. Si vous utilisez des droits personnalisés ou non, vous devez toujours créer un ID unique pour votre application Xamarin.tvOS avant de pouvoir être publiée sur l’App Store d’Apple TV.

La création d’un ID d’application et, éventuellement, la sélection de droits impliquent l’exécution des étapes suivantes via le portail web de provisionnement iOS d’Apple :

1. Sélectionnez **configuration** > **développement**.
2. Cliquez sur le bouton **+**, puis indiquez un **Name (Nom)** et un **Bundle ID (ID de bundle)** pour la nouvelle application.
3. Faites défiler vers le bas de l’écran et sélectionnez les **des Services d’application** qui est requis par votre application Xamarin.tvOS.
4. Cliquez sur le bouton **Continue** et suivez les instructions affichées à l’écran pour créer l’ID d’application.

Outre les sélectionnant et en configurant les Services d’Application requis lors de la définition de votre ID d’application, vous devez également configurer l’ID d’application et les droits dans votre projet Xamarin.tvOS en modifiant les deux le `Info.plist` et `Entitlements.plist` fichiers.

Procédez comme suit dans Visual Studio pour Mac :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Info.plist` pour l’ouvrir et le modifier.
2. Dans le **tvOS Application cible** section, entrez un nom pour votre application et entrez le **identificateur de lot** que vous avez créé lorsque vous avez défini l’application ID.
3. Enregistrez les changements dans le fichier `Info.plist`.
4. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Entitlements.plist` pour l’ouvrir et le modifier.
5. Sélectionner et configurer les droits requis pour vous Xamarin.tvOS application afin qu’ils correspondent le programme d’installation que vous avez effectué ci-dessus lorsque vous avez défini l’application ID.
6. Enregistrez les changements dans le fichier `Entitlements.plist`.

Pour obtenir des instructions détaillées, consultez [Provisionnement des services d’application](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dans notre documentation. Alors que ce document a été écrite pour iOS, les mêmes étapes sont utilisés pour configurer une application Xamarin.tvOS.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Définir les icônes d’applications, lancez et haut de l’étagère Image

Pour une application tvOS d’être acceptés par Apple pour être inclus dans l’App Store d’Apple TV, il requiert des icônes correctes, d’exécution et d’images en haut de l’étagère pour tous les appareils d’Apple TV qui fonctionnent sur. Vous devez ajouterez les ressources d’Image requis qui seront compilés en un `Assets.car` de fichiers et inclus dans votre Xamarin.tvOS d’offre groupée avant d’être téléchargée à iTunes Connect.

Pour obtenir des instructions détaillées, consultez notre [fonctionne avec des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) documentation.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Création et installation d’un profil de distribution

tvOS utilise *profils de configuration* pour contrôler la façon dont une build d’application particulière peut être déployée. Il s’agit de fichiers qui contiennent des informations sur le certificat utilisé pour signer une application, sur l’*ID de l’application* et sur l’emplacement d’installation de l’application. Pour des tâches de développement et de distribution ad hoc, le profil de provisionnement inclut également la liste des appareils autorisés sur lesquels vous pouvez déployer l’application. Toutefois, pour la distribution de l’App Store d’Apple TV, informations d’ID de certificat et d’application uniquement sont inclus, étant donné que le seul mécanisme pour la distribution publique est via l’App Store d’Apple TV.

Le provisionnement implique l’exécution des étapes suivantes à partir du portail web de provisionnement iOS d’Apple :

1.  Sélectionnez **Provisioning (Provisionnement)** > **Distribution**.
2.  Cliquez sur le **+** bouton et sélectionnez le type de profil de Distribution que vous souhaitez créer en tant que **App Store d’Apple TV**.
3.  Dans la liste déroulante, sélectionnez l’**ID d’application** pour lequel vous souhaitez créer un profil de distribution.
4.  Sélectionnez le certificat requis pour signer l’application.
5.  Entrez un **Name (Nom)** pour le nouveau **Distribution Profile (Profil de distribution)**, puis générez le profil.
6.  Actualiser la liste des profils disponibles dans Xcode.
7.  Sélectionnez la Distribution de configuration du profil dans Visual Studio pour le **App Store** _Configuration de Build_.

Pour obtenir des instructions détaillées, consultez [Création d’un profil de distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) et [Sélection d’un profil de distribution dans un projet Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile). Là encore, les deux de ces documents sont spécifiques à iOS, mais la même technique est utilisée pour les applications de tvOS.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Définition de la configuration de build pour votre application

Par défaut, lorsque vous créez une nouvelle application de Xamarin.tvOS _Configurations de Build_ sont créées automatiquement pour les deux **déboguer** et **version** déploiement. Avant de procéder à la version finale de votre application vous soumettrez à Apple, il existe quelques modifications que vous devez apporter à la base de **version** configuration.

Effectuez ce qui suit :

1. Avec le bouton droit sur le **nom du projet** dans les **l’Explorateur de solutions** et sélection **Options** pour les ouvrir pour le modifier.
2. Si vous ciblez une version spécifique de tvOS, sélectionnez-la sous **tvOS Build** > **iOS Version du Kit de développement logiciel**. Pour la version préliminaire de la prise en charge de tvOS, laissez cette valeur est définie sur **par défaut**.
3. Réduit la taille globale de votre application distribuable en supprimant des Classes de méthodes, propriétés inutilisées, etc. et dans la plupart des cas doivent rester sur la valeur par défaut **framework lien SDK uniquement**. Dans certaines situations, par exemple lorsque certains spécifique 3e bibliothèques de tiers, vous devrez peut-être définir cette valeur sur **ne sont pas liées** pour conserver l’élément nécessaire d’être supprimé.
4. Pour expédier une application Xamarin.tvOS, vous devez utiliser le compilateur d’optimisation LLVM. Vérifiez que le **utiliser le compilateur d’optimisation de LLVM** case à cocher est activée sous le **version** configuration.
5. Apple requis également que les applications de tvOS utilisent bitcode. À nouveau sous le **version** configuration, ajouter `--bitcode=asmonly` à la **les arguments supplémentaires mtouch** boîte.
6. Le **fichiers d’image PNG d’optimiser pour iOS** case à cocher doit être vérifiée car cela vous aidera à recul taille du livrable de votre application.
7. Le débogage doit *pas* être activée car il s’agrandir la build inutilement.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Génération et soumission du fichier distribuable

Avec votre application Xamarin.tvOS est correctement configurée, vous êtes maintenant prêt à effectuer la génération de sa distribution finale que vous soumettrez à Apple pour révision et mise en production.

#### <a name="build-your-archive"></a>Générer votre archive

1. Sélectionnez la configuration **Mise en production | Appareil** dans Visual Studio pour Mac :

    ![](app-store-publishing-images/buildxs01new.png "Sélectionnez la configuration Release")
2. Dans le menu **Générer**, sélectionnez **Archiver pour publication** :

    [![](app-store-publishing-images/buildxs02new.png "Sélectionner Archiver pour publication")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Une fois l’archive créée, l’affichage **Archives** est présenté :

    [![](app-store-publishing-images/buildxs03new.png "La vue des Archives")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Signer et distribuer votre application

Chaque fois que vous générez votre application pour qu’elle soit archivée, l’*affichage Archives* s’ouvre automatiquement, ce qui entraîne l’affichage de tous les projets archivés, regroupés par solution. Par défaut, cet affichage montre uniquement la solution actuelle, ouverte. Pour afficher toutes les solutions ayant des archives, cliquez sur l’option **Afficher toutes les archives**.

Il est recommandé de conserver les archives déployées auprès des clients (déploiements App Store ou Entreprise) pour que les informations de débogage générées puissent être symbolisées plus tard.

Pour signer votre application et préparer sa distribution :

1. Sélectionnez le **signer et distribuer...** , illustré ci-dessous :

    [![](app-store-publishing-images/buildxs04new.png ", Sélectionnez theSign et distribuer...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Cela entraîne l’ouverture de l’Assistant Publication. Sélectionnez le canal de distribution **App Store** pour créer un paquet, puis ouvrez Application Loader :

    [![](app-store-publishing-images/distribute01.png "Sélectionnez le canal de distribution App Store")](app-store-publishing-images/distribute01.png#lightbox)
3. Dans l’écran de configuration de profil, sélectionnez votre identité de signature et de correspondant du profil de configuration ou signer à nouveau avec une autre identité :

    [![](app-store-publishing-images/distribute02.png "Sélectionnez l’identité de signature et correspondant du profil de configuration")](app-store-publishing-images/distribute02.png#lightbox)
4. Vérifiez les détails du paquet, puis cliquez sur **Publier** pour enregistrer votre paquet `.ipa` :

    [![](app-store-publishing-images/distribute03.png "Vérifiez les détails du package")](app-store-publishing-images/distribute03.png#lightbox)
5. Une fois le paquet `.ipa` enregistré, votre application est prête à être chargée sur iTunes Connect via Application Loader :

    [![](app-store-publishing-images/distribute04.png "Téléchargé dans iTunes Connect via le chargeur d’Application")](app-store-publishing-images/distribute04.png#lightbox)

Une fois votre build de distribution créée et archivée, vous êtes prêt à soumettre votre application à iTunes Connect.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Soumission de votre application à Apple

Une fois que vous avez effectué la build de distribution, vous êtes prêt à soumettre votre application iOS à Apple pour qu’elle soit évaluée et, le cas échéant, publiée sur l’App Store.


Le flux de travail archive dans Visual Studio pour Mac s’ouvre automatiquement, chargeur d’Application une fois que vous avez enregistré le `.ipa`:

2. Sélectionnez *Deliver Your App (Livrer votre application)*, puis cliquez sur le bouton *Choose (Choisir)* :

    [![](app-store-publishing-images/publishvs01.png "Sélectionner Livrer votre application")](app-store-publishing-images/publishvs01.png#lightbox)

3. Sélectionnez le fichier zip ou IPA créé ci-dessus, puis cliquez sur le bouton **OK**.
4. Application Loader valide le fichier :

    [![](app-store-publishing-images/publishvs02.png "L’écran de validation du chargeur d’Application")](app-store-publishing-images/publishvs02.png#lightbox)
5. Cliquez sur le bouton *Suivant* pour permettre la validation de l’application par l’App Store :

    [![](app-store-publishing-images/publishvs03.png "L’application en cours de validation par rapport à l’App Store")](app-store-publishing-images/publishvs03.png#lightbox)
6. Cliquez sur le bouton **Envoyer** pour envoyer l’application à Apple pour évaluation.
7. Application Loader vous informe une fois le chargement du fichier réussi.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>État dans iTunes Connect

Si vous ouvrez une session dans iTunes Connect et sélectionnez votre application à partir de la liste des applications disponibles, l’état dans iTunes Connect devrait maintenant indiquer qu’il s’agit **en attente pour révision** (il peut lire temporairement **télécharger reçu** pendant son traitement) :

[![](app-store-publishing-images/image21.png "L’état dans iTunes connecter affichant en attente de révision")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Résolution des problèmes

Si vous rencontrez des problèmes de soumission de votre application Xamarin.tvOS vers le magasin d’applications Apple TV, veuillez consulter notre [dépannage](~/ios/tvos/troubleshooting.md) guide. Il contient plusieurs problèmes connus que vous pouvez rencontrer et comment les résoudre dans les Xamarin.tvOS.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article affiche un guide pas à pas pour la configuration, la création et la soumission d’une application pour la publication de l’App Store d’Apple TV. Tout d’abord, il a décrit les étapes nécessaires pour créer et installer un profil de provisionnement de distribution. Ensuite, elle est parcourue via l’utilisation de Visual Studio pour Mac pour créer une build de distribution. Enfin, il vous a montré comment utiliser iTunes Connect et l’outil d’archivage Xcode soumettre une demande à l’App Store d’Apple TV.


## <a name="related-links"></a>Liens associés

- [Utilisation des icônes et des images](~/ios/tvos/app-fundamentals/icons-images.md)
- [Préparer l’envoi de votre application pour le nouveau Apple TV](https://developer.apple.com/tvos/submit/)
- [Conseils pour la soumission à l’App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Motifs courants de rejet d’application](https://developer.apple.com/app-store/review/rejections/)
- [Recommandations pour l’évaluation dans l’App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
