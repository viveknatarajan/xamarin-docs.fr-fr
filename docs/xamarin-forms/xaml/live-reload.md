---
title: Rechargement dynamique
description: Voir les modifications apportées à votre code XAML répercutées en temps réel, sans nécessiter de compilation d’un autre et déployez.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/08/2018
ms.openlocfilehash: 96054505af44c5d3e198c2b9e7e7cb30d39b02b1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-live-reload"></a>Rechargement dynamique de Xamarin

![Preview](~/media/shared/preview.png)

Vous permet de recharger de Live Xamarin **apporter des modifications à votre code XAML et ensuite répercutées en temps réel, sans nécessiter de compilation d’un autre et déployer**. Toutes les modifications apportées à votre code XAML seront redéployées sur Enregistrer et répercutées sur la cible de votre déploiement.

Étant donné que votre application est compilée lors de l’utilisation dynamique de rechargement, il fonctionne avec toutes les bibliothèques et des contrôles tiers. Dynamique fonctionne de rechargement sur toutes les plateformes Xamarin.Forms prend en charge, y compris Android, iOS et UWP et fonctionne sur toutes les cibles de déploiement valide, y compris des simulateurs, émulateurs et des périphériques physiques.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Rechargement dynamique est actuellement disponible uniquement dans Visual Studio 2017.

## <a name="requirements"></a>Spécifications

* [Visual Studio 2017 version 15.7 ou ci-dessus](https://www.visualstudio.com/vs/) ou version ultérieure avec le **développement pour appareils mobiles avec .NET** la charge de travail.
* [Xamarin.Forms 3.0.0 ou ci-dessus](https://www.nuget.org/packages/Xamarin.Forms/) ou version ultérieure.

## <a name="getting-started"></a>Prise en main
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Installer Xamarin les recharger en direct à partir de Visual Studio Marketplace

Rechargement de Live Xamarin est distribué via Visual Studio Marketplace. Pour installer l’extension, visitez le [page de rechargement de Live Xamarin dans Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) site Web, puis cliquez sur **télécharger**.

Ouvrez le .vsix qui est téléchargé, puis cliquez sur **installer**.

![Programme d’installation de Visual Studio confirmation de rechargement de Live Xamarin](images/LiveReloadVSIXInstall.png)

Ou bien, vous pouvez le rechercher dans le **Online** onglet dans le **Extensions et mises à jour** boîte de dialogue à l’intérieur de Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Configurer votre application pour utiliser le rechargement de Live

Ajout de rechargement de Live aux applications mobiles existantes peut être effectué en trois étapes :

1. Vérifiez tous les projets sont mis à jour pour utiliser [Xamarin.Forms 3.0.0 ou ci-dessus](https://www.nuget.org/packages/Xamarin.Forms/) ou version ultérieure.

2. Ajouter le **Xamarin.LiveReload** package NuGet :

    a. **.NET standard** : installer le **Xamarin.LiveReload** NuGet dans votre bibliothèque Standard de .NET 2.0. Cela n’a pas besoin d’être installé dans les projets de votre plateforme. Vérifiez que le **source du Package** a la valeur **tous les**.
    
    b. **Projets partagés** : installer le **Xamarin.LiveReload** NuGet dans tous les projets de plateforme (comme Android, iOS, UWP, etc.). Vérifiez que le **source du Package** a la valeur **tous les**.

![Ajouter Xamarin recharger dynamique NuGet avec le Gestionnaire de Package NuGet](images/addlivereloadnuget.png)

3. Ajouter `LiveReload.Init();` au constructeur dans le `Application` de classe, comme indiqué dans l’extrait de code suivant :

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Démarrer le rechargement dynamique

Compilez et déployez votre application. Une fois que l’application est déployée, ouvrez un fichier XAML, apporter des modifications et enregistrez le fichier. Vos modifications sont redéployées à la cible de déploiement.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Dynamique fonctionne de recharger avec les modifications apportées à n’importe quel fichier XAML. Modifications apportées au langage c# ou Ajout/Suppression de packages NuGet requiert une nouvelle build et déployez prenne effet.

## <a name="frequently-asked-questions"></a>Questions fréquemment posées 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Est le rechargement de Live Xamarin disponible dans Visual Studio pour Mac ? 

La version initiale de rechargement de Live Xamarin est uniquement disponible pour Visual Studio 2017. Prise en charge pour Visual Studio pour Mac est prévue pour une version ultérieure.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Cela fonctionne avec toutes les bibliothèques, telles que prisme ? 

Étant donné que votre application est compilée, Live de rechargement fonctionne avec toutes les bibliothèques, telles que prisme et bibliothèques de contrôle tiers, tels que Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity et d’autres fournisseurs de contrôle.

### <a name="what-changes-does-live-reload-redeploy"></a>Quelles modifications redéployer par Live le recharger ? 

Rechargement dynamique uniquement les modifications de code XAML ou CSS. Si vous apportez des modifications dans un fichier c#, une recompilation sera requise. Prise en charge de rechargement c# est prévue pour une version ultérieure.

### <a name="what-platforms-are-supported"></a>Quelles plateformes sont prises en charge ? 

Rechargement dynamique fonctionne sur n’importe quelle plateforme prise en charge par Xamarin.Forms, y compris la plateforme Windows universelle, iOS et Android.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Cela fonctionne sur les émulateurs et simulateurs appareils physiques ? 

Oui, le rechargement de Live fonctionne avec toutes les cibles de déploiement valide, y compris les émulateurs Android, iOS simulateurs et appareils physiques. Déploiement sur un périphérique exige que l’appareil et l’ordinateur sur le même réseau Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>Cela fonctionne avec les réseaux d’entreprise ?

Si vous déboguez un émulateur Android ou le simulateur iOS, Live de rechargement utilise localhost pour communiquer. Si vous souhaitez déployer sur un périphérique, l’appareil et l’ordinateur doivent se trouver sur le même réseau Wi-Fi. Dans les scénarios où cela n’est pas possible, vous pouvez [configurer votre propre serveur de rechargement de Live](#live-reload-server), ce qui vous permet de recharger Live, indépendamment des paramètres de connectivité réseau.

### <a name="does-it-require-debugging-the-app"></a>Nécessite l’application de débogage ? 

Non. En fait, vous pouvez même démarrer toutes les cibles votre application prise en charge (Android, iOS et UWP) sur n’importe quel nombre de périphériques ou les simulateurs/émulateurs et les afficher tous à la fois la mise à jour. 

## <a name="limitations"></a>Limitations

* Rechargement uniquement du code XAML est pris en charge.
* État de l’interface utilisateur ne peut pas être maintenu entre redéploiements ultérieurs, sauf si, à l’aide de MVVM.

## <a name="known-issues"></a>Problèmes connus

* Prise en charge uniquement dans Visual Studio.
* Recharger les ressources à l’échelle de l’application (par exemple, **App.xaml** ou partagé les dictionnaires de ressources), la réinitialisation de navigation de l’application. Cela sera résolu dans la prochaine version de la version préliminaire.
* Modification du langage XAML pendant le débogage UWP peut provoquer une panne du runtime. Solution : Utilisez **exécuter sans débogage (Ctrl + F5)** au lieu de **démarrer le débogage (F5)**.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-codes"></a>Codes d’erreur

* **XLR001**: *le projet actuel fait référence à la version du package NuGet de 'Xamarin.LiveReload' '[VERSION]', mais l’extension de rechargement de Live Xamarin requiert la version '[VERSION]'.*

  Pour permettre une itération rapide et l’évolution de la fonctionnalité de rechargement de Live, le package nuget et l’extension Visual Studio doivent correspondre exactement. Mettre à jour le package nuget vers la même version de l’extension que vous avez installé.

* **XLR002**: *Live de rechargement doit posséder au moins la propriété 'MqttHostname' lors de la génération à partir de la ligne de commande. Ou bien, la valeur 'EnableLiveReload' 'false' pour désactiver la fonctionnalité.*

  Les propriétés requises par Live de rechargement ne sont pas disponibles quand générer à partir de la ligne de commande (ou dans l’intégration continue) et doit par conséquent être fourni explicitement. 

* **XLR003**: *package nuget de rechargement de Live nécessite l’installation de l’extension Xamarin Live recharger Visual Studio.*

  A tenté de générer un projet qui référence le package nuget de rechargement de Live, mais l’Extension Visual n’est pas installée.  

### <a name="app-doesnt-connect"></a>Application ne connecte pas

Lorsque l’application est générée, les informations de **Outils > Options > Xamarin > recharger de Live** (clés nom, port et le chiffrement d’hôte) sont incorporées dans l’application, ainsi que quand `LiveReload.Init()` s’exécute, aucune correspondance ou la configuration est nécessaire pour la connexion réussisse.

Autres que des problèmes de mise en réseau normales (pare-feu, l’appareil sur un autre réseau), la principale raison de que l’application ne peut pas se connecter IDE est parce que sa configuration est différent de celui de Visual Studio. Cela peut se produire si :

* Application a été compilée sur un autre ordinateur.
* Application a été compilée et déployée dans une autre session de Visual Studio, et **générer automatiquement des clés de chiffrement** est activée (valeur par défaut) **Outils > Options > Xamarin > recharger de Live**.
* Paramétrage de Visual Studio ont été modifiés (par exemple, les clés de nom d’hôte, port ou le chiffrement), mais l’application n’a pas générée et déployé à nouveau.

Ces cas sont résolus par la génération et le déploiement de l’application à nouveau.

### <a name="uninstalling-preview-1"></a>Désinstaller la version préliminaire 1

Si vous avez un aperçu plus anciens et que vous rencontrez des problèmes de désinstaller, procédez comme suit :

1. Supprimez le dossier **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Remarque : Remplacez « Enterprise » de votre édition installée et « Aperçu » avec « 2017 » if vous installé dans un stable VS)
2. Ouvrir un **invite de commandes développeur** pour que Visual Studio et les exécuter `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Conseils et astuces

* Tant que vous ne modifiez pas les paramètres de rechargement de Live (y compris les clés de chiffrement, par exemple si vous désactivez **générer automatiquement des clés de chiffrement**) et que vous générez à partir de la même machine, vous n’avez pas besoin générer et déployer l’application après le premier déployer, sauf si vous modifiez le code ou les dépendances. Vous pouvez simplement lancer à nouveau une application déjà déployée, et il se connecte au dernier hôte utilisé.

* Il n’existe aucune limitation sur le nombre de périphériques vous pouvez vous connecter à la même session de Visual Studio. Vous pouvez déployer et démarrer l’application dans les appareils/simulateurs autant que nécessaire pour afficher le travail de rechargement dynamique sur chacun d'entre eux en même temps.

* Rechargement dynamique recharge uniquement la partie interface utilisateur de votre application, mais il ne *pas* recréer vos pages, ni n’il remplace votre modèle d’affichage (ou le contexte de liaison). Cela signifie que la *ensemble* état de l’application est toujours conservé au-delà des rechargements, y compris vos dépendances injectées.

## <a name="live-reload-server"></a>Serveur de rechargement dynamique

Dans les scénarios où une connexion à partir de l’application en cours d’exécution sur votre ordinateur (comme indiqué à l’aide de `localhost` ou `127.0.0.1` dans **Outils > Options > Xamarin > recharger de Live**) n’est pas possible (pare-feux, réseaux différents), Vous pouvez configurer un serveur distant à la place, ce qui seront de l’IDE et l’application pour établir la connexion.

Rechargement dynamique utilise la norme [protocole MQTT](http://mqtt.org/) pour échanger des messages et peut donc communiquer avec [des serveurs tiers](https://github.com/mqtt/mqtt.github.io/wiki/servers). Il existe même [serveurs publics](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (également appelé *courtiers*) que vous pouvez utiliser. Rechargement dynamique a été testée avec `broker.hivemq.com` et `iot.eclipse.org` les noms d’hôte, ainsi que les services fournis par [www.cloudmqtt.com](https://www.cloudmqtt.com) et [www.cloudamqp.com](https://www.cloudamqp.com). Vous pouvez également déployer votre propre serveur MQTT dans le cloud, tels que [HiveMQ sur Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Vous pouvez configurer n’importe quel port, mais il est courant d’utiliser le port 1883 par défaut pour les serveurs distants. Live recharger les messages utilisent bout en bout AES symétrique un chiffrement renforcé, par conséquent, il est prudent de se connecter aux serveurs distants. Par défaut, la clé de chiffrement et le vecteur d’initialisation (IV) sont régénérées sur chaque session de Visual Studio.

Probablement le plus simple consiste à installer le [mosquitto](https://mosquitto.org) serveur dans un VM Ubuntu vide dans Azure :

1. Créer une nouvelle machine virtuelle Ubuntu Server dans le portail Azure
2. Ajouter une nouvelle règle de port entrant pour 1883 (port MQTT par défaut) dans l’onglet mise en réseau
3. Ouvrez le [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash mode)
4. Tapez `ssh [USERNAME]@[PUBLIC_IP]` à l’aide du nom d’utilisateur que vous avez choisi de 1) et l’adresse IP publique indiqué dans votre page de vue d’ensemble de machine virtuelle
5. Exécutez `sudo apt-get install mosquitto`, entrez le mot de passe que vous avez choisi de 1)

Maintenant, vous pouvez utiliser cette IP pour se connecter à votre propre serveur MQTT.
