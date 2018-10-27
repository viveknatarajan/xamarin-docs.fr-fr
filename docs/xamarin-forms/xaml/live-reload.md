---
title: Rechargement en direct de Xamarin (version préliminaire)
description: Voir les modifications apportées à votre XAML répercutées en temps réel, sans nécessiter une autre compilation et déployez.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
robots: noindex
ms.date: 10/26/2018
ms.openlocfilehash: bb30f9264cb5f3b762088e3523223b2bff8416b9
ms.sourcegitcommit: ebc0b3838836ee5cf9e54d58866d23916664a43d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142334"
---
# <a name="xamarin-live-reload-preview"></a>Rechargement en direct de Xamarin (version préliminaire)

> [!NOTE]
> La version préliminaire de Xamarin Live recharger s’est terminée, et nous souhaitons remercier tout le monde de vos commentaires. Veuillez lire notre [feuille de route](https://docs.microsoft.com/en-us/visualstudio/productinfo/vs-roadmap) pour Visual Studio 2019 pour en savoir plus sur les nouvelles fonctionnalités de productivité que nous travaillons pour Xamarin.Forms. Cette extension reste disponible pour Visual Studio 2017, mais vous ne recevrez pas les mises à jour futures.

Vous permet de Xamarin Live recharger **apporter des modifications à votre XAML et mis à jour en temps réel, sans nécessiter une autre compilation et déployer**. Toute modification apportée à votre XAML sera redéployée sur Enregistrer et répercutées sur votre cible de déploiement.

## <a name="requirements"></a>Configuration requise

* [Visual Studio 2017 version 15.7 ou version ultérieure](https://visualstudio.microsoft.com/vs/) avec la **développement Mobile en .NET** charge de travail.
* [Xamarin.Forms 3.0.0 ou version ultérieure](https://www.nuget.org/packages/Xamarin.Forms/).

## <a name="getting-started"></a>Prise en main
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Installer Xamarin le rechargement en direct à partir de la place de marché Visual Studio

Xamarin Live recharger est distribué via la place de marché Visual Studio. Pour installer l’extension, visitez le [page Xamarin Live recharger sur la place de marché Visual Studio](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) site Web et cliquez sur **télécharger**.

Ouvrez le fichier .vsix qui est téléchargé, puis cliquez sur **installer**.

![Programme d’installation de Visual Studio confirmation de rechargement en direct de Xamarin](images/LiveReloadVSIXInstall.png)

Vous pouvez également rechercher dans le **Online** onglet dans le **Extensions et mises à jour** boîte de dialogue à l’intérieur de Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Configurer votre application pour utiliser le rechargement en direct

Ajout de rechargement en direct pour les applications mobiles existantes est possible en trois étapes :

1. Vérifiez tous les projets sont mis à jour pour utiliser [Xamarin.Forms 3.0.0 ou version ultérieure](https://www.nuget.org/packages/Xamarin.Forms/) ou version ultérieure.

2. Ajouter le **Xamarin.LiveReload** package NuGet :

    a. **.NET standard** : installer le **Xamarin.LiveReload** NuGet dans votre bibliothèque .NET Standard 2.0. Il ne doit pas être installé dans vos projets de plateforme. Vérifiez que le **source du Package** a la valeur **tous les**.
    
    b. **Projets partagés** : installer le **Xamarin.LiveReload** NuGet dans tous les projets de plateforme (par exemple Android, iOS, UWP, etc.). Vérifiez que le **source du Package** a la valeur **tous les**.

    [![Ajouter un NuGet de rechargement en direct de Xamarin avec le Gestionnaire de Package NuGet](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Ajouter `LiveReload.Init();` au constructeur dans le `Application` classe, comme indiqué dans l’extrait de code suivant :

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Démarrer le rechargement en direct

Compilez et déployez votre application. Une fois que l’application est déployée, ouvrir un fichier XAML, apporter des modifications et enregistrez le fichier. Vos modifications sont redéployées à la cible de déploiement.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live fonctionne de recharger avec les modifications apportées à n’importe quel fichier XAML. Modifications apportées à c# ou Ajout/Suppression de packages NuGet nécessite une nouvelle build et déployez prenne effet.

## <a name="frequently-asked-questions"></a>Questions fréquemment posées 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Est Xamarin Live recharger disponibles dans Visual Studio pour Mac ? 

Non, la version préliminaire de Xamarin Live recharger est uniquement disponible pour Visual Studio 2017.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Cela fonctionne avec toutes les bibliothèques, telles que Prism ? 

Étant donné que votre application est compilée, rechargement en direct fonctionne avec toutes les bibliothèques, telles que Prism et les bibliothèques de contrôle tiers, tels que Telerik Infragistics, Syncfusion, ArcGIS, GrapeCity et autres fournisseurs de contrôle.

### <a name="what-changes-does-live-reload-redeploy"></a>Quelles modifications redéployer pour le rechargement en direct ? 

Le rechargement en direct s’applique uniquement les modifications apportées à XAML ou CSS. Si vous apportez des modifications dans un fichier c#, une recompilation sera nécessaire. 

### <a name="what-platforms-are-supported"></a>Quelles plateformes sont prises en charge ? 

Le rechargement en direct fonctionne sur n’importe quelle plateforme prise en charge par Xamarin.Forms, y compris Android, iOS et UWP.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Cela fonctionne-t-il sur des appareils physiques, des simulateurs et des émulateurs ? 

Oui, le rechargement en direct fonctionne avec toutes les cibles de déploiement valide, y compris les émulateurs Android, iOS simulateurs et appareils physiques. Déploiement sur un appareil nécessite l’appareil et l’ordinateur sur le même réseau Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>Cela fonctionne-t-il avec les réseaux d’entreprise ?

Si vous effectuez un débogage sur un émulateur Android ou d’un simulateur iOS, rechargement en direct utilise localhost pour communiquer. Si vous souhaitez déployer sur un appareil, l’appareil et l’ordinateur doivent être sur le même réseau Wi-Fi. Dans les scénarios où cela n’est pas possible, vous pouvez [configurer votre propre serveur de rechargement en direct](#live-reload-server), ce qui vous permet de recharger Live, indépendamment des paramètres de connectivité réseau.

### <a name="does-it-require-debugging-the-app"></a>Nécessite le débogage de l’application ? 

Non. En fait, vous pouvez même démarrer toutes les cibles votre application pris en charge (Android, iOS et UWP) sur n’importe quel nombre de périphériques ou les simulateurs/émulateurs et les afficher tous les mettre à jour à la fois. 

## <a name="limitations"></a>Limitations

* Rechargement uniquement du XAML est pris en charge.
* État de l’interface utilisateur ne peut pas être maintenu entre redéploiements, à moins d’utiliser MVVM.

## <a name="known-issues"></a>Problèmes connus

* Uniquement pris en charge dans Visual Studio.
* Liaison doit être définie sur **ne pas lier** ou **lien les SDK Framework uniquement** 
* Recharger les ressources à l’échelle de l’application (par exemple, **App.xaml** ou partagé les dictionnaires de ressources), la réinitialisation de navigation de l’application. 
* Rechargement de ContentView actuellement nécessite de recharger la page conteneur.
* Les éléments contenant AutomationId peuvent provoquer un échec de rechargement.
* La modification XAML pendant le débogage UWP peut entraîner un blocage de l’exécution. Solution de contournement : Utilisez **exécuter sans débogage (Ctrl + F5)** au lieu de **démarrer le débogage (F5)**.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-codes"></a>Codes d’erreur

* **XLR001**: *le projet actuel fait référence à 'Xamarin.LiveReload' NuGet package version « [VERSION] », mais l’extension Xamarin Live recharger requiert la version « [VERSION] ».*

  Afin de permettre une itération rapide et l’évolution de la fonctionnalité de rechargement en direct, le package nuget et l’extension de Visual Studio doivent correspondre exactement. Mettre à jour votre package nuget pour la même version de l’extension que vous avez installé.

* **XLR002**: *rechargement en direct exige au moins que la propriété « MqttHostname » lors de la génération à partir de la ligne de commande. Également définir 'EnableLiveReload' à 'false' pour désactiver la fonctionnalité.*

  Les propriétés requises par le rechargement en direct ne sont pas disponibles quand générer à partir de la ligne de commande (ou dans l’intégration continue) et doit par conséquent être fourni explicitement. 

* **XLR003**: *package nuget de rechargement en direct nécessite l’installation de l’extension Xamarin Live recharger Visual Studio.*

  A tenté de générer un projet qui référence le package nuget de rechargement en direct, mais l’Extension Visual n’est pas installée.  

* *Exception lors du chargement des assemblys : System.IO.FileNotFoundException : Impossible de charger l’assembly ' Xamarin.Live.Reload, Version = 0.3.27.0, Culture = neutral, PublicKeyToken ='.*

  Le projet d’hôte doit être à l’aide de `PackageReference` au lieu de `packages.config`

### <a name="app-doesnt-connect"></a>Application ne se connecte.

Quand l’application est générée, les informations à partir de **Outils > Options > Xamarin > rechargement en direct** (clés nom, port et le chiffrement d’hôte) sont incorporés dans l’application, ainsi qu’au moment où `LiveReload.Init()` s’exécute, aucune association ou la configuration est nécessaire pour la connexion réussisse.

Autre que normal des problèmes de réseau (pare-feu, périphérique sur un réseau différent), la principale raison de que l’application ne peut pas se connecter IDE est parce que sa configuration est différente de celle de Visual Studio. Cela peut se produire si :

* Application a été compilée sur un autre ordinateur.
* Application a été compilée et déployée dans une autre session de Visual Studio, et **générer automatiquement des clés de chiffrement** est activée (valeur par défaut) **Outils > Options > Xamarin > recharger Live**.
* Visual Studio ont été changées (autrement dit, les clés de nom d’hôte, port ou le chiffrement), mais l’application n’a pas créée et déployé à nouveau.

Ces cas sont résolus par la création et déploiement de l’application à nouveau.

### <a name="uninstalling-preview-1"></a>Désinstallation de Preview 1

Si vous avez un aperçu plus anciens, et vous rencontrez des problèmes de sa désinstallation, procédez comme suit :

1. Supprimez le dossier **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Remarque : Remplacez « Enterprise » par votre édition installée et « Aperçu » avec « 2017 » if vous installé à un Visual Studio stable)
2. Ouvrir un **invite de commandes développeur** pour que Visual Studio et l’exécution `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Trucs et astuces

* Tant que vous ne modifiez pas les paramètres de rechargement en direct (y compris les clés de chiffrement, par exemple si vous désactivez **générer automatiquement des clés de chiffrement**) et que vous générez à partir de la même machine, vous n’avez pas besoin générer et déployer l’application après le premier déployer, sauf si vous modifiez le code ou les dépendances. Vous pouvez simplement lancer à nouveau une application déjà déployée, et il se connectera au dernier hôte utilisé.

* Il n’existe aucune limitation sur combien d’appareils vous pouvez vous connecter à la même session de Visual Studio. Vous pouvez déployer et démarrer l’application dans des appareils/simulateurs autant que nécessaire pour afficher le travail de rechargement en direct sur chacun d'entre eux en même temps.

* Le rechargement en direct recharge uniquement la partie d’interface utilisateur de votre application, mais il ne *pas* recréer vos pages, ni n’il remplace votre modèle de vue (ou le contexte de liaison). Cela signifie que le *ensemble* état de l’application est toujours conservé au-delà des rechargements, y compris vos dépendances injectés.

## <a name="live-reload-server"></a>Serveur de rechargement en direct

Dans les scénarios où une connexion à partir de l’application en cours d’exécution sur votre ordinateur (comme indiqué à l’aide de `localhost` ou `127.0.0.1` dans **Outils > Options > Xamarin > recharger Live**) n’est pas possible (par exemple, les pare-feux, réseaux différents), Vous pouvez configurer un serveur distant au lieu de cela, ce qui seront de l’IDE et l’application pour établir la connexion.

Le rechargement en direct utilise la norme [protocole MQTT](http://mqtt.org/) pour échanger des messages et peut donc communiquer avec [des serveurs tiers](https://github.com/mqtt/mqtt.github.io/wiki/servers). Il existe même [serveurs publics](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (également appelé *courtiers*) que vous pouvez utiliser. Le rechargement en direct a été testé avec `broker.hivemq.com` et `iot.eclipse.org` des noms d’hôte, ainsi que les services fournis par [www.cloudmqtt.com](https://www.cloudmqtt.com) et [www.cloudamqp.com](https://www.cloudamqp.com). Vous pouvez également déployer votre propre serveur MQTT dans le cloud, tels que [HiveMQ sur Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Vous pouvez configurer n’importe quel port, mais il est courant d’utiliser le port 1883 des serveurs distants. Live recharger les messages utilisent end-to-end AES symétrique cryptage renforcé, qu’il s’agisse de se connecter aux serveurs distants. Par défaut, la clé de chiffrement et le vecteur d’initialisation (IV) sont régénérées sur chaque session de Visual Studio.

Probablement le moyen le plus simple consiste à installer le [mosquitto](https://mosquitto.org) server dans une VM Ubuntu vide dans Azure :

1. Créer une nouvelle machine virtuelle Ubuntu Server dans le portail Azure
2. Ajouter une nouvelle règle de port entrant pour 1883 (port MQTT par défaut) dans l’onglet mise en réseau
3. Ouvrez le [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash mode)
4. Tapez `ssh [USERNAME]@[PUBLIC_IP]` à l’aide du nom d’utilisateur que vous avez choisi dans 1) et l’adresse IP publique, indiqué dans votre page de vue d’ensemble de machine virtuelle
5. Exécutez `sudo apt-get install mosquitto`, entrez le mot de passe que vous avez choisi dans 1)

Vous pouvez maintenant utiliser cette adresse IP pour se connecter à votre propre serveur MQTT.
