---
title: Connexion au Mac
description: Xamarin.iOS pour Visual Studio permet aux développeurs de créer, de générer et de déboguer des applications iOS sur un ordinateur Windows à l’aide de l’environnement IDE de Visual Studio. Ce guide décrit les fonctionnalités fournies par Xamarin.iOS pour Visual Studio et la façon dont la connexion à l’hôte de build Mac est établie.
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e4f7b55fa859473e84298151bc08878bc2161192
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="connecting-to-the-mac"></a>Connexion au Mac

_Xamarin.iOS pour Visual Studio permet aux développeurs de créer, de générer et de déboguer des applications iOS sur un ordinateur Windows à l’aide de l’IDE Visual Studio. Ce guide décrit les fonctionnalités fournies par Xamarin.iOS pour Visual Studio, et explique comment la connexion à l’hôte de build Mac est établie._

Visual Studio se connecte au Mac via SSH, ce qui offre plusieurs avantages, notamment les suivants :

- Visual Studio peut lancer et contrôler directement l’agent de build. Il n’existe plus d’application visible par l’utilisateur qui nécessite un démarrage et un arrêt manuels.

- Le nouveau Gestionnaire des connexions de Visual Studio détecte, authentifie et mémorise l’hôte de build Mac.

- Étant donné que toutes les communications passent de manière sécurisée par un tunnel via SSH, une seule connexion de port unique au port 22 est nécessaire.

- Visual Studio est averti des modifications dès qu’elles se produisent. Par exemple, quand un appareil iOS est branché, la barre d’outils se met immédiatement à jour.

- Plusieurs instances de Visual Studio peuvent se connecter simultanément.

- La connexion ne porte pas atteinte au développement. Vous êtes simplement invité à établir une connexion au Mac quand vous effectuez une opération pour laquelle le Mac est nécessaire, par exemple le débogage ou l’utilisation du Designer iOS.

La connexion au Mac se compose de plusieurs processus correspondant aux différentes parties de ses fonctionnalités (par exemple, l’agent Designer iOS et l’agent de build) qui sont contrôlées par un service répartiteur. Ce répartiteur est contrôlé et mis à jour par Visual Studio, et redémarre automatiquement tous les processus indépendants quand ils se bloquent.

Le diagramme ci-dessous présente une vue d’ensemble du workflow de développement Xamarin.iOS :

[![Workflow de développement iOS](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> En fait, Visual Studio lance un processus MSBuild distinct pour générer les projets. Ce processus crée une autre connexion au Mac, ce qui signifie qu’il existe en fait deux connexions SSH de Windows vers Mac quand Visual Studio effectue la génération. La génération à partir de la [ligne de commande](#commandline) crée uniquement le processus MSBuild. Pour simplifier ce diagramme, toutes les connexions sont simplement représentées par une seule flèche.

## <a name="requirements"></a>Configuration requise

Xamarin.iOS pour Visual Studio accomplit un tour de force : il permet aux développeurs de créer, de générer et de déboguer des applications iOS sur un ordinateur Windows à l’aide de l’environnement IDE de Visual Studio. Il ne peut pas y parvenir seul : il n’est pas possible de créer des applications iOS sans le compilateur d’Apple, ni de les déployer sans les certificats et les outils de signature du code d’Apple. Cela signifie que votre installation de Xamarin.iOS pour Visual Studio exige une connexion à un ordinateur Mac OS X en réseau (appelé *hôte* ou *hôte de build*) pour effectuer automatiquement ces tâches. Une fois configurés, les outils de Xamarin rendent le processus aussi transparent que possible.

### <a name="system-requirements"></a>Configuration système requise

Vous trouverez la configuration requise dans le guide [Installer Xamarin.iOS sous Windows](~/ios/get-started/installation/windows/index.md#system-requirements).


#### <a name="compatibility"></a>Compatibilité

> [!IMPORTANT]
> L’ordinateur Windows doit utiliser la même version de Xamarin.iOS que le Mac auquel il est connecté. Pour le vérifier :                                                    
>                                                                                                                 
> - **Visual Studio 2015 et les versions antérieures** : vérifiez que vous êtes sur le même [canal de mise à jour](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) que Visual Studio pour Mac.
>                                                                                                                 
> - **Visual Studio 2017, version Release** : vérifiez que vous êtes sur le canal **Stable** de Visual Studio pour Mac.
>                                                                                                                 
> - **Visual Studio 2017, préversion** : vérifiez que vous êtes sur le canal **Alpha** de Visual Studio pour Mac. Visual Studio ne vérifie pas que le SDK Xamarin.iOS et Xcode existent et que leurs versions sont compatibles.
>   Ces vérifications sont effectuées par l’agent de build, ce qui aboutit à des erreurs de build, et par l’agent Designer iOS, ce qui aboutit à des erreurs du concepteur.

### <a name="connecting-to-the-mac"></a>Connexion au Mac

#### <a name="mac-setup"></a>Configuration du Mac

Pour configurer l’hôte Mac, vous devez activer la communication entre l’extension Xamarin pour Visual Studio et votre Mac. Pour ce faire, vous devez autoriser la **Session à distance** sur votre Mac en effectuant les étapes suivantes :

1. Ouvrez *Spotlight* (**⌘-Espace**), recherchez *Session à distance*, puis sélectionnez le résultat *Partage*. Les *Préférences Système* s’affichent dans le panneau *Partage* :

   [![Recherche Spotlight pour la session à distance](images/spotlight.png)](images/spotlight.png#lightbox)

2. Cochez l’option *Session à distance* dans la liste *Service* à gauche pour permettre à Xamarin pour Visual Studio de se connecter au Mac :

   [![Cocher l’option Session à distance dans la liste Service](images/sharing.png)](images/sharing.png#lightbox)

3. Vérifiez que l’option *Session à distance* est définie pour autoriser l’accès pour *Tous les utilisateurs*, ou que votre nom d’utilisateur ou groupe Mac figure dans la liste des utilisateurs autorisés affichée à droite.

En outre, si le pare-feu OS X est configuré pour bloquer par défaut les applications signées, vous devrez peut-être autoriser `mono-sgen` à recevoir les connexions entrantes. Une boîte de dialogue d’alerte s’affiche pour vous demander si c’est le cas.

À condition qu’il y ait une session ouverte et active sur votre Mac, ce dernier doit maintenant être détectable par Visual Studio s’il se trouve sur le même réseau.

Visual Studio démarre et arrête l’agent sur votre Mac. En tant qu’utilisateur, vous n’avez donc rien d’autre à exécuter.

### <a name="windows-setup"></a>Configuration de Windows

Veillez à [installer](~/ios/get-started/installation/windows/index.md) les outils Xamarin sur votre ordinateur Windows.

### <a name="connecting-to-the-mac-build-host"></a>Connexion à l’hôte de build Mac

Il existe deux façons de se connecter à l’hôte de build Mac :

Dans la barre d’outils iOS :

[![La barre d’outils iOS](images/image1.png)](images/image1.png#lightbox)

Ou en accédant à **Outils > Options** dans Visual Studio, en sélectionnant **Xamarin > Paramètres iOS**, puis en cliquant sur le bouton **Rechercher Mac Agent Xamarin** :

[![Rechercher Mac Agent Xamarin](images/image2.png)](images/image2.png#lightbox)

Quel que soit le mode de navigation utilisé, la boîte de dialogue **Mac Agent**, illustrée ci-dessous, s’affiche :

[![La boîte de dialogue Mac Agent](images/image3.png)](images/image3.png#lightbox)

La liste alors affichée contient tous les ordinateurs qui ont déjà été connectés et qui sont stockés comme ordinateurs connus, ou bien ceux qui sont disponibles pour la *session à distance*.

Sélectionnez un Mac en double-cliquant dessus pour vous y connecter. La première fois que vous vous connectez à un Mac, vous êtes invité à entrer vos informations d’identification d’utilisateur Mac pour autoriser la session à distance :

[![Entrer les informations d’identification d’utilisateur Mac](images/image4.png)](images/image4.png#lightbox)

L’agent utilise ces informations d’identification pour créer une connexion SSH au Mac. En cas de réussite, une clé SSH est créée et est [inscrite](#commandline) dans le fichier `authorized_keys` sur ce Mac. Lors des connexions suivantes, l’agent utilise le nom d’utilisateur et le fichier de clé pour se connecter à l’hôte de build connu le plus récemment connecté.

> [!NOTE]
> Vous devez utiliser le _nom d’utilisateur_ et pas le _nom complet_ en entrant vos informations d’identification.  Pour le connaître, utilisez la commande `whoami` dans Terminal.  Par exemple, dans la capture d’écran ci-dessous, le nom du compte sera **amyb** et non **Amy Burns** :
>
> ![Recherche du nom d’utilisateur dans l’application Terminal](images/image5.png)

Quand une connexion a été établie avec succès, elle s’affiche dans la boîte de dialogue Sélection de l’hôte avec une icône de **connexion établie** en regard de celle-ci, comme illustré ci-dessous :

[![La boîte de dialogue Sélection de l’hôte avec une icône de connexion établie](images/image6.png)](images/image6.png#lightbox)

Il ne peut y avoir qu’un seul Mac connecté à la fois.

Chaque ordinateur de la liste, connecté ou non, affiche un menu contextuel quand vous cliquez dessus avec le bouton droit, vous permettant de vous **Connecter**, de vous **Déconnecter** ou d’**Oublier ce Mac** en fonction des besoins :

[![Les menus contextuels Connecter, Déconnecter ou Oublier ce Mac](images/image7.png)](images/image7.png#lightbox)

Si vous choisissez **Oublier ce Mac**, vous devrez entrer à nouveau vos informations d’identification pour vous y reconnecter.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>Ajout manuel d’un Mac

Dans certaines circonstances, vous souhaiterez peut-être ajouter manuellement un Mac si son nom mDNS n’est pas répertorié dans la boîte de dialogue Sélection de l’hôte. Pour ce faire, effectuez les étapes suivantes :

1. Recherchez l’adresse IP de votre Mac en accédant à **Préférences Système > Partage > Session à distance** sur votre Mac :

   [![L’adresse IP du Mac dans les préférences système](images/image8.png)](images/image8.png#lightbox)

   Si vous préférez utiliser la ligne de commande, vous trouverez votre adresse IP en entrant `ipconfig getifaddr en0` dans Terminal. (Notez qu’en fonction du type de connexion, la variable peut être `en1`, `en2`, etc.) :

   [![L’adresse IP dans l’application Terminal](images/image9.png)](images/image9.png#lightbox)

2. Retournez dans Visual Studio, puis dans la boîte de dialogue Sélection de l’hôte, sélectionnez **Ajouter un Mac...** :

   [![La boîte de dialogue Sélection de l’hôte](images/image10.png)](images/image10.png#lightbox)

3. Entrez l’adresse IP de votre Mac dans la boîte de dialogue Ajouter un Mac, puis cliquez sur **Ajouter** :

   [![Entrer l’adresse IP du Mac dans la boîte de dialogue Ajouter un Mac](images/image11.png)](images/image11.png#lightbox)

4. Enfin, entrez le nom d’utilisateur (et non pas le nom complet) de votre compte administrateur Mac et le mot de passe correspondant :

   [![Entrer le nom d’utilisateur et le mot de passe](images/image12.png)](images/image12.png#lightbox)

Une fois que vous cliquez sur **Connexion**, Visual Studio se connecte à l’ordinateur Mac à l’aide de SSH et ajoute ce Mac comme ordinateur connu.

<a name="commandline"/>

### <a name="command-line-support"></a>Prise en charge des lignes de commande

L’agent prend également en charge la génération d’une configuration Xamarin.iOS en ligne de commande.  Pour l’utiliser, vous devez passer les paramètres obligatoires suivants à MSBuild :

- `ServerAddress` : adresse IP du serveur Mac.

- `ServerUser` : nom d’utilisateur (et non pas le nom complet) à utiliser pour se connecter au serveur Mac.

- `ServerPassword` : mot de passe utilisé pour se connecter à l’hôte Mac (facultatif).

Le paramètre `ServerPassword` n’est pas obligatoire.

Au lieu de cela, la première fois qu’un mot de passe est passé à l’aide de Visual Studio ou de la ligne de commande pour cette configuration Windows, Mac et utilisateur particulière, une paire de clés est générée et stockée sur l’ordinateur Windows pour une utilisation future. Elle se trouve dans **%localappdata%\Xamarin\MonoTouch\id_rsa**.  Si vous ne passez pas le paramètre `ServerPassword`, le keyfile `id_rsa` est utilisé pour l’authentification.

Un exemple de commande permettant de se connecter au Mac 10.211.55.2 à l’aide du compte **xamUser** avec le mot de passe **mypassword** est illustré ci-dessous :

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>Récapitulatif

Cet article a exploré la connexion entre Visual Studio et les outils de build et du Concepteur iOS sur le Mac, vous permettant de générer des applications Xamarin.iOS à l’aide de Visual Studio.

### <a name="related-links"></a>Liens associés

- [Installation](~/ios/get-started/installation/windows/index.md)
- [Résolution des problèmes de connexion](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Connecter un Mac à un environnement Visual Studio avec XMA (vidéo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
