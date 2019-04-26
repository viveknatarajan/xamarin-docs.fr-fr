---
title: Procédure pas à pas - l’emplacement en arrière-plan dans Xamarin.iOS
description: Ce document fournit une procédure pas à pas montrant comment utiliser les informations d’emplacement dans une application Xamarin.iOS backgrounded. Elle décrit le programme d’installation nécessaires, interface utilisateur et états de l’application.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: fa8a48e165764a449af4bc5414d2e66aecea8269
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392270"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>Procédure pas à pas - l’emplacement en arrière-plan dans Xamarin.iOS

Dans cet exemple, nous allons créer une application d’emplacement qui imprime des informations sur notre site actuel iOS : latitude, longitude et autres paramètres à l’écran. Cette application va vous montrer comment effectuer correctement les mises à jour de l’emplacement lorsque l’application est actif ou Backgrounded.

Cette procédure pas à pas explique certaines clé backgrounding concepts, y compris l’inscription d’une application comme une application nécessaire à l’arrière-plan, la suspension des mises à jour de l’interface utilisateur lorsque l’application est lancé en arrière-plan et l’utilisation de la `WillEnterBackground` et `WillEnterForeground` `AppDelegate` méthodes .

## <a name="application-set-up"></a>Configuration des applications


1. Tout d’abord, créez un **iOS > application > Application avec affichage unique (C#)**. Appelez-le _emplacement_ et vous assurer que les iPad et iPhone ont été sélectionnés.

1. Une application de l’emplacement correspond à une application d’arrière-plan-nécessaire dans iOS. Inscrire l’application en tant qu’emplacement application en modifiant le **Info.plist** fichier pour le projet.

    Sous l’Explorateur de solutions, double-cliquez sur le **Info.plist** fichier pour l’ouvrir et faites défiler vers le bas de la liste. Cochez la case à la fois par le **Enable Background Modes** et **mises à jour de l’emplacement** cases à cocher.

    Dans Visual Studio pour Mac, il ressemblera à quelque chose comme ceci :

    [![](location-walkthrough-images/image7.png "Cochez la case en Enable Background Modes et les cases à cocher des mises à jour de l’emplacement")](location-walkthrough-images/image7.png#lightbox)

    Dans Visual Studio, **Info.plist** doit être mis à jour manuellement en ajoutant la paire clé/valeur suivantes :

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Maintenant que l’application est inscrite, il peut récupérer les données d’emplacement de l’appareil. Dans iOS, le `CLLocationManager` classe est utilisée pour accéder aux informations d’emplacement et peut déclencher des événements qui fournissent des mises à jour de l’emplacement.

1. Dans le code, créez une classe appelée `LocationManager` qui fournit un emplacement unique pour les différents écrans et de code pour vous abonner aux mises à jour de l’emplacement. Dans le `LocationManager` class, créer une instance de la `CLLocationManager` appelée `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Le code ci-dessus définit un nombre de propriétés et les autorisations sur le [CLLocationManager](xref:CoreLocation.CLLocationManager) classe :

    - `PausesLocationUpdatesAutomatically` : Il s’agit d’une valeur booléenne qui peut être définie en fonction de si le système est autorisé à suspendre les mises à jour de l’emplacement. Sur certains appareils par défaut `true`, ce qui peut entraîner l’appareil arrêter de recevoir des mises à jour de l’emplacement après environ 15 minutes en arrière-plan.
    - `RequestAlwaysAuthorization` -Vous devez passer cette méthode pour permettre à l’utilisateur de l’application pour autoriser l’emplacement accessible en arrière-plan. `RequestWhenInUseAuthorization` peut également être passé si vous souhaitez donner à l’utilisateur de l’option pour autoriser l’emplacement accessible uniquement lorsque l’application est au premier plan.
    - `AllowsBackgroundLocationUpdates` – Il s’agit d’une propriété booléenne, introduite dans iOS 9 qui peuvent être définies pour permettre à une application recevoir des mises à jour de l’emplacement lorsque suspendu.

    > [!IMPORTANT]
    > iOS 8 (et supérieur) nécessite également une entrée dans le **Info.plist** fichier permettant à l’utilisateur dans le cadre de la demande d’autorisation.

1. Ajouter une clé `NSLocationAlwaysUsageDescription` ou `NSLocationWhenInUseUsageDescription` avec une chaîne qui sera affichée à l’utilisateur dans l’alerte qui demande l’accès de données d’emplacement.

1. iOS 9 exige que lorsque vous utilisez `AllowsBackgroundLocationUpdates` le **Info.plist** inclut la clé `UIBackgroundModes` avec la valeur `location`. Si vous avez terminé l’étape 2 de cette procédure pas à pas, cela devrait déjà été dans votre fichier Info.plist.


1. À l’intérieur de la `LocationManager` classe, créez une méthode appelée `StartLocationUpdates` par le code suivant. Ce code illustre la façon dont pour recevoir des mises à jour de l’emplacement à partir de la `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    Voici quelques points importants qui se produit dans cette méthode. Tout d’abord, nous effectuons une vérification pour voir si l’application a accès aux données d’emplacement sur l’appareil. Nous le vérifier en appelant `LocationServicesEnabled` sur le `CLLocationManager`. Cette méthode retournera **false** si l’utilisateur a refusé l’application à accéder aux informations d’emplacement.

1. Ensuite, indiquez la fréquence à laquelle le Gestionnaire d’emplacement pour mettre à jour. `CLLocationManager` offre de nombreuses options pour le filtrage et la configuration des données d’emplacement, y compris la fréquence des mises à jour. Dans cet exemple, définissez le `DesiredAccuracy` pour mettre à jour chaque fois que l’emplacement est modifié par un compteur. Pour plus d’informations sur la configuration de fréquence de mise à jour d’emplacement et d’autres préférences, reportez-vous à la [référence de classe CLLocationManager](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) dans la documentation Apple.

1. Enfin, appelez `StartUpdatingLocation` sur la `CLLocationManager` instance. Cela indique le Gestionnaire d’emplacement pour obtenir un correctif initial sur l’emplacement actuel et démarrer l’envoi des mises à jour

Jusqu’ici, le Gestionnaire d’emplacement a été créé, configuré avec les types de données que nous souhaitons recevoir, et a déterminé l’emplacement initial. Le code doit à présent afficher les données de localisation de l’interface utilisateur. Nous pouvons le faire avec un événement personnalisé qui prend un `CLLocation` en tant qu’argument :

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

L’étape suivante consiste à s’abonner aux mises à jour de l’emplacement à partir de la `CLLocationManager`et déclencher personnalisé `LocationUpdated` événement lorsque de nouvelles données d’emplacement devient disponibles, en passant l’emplacement en tant qu’argument. Pour ce faire, créez une nouvelle classe **LocationUpdateEventArgs.cs**. Ce code est accessible au sein de l’application principale et retourne l’emplacement de l’appareil lorsque l’événement est déclenché :

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Interface utilisateur

1. Utilisez le concepteur iOS pour créer l’écran qui affiche des informations d’emplacement. Double-cliquez sur le **Main.storyboard** fichier pour commencer.

    Sur le plan conceptuel, faites glisser plusieurs étiquettes à l’écran comme espaces réservés pour les informations d’emplacement. Dans cet exemple, voici les étiquettes pour la latitude, longitude, altitude, cours et la vitesse.

    La mise en page doit ressembler à ce qui suit :

    ![](location-walkthrough-images/image8.png "Un exemple de disposition de l’interface utilisateur dans le concepteur iOS")

1. Dans le panneau de solutions, double-cliquez sur le `ViewController.cs` fichier et modifiez-la pour créer une nouvelle instance de la LocationManager et appelez `StartLocationUpdates`dessus.
  Modifier le code ressemble à ce qui suit :

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Cette commande démarre les mises à jour de l’emplacement sur le démarrage de l’application, même si aucune donnée ne s’affichera.

1. Maintenant que les mises à jour de l’emplacement sont reçus, mettre à jour l’écran avec les informations d’emplacement. La méthode suivante obtient l’emplacement à partir de notre `LocationUpdated` événements et les affiche dans l’interface utilisateur :

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Nous avons besoin pour vous abonner à la `LocationUpdated` événement notre AppDelegate, puis appelez la nouvelle méthode pour mettre à jour de l’interface utilisateur. Ajoutez le code suivant dans `ViewDidLoad,` juste après le `StartLocationUpdates` appeler :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Maintenant, lorsque l’application est exécutée, elle doit ressembler à ceci :

[![](location-walkthrough-images/image5.png "Exécution d’une application exemple")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Gestion des états actif et d’arrière-plan

1. L’application est sortie de mises à jour de l’emplacement bien qu’il soit au premier plan et active. Pour illustrer ce qui se passe quand l’application entre dans l’arrière-plan, substituez le `AppDelegate` méthodes qui effectuent le suivi d’application les modifications d’état afin que l’application écrit dans la console durant les transitions entre le premier plan et arrière-plan :

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Ajoutez le code suivant dans le `LocationManager` à l’emplacement mis à jour de l’impression en continu des données dans la sortie de l’application, pour vérifier les informations d’emplacement sont toujours disponibles dans l’arrière-plan :

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Il existe un problème restant avec le code : tente de mettre à jour l’interface utilisateur lorsque l’application est lancé en arrière-plan sera cause iOS s’y mettre fin. Lorsque l’application passe en arrière-plan, le code a besoin pour vous désabonner de mises à jour de l’emplacement et d’arrêter la mise à jour de l’interface utilisateur.

    iOS nous fournit des notifications lorsque l’application est sur le point de transition vers une autre application États. Dans ce cas, nous pouvons vous abonner à la `ObserveDidEnterBackground` Notification.

    L’extrait de code suivant montre comment utiliser une notification pour informer la vue quand arrêter les mises à jour de l’interface utilisateur. Celui-ci s’insère `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Lorsque l’application est en cours d’exécution, la sortie se présente comme suit :

    ![](location-walkthrough-images/image6.png "Exemple de la sortie de l’emplacement dans la console")

1. L’application imprime les mises à jour de l’emplacement à l’écran lors de l’exploitation au premier plan et continue à imprimer des données dans la fenêtre de sortie d’application lors de l’utilisation en arrière-plan.

Reste qu’un seul problème en suspens : l’écran de démarrage mises à jour de l’interface utilisateur lorsque l’application est chargée en premier, mais il n’a aucun moyen de savoir quand l’application est rentré de premier plan. Si l’application backgrounded est remise au premier plan, les mises à jour de l’interface utilisateur ne sont pas reprendre.

Pour résoudre ce problème, imbriquer un appel à démarrer des mises à jour de l’interface utilisateur à l’intérieur d’une autre Notification, qui est déclenché lorsque l’application passe à l’état Active :

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Désormais, l’interface utilisateur commencera la mise à jour lors du premier démarrage de l’application et de reprendre la mise à jour n’importe quel moment l’application revient au premier plan.

Dans cette procédure pas à pas, nous avons créé une application iOS se comportant bien, prenant en charge en arrière-plan qui imprime les données d’emplacement à l’écran et la fenêtre de sortie d’application.


## <a name="related-links"></a>Liens associés

- [Emplacement (partie 4) (exemple)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Référence de Framework l’emplacement principal](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
