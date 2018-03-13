---
title: HomeKit
description: "HomeKit est un framework d’Apple pour le contrôle des appareils de l’automation de base. Cet article présente HomeKit et couvre les accessoires de test de configuration dans le simulateur d’accessoire HomeKit et l’écriture d’une simple application Xamarin.iOS pour interagir avec ces Accessoires."
ms.topic: article
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: ea51dc2c7dadc5cc430df990c9ce79eac6e941da
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit est un framework d’Apple pour le contrôle des appareils de l’automation de base. Cet article présente HomeKit et couvre les accessoires de test de configuration dans le simulateur d’accessoire HomeKit et l’écriture d’une simple application Xamarin.iOS pour interagir avec ces Accessoires._

[![](homekit-images/accessory01.png "Un exemple HomeKit activé l’application")](homekit-images/accessory01.png#lightbox)

Apple a introduit le HomeKit dans iOS 8 comme un moyen d’intégrer en toute transparence de plusieurs appareils automation de base à partir d’un fournisseurs différents en une unité unique et cohérente. En promouvant un protocole commun pour la détection, configurer et contrôler les appareils automation de base, HomeKit permet aux appareils de fournisseurs non lié fonctionnent ensemble, tout cela sans avoir à coordonner les efforts de fournisseurs individuels.

Avec HomeKit, vous pouvez créer une application Xamarin.iOS qui contrôle n’importe quel appareil HomeKit activé sans l’aide du fournisseur fourni API ou des applications. HomeKit, vous pouvez effectuer les opérations suivantes :

- Détecter les nouveaux appareils d’automatisation domestique HomeKit activé et les ajouter à une base de données qui rend persistantes sur l’ensemble des appareils iOS de l’utilisateur.
- Le programme d’installation, configurer, afficher et contrôler n’importe quel appareil dans le HomeKit _base de données de Configuration accueil_.
- Communiquer avec n’importe quel appareil HomeKit préconfiguré et les commandes pour effectuer des actions individuelles ou fonctionner conjointement, telles que l’activation sur tous les témoins lumineux de la cuisine.

Outre leur utilisation des appareils dans la base de Configuration accueil pour les applications HomeKit activé, HomeKit donne accès aux commandes vocales de Siri. Étant donné une installation HomeKit correctement configurée, l’utilisateur peut émettre les commandes vocales tels que « Siri, activer les témoins lumineux du salon. »

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>La base de données de Configuration de base

HomeKit organise tous les appareils d’automation dans un emplacement donné dans une Collection d’accueil. Cette collection fournit un moyen de l’utilisateur regrouper leurs appareils domestique automation en unités logiquement réorganisées avec étiquettes explicites, contrôlable de visu.

La Collection accueil est stockée dans une base de Configuration accueil sera automatiquement sauvegardées et synchronisés sur tous les appareils iOS de l’utilisateur. HomeKit fournit les classes suivantes pour travailler avec la base de données accueil Configuration :

- `HMHome` -C’est le conteneur de niveau supérieur qui contient toutes les configurations pour tous les périphériques automation de base et les informations dans un seul emplacement physique (par exemple). une seule famille résidence). L’utilisateur peut avoir plusieurs résidence, telles que leur accueil principale et une maison de vacances. Ou qu’ils peuvent avoir différentes « héberge » sur la même propriété, telles que la maison principale et une maison invité sur le garage. Dans les deux cas, au moins un `HMHome` objet _doit_ être le programme d’installation et stockées avant toutes les autres informations HomeKit peuvent être entrées.
- `HMRoom` Bien que facultative, un `HMRoom` permet à l’utilisateur de définir des salles spécifiques à l’intérieur d’une maison (`HMHome`) tel que : cuisine, salle de bain, Garage ou domestiques. L’utilisateur peut grouper tous les périphériques automation de base dans un emplacement spécifique de maison dans un `HMRoom` et les exécuter en tant qu’unité. Par exemple, vous demandant Siri à éteindre la lumière garage.
- `HMAccessory` -Il s’agit d’un individu, HomeKit physique activé dispositif d’automatisation qui a été installé sur le lieu de résidence de l’utilisateur (par exemple, un thermostat intelligent). Chaque `HMAccessory` est affectée à un `HMRoom`. Si l’utilisateur n’a pas configuré les salles, HomeKit affecte accessoires à une salle par défaut spéciale.
- `HMService` -Représente un service fourni par une donnée `HMAccessory`, telles que l’état activé/désactivé d’un éclairage ou sa couleur (si le changement de couleur est pris en charge). Chaque `HMAccessory` peut avoir plus d’un service, par exemple une porte de garage qui inclut également un éclairage. En outre, une donnée `HMAccessory` peut avoir des services, tels que de la mise à jour du microprogramme, qui sont en dehors du contrôle utilisateur.
- `HMZone` -Permet à l’utilisateur regrouper un ensemble de `HMRoom` des objets dans des zones logiques, telles qu’attirail, Downstairs ou sous-sol. Bien que facultative, ainsi, pour les interactions comme demander Siri pour activer la lumière downstairs hors tension.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Configuration d’une application HomeKit

En raison des exigences de sécurité imposées par HomeKit, une application Xamarin.iOS qui utilise l’infrastructure HomeKit doit être correctement configurée dans les deux le portail des développeurs Apple et dans le fichier de projet Xamarin.iOS.

Effectuez ce qui suit :

1. Connectez-vous à la [portail des développeurs Apple](http://developer.apple.com).
2. Cliquez sur **certificats, les identificateurs et les profils**.
3. Si vous n’avez pas déjà fait, cliquez sur **identificateurs** et créer un ID de votre application (par exemple, `com.company.appname`), sinon modifier votre code existant.
4. Vérifiez que le **HomeKit** service a été activé pour l’ID donné : 

    [![](homekit-images/provision01.png "Activer le service HomeKit pour l’ID donné")](homekit-images/provision01.png#lightbox)
5. Enregistrez les modifications apportées.
4. Cliquez sur **profils de configuration** > **développement** et créer un nouveau développement, profil de configuration pour votre application : 

    [![](homekit-images/provision02.png "Créer un nouveau développement, profil de configuration pour l’application")](homekit-images/provision02.png#lightbox)
5. Télécharger et installer le profil de configuration ou utiliser Xcode pour télécharger et installer le profil.
6. Modifier les options de votre projet de Xamarin.iOS et vérifiez que vous utilisez le profil de configuration que vous venez de créer : 

    [![](homekit-images/provision03.png "Sélectionnez le profil de configuration venez de créer")](homekit-images/provision03.png#lightbox)
7. Ensuite, modifiez votre **Info.plist** de fichier et vérifiez que vous utilisez l’ID d’application qui a été utilisé pour créer le profil de configuration : 

    [![](homekit-images/provision04.png "Définir l’ID d’application ")](homekit-images/provision04.png#lightbox)
8. Enfin, modifiez votre **Entitlements.plist** de fichier et vérifiez que le **HomeKit** droits a été sélectionné : 

    [![](homekit-images/provision05.png "Activer les droits HomeKit")](homekit-images/provision05.png#lightbox)
9. Enregistrez les modifications à tous les fichiers.

Avec ces paramètres en place, l’application est maintenant prête à accéder aux API HomeKit Framework. Pour plus d’informations sur la configuration, consultez notre [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) et [de configuration de votre application](~/ios/get-started/installation/device-provisioning/index.md) guides.

> [!IMPORTANT]
> **Remarque :** test d’une application HomeKit activé requiert un appareil iOS réel qui a été configuré correctement pour le développement. HomeKit ne peut pas être testée dans le simulateur iOS.

## <a name="the-homekit-accessory-simulator"></a>Le simulateur HomeKit accessoire

Pour fournir un moyen de tester tous les périphériques d’automatisation domestique possibles et les services, sans devoir disposer d’une unité physique, Apple créé le _HomeKit accessoire simulateur_. À l’aide de ce simulateur, vous pouvez le programme d’installation et configurer des périphériques HomeKit virtuels.

### <a name="installing-the-simulator"></a>L’installation du simulateur

Apple offre le simulateur d’accessoire HomeKit en tant que téléchargement séparé dans Xcode, vous devez installer avant de continuer.

Effectuez ce qui suit :

1. Dans un navigateur web, visitez [téléchargements pour les développeurs d’Apple](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Téléchargez le **des outils supplémentaires pour Xcode xxx** (où xxx est la version de Xcode que vous avez installée) : 

    [![](homekit-images/simulator01.png "Téléchargez les outils supplémentaires pour Xcode")](homekit-images/simulator01.png#lightbox)
3. Ouvrir l’image de disque et installez les outils dans votre **Applications** active.

Avec le simulateur d’accessoire HomeKit installé, Accessoires virtuels peuvent être créés pour le test.

### <a name="creating-virtual-accessories"></a>Création d’accessoires virtuels

Pour démarrer le simulateur d’accessoire HomeKit et créer quelques accessoires virtuels, procédez comme suit :

1. Dans le dossier Applications, démarrer le simulateur d’accessoire HomeKit : 

    [![](homekit-images/simulator02.png "Le simulateur HomeKit accessoire")](homekit-images/simulator02.png#lightbox)
2. Cliquez sur le  **+**  sélectionnez **nouvel accessoire...** : 

    [![](homekit-images/simulator03.png "Ajouter un nouvel accessoire")](homekit-images/simulator03.png#lightbox)
3. Entrez les informations sur le nouvel accessoire et cliquez sur le **Terminer** bouton : 

    [![](homekit-images/simulator04.png "Renseignez les informations sur le nouvel accessoire")](homekit-images/simulator04.png#lightbox)
4. Cliquez sur le **ajouter Service...** bouton et sélectionnez un type de service dans la liste déroulante : 

    [![](homekit-images/simulator05.png "Sélectionnez un type de service dans la liste déroulante")](homekit-images/simulator05.png#lightbox)
5. Fournir un **nom** pour le service et cliquez sur le **Terminer** bouton : 

    [![](homekit-images/simulator06.png "Entrez un nom pour le service")](homekit-images/simulator06.png#lightbox)
6. Vous pouvez fournir des caractéristiques facultatives pour un service en cliquant sur le **ajouter une caractéristique** bouton et la configuration des paramètres requis : 

    [![](homekit-images/simulator07.png "Configuration des paramètres requis")](homekit-images/simulator07.png#lightbox)
7. Répétez les étapes ci-dessus pour créer l’un de chaque type d’appareil virtuel automation de base qui HomeKit prend en charge.

Avec certains exemples virtuel HomeKit accessoires créé et configuré, vous pouvez maintenant utiliser et contrôler ces périphériques à partir de votre application Xamarin.iOS.

## <a name="configuring-the-infoplist-file"></a>Configuration du fichier Info.plist

Nouveau pour iOS 10 (et version ultérieure), le développeur doit ajouter la `NSHomeKitUsageDescription` clé pour l’application `Info.plist` de fichier et fournir une chaîne de déclaration pourquoi l’application souhaite accéder à la base de données de l’utilisateur HomeKit. Cette chaîne s’affiche à l’heure de l’utilisateur à la première qu'exécution de l’application :

[![](homekit-images/info01.png "La boîte de dialogue Autorisations HomeKit")](homekit-images/info01.png#lightbox)

Pour définir cette clé, procédez comme suit :

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Au bas de l’écran, basculez vers le **Source** vue.
3. Ajouter un nouveau **entrée** à la liste.
4. Dans la liste déroulante, sélectionnez **confidentialité - Description de l’utilisation de HomeKit**: 

    [![](homekit-images/info02.png "Sélectionnez confidentialité - Description de l’utilisation de HomeKit")](homekit-images/info02.png#lightbox)
5. Entrez une description pour la raison pour laquelle l’application souhaite accéder à la base de données de l’utilisateur HomeKit : 

    [![](homekit-images/info03.png "Entrez une description")](homekit-images/info03.png#lightbox)
6. Enregistrez les modifications dans le fichier.

> [!IMPORTANT]
> **Remarque :** Échec de définition de la `NSHomeKitUsageDescription` clé dans le `Info.plist` fichier entraîne l’application _en mode silencieux défaillant_ (qui est fermé par le système lors de l’exécution) sans erreur lorsque exécutez dans iOS (ou supérieure à 10).

## <a name="connecting-to-homekit"></a>Connexion à HomeKit

Pour communiquer avec HomeKit, votre application Xamarin.iOS doit tout d’abord instancier une instance de la `HMHomeManager` classe. Le Gestionnaire d’accueil est le point d’entrée central dans HomeKit et est chargé de fournir une liste de foyers disponibles, la mise à jour et en conservant cette liste et retour de l’utilisateur _principal accueil_.

Le `HMHome` objet contient toutes les informations concernant une maison donnent y compris les salles, les groupes ou les zones qui peuvent contenir, ainsi que tous les accessoires automation de base qui ont été installés. Avant de pouvoir effectuer des opérations dans HomeKit, au moins un `HMHome` doit être créé et affecté en tant que la page d’accueil principale.

Votre application est chargée de vérifier si une maison principal existe et créer et affecter si elle n’est pas le cas.

### <a name="adding-a-home-manager"></a>Ajout d’un gestionnaire de base

Pour ajouter la reconnaissance de HomeKit à une application Xamarin.iOS, modifiez le **AppDelegate.cs** fichier pour le modifier et de rendre l’aspect suivant :

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

Lors de la première exécution de l’application, l’utilisateur sera invité s’ils souhaitent pouvoir accéder à leurs informations HomeKit :

[![](homekit-images/home01.png "L’utilisateur invité s’ils souhaitent pouvoir accéder à leurs informations HomeKit")](homekit-images/home01.png#lightbox)

Si l’utilisateur répond **OK**, l’application sera alors en mesure de travailler avec leurs accessoires HomeKit sinon il ne sera pas et tous les appels à HomeKit échoue avec une erreur.

Avec le gestionnaire accueil en place, l’application devez ensuite voir si une maison principal a été configurée, dans le cas contraire, offrent un moyen de l’utilisateur créer et affecter une.

### <a name="accessing-the-primary-home"></a>L’accès à la page d’accueil principale

Comme indiqué ci-dessus, une maison principal doit être créée et configurée avant HomeKit est disponible et elle est la responsabilité de l’application pour fournir un moyen de l’utilisateur créer et affecter une maison principal s’il n’existe pas déjà.

Lorsque votre application démarre tout d’abord ou renvoie à partir de l’arrière-plan, il doit surveiller le `DidUpdateHomes` l’événement de la `HMHomeManager` classe pour vérifier l’existence d’une maison principal. S’il n’existe pas, elle doit fournir une interface pour l’utilisateur pour en créer un.

Le code suivant peut être ajouté à un contrôleur de vue à vérifier pour la page d’accueil principale :

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Lorsque le Gestionnaire d’accueil établit une connexion vers HomeKit, le `DidUpdateHomes` événement est déclenché, n’importe quel particuliers existants seront chargés à la collection du Gestionnaire de domicile et la page d’accueil principale sera chargé, s’il est disponible.

### <a name="adding-a-primary-home"></a>Ajout d’une maison principal

Si le `PrimaryHome` propriété de la `HMHomeManager` est `null` après un `DidUpdateHomes` événement, vous devez fournir un moyen de l’utilisateur créer et affecter une maison principal avant de continuer.

En général, l’application présente un formulaire de l’utilisateur nommer un nouveau portail, ce qui est ensuite passé au Gestionnaire de base pour le programme d’installation en tant que la page d’accueil principale. Pour le **HomeKitIntro** exemple d’application, un affichage modal a été créé dans le concepteur IOS et appelé le `AddHomeSegue` segue à partir de l’interface principale de l’application.

Il fournit un champ de texte pour l’utilisateur à entrer un nom pour la nouvelle page d’accueil et un bouton pour ajouter la page d’accueil. Lorsque l’utilisateur appuie sur le **ajouter accueil** bouton, le code suivant appelle le Gestionnaire de base pour ajouter la page d’accueil :

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

Le `AddHome` méthode va tenter de créer le nouveau portail et le renvoyer à la routine de rappel donné. Si le `error` propriété n’est pas `null`, une erreur s’est produite et il doit être présenté à l’utilisateur. Erreurs les plus courantes sont dus à un nom de base non unique ou le Gestionnaire de base n’est ne pas en mesure de communiquer avec HomeKit.

Si la page d’accueil a été créé avec succès, vous devez appeler la `UpdatePrimaryHome` pour définir la nouvelle page d’accueil en tant que la page d’accueil principale. Là encore, si le `error` propriété n’est pas `null`, une erreur s’est produite et il doit être présenté à l’utilisateur.

Vous devez aussi surveiller l’accueil du Gestionnaire de `DidAddHome` et `DidRemoveHome` événements et la mise à jour de l’interface utilisateur de l’application en fonction des besoins.

> [!IMPORTANT]
> **Remarque :** le `AlertView.PresentOKAlert` méthode utilisée dans l’exemple de code ci-dessus est une classe d’assistance dans l’application HomeKitIntro qui permet de travailler avec iOS alertes plus facile.


## <a name="finding-new-accessories"></a>Recherche de nouveaux accessoires

Une fois qu’une maison principal a été définie ou chargée à partir du Gestionnaire de base, votre application Xamarin.iOS peut appeler le `HMAccessoryBrowser` pour rechercher les nouveaux accessoires automation de base et de les ajouter à une page d’accueil.

Appelez le `StartSearchingForNewAccessories` méthode pour commencer la recherche de nouveaux accessoires et les `StopSearchingForNewAccessories` méthode une fois.

> [!IMPORTANT]
> **Remarque :** `StartSearchingForNewAccessories` ne doit pas rester en cours d’exécution pendant de longues périodes de temps, car cela affectera négativement autonomie et performances de l’appareil iOS. Apple suggère appel `StopSearchingForNewAccessories` après une minute ou recherche uniquement lorsque l’interface de l’utilisateur de trouver d’accessoire est présentée à l’utilisateur.

Le `DidFindNewAccessory` événement sera appelé lorsque de nouveaux accessoires sont découverts et ils seront ajoutés à la `DiscoveredAccessories` liste dans le navigateur accessoire.

Le `DiscoveredAccessories` liste contient une collection de `HMAccessory` les objets qui définissent un donnent HomeKit activés dispositif d’automatisation de base et de ses services disponibles, telles que les témoins lumineux ou contrôle de la porte de garage.

Une fois que le nouvel accessoire a été trouvé, il doit être présenté à l’utilisateur et par conséquent, ils peuvent sélectionner et l’ajouter à une maison. Exemple :

[![](homekit-images/accessory01.png "Recherche d’un nouvel accessoire")](homekit-images/accessory01.png#lightbox)

Appelez le `AddAccessory` méthode pour ajouter l’accesseur sélectionné à la collection de la page d’accueil. Exemple :

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Si le `err` propriété n’est pas `null`, une erreur s’est produite et il doit être présenté à l’utilisateur. Dans le cas contraire, l’utilisateur sera invité à entrer le code de programme d’installation pour le périphérique à ajouter :

[![](homekit-images/accessory02.png "Entrez le code d’installation pour le périphérique à ajouter")](homekit-images/accessory02.png#lightbox)

Dans le simulateur d’accessoire HomeKit ce nombre se trouve dans le **Code d’installation** champ :

[![](homekit-images/accessory03.png "Le champ Code d’installation dans le simulateur d’accessoire HomeKit")](homekit-images/accessory03.png#lightbox)

Pour les accessoires HomeKit réels, le code d’installation soit est imprimé sur une étiquette sur l’appareil lui-même, sur l’emballage du produit ou dans le manuel de l’utilisateur de l’accesseur.

Vous devez surveiller du navigateur accessoire `DidRemoveNewAccessory` événement et mise à jour de l’utilisateur de l’interface pour supprimer un accessoire dans la liste disponible une fois que l’utilisateur a ajouté à leur Collection accueil.

## <a name="working-with-accessories"></a>Utilisation des accessoires

Une fois une maison principal et établi et accessoires ont été ajoutés à ce dernier, vous pouvez présenter une liste des accessoires (et éventuellement des salles) pour l’utilisateur à utiliser.

Le `HMRoom` objet contient toutes les informations concernant une salle donnée et tous les accessoires qui lui appartiennent. Salles peuvent éventuellement être organisés en une ou plusieurs zones. A `HMZone` contient toutes les informations sur une zone donnée et toutes les pièces qui lui appartiennent.

Cet exemple, nous allons conserver choses simples et utiliser des accessoires d’une maison directement, au lieu de les organiser dans des locaux ou des zones.

Le `HMHome` objet contient une liste d’accessoire attribué qui peut être présentée à l’utilisateur dans son `Accessories` propriété. Exemple :

[![](homekit-images/accessory04.png "Un accessoire d’exemple")](homekit-images/accessory04.png#lightbox)

Écran ici, l’utilisateur peut sélectionner un accessoire donné et travailler avec les services qu’il fournit.

## <a name="working-with-services"></a>Utilisation des Services

Lorsque l’utilisateur interagit avec un périphérique d’automatisation domestique activé HomeKit donné, il est généralement via les services qu’il fournit. Le `Services` propriété de la `HMAccessory` classe contient une collection de `HMService` propose des objets qui définissent les services un appareil.

Les services sont des éléments tels que les témoins lumineux, thermostats, garage porte OUVREURS, commutateurs ou les verrous. Certains périphériques (par exemple, une porte de garage) fournit plusieurs services, tels que claire et la possibilité d’ouvrir ou fermer une porte.

En plus des services spécifiques qui fournit d’un accessoire donné, chaque accesseur contient un `Information Service` qui définit des propriétés telles que son nom, le fabricant, le modèle et le numéro de série.

### <a name="accessory-service-types"></a>Types de Service accessoire

Les Types de Service suivants sont disponibles via le `HMServiceType` enum :

- **AccessoryInformation** -fournit des informations sur le périphérique donné automation de base (accessoire).
- **AirQualitySensor** -définit un capteur de qualité air.
- **Batterie** -définit l’état de la batterie de d’accessoire.
- **CarbonDioxideSensor** -définit un capteur de dioxyde de carbone.
- **CarbonMonoxideSensor** -définit un capteur monoxyde.
- **ContactSensor** -définit un capteur de contact (par exemple, une fenêtre qui est ouvert ou fermé).
- **La porte** -définit un capteur d’état porte (tel qu’ouvert ou fermé).
- **Ventilateur** -définit un ventilateur contrôlé à distance.
- **GarageDoorOpener** -définit une porte de garage.
- **HumiditySensor** -définit un capteur humidité.
- **LeakSensor** -définit un capteur de fuite (comme pour un chauffage d’eau ou d’une machine à laver).
- **Ampoule** -définit un voyant autonome ou un qui fait partie d’un autre accessoire (par exemple, une porte de garage).
- **LightSensor** -définit un capteur de luminosité.
- **LockManagement** -définit un service qui gère un verrou porte automatisée.
- **LockMechanism** -définit un verrou contrôlé à distance (par exemple, un verrou porte).
- **MotionSensor** -définit un capteur de mouvement.
- **OccupancySensor** -définit un capteur de niveau d’occupation.
- **Sortie** -définit une prise murale contrôlée à distance.
- **SecuritySystem** -définit un système de sécurité de base.
- **StatefulProgrammableSwitch** -définit un commutateur programmable qui reste dans un état de donner une fois déclenché (par exemple, un commutateur retournement).
- **StatelessProgrammableSwitch** -définit un commutateur programmable qui retourne à son état initial après le déclenchement (par exemple, un bouton de commande).
- **SmokeSensor** -définit un capteur de détection de fumée.
- **Commutateur** -définit un commutateur marche/arrêt comme un commutateur de mur standard.
- **Capteur de température** -définit un capteur de température.
- **Thermostat** -définit un thermostat intelligent permet de contrôler un système de climatisation.
- **Fenêtre** -définit une fenêtre automatisée que canne est à distance ouverte ou fermée.
- **WindowCovering** -définit une fenêtre contrôlée à distance couvrant comme Stores qui peuvent être ouvert ou fermés.

### <a name="displaying-service-information"></a>Affichage des informations de Service

Après avoir chargé un `HMAccessory` vous pouvez interroger la personne `HNService` objets il fournit et afficher ces informations à l’utilisateur :

[![](homekit-images/accessory05.png "Affichage des informations de Service")](homekit-images/accessory05.png#lightbox)

Vous devez toujours doit vérifier la `Reachable` propriété d’un `HMAccessory` avant d’essayer de les utiliser. Un accessoire peut être inaccessible l’utilisateur n’est pas dans la plage de l’appareil ou si elle a été déconnecté.

Une fois qu’un service a été sélectionné, l’utilisateur peut afficher ou modifier une ou plusieurs caractéristiques de ce service pour surveiller et de contrôler un appareil donné automation de base.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Utilisation des caractéristiques

Chaque `HMService` objet peut contenir une collection de `HMCharacteristic` des objets qui peuvent fournir des informations sur l’état du service (par exemple, une porte sont ouverts ou fermés) ou autoriser l’utilisateur d’ajuster un état (par exemple, pour définir la couleur d’une lumière).

`HMCharacteristic` fournit non seulement des informations sur une caractéristique et son état, mais fournit également des méthodes pour travailler avec l’état via _caractéristique métadonnées_ (`HMCharacteristisMetadata`). Ces métadonnées peuvent fournir des propriétés (telles que les plages de valeur minimale et maximale) qui sont utiles lors de l’affichage des informations à l’utilisateur ou ce qui leur permet de modifier les États.

Le `HMCharacteristicType` enum fournit un ensemble de valeurs de métadonnées de la caractéristique qui peut être définie ou modifiée comme suit :

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - Luminosité
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - Teinte
 - Identifier
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - Journaux
 - Fabricant
 - Modèle
 - MotionDetected
 - Name
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - Saturation
 - Numéro de série
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Version

### <a name="working-with-a-characteristics-value"></a>Utilisez valeur d’une caractéristique

Pour vérifier que votre application a le dernier état d’une caractéristique donné, appelez le `ReadValue` méthode de la `HMCharacteristic` classe. Si le `err` propriété n’est pas `null`, une erreur s’est produite et il peut ou ne peut pas être présentée à l’utilisateur.

De la caractéristique `Value` propriété contient l’état actuel de la caractéristique donnée en tant qu’un `NSObject`et par conséquent ne peut pas être manipulées directement en langage c#.

Pour lire la valeur, la classe d’assistance suivante a été ajoutée à la **HomeKitIntro** exemple d’application :

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

Le `NSObjectConverter` est utilisé chaque fois que l’application doit lire l’état actuel d’une caractéristique. Par exemple :

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

La ligne ci-dessus convertit la valeur dans un `float` qui peut ensuite être utilisé dans le code c# Xamarin.

Pour modifier un `HMCharacteristic`, appelez sa `WriteValue` (méthode) et encapsuler la nouvelle valeur dans un `NSObject.FromObject` appeler. Par exemple :

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Si le `err` propriété n’est pas `null`, une erreur s’est produite et doit être présentée à l’utilisateur.

### <a name="testing-characteristic-value-changes"></a>Test des modifications de valeur caractéristique

Lorsque vous travaillez avec `HMCharacteristics` et accessoires simulés, les modifications apportées à la `Value` propriété peut être surveillée dans le simulateur d’accessoire HomeKit.

Avec la **HomeKitIntro** application s’exécutant sur iOS réel matériels des périphériques, modifications apportées à valeur d’une caractéristique doit être visible presque instantanément dans le simulateur d’accessoire HomeKit. Par exemple, modification de l’état d’une lumière dans l’application iOS :

[![](homekit-images/test01.png "Modification de l’état d’une lumière dans une application iOS")](homekit-images/test01.png#lightbox)

Devez modifier l’état de la lumière dans le simulateur d’accessoire HomeKit. Si la valeur ne change pas, vérifiez l’état du message d’erreur lors de l’écriture de nouvelles valeurs de caractéristiques et assurez-vous que l’accesseur est toujours accessible.

## <a name="advanced-homekit-features"></a>Fonctionnalités avancées HomeKit

Cet article a couvert les fonctionnalités de base requises pour l’utilisation des accessoires HomeKit dans une application Xamarin.iOS. Toutefois, il existe plusieurs fonctionnalités avancées de HomeKit qui ne sont pas traitées dans cette introduction :

- **Salles** -HomeKit activé accessoires peuvent éventuellement organisés en salles par l’utilisateur final. Ainsi, HomeKit accessoires présents dans un moyen simple pour l’utilisateur à comprendre et utiliser. Pour plus d’informations sur la création et la gestion des espaces, consultez d’Apple [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) documentation.
- **Zones** -salles peuvent éventuellement être organisés en zones par l’utilisateur final. Une zone fait référence à une collection de classes que l’utilisateur peut traiter comme une seule unité. Par exemple : attirail, Downstairs ou sous-sol. Là encore, cela permet de HomeKit présenter et utiliser des accessoires d’une façon qui soit parlante pour l’utilisateur final. Pour plus d’informations sur la création et la gestion de zones, consultez Apple [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) documentation.
- **Actions et jeux d’Action** -Actions modifier les caractéristiques du service accessoires et peuvent être regroupées en ensembles. Jeux d’action agisse comme des scripts pour contrôler un groupe d’accessoires et de coordonner leurs actions. Par exemple, un script « Télévision » peut fermer les stores atténue l’éclairage et activer la télévision et son système son. Pour plus d’informations sur la création et la gestion des Actions et jeux d’Action, consultez Apple [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) et [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) documentation.
- **Déclencheurs** : un déclencheur peut activer une ou plusieurs Action définie lorsqu’un ensemble donné de conditions ont été remplies. Par exemple, activer la lumière portch et verrouiller toutes les portes externes lorsqu’il reçoit sombre à l’extérieur. Pour plus d’informations sur la création et la maintenance des déclencheurs, consultez Apple [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) documentation.

Étant donné que ces fonctionnalités utilisent les mêmes techniques présentées ci-dessus, ils doivent être faciles à implémenter par Apple suivant [HomeKitDeveloper Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [indications de l’Interface utilisateur HomeKit](https://developer.apple.com/homekit/ui-guidelines/) et [ Référence HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>Directives de révision d’application HomeKit

Avant de soumettre un HomeKit application Xamarin.iOS compatible à iTunes Connect pour cette version de l’iTunes App Store, veillez à suivre les instructions d’Apple pour les applications HomeKit activé :

 - Objectif principal de l’application _doit_ être automation de base si vous utilisez l’infrastructure HomeKit.
 - Texte de marketing de l’application doit notifier aux utilisateurs que HomeKit est utilisé et il doit fournir une politique de confidentialité.
 - Collecte des informations sur l’utilisateur ou à l’aide de HomeKit pour la publicité est strictement interdite.

Pour la version complète, consultez instructions, consultez Apple [App Store passez en revue les instructions](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>Nouveautés dans iOS 9

Apple a effectué les modifications et additions suivantes à HomeKit IOS 9 :

- **Conservation d’objets existants** - si un accessoire existant est modifié, le Gestionnaire d’accueil (`HMHomeManager`) vous informe de l’élément qui a été modifié.
- **Identificateurs persistants** -toutes les classes HomeKit pertinentes incluent désormais un `UniqueIdentifier` propriété pour identifier de façon unique un élément donné dans HomeKit activée applications (ou des instances de la même application).
- **Gestion des utilisateurs** -ajouter un contrôleur d’affichage intégré pour fournir la gestion des utilisateurs sur les utilisateurs qui ont accès aux appareils HomeKit dans accueil de l’utilisateur principal.
- **Les fonctionnalités utilisateur** - HomeKit utilisateurs ont désormais un ensemble de privilèges qui contrôlent les fonctions qu’ils sont en mesure d’utiliser dans HomeKit et HomeKit activé accessoires. Votre application doit afficher uniquement les fonctionnalités appropriées à l’utilisateur actuel. Par exemple, seuls les administrateurs doivent être en mesure de mettre à jour des autres utilisateurs.
- **Prédéfinies scènes** -scènes prédéfinis ont été créés pour les quatre événements courants qui se produisent pour l’utilisateur HomeKit moyen : obtenir, laissez, retourner, accédez à lit. Impossible de supprimer ces séquences prédéfinis à partir d’une maison.
- **En arrière-plan et Siri** -Siri a une prise en charge accrue pour les scènes dans iOS 9 et peut reconnaissent le nom de toutes les scènes défini dans HomeKit. Un utilisateur peut exécuter une scène simplement en parlant son nom par Siri.
- **Catégories Accessoires** -un ensemble de catégories prédéfinies qui a été ajouté à tous les accessoires et permet d’identifier le type d’accessoire ajouté à une page d’accueil ou travaillé sur à partir de votre application. Ces nouvelles catégories sont disponibles lors de l’installation d’accessoire.
- **Prise en charge d’Apple Watch** - HomeKit est désormais disponible pour watchOS et l’Apple Watch sera en mesure de contrôler HomeKit activé des périphériques sans un iPhone à proximité l’observation. HomeKit pour watchOS prend en charge les fonctionnalités suivantes : Consultation particuliers, accessoires de contrôle et l’exécution des séquences.
- **Type de déclencheur d’événement** : outre les déclencheurs de type minuterie pris en charge dans iOS 8, iOS 9 maintenant prend en charge les déclencheurs d’événements selon état accessoire (par exemple, les données de capteur) ou de géolocalisation. Utilisent des déclencheurs d’événements `NSPredicates` pour définir des conditions pour leur exécution.
- **Accès à distance** -avec accès à distance, l’utilisateur est désormais en mesure de contrôler leur HomeKit activé accueil Automation accessoires lorsqu’ils sont en dehors de la maison à un emplacement distant. Dans iOS 8 cela est uniquement pris en charge si l’utilisateur avait 3e génération Apple TV dans la page d’accueil. Dans iOS 9, cette limitation est levée et l’accès à distance est prise en charge via le protocole d’accessoire HomeKit (HAP) et les iCloud.
- **Fonctionnalités Bluetooth faible énergie (BLE)** -HomeKit prend désormais en charge d’autres types d’accessoires capable de communiquer via le protocole Bluetooth faible énergie (tableau). À l’aide de HAP Secure Tunneling, un accessoire HomeKit peut exposer un autre accessoire Bluetooth via le Wi-Fi (si elle est en dehors de la fonction Bluetooth). Dans iOS 9, BLE accessoires ont une prise en charge complète pour les notifications et les métadonnées.
- **Nouvelles catégories accessoire** -Apple ajouté les nouvelles catégories accessoire suivantes dans iOS 9 : FENETRES, portes de moteur, des fenêtres, des systèmes d’alarme, capteurs et commutateurs Programmable.

Pour plus d’informations sur les nouvelles fonctionnalités de HomeKit dans iOS 9, consultez le site d’Apple [HomeKit Index](https://developer.apple.com/homekit/) et [Nouveautés HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) vidéo.

## <a name="summary"></a>Récapitulatif

Cet article a introduit l’infrastructure d’automatisation de base des HomeKit d’Apple. Il a montré comment installer et configurer des appareils de test à l’aide du simulateur d’accessoire HomeKit et comment créer une simple application Xamarin.iOS pour découvrir, de communiquer avec et contrôler les périphériques automation de base à l’aide de HomeKit.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Indications de l’Interface utilisateur de HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Référence HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
