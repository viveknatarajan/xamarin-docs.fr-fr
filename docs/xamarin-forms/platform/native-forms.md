---
title: Formulaires natifs
description: "Formulaires natifs autorisent Xamarin.Forms ContentPage dérivé des pages à être consommés par les projets natifs Xamarin.iOS, Xamarin.Android et plateforme Windows universelle (UWP). Les projets natifs peuvent consommer des pages ContentPage dérivé directement ajoutés au projet, ou à partir d’une bibliothèque de classes Portable (PCL), la bibliothèque Standard de .NET ou le projet partagé. Cet article explique comment consommer des pages ContentPage dérivées qui sont ajoutés directement à des projets natifs et comment naviguer entre elles."
ms.topic: article
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 89636b874f8dbc8f66280dcc1ed99d0f832ff312
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="native-forms"></a>Formulaires natifs

_Formulaires natifs autorisent Xamarin.Forms ContentPage dérivé des pages à être consommés par les projets natifs Xamarin.iOS, Xamarin.Android et plateforme Windows universelle (UWP). Les projets natifs peuvent consommer des pages ContentPage dérivé directement ajoutés au projet, ou à partir d’une bibliothèque de classes Portable (PCL), la bibliothèque Standard de .NET ou le projet partagé. Cet article explique comment consommer des pages ContentPage dérivées qui sont ajoutés directement à des projets natifs et comment naviguer entre elles._

En règle générale, une application de Xamarin.Forms inclut une ou plusieurs pages qui dérivent de [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), et ces pages sont partagés par toutes les plateformes dans une bibliothèque PCL, la bibliothèque Standard de .NET ou le projet partagé. Toutefois, les formulaires natif permet `ContentPage`-dérivées des pages à ajouter directement à des applications natives de Xamarin.iOS, Xamarin.Android et UWP. Par rapport à un projet natif consommer `ContentPage`-pages dérivées d’une bibliothèque de classes portables, bibliothèque Standard de .NET ou projet partagé, l’avantage de l’ajout de pages directement à des projets natifs est que les pages peuvent être étendus avec les vues natifs. Vues natives peuvent ensuite être définis dans XAML avec `x:Name` et référencés à partir du code-behind. Pour plus d’informations sur les vues natifs, consultez [vues natif](~/xamarin-forms/platform/native-views/index.md).

Le processus pour l’utilisation d’un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page dérivée dans un projet natif est la suivante :

1. Ajoutez le package Xamarin.Forms NuGet au projet natif.
1. Ajouter le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page et toutes les dépendances, le projet natif dérivés.
1. Appelez la méthode `Forms.Init`.
1. Construire une instance de la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page dérivée et la convertir en type natif approprié à l’aide d’une des méthodes d’extension suivantes : `CreateViewController` pour iOS, `CreateFragment` ou `CreateSupportFragment` pour Android, ou `CreateFrameworkElement` pour la plateforme Windows universelle.
1. Accédez à la représentation sous forme de type natif de la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée à l’aide de l’API native de la navigation de page.

Xamarin.Forms doit être initialisé en appelant le `Forms.Init` avant un projet natif peut construire un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page dérivée. En choisissant quand procéder principalement dépend du moment où il est plus pratique dans le flux de votre application, il peut être effectuée au démarrage de l’application, ou juste avant le `ContentPage`-page dérivée est construit. Dans cet article et les exemples d’applications qui l’accompagne, le `Forms.Init` méthode est appelée au démarrage de l’application.

> [!NOTE]
> **Remarque**: le **NativeForms** solution d’application ne contient pas tous les projets Xamarin.Forms. Au lieu de cela, il se compose d’un projet Xamarin.iOS, un projet Xamarin.Android et un projet UWP. Chaque projet est un projet natif qui utilise des formulaires natifs pour consommer [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivées des pages. Toutefois, il n’existe aucune raison pourquoi les projets natifs n’a pas pu utiliser `ContentPage`-pages dérivé d’une bibliothèque de classes portables, bibliothèque Standard de .NET ou projet partagé.

Lors de l’utilisation des formulaires natifs, Xamarin.Forms fonctionnalités, telles que [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)et le moteur de liaison de données, tout le travail fixe.

## <a name="ios"></a>iOS

Sur iOS, le `FinishedLaunching` remplacer dans la `AppDelegate` classe est généralement l’endroit pour exécuter l’application des tâches de démarrage. Elle est appelée une fois que l’application a lancé et est généralement substituée pour configurer la fenêtre principale et afficher le contrôleur. Le code suivant exemple illustre la `AppDelegate` classe dans l’exemple d’application :

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

Le `FinishedLaunching` méthode effectue les tâches suivantes :

- Xamarin.Forms est initialisé en appelant le `Forms.Init` (méthode).
- Une référence à la `AppDelegate` classe est stockée dans le `static` `Instance` champ. Il s’agit de fournir un mécanisme pour d’autres classes appeler les méthodes définies dans la `AppDelegate` classe.
- Le `UIWindow`, qui est le conteneur principal pour les vues dans les applications natives iOS, est créé.
- Le `PhonewordPage` (classe), qui est un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page défini dans XAML, qui est construit et converti en un `UIViewController` à l’aide de la `CreateViewController` méthode d’extension.
- Le `Title` propriété de la `UIViewController` est défini, ce qui sera affiché sur le `UINavigationBar`.
- A `UINavigationController` est créé pour la gestion de la navigation hiérarchique. Le `UINavigationController` classe gère une pile de contrôleurs d’affichage et le `UIViewController` passé dans le constructeur s’affiche initialement lorsque le `UINavigationController` est chargé.
- Le `UINavigationController` instance est définie comme niveau supérieur `UIViewController` pour le `UIWindow`et le `UIWindow` est défini en tant que la fenêtre de clé pour l’application et est rendu visible.

Une fois la `FinishedLaunching` la méthode est exécutée, l’interface utilisateur définie dans le Xamarin.Forms `PhonewordPage` classe sera affiché, comme indiqué dans la capture d’écran suivante :

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png "iOS PhonewordPage")

Interaction avec l’interface utilisateur, par exemple en appuyant sur un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), entraîne des gestionnaires d’événements dans le `PhonewordPage` l’exécution de code-behind. Par exemple, quand un utilisateur appuie sur le **l’historique des appels** bouton, le Gestionnaire d’événements suivante est exécutée :

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

Le `static` `AppDelegate.Instance` champ permet la `AppDelegate.NavigateToCallHistoryPage` méthode à appeler, comme dans l’exemple de code suivant :

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

Le `NavigateToCallHistoryPage` méthode convertit le Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page pour un `UIViewController` avec la `CreateViewController` méthode d’extension et les jeux de la `Title` propriété de la `UIViewController`. Le `UIViewController` incombe puis `UINavigationController` par le `PushViewController` (méthode). Par conséquent, l’interface utilisateur définie dans le Xamarin.Forms `CallHistoryPage` classe sera affiché, comme indiqué dans la capture d’écran suivante :

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png "iOS CallHistoryPage")

Lorsque le `CallHistoryPage` s’affiche, en appuyant sur la sauvegarde flèche s’affiche le `UIViewController` pour le `CallHistoryPage` classe à partir de la `UINavigationController`, renvoyer l’utilisateur à la `UIViewController` pour la `PhonewordPage` classe.

## <a name="android"></a>Android

Sur Android, le `OnCreate` remplacer dans la `MainActivity` classe est généralement l’endroit pour exécuter l’application des tâches de démarrage. Le code suivant exemple illustre la `MainActivity` classe dans l’exemple d’application :

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

Le `OnCreate` méthode effectue les tâches suivantes :

- Xamarin.Forms est initialisé en appelant le `Forms.Init` (méthode).
- Une référence à la `MainActivity` classe est stockée dans le `static` `Instance` champ. Il s’agit de fournir un mécanisme pour d’autres classes appeler les méthodes définies dans la `MainActivity` classe.
- Le `Activity` contenu est défini à partir d’une ressource de mise en page. Dans l’exemple d’application, la disposition se compose d’un `LinearLayout` qui contient un `Toolbar`et un `FrameLayout` d’agir comme un conteneur de fragment.
- Le `Toolbar` est récupéré et définir en tant que la barre d’action pour le `Activity`, et le titre de l’action de la barre est défini.
- Le `PhonewordPage` (classe), qui est un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page défini dans XAML, qui est construit et converti en un `Fragment` à l’aide de la `CreateFragment` méthode d’extension.
- Le `FragmentManager` classe crée et valide une transaction qui remplace le `FrameLayout` de l’instance avec la `Fragment` pour la `PhonewordPage` classe.

Pour plus d’informations sur les Fragments, consultez [Fragments](~/android/platform/fragments/index.md).

> [!NOTE]
> **Remarque**: outre la `CreateFragment` méthode d’extension, Xamarin.Forms inclut également un `CreateSupportFragment` (méthode). Le `CreateFragment` méthode crée un `Android.App.Fragment` qui peut être utilisée dans les applications qui ciblent des API 11 et supérieur. Le `CreateSupportFragment` méthode crée un `Android.Support.V4.App.Fragment` qui peut être utilisé dans les applications qui ciblent des versions antérieures à 11 de l’API.

Une fois la `OnCreate` la méthode est exécutée, l’interface utilisateur définie dans le Xamarin.Forms `PhonewordPage` classe sera affiché, comme indiqué dans la capture d’écran suivante :

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png "PhonewordPage Android")

Interaction avec l’interface utilisateur, par exemple en appuyant sur un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), entraîne des gestionnaires d’événements dans le `PhonewordPage` l’exécution de code-behind. Par exemple, quand un utilisateur appuie sur le **l’historique des appels** bouton, le Gestionnaire d’événements suivante est exécutée :

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

Le `static` `MainActivity.Instance` champ permet la `MainActivity.NavigateToCallHistoryPage` méthode à appeler, comme dans l’exemple de code suivant :

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

Le `NavigateToCallHistoryPage` méthode convertit le Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page pour un `Fragment` avec la `CreateFragment` méthode d’extension et ajoute le `Fragment` au fragment de la pile back. Par conséquent, l’interface utilisateur définie dans le Xamarin.Forms `CallHistoryPage` s’affichera, comme illustré dans la capture d’écran suivante :

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png "CallHistoryPage Android")

Lorsque le `CallHistoryPage` s’affiche, en touchant l’arrière flèche s’affiche le `Fragment` pour le `CallHistoryPage` à partir de la pile back du fragment, retour à l’utilisateur le `Fragment` pour la `PhonewordPage` classe.

### <a name="enabling-back-navigation-support"></a>Prise en charge de Navigation précédent

Le `FragmentManager` classe a un `BackStackChanged` événement qui se déclenche chaque fois que le contenu de la pile back du fragment change. Le `OnCreate` méthode dans la `MainActivity` classe contient un gestionnaire d’événements anonyme pour cet événement :

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Ce gestionnaire d’événements affiche un bouton précédent sur la barre d’action à condition qu’il existe un ou plusieurs `Fragment` instances sur le fragment de pile back. La réponse à en appuyant sur le bouton précédent est gérée par le `OnOptionsItemSelected` remplacer :

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

Le `OnOptionsItemSelected` override est appelé chaque fois qu’un élément dans le menu options est sélectionné. Cette implémentation dépile le fragment en cours à partir de la pile back du fragment, sous réserve que le bouton précédent a été sélectionné et il existe un ou plusieurs `Fragment` instances sur le fragment de pile back.

### <a name="multiple-activities"></a>Plusieurs activités

Lorsqu’une application est composée de plusieurs activités, [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pages dérivées peuvent être incorporées dans chacune des activités. Dans ce scénario, le `Forms.Init` méthode doive être appelée uniquement dans le `OnCreate` la substitution de la première `Activity` qui incorpore un Xamarin.Forms `ContentPage`. Toutefois, cela entraîne les conséquences suivantes :

- La valeur de `Xamarin.Forms.Color.Accent` seront exécutées à partir de la `Activity` qui a appelé le `Forms.Init` (méthode).
- La valeur de `Xamarin.Forms.Application.Current` sera associé le `Activity` qui a appelé le `Forms.Init` (méthode).

### <a name="choosing-a-file"></a>Choix d’un fichier

Lors de l’incorporation un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page qui utilise un [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) qui a besoin pour prendre en charge HTML « Choisir un fichier » bouton, le `Activity` devra remplacer le `OnActivityResult` méthode :

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Sur la plateforme Windows universelle, natif `App` classe est généralement l’endroit pour exécuter l’application des tâches de démarrage. Xamarin.Forms est généralement initialisé, dans les applications UWP de Xamarin.Forms, dans le `OnLaunched` remplacer dans natif `App` (classe), pour passer le `LaunchActivatedEventArgs` l’argument de la `Forms.Init` (méthode). Pour cette raison, les applications UWP natives qui consomment un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page dérivée peut appeler plus facilement le `Forms.Init` méthode à partir de la `App.OnLaunched` (méthode).

Par défaut, natif `App` classe lance la `MainPage` classe en tant que la première page de l’application. Le code suivant exemple illustre la `MainPage` classe dans l’exemple d’application :

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

Le `MainPage` constructeur effectue les tâches suivantes :

- Mise en cache est activée pour la page, afin qu’un nouveau `MainPage` n’est pas construit lorsqu’un utilisateur navigue vers la page.
- Une référence à la `MainPage` classe est stockée dans le `static` `Instance` champ. Il s’agit de fournir un mécanisme pour d’autres classes appeler les méthodes définies dans la `MainPage` classe.
- Le `PhonewordPage` (classe), qui est un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page défini dans XAML, qui est construit et converti en un `FrameworkElement` à l’aide de la `CreateFrameworkElement` méthode d’extension, puis définir en tant que le contenu de la `MainPage` classe.

Une fois la `MainPage` constructeur a été exécutée, l’interface utilisateur définie dans le Xamarin.Forms `PhonewordPage` classe sera affiché, comme indiqué dans la capture d’écran suivante :

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png "UWP PhonewordPage")

Interaction avec l’interface utilisateur, par exemple en appuyant sur un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), entraîne des gestionnaires d’événements dans le `PhonewordPage` l’exécution de code-behind. Par exemple, quand un utilisateur appuie sur le **l’historique des appels** bouton, le Gestionnaire d’événements suivante est exécutée :

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

Le `static` `MainPage.Instance` champ permet la `MainPage.NavigateToCallHistoryPage` méthode à appeler, comme dans l’exemple de code suivant :

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Navigation dans la plateforme Windows universelle est généralement effectuée avec la `Frame.Navigate` (méthode), qui prend un `Page` argument. Xamarin.Forms définit un `Frame.Navigate` méthode d’extension qui prend un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-page instance dérivée. Par conséquent, lorsque le `NavigateToCallHistoryPage` méthode s’exécute, l’interface utilisateur définie dans le Xamarin.Forms `CallHistoryPage` s’affichera, comme illustré dans la capture d’écran suivante :

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png "UWP CallHistoryPage")

Lorsque le `CallHistoryPage` s’affiche, en touchant l’arrière flèche s’affiche le `FrameworkElement` pour le `CallHistoryPage` à partir de la pile de retour dans l’application, retour à l’utilisateur le `FrameworkElement` pour la `PhonewordPage` classe.

### <a name="enabling-back-navigation-support"></a>Prise en charge de Navigation précédent

Sur la plateforme Windows universelle, applications doivent activer la navigation dans arrière pour tous les matériels et logiciels arrière boutons, sur les facteurs de forme autre appareil. Cela peut être accompli en inscrivant un gestionnaire d’événements pour le `BackRequested` événement, qui peut être effectuée dans le `OnLaunched` méthode dans natif `App` classe :

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

Lorsque l’application est lancée, le `GetForCurrentView` méthode récupère le `SystemNavigationManager` objet associé à l’affichage actuel, puis inscrit un gestionnaire d’événements pour le `BackRequested` événement. L’application ne reçoit que cet événement s’il est de l’application de premier plan, en réponse, appelle la `OnBackRequested` Gestionnaire d’événements :

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

Le `OnBackRequested` Gestionnaire d’événements appelle les `GoBack` méthode sur le frame racine de l’application et les jeux de la `BackRequestedEventArgs.Handled` propriété `true` pour marquer l’événement comme géré. Pour marquer l’événement comme géré peut provoquer le système vous obliger à quitter l’application (sur la famille de périphériques mobiles) ou en ignorant les événements (sur la famille de périphériques de bureau).

L’application s’appuie sur un bouton précédent du système fourni sur un téléphone, mais il choisit s’il faut afficher un bouton précédent sur la barre de titre sur les appareils de bureau. Cela s’effectue en définissant le `AppViewBackButtonVisibility` la propriété de la `AppViewBackButtonVisibility` valeurs d’énumération :

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Le `OnNavigated` Gestionnaire d’événements, qui est exécuté en réponse à la `Navigated` déclenchement des événements, des mises à jour la visibilité du titre de la barre du bouton précédent lors de la navigation de page se produit. Cela garantit que le bouton précédent de barre de titre est visible si la pile de retour dans l’application n’est pas vide, ou supprimé de la barre de titre, si la pile de retour dans l’application est vide.

Pour plus d’informations sur la prise en charge de la navigation arrière sur la plateforme Windows universelle, consultez [l’historique de Navigation et vers l’arrière navigation pour applications UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Récapitulatif

Formulaires natifs autorisent Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivées des pages à être consommés par les projets natifs Xamarin.iOS, Xamarin.Android et plateforme Windows universelle (UWP). Les projets natifs peuvent consommer `ContentPage`-dérivées des pages qui sont directement ajoutés au projet, ou à partir d’une bibliothèque de classes portables, bibliothèque Standard de .NET ou projet partagé. Cet article a expliqué comment consommer `ContentPage`-dérivées des pages qui sont ajoutées directement pour les projets natifs et comment naviguer entre elles.


## <a name="related-links"></a>Liens associés

- [NativeForms (sample)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Vues natives](~/xamarin-forms/platform/native-views/index.md)
