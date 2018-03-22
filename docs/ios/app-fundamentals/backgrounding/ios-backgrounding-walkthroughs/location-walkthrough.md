---
title: "Procédure pas à pas - à l’aide d’emplacement de l’arrière-plan"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: dfae07d36c67c12d254cd70c22acb01b2fc1c9b0
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="walkthrough---using-background-location"></a>Procédure pas à pas - à l’aide d’emplacement de l’arrière-plan

Dans cet exemple, nous allons créer un iOS application emplacement qui imprime des informations sur votre emplacement actuel : latitude et longitude autres paramètres de l’écran. Cette application va vous montrer comment effectuer correctement les mises à jour de l’emplacement pendant que l’application est actif ou Backgrounded.

Cette procédure pas à pas explique certains clé backgrounding concepts, y compris l’inscription d’une application comme une application nécessaire à l’arrière-plan, la suspension des mises à jour de l’interface utilisateur lorsque l’application est backgrounded et l’utilisation de la `WillEnterBackground` et `WillEnterForeground` `AppDelegate` méthodes .

## <a name="application-set-up"></a>Configuration des applications


1. Tout d’abord, créez un **iOS > applications > Application de vue unique (c#)**. Appelez-le _emplacement_ et assurez-vous que les iPad et iPhone ont été sélectionnés.

1. Une application d’emplacement correspond à une application nécessaire à l’arrière-plan dans iOS. Inscrire l’application en tant qu’emplacement application en modifiant le **Info.plist** fichier pour le projet.

    L’Explorateur de solutions, double-cliquez sur le **Info.plist** fichier pour l’ouvrir, faites défiler vers le bas de la liste. Cochez la case à la fois par le **activer les Modes d’arrière-plan** et **mises à jour de l’emplacement** cases à cocher.


    Dans Visual Studio pour Mac, il ressemblera à quelque chose comme ceci :

    [![](location-walkthrough-images/image7.png "Cochez la case par les Modes d’arrière-plan activer et les cases à cocher des mises à jour de l’emplacement")](location-walkthrough-images/image7.png#lightbox)

    Dans Visual Studio, **Info.plist** doit être mis à jour manuellement en ajoutant la paire clé/valeur suivantes :

    ```xml
    <key>UIBackgroundModes</key>
    <array>
        <string>location</string>
    </array>
    ```

1. Maintenant que l’application est inscrite, elle peut récupérer les données de l’emplacement de l’appareil. Dans iOS, le `CLLocationManager` classe est utilisée pour accéder aux informations d’emplacement et peut déclencher des événements qui fournissent des mises à jour de l’emplacement.

1. Dans le code, créez une classe appelée `LocationManager` qui fournit un emplacement unique pour différents écrans et s’abonner aux mises à jour de l’emplacement de code. Dans le `LocationManager` de classe, de créer une instance de la `CLLocationManager` appelée `LocMgr`:

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

    Le code ci-dessus définit un nombre de propriétés et les autorisations sur le [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) classe :

    - `PausesLocationUpdatesAutomatically` – Il s’agit d’une valeur booléenne qui peut être définie selon si le système est autorisé à suspendre les mises à jour de l’emplacement. Sur certains appareils la valeur par défaut `true`, ce qui peut entraîner l’appareil arrêter de recevoir des mises à jour de l’emplacement après environ 15 minutes en arrière-plan.
    - `RequestAlwaysAuthorization` -Vous devez passer cette méthode pour permettre à l’utilisateur de l’application pour autoriser l’emplacement accessible en arrière-plan. `RequestWhenInUseAuthorization` peut également être passé si vous souhaitez permettre à l’utilisateur pour autoriser l’emplacement accessible uniquement lorsque l’application est au premier plan.
    - `AllowsBackgroundLocationUpdates` – Il s’agit d’une propriété booléenne, introduite dans iOS 9 qui peuvent être définies pour permettre à une application recevoir des mises à jour de l’emplacement lorsque suspendu.

    > [!IMPORTANT]
    > iOS 8 (et supérieur) nécessite également une entrée dans le **Info.plist** fichier permettant à l’utilisateur dans le cadre de la demande d’autorisation.

1. Ajouter une clé `NSLocationAlwaysUsageDescription` ou `NSLocationWhenInUseUsageDescription` avec une chaîne qui sera affichée à l’utilisateur dans l’alerte qui demande l’accès de données d’emplacement.

1. iOS 9 requiert que lorsque vous utilisez `AllowsBackgroundLocationUpdates` le **Info.plist** inclut la clé `UIBackgroundModes` avec la valeur `location`. Si vous avez effectué l’étape 2 de cette procédure pas à pas, il doit déjà été dans votre fichier Info.plist.


1. À l’intérieur de la `LocationManager` de classe, créez une méthode appelée `StartLocationUpdates` avec le code suivant. Ce code illustre la façon dont pour recevoir des mises à jour de l’emplacement à partir de la `CLLocationManager`:

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

    Il existe plusieurs points importants qui passe à cette méthode. Tout d’abord, nous effectuons une vérification pour voir si l’application a accès aux données d’emplacement sur l’appareil. Nous le vérifier en appelant `LocationServicesEnabled` sur la `CLLocationManager`. Cette méthode retournera **false** si l’utilisateur a refusé l’accès de l’application pour les informations d’emplacement.

1. Ensuite, indiquez la fréquence à laquelle le Gestionnaire d’emplacement pour mettre à jour. `CLLocationManager` fournit de nombreuses options pour le filtrage et la configuration des données d’emplacement, y compris la fréquence des mises à jour. Dans cet exemple, définissez la `DesiredAccuracy` pour mettre à jour chaque fois que l’emplacement est modifié par un compteur. Pour plus d’informations sur la configuration de fréquence de mise à jour d’emplacement et d’autres préférences, reportez-vous à la [référence de classe CLLocationManager](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) dans la documentation Apple.

1. Enfin, appelez `StartUpdatingLocation` sur la `CLLocationManager` instance. Cela indique le Gestionnaire d’emplacement pour obtenir un correctif initial de l’emplacement actuel et commencer l’envoi des mises à jour

Jusqu'à présent, le Gestionnaire d’emplacement a été créé, configuré avec les types de données que vous souhaitez recevoir, et a déterminé l’emplacement d’origine. Le code doit à présent afficher les données d’emplacement de l’interface utilisateur. Nous pouvons effectuer cette opération avec un événement personnalisé qui prend un `CLLocation` en tant qu’argument :

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

L’étape suivante consiste à s’abonner aux mises à jour de l’emplacement à partir de la `CLLocationManager`et déclencher personnalisé `LocationUpdated` événement lorsque de nouvelles données d’emplacement devient disponibles, en passant l’emplacement en tant qu’argument. Pour ce faire, créez une nouvelle classe **LocationUpdateEventArgs.cs**. Ce code est accessible dans l’application principale et retourne l’emplacement de l’appareil lors de l’événement est déclenché :

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

1. Utilisez le concepteur iOS pour générer l’écran qui affiche des informations d’emplacement. Double-cliquez sur le **Main.storyboard** fichier pour commencer.

    Sur le plan conceptuel, faites glisser plusieurs étiquettes à l’écran en tant que des espaces réservés pour les informations d’emplacement. Dans cet exemple, il existe les étiquettes de latitude, longitude, altitude, cours et la vitesse.

    La mise en page doit ressembler à ceci :

    ![](location-walkthrough-images/image8.png "Un exemple de disposition de l’interface utilisateur dans le concepteur iOS")

1. Dans la zone de solutions, double-cliquez sur le `ViewController.cs` de fichier et le modifier pour créer une nouvelle instance de la LocationManager et l’appel `StartLocationUpdates`sur celui-ci.
  Modifiez le code pour présenter l’aspect suivant :

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

    Démarre les mises à jour de l’emplacement sur le démarrage de l’application, bien qu’aucune donnée ne s’affichera.

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

Nous devons encore pour vous abonner à la `LocationUpdated` événement dans notre AppDelegate, puis appelez la nouvelle méthode de mise à jour de l’interface utilisateur. Ajoutez le code suivant dans `ViewDidLoad,` juste après le `StartLocationUpdates` appeler :

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

[![](location-walkthrough-images/image5.png "Un exemple d’application exécuter")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>La gestion des états actif et d’arrière-plan

1. L’application est sortie de mises à jour de l’emplacement alors qu’il est au premier plan et active. Pour illustrer ce qui se passe lors de l’application passe à l’arrière-plan, substituez le `AppDelegate` les méthodes qui effectuent le suivi d’application de modifications d’état afin que l’application écrit dans la console lors des transitions entre le premier plan et arrière-plan :

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

    Ajoutez le code suivant dans le `LocationManager` en permanence imprimer emplacement mis à jour des données à la sortie de l’application, pour vérifier les informations d’emplacement sont toujours disponibles dans l’arrière-plan :

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

1. Il est l’un des problèmes avec le code restant : tente de mettre à jour l’interface utilisateur lorsque l’application est backgrounded sera cause iOS sera mettez-y fin. Lorsque l’application passe en arrière-plan, le code doit annuler l’abonnement à partir de l’emplacement des mises à jour et d’arrêter la mise à jour de l’interface utilisateur.

    iOS nous fournit des notifications lorsque l’application est sur la transition vers une autre application États. Dans ce cas, nous pouvons vous abonner à la `ObserveDidEnterBackground` Notification.

    L’extrait de code suivant montre comment utiliser une notification pour informer la vue quand arrêter les mises à jour de l’interface utilisateur. Cela passera `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Lorsque l’application est en cours d’exécution, la sortie sera similaire à ceci :

    ![](location-walkthrough-images/image6.png "Exemple de la sortie de l’emplacement dans la console")

1. L’application imprime les mises à jour de l’emplacement à l’écran en premier plan et continue à imprimer des données dans la fenêtre Sortie de l’application lors de l’utilisation en arrière-plan.

Uniquement l’un des problèmes en suspens restent : l’écran de démarrage mises à jour de l’interface utilisateur lors du premier chargement de l’application, mais il n’a aucun moyen de savoir quand l’application a entrés à nouveau au premier plan. Si l’application backgrounded retrouve au premier plan, les mises à jour de l’interface utilisateur ne sera pas reprendre.

Pour résoudre ce problème, imbriquer un appel à démarrer les mises à jour de l’interface utilisateur à l’intérieur d’une autre Notification, qui est déclenché lorsque l’application passe à l’état actif :

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

L’interface utilisateur commencent désormais mise à jour lors du premier démarrage de l’application et de reprendre la mise à jour n’importe quel moment l’application revient au premier plan.

Dans cette procédure pas à pas, nous avons créé une application iOS valide, prenant en charge en arrière-plan qui imprime les données d’emplacement à l’écran et la fenêtre Sortie de l’application.


## <a name="related-links"></a>Liens associés

- [Emplacement (partie 4) (exemple)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Référence de Framework l’emplacement principal](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
