---
title: "Installation de Xamarin.iOS sur un système Windows"
description: "Cet article explique comment configurer Xamarin.iOS pour Visual Studio. Il aborde le processus d’installation de l’extension Xamarin pour Visual Studio et décrit la connexion au SDK Apple installé sur le Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: cfbe2df23317ee3ad11c9970ab892ddcc251b9d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="installing-xamarinios-on-windows"></a>Installation de Xamarin.iOS sur un système Windows

_Cet article explique comment configurer Xamarin.iOS pour Visual Studio. Il aborde le processus d’installation de l’extension Xamarin pour Visual Studio, et décrit la connexion au kit SDK Apple installé sur le Mac._

## <a name="overview"></a>Vue d'ensemble

Xamarin.iOS pour Visual Studio permet que des applications iOS soient écrites et testées sur des ordinateurs Windows, avec un Mac en réseau qui fournit le service de build et de déploiement.

Développer du contenu pour iOS dans Visual Studio offre la possibilité d’effectuer les opérations suivantes :

- Créer des solutions multiplateformes pour applications iOS, Android et Windows.
- Utiliser des outils Visual Studio (comme *ReSharper* et *Team Foundation Server*) pour tous vos projets multiplateformes, dont le code source iOS.
- Travailler avec un environnement IDE familier, tout en tirant parti des liaisons Xamarin.iOS de toutes les API d’Apple.

Xamarin.iOS pour Visual Studio prend en charge les configurations dans lesquelles Visual Studio s’exécute dans une machine virtuelle Windows sur un Mac (à l’aide de Parallels ou de VMWare), ou quand il se trouve sur un ordinateur distinct qui est visible sur le même réseau qu’un Mac. Quelle que soit la configuration qui vous convient le mieux, Visual Studio se connecte au Mac rapidement et de manière sécurisée via SSH.

Cet article décrit les étapes d’installation et de configuration des outils Xamarin.iOS à la fois sur le Mac et sur l’ordinateur Windows, ainsi que les étapes de connexion à l’hôte Mac pour permettre aux développeurs de générer, de déboguer et de déployer des applications Xamarin.iOS à l’aide de Visual Studio.

Le diagramme ci-dessous présente une vue d’ensemble du workflow de développement Xamarin.iOS :

[![Flux de travail de développement Xamarin.iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  En fait, Visual Studio lance un processus MSBuild distinct pour générer les projets. Ce processus crée une autre connexion au Mac, ce qui signifie qu’il existe en fait deux connexions SSH de Windows vers Mac quand Visual Studio effectue la génération. La génération à partir de la [ligne de commande](~/ios/get-started/installation/windows/connecting-to-mac/index.md) crée uniquement le processus MSBuild. Pour simplifier ce diagramme, toutes les connexions sont simplement représentées par une seule flèche.

## <a name="requirements"></a>Configuration requise

Xamarin.iOS pour Visual Studio accomplit un tour de force : il permet aux développeurs de créer, de générer et de déboguer des applications iOS sur un ordinateur Windows à l’aide de l’environnement IDE de Visual Studio. Il ne peut pas y parvenir seul : il n’est pas possible de créer des applications iOS sans le compilateur d’Apple, ni de les déployer sans les certificats et les outils de signature du code d’Apple. Cela signifie qu’une installation de Xamarin.iOS pour Visual Studio exige une connexion à un ordinateur Mac OS X en réseau pour effectuer ces tâches. Une fois configurés, les outils de Xamarin rendent le processus aussi transparent que possible.


<a name="system-requirements"/>

### <a name="system-requirements"></a>Configuration système requise

La configuration système requise est la suivante :

### <a name="windows"></a>Windows

1. Windows 7 ou une version ultérieure

2. Visual Studio 2015 Professional ou une version ultérieure

    a. Si vous disposez d’une licence d’entreprise, vous devez installer Visual Studio Enterprise.

3. Xamarin pour Visual Studio.

Les outils Xamarin ne peuvent pas être utilisés avec les éditions Express de Visual Studio en raison de l’absence de prise en charge des plug-ins. Xamarin est pris en charge dans Visual Studio Community.

### <a name="mac"></a>Mac

1. Un Mac exécutant macOS Sierra (10.12) ou une version ultérieure (bien que la dernière version stable soit recommandée).

2. SDK Xamarin iOS. Il est installé lors du téléchargement de Visual Studio pour Mac.

3. L’IDE Xcode et le kit SDK iOS d’Apple (la dernière version stable disponible dans le Mac App Store est recommandée).

**L’ordinateur Windows doit être en mesure d’établir une connexion avec le Mac via le réseau.**

### <a name="apple-developer-account"></a>Compte de développeur Apple

Pour déployer des applications sur un appareil ou les envoyer à l’App Store, un compte de développeur Apple est nécessaire. Pour que Xamarin.iOS pour Visual Studio puisse fonctionner, les certificats de développeur et profils de provisionnement appropriés doivent être créés et installés sur le Mac en réseau. Pour connaître les étapes à suivre pour obtenir un certificat de développement et provisionner un appareil, consultez l’article [Provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md).

## <a name="features"></a>Fonctionnalités 

Xamarin.iOS pour Visual Studio permet la création, la modification, la génération et le déploiement de projets Xamarin.iOS à partir de Windows. Cela inclut les fonctionnalités suivantes :

- Créer des projets iOS.

- Modifier les solutions multiplateformes et les projets iOS qui incluent également des projets Xamarin.Android et UWP.

- Compiler les solutions multiplateformes et les projets iOS qui incluent également des projets Xamarin.Android et UWP.

- Prendre en charge les plans séquentiels et les fichiers .xib à l’aide du concepteur iOS.

- Déployer et déboguer les applications iOS, qui s’exécutent elles-mêmes dans un simulateur, ou sur un appareil connecté au Mac.

- Simulateur iOS sur Windows : Pour plus d’informations sur l’utilisation du simulateur iOS sur Windows, reportez-vous à [ce guide](~/tools/ios-simulator.md).

<a name="configuring" />

## <a name="configuring-your-mac"></a>Configuration de votre Mac

<a name="installation"/>

### <a name="installation"></a>Installation

Pour installer les outils Xamarin.iOS sur votre hôte Mac, vous devez [installer Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/installation).

Une fois le logiciel installé, suivez les étapes décrites dans les sections suivantes pour configurer Xamarin.iOS sur macOS afin de permettre à Xamarin pour Visual Studio de s’y connecter.

> [!IMPORTANT]
>  L’ordinateur Windows doit utiliser la même version de Xamarin.iOS que le Mac auquel il est connecté. Pour le vérifier :
>
> - **Visual Studio 2015 et les versions antérieures** : vérifiez que vous êtes sur le même [canal de mise à jour](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) que Visual Studio pour Mac.
>
> - **Visual Studio 2017, version Release** : vérifiez que vous êtes sur le canal **Stable** de Visual Studio pour Mac.
>
> - **Visual Studio 2017, préversion** : vérifiez que vous êtes sur le canal **Alpha** de Visual Studio pour Mac.

<a name="configuration" />


### <a name="configuration"></a>Configuration

Pour accéder à la communication entre l’extension Xamarin pour Visual Studio et votre Mac, vous devez autoriser la **Session à distance** sur votre Mac. Suivez les étapes ci-dessous pour définir cette option :

1. Ouvrez *Spotlight* (**Cmd-Espace**), recherchez **Session à distance**, puis sélectionnez le résultat **Partage**. Les **Préférences Système** s’affichent dans le panneau **Partage**.

![Recherche Spotlight pour la session à distance](images/spotlight.png)

2. Cochez l’option **Session à distance** dans la liste **Service** à gauche afin de permettre à Xamarin pour Visual Studio de se connecter au Mac.

![Cocher l’option Session à distance dans la liste Service](images/sharing.png)

3. Vérifiez que l’option **Session à distance** est définie pour autoriser l’accès pour **Tous les utilisateurs**, ou que votre nom d’utilisateur ou groupe Mac figure dans la liste des utilisateurs autorisés affichée à droite.

Le Mac doit maintenant être détectable par Visual Studio s’il se trouve sur le même réseau.

> [!NOTE]
> Si le pare-feu macOS est configuré pour bloquer par défaut les applications signées, vous devrez peut-être autoriser `mono-sgen` à recevoir les connexions entrantes. Une boîte de dialogue d’alerte s’affiche pour vous demander si c’est le cas.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>Configuration développeur iOS

Pour le développement d’iOS, il est important que l’ordinateur Mac soit configuré avec les identités de signature appropriées. Cela vous permet de signer correctement vos applications afin qu’elles puissent être distribuées par le biais de l’App Store ou d’Ad Hoc. Suivez le lien ci-dessous pour obtenir des instructions sur la configuration d’un Mac pour le développement iOS avec Xamarin :

- [Provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Une fois votre Mac configuré, il est temps de passer à la configuration de votre ordinateur Windows.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Installation sur un système Windows

Xamarin peut être installé dans le cadre de votre installation de Studio Visual 2017 ou 2015. Pour installer Visual Studio Tools pour Xamarin, consultez le guide [d’installation Windows](~/cross-platform/get-started/installation/windows.md).

## <a name="installation-complete"></a>Installation terminée

Une fois le processus d’installation terminé, quelques étapes supplémentaires doivent encore être effectuées pour que tout fonctionne :

- [Connecter Visual Studio au Mac](#connectingtomac) : Visual Studio doit être connecté à l’hôte de build Mac pour pouvoir générer des projets Xamarin.iOS.
- [Configurer la barre d’outils de Studio Visual](#toolbar) : cela vous permet d’accéder facilement aux fonctionnalités Xamarin.iOS dans Visual Studio.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Connexion au Mac

Une connexion est établie à partir de Xamarin.iOS pour Visual Studio vers votre hôte de build Mac via une connexion SSH entre ordinateurs. Pour plus d’informations sur la connexion, reportez-vous au guide [Connexion au Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

Pour connecter votre Mac, suivez les étapes ci-dessous :

- Accédez à **Outils > Options**, puis sous **Xamarin** sélectionnez **Paramètres iOS** :

  [![L’écran Paramètres iOS](images/image2.png)](images/image2.png#lightbox)

- À condition que le Mac ait été correctement [configuré](#configuration) pour autoriser la **Session à distance**, vous devriez pouvoir sélectionner votre Mac dans la liste :

  [![La boîte de dialogue Hôte distant](images/xma3.png)](images/xma3.png#lightbox)

- Vous êtes alors invité à fournir les informations d’identification d’administration de votre hôte Mac :

  [![La boîte de dialogue Connexion](images/xma4.png)](images/xma4.png#lightbox)

- Une fois la connexion établie, l’icône « Connexion établie » s’affiche en regard du nom de l’ordinateur :

  [![La boîte de dialogue de session à distance affichant l’icône de connexion établie en regard du nom de l’ordinateur](images/image6.png)](images/image6.png#lightbox)

Vous êtes reconnecté chaque fois que vous démarrez Visual Studio.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Configuration de la barre d’outils de Visual Studio

Quand un projet iOS est ouvert, la barre d’outils iOS est visible par défaut et n’a pas besoin d’être configurée.

Vous pouvez suivre les étapes ci-dessous si la barre d’outils iOS ne s’affiche pas.

Pour configurer la barre d’outils, commencez par ouvrir le menu **Affichage > Barres d’outils**, puis vérifiez que l’entrée **iOS** est sélectionnée. Choisissez l’élément de menu comme indiqué dans cette capture d’écran : il doit être coché pour indiquer que la barre d’outils est visible :

[![Choisir Barres d’outils > iOS](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

Dans les versions antérieures à Visual Studio 2017, il peut être nécessaire d’ajouter le bouton **Plateformes solution** à la barre d’outils standard. Cela permet à un appareil iOS ou au simulateur iOS d’être sélectionné lors du débogage. Suivez les instructions ci-dessous pour définir cette possibilité.

Cliquez sur le bouton de menu situé à droite de la barre d’outils standard :

- Choisissez **Ajouter/supprimer des boutons**
- Sélectionnez **Plateformes solution**

[![Sélectionner Plateformes solution](images/image35.png)](images/image35.png#lightbox)

Les barres d’outils **standard** et **iOS** doivent maintenant ressembler à cette capture d’écran :

[![Les barres d’outils standard et iOS doivent maintenant ressembler à cette capture d’écran](images/image36.png)](images/image36.png#lightbox)

Une fois la configuration des barres d’outils terminée, vous êtes prêt à commencer à utiliser Xamarin iOS pour Visual Studio.

## <a name="summary"></a>Récapitulatif

Cet article a présenté un guide pas à pas pour l’installation, la configuration et l’utilisation de Xamarin iOS pour Visual Studio.

Il a traité l’installation et la configuration des outils prérequis sur Windows et Mac OS X.


## <a name="related-links"></a>Liens associés

- [Installation](~/cross-platform/get-started/installation/windows.md)
- [Provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md)
- [Introduction à Xamarin.iOS pour Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Connecter un Mac à un environnement Visual Studio avec XMA (vidéo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
