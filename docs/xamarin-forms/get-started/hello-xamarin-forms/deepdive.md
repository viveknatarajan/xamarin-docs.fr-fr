---
title: Xamarin.Forms - En profondeur
description: Cet article examine les notions de base du développement d’applications avec Xamarin.Forms. Les sujets abordés étaient notamment la structure d’une application Xamarin.Forms, les concepts fondamentaux d’une architecture et d’une application, et l’interface utilisateur.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: def4ecccf92c47a5cc7c08e2821e5f3387fb752f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118706"
---
# <a name="xamarinforms-deep-dive"></a>Xamarin.Forms - En profondeur

Dans [Xamarin.Forms - Démarrage rapide](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), l’application Phoneword a été générée. Cet article passe en revue les éléments générés afin d’acquérir les notions de base du fonctionnement des applications Xamarin.Forms.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Introduction à Visual Studio

Visual Studio organise le code en *solutions* et en *projets*. Une solution est un conteneur qui peut comprendre un ou plusieurs projets. Un projet peut être une application, une bibliothèque de prise en charge, une application de test, etc. L’application Phoneword se compose d’une solution qui contient quatre projets, comme le montre la capture d’écran suivante :

![](deepdive-images/vs/solution.png "Explorateur de solutions Visual Studio")

Les projets sont :

- Phoneword : ce projet est le projet de bibliothèque de classes .NET Standard qui contient l’ensemble du code partagé et de l’interface utilisateur partagée.
- Phoneword.Android : ce projet contient le code spécifique à Android et constitue le point d’entrée de l’application Android.
- Phoneword.iOS : ce projet contient le code spécifique à iOS et constitue le point d’entrée pour l’application iOS.
- Phoneword.UWP : ce projet contient le code spécifique à la plateforme Windows universelle (UWP) et constitue le point d’entrée pour l’application UWP.

## <a name="anatomy-of-a-xamarinforms-application"></a>Structure d’une application Xamarin.Forms

La capture d’écran suivante montre le contenu du projet de bibliothèque de classes .NET Standard Phoneword dans Visual Studio :

![](deepdive-images/vs/net-standard-project.png "Contenu du projet .NET Standard Phoneword ")

Le projet a un nœud **Dépendances** qui contient les nœuds **NuGet** et **SDK** :

- **NuGet** &ndash; Paquet Xamarin.Forms NuGet ajouté au projet.
- **SDK** &ndash; Métapaquet `NETStandard.Library` qui référence l’ensemble complet de paquets NuGet définissant .NET Standard.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Introduction à Visual Studio pour Mac

Tout comme Visual Studio, [Visual Studio pour Mac](/visualstudio/mac/) organise le code en *solutions* et en *projets*. Une solution est un conteneur qui peut comprendre un ou plusieurs projets. Un projet peut être une application, une bibliothèque de prise en charge, une application de test, etc. L’application Phoneword se compose d’une solution qui contient trois projets, comme le montre la capture d’écran suivante :

![](deepdive-images/xs/solution.png "Volet Solution Visual Studio pour Mac")

Les projets sont :

- Phoneword : ce projet est le projet de bibliothèque de classes .NET Standard qui contient l’ensemble du code partagé et de l’interface utilisateur partagée.
- Phoneword.Droid : ce projet contient le code spécifique à Android et constitue le point d’entrée pour les applications Android.
- Phoneword.iOS : ce projet contient le code spécifique à iOS et constitue le point d’entrée pour les applications iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Structure d’une application Xamarin.Forms

La capture d’écran suivante montre le contenu du projet de bibliothèque .NET Standard Phoneword dans Visual Studio pour Mac :

![](deepdive-images/xs/library-project.png "Contenu du projet de bibliothèque .NET Standard Phoneword ")

Le projet a un nœud **Dépendances** qui contient les nœuds **NuGet** et **SDK** :

- **NuGet** &ndash; Paquet Xamarin.Forms NuGet ajouté au projet.
- **SDK** &ndash; Métapaquet `NETStandard.Library` qui référence l’ensemble complet de paquets NuGet définissant .NET Standard.

::: zone-end

Le projet comprend également un certain nombre de fichiers :

- **App.xaml** : balisage XAML pour la classe `App`, qui définit un dictionnaire de ressources pour l’application.
- **App.xaml.cs** : code-behind pour la classe `App`, qui est chargé de l’instanciation de la première page affichée par l’application sur chaque plateforme et de la gestion des événements du cycle de vie de l’application.
- **IDialer.cs** : interface `IDialer` qui spécifie que la méthode `Dial` doit être fournie par les classes d’implémentation.
- **MainPage.xaml** : balisage XAML pour la classe `MainPage`, qui définit l’interface utilisateur pour la page affichée quand l’application est lancée.
- **MainPage.xaml.cs** : code-behind pour la classe `MainPage`, qui contient la logique métier exécutée quand l’utilisateur interagit avec la page.
- **PhoneTranslator.cs** : logique métier chargée de convertir un mot composé sur le clavier du téléphone en numéro de téléphone, appelé à partir de **MainPage.xaml.cs**.

Pour plus d’informations sur la structure d’une application Xamarin.iOS, consultez [Structure d’une application Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Pour plus d’informations sur la structure d’une application Xamarin.Android, consultez [Structure d’une application Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Concepts fondamentaux d’une architecture et d’une application

::: zone pivot="windows"

Une application Xamarin.Forms est structurée de la même façon qu’une application multiplateforme classique. En général, le code partagé est placé dans une bibliothèque .NET Standard et les applications spécifiques à la plateforme consomment le code partagé. Le diagramme suivant montre une vue d’ensemble de cette relation pour l’application Phoneword :

![](deepdive-images/vs/architecture.png "Architecture Phoneword")

::: zone-end
::: zone pivot="macos"

Une application Xamarin.Forms est structurée de la même façon qu’une application multiplateforme classique. En général, le code partagé est placé dans une bibliothèque .NET Standard et les applications spécifiques à la plateforme consomment le code partagé. Le diagramme suivant montre une vue d’ensemble de cette relation pour l’application Phoneword :

![](deepdive-images/xs/architecture.png "Architecture Phoneword")

::: zone-end

Pour optimiser la réutilisation du code de démarrage, les applications Xamarin.Forms ont une seule classe nommée `App` qui est chargée de l’instanciation de la première page affichée par l’application sur chaque plateforme, comme indiqué dans l’exemple de code suivant :

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

Ce code affecte à la propriété `MainPage` de la classe `App` une nouvelle instance de la classe [`MainPage`](xref:Xamarin.Forms.Application.MainPage). En outre, l’attribut [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) active le compilateur XAML afin que le code XAML soit compilé directement en langage intermédiaire. Pour plus d’informations, consultez [Compilation XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Lancement de l’application sur chaque plateforme

### <a name="ios"></a>iOS

Pour lancer la page Xamarin.Forms initiale dans iOS, le projet Phoneword.iOS inclut la classe `AppDelegate` qui hérite de la classe `FormsApplicationDelegate`, comme indiqué dans l’exemple de code suivant :

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

La valeur de remplacement `FinishedLaunching` initialise le framework Xamarin.Forms en appelant la méthode `Init`. Cela entraîne le chargement de l’implémentation spécifique à iOS de Xamarin.Forms dans l’application avant que le contrôleur d’affichage racine ne soit défini par l’appel à la méthode `LoadApplication`.

### <a name="android"></a>Android

Pour lancer la page Xamarin.Forms initiale dans Android, le projet Phoneword.Droid inclut du code qui crée une `Activity`avec l’attribut `MainLauncher`, l’activité héritant de la classe `FormsAppCompatActivity`, comme le montre l’exemple de code suivant :

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword", 
              Icon = "@mipmap/icon", 
              Theme = "@style/MainTheme", 
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);
            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

La valeur de remplacement `OnCreate` initialise le framework Xamarin.Forms en appelant la méthode `Init`. Cela entraîne le chargement de l’implémentation spécifique à Android de Xamarin.Forms dans l’application avant que l’application Xamarin.Forms ne soit chargée. De plus, la classe `MainActivity` stocke une référence à elle-même dans la propriété `Instance`. La propriété `Instance` constitue le contexte local et est référencée depuis la classe `PhoneDialer`.

::: zone pivot="windows"

## <a name="universal-windows-platform"></a>Plateforme Windows universelle

Dans les applications de plateforme Windows universelle (UWP), la méthode `Init` qui initialise le framework Xamarin.Forms est appelée à partir de la classe `App` :

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Cela entraîne le chargement de l’implémentation spécifique à UWP de Xamarin.Forms dans l’application. La page Xamarin.Forms initiale est lancée par la classe `MainPage`, comme indiqué dans l’exemple de code suivant :

```csharp
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```

L’application Xamarin.Forms est chargée avec la méthode `LoadApplication`.

> [!NOTE]
> Les applications de plateforme Windows universelle (UWP) peuvent être générées avec Xamarin.Forms, mais uniquement à l’aide de Visual Studio sur Windows.

::: zone-end

## <a name="user-interface"></a>Interface utilisateur

Il existe quatre principaux groupes de contrôles permettant de créer l’interface utilisateur d’une application Xamarin.Forms.

1. **Pages** : les pages Xamarin.Forms représentent les écrans de l’application mobile multiplateforme. L’application Phoneword utilise la classe [`ContentPage`](xref:Xamarin.Forms.ContentPage) pour afficher un seul écran. Pour plus d’informations sur les pages, consultez [Pages Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Dispositions** : les dispositions Xamarin.Forms sont des conteneurs utilisés pour composer des vues dans des structures logiques. L’application Phoneword utilise la classe [`StackLayout`](xref:Xamarin.Forms.StackLayout) pour disposer les contrôles selon une pile verticale. Pour plus d’informations sur les dispositions, consultez [Dispositions Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Vues** : les vues Xamarin.Forms sont les contrôles affichés sur l’interface utilisateur, tels que les étiquettes, les boutons et les zones d’entrée de texte. L’application Phoneword utilise les contrôles [`Label`](xref:Xamarin.Forms.Label), [`Entry`](xref:Xamarin.Forms.Entry) et [`Button`](xref:Xamarin.Forms.Button). Pour plus d’informations sur les vues, consultez [Vues Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Cellules** : les cellules Xamarin.Forms sont des éléments spécialisés utilisés pour des éléments dans une liste et décrivent comment chaque élément dans une liste doit être dessiné. L’application Phoneword n’utilise aucune cellule. Pour plus d’informations sur les cellules, consultez [Cellules Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

Lors de l’exécution, chaque contrôle est mappé à son équivalent natif, ce qui sera affiché.

Quand l’application Phoneword est exécutée sur n’importe quelle plateforme, elle affiche un écran unique qui correspond à un élément [`Page`](xref:Xamarin.Forms.Page) dans Xamarin.Forms. Un élément `Page` représente un *ViewGroup* dans Android, un *Contrôleur d’affichage* dans iOS ou une *Page* dans la plateforme Windows universelle. L’application Phoneword instancie également un objet [`ContentPage`](xref:Xamarin.Forms.ContentPage) qui représente la classe `MainPage`, dont le balisage XAML est indiqué dans l’exemple de code suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButton" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

La classe `MainPage` utilise un contrôle [`StackLayout`](xref:Xamarin.Forms.StackLayout) pour disposer automatiquement les contrôles sur l’écran, quelle que soit la taille de l’écran. Les éléments enfants sont placés les uns après les autres, verticalement dans l’ordre dans lequel ils sont ajoutés. Le contrôle `StackLayout` contient un contrôle [`Label`](xref:Xamarin.Forms.Label) pour afficher le texte dans la page, un contrôle [`Entry`](xref:Xamarin.Forms.Entry) pour accepter l’entrée de texte de l’utilisateur et deux contrôles [`Button`](xref:Xamarin.Forms.Button) utilisés pour exécuter du code en réponse à des événements tactiles.

Pour plus d’informations sur XAML dans Xamarin.Forms, consultez [Notions de base XAML Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Réponse aux interactions de l’utilisateur

Un objet défini en XAML peut déclencher un événement qui est géré dans le fichier code-behind. L’exemple de code suivant illustre la méthode `OnTranslate` dans le code-behind pour la classe `MainPage`, qui est exécutée en réponse à l’événement [`Clicked`](xref:Xamarin.Forms.Button.Clicked) déclenché sur le bouton *Translate* (Traduire).

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

La méthode `OnTranslate` traduit le mot composé sur le clavier du téléphone en numéro de téléphone correspondant et, en réponse, définit des propriétés sur le bouton d’appel. Le fichier code-behind d’une classe XAML peut accéder à un objet défini en XAML en utilisant le nom qui lui est affecté avec l’attribut `x:Name`. La valeur affectée à cet attribut suit les mêmes règles que les variables C# : elle doit commencer par une lettre ou un trait de soulignement et ne contenir aucun espace incorporé.

L’association du bouton Traduire à la méthode `OnTranslate` se produit dans le balisage XAML pour la classe `MainPage` :

```xaml
<Button x:Name="translateButton" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Autres concepts introduits dans Phoneword

L’application Phoneword pour Xamarin.Forms a introduit plusieurs concepts qui ne sont pas traités dans cet article. Ces concepts sont notamment :

- Activation et désactivation des boutons. Un [`Button`](xref:Xamarin.Forms.Button) peut être activé ou désactivé en changeant sa propriété [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled). Par exemple, l’exemple de code suivant désactive le bouton `callButton` :

    ```csharp
    callButton.IsEnabled = false;
    ```

- Affichage d’une boîte de dialogue d’alerte. Quand l’utilisateur appuie sur le **bouton** d’appel, l’application Phoneword affiche une *boîte de dialogue d’alerte* avec l’option d’effectuer ou d’annuler un appel. La méthode [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) est utilisée pour créer la boîte de dialogue, comme illustré dans l’exemple de code suivant :

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Accès aux fonctionnalités natives via la classe [`DependencyService`](xref:Xamarin.Forms.DependencyService). L’application Phoneword utilise la classe `DependencyService` pour résoudre l’interface `IDialer` en implémentations de numérotation téléphonique spécifiques à la plateforme, comme indiqué dans l’exemple de code suivant à partir du projet Phoneword :

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Pour plus d’informations sur la classe [`DependencyService`](xref:Xamarin.Forms.DependencyService), consultez [Accès aux fonctionnalités natives avec DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Appel téléphonique avec une URL. L’application Phoneword utilise `OpenURL` pour lancer l’application du téléphone système. L’URL comprend un préfixe `tel:` qui est suivi du numéro de téléphone à appeler, comme illustré dans l’exemple de code suivant à partir du projet iOS :

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Ajustement de la disposition de la plateforme. La classe [`Device`](xref:Xamarin.Forms.Device) permet aux développeurs de personnaliser la disposition et les fonctionnalités de l’application selon la plateforme, comme indiqué dans l’exemple de code suivant qui utilise différentes valeurs [`Padding`](xref:Xamarin.Forms.Layout.Padding) sur différentes plateformes pour afficher correctement chaque page :

    ```xaml
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms" ... >
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        ...
    </ContentPage>
    ```

  Pour plus d’informations sur les ajustements de plateforme, consultez [Device, classe](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Tests et déploiement

Visual Studio pour Mac et Visual Studio proposent tous deux de nombreuses options pour tester et déployer les applications. Le débogage d’applications est une partie courante du cycle de vie de développement des applications et permet de diagnostiquer les problèmes de code. Pour plus d’informations, consultez [Définir un point d’arrêt](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Exécuter du code pas à pas](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) et [Informations de sortie dans la fenêtre de journal](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Les simulateurs constituent un endroit judicieux pour commencer à déployer et tester une application, et comportent des fonctionnalités utiles pour le test des applications. Toutefois, les utilisateurs ne consommant pas l’application finale dans un simulateur, les applications doivent être testées tôt et souvent sur des appareils réels. Pour plus d’informations sur le provisionnement des appareils iOS, consultez [Provisionnement des appareils](~/ios/get-started/installation/device-provisioning/index.md). Pour plus d’informations sur le provisionnement des appareils Android, consultez [Configurer un appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Récapitulatif

Cet article a examiné les notions de base du développement d’applications avec Xamarin.Forms. Les sujets abordés étaient notamment la structure d’une application Xamarin.Forms, les concepts fondamentaux d’une architecture et d’une application, et l’interface utilisateur.

Dans la section suivante de ce guide, l’application sera étendue pour inclure plusieurs écrans afin de découvrir une architecture et des concepts Xamarin.Forms plus avancés.
