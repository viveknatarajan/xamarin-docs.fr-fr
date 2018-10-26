---
title: Classe d’application Xamarin.Forms
description: Cet article explique les fonctionnalités de la classe d’application par défaut, qui inclut une propriété à définir à la page initiale de l’application, et un dictionnaire persistant pour stocker des valeurs simples aux changements d’état du cycle de vie.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: cebdd6fafafae7d1cd6258e6200808731e3c4f29
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118810"
---
# <a name="xamarinforms-app-class"></a>Classe d’application Xamarin.Forms

Le `Application` classe de base offre les fonctionnalités suivantes, qui sont exposées dans la valeur par défaut des projets `App` sous-classe :

* Un `MainPage` propriété, qui est l’emplacement de définition de la page initiale de l’application.
* Un persistant [ `Properties` dictionnaire](#Properties_Dictionary) pour stocker des valeurs simples aux changements d’état du cycle de vie.
* Statique `Current` propriété qui contient une référence à l’objet d’application actuel.

Elle expose également [les méthodes de cycle de vie](~/xamarin-forms/app-fundamentals/app-lifecycle.md) comme `OnStart`, `OnSleep`, et `OnResume` , ainsi que les événements de navigation modale.

Selon le modèle que vous avez choisi, la `App` classe peut être définie de deux manières :

* **C#**, ou
* **XAML ET C#**

Pour créer un **application** à l’aide de XAML, la valeur par défaut de la classe **application** classe doit être remplacée par un XAML **application** classe et code-behind associé, comme indiqué dans l’exemple de code suivant :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

L’exemple de code suivant montre le code-behind associé :

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

Ainsi que le paramètre le [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) propriété, le code-behind doit également appeler le `InitializeComponent` méthode pour charger et analyser le XAML associé.

## <a name="mainpage-property"></a>Propriété de MainPage

Le `MainPage` propriété sur le `Application` classe définit la page racine de l’application.

Par exemple, vous pouvez créer une logique dans votre `App` classe pour afficher une page différente selon que l’utilisateur est connecté ou non.

Le `MainPage` propriété doit être définie le `App` constructeur,

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Dictionnaire de propriétés

Le `Application` sous-classe a statique `Properties` dictionnaire qui peut être utilisé pour stocker les données, en particulier pour une utilisation dans le `OnStart`, `OnSleep`, et `OnResume` méthodes. Vous pouvez y accéder à partir de n’importe où dans votre code Xamarin.Forms à l’aide `Application.Current.Properties`.

Le `Properties` dictionary utilise un `string` clé et stocke un `object` valeur.

Par exemple, vous pouvez définir un persistant `"id"` propriété n’importe où dans votre code (lorsqu’un élément est sélectionné, dans une page `OnDisappearing` (méthode), ou dans le `OnSleep` (méthode)) comme suit :

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

Dans le `OnStart` ou `OnResume` méthodes que vous pouvez ensuite utiliser cette valeur pour recréer l’expérience utilisateur d’une certaine façon. Le `Properties` dictionnaire magasins `object`s, vous devez effectuer un cast de sa valeur avant de l’utiliser.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Vérifiez toujours la présence de la clé avant d’y accéder pour éviter des erreurs inattendues.

> [!NOTE]
> Le `Properties` dictionnaire peut sérialiser uniquement les types primitifs pour le stockage. Tente de stocker d’autres types (tel que `List<string>`) peut échouer en mode silencieux.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Persistance

Le `Properties` dictionnaire est automatiquement enregistré à l’appareil.
Données ajoutées au dictionnaire sera disponibles lorsque l’application est retournée à partir de l’arrière-plan ou même après son redémarrage.

Xamarin.Forms 1.4 a introduit une méthode supplémentaire sur le `Application` classe - `SavePropertiesAsync()` -qui peut être appelé pour conserver de manière proactive le `Properties` dictionnaire. Il s’agit de vous permettent d’enregistrer les propriétés après les mises à jour importantes plutôt que de risquer des ne pas en cours de sérialisation en raison d’un incident ou les cours de fermeture par le système d’exploitation.

Vous pouvez trouver des références à l’aide la `Properties` dictionnaire dans le **création d’applications mobiles avec Xamarin.Forms** livre chapitres [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), et [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)et le texte associé [exemples](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Classe Application

Complète `Application` implémentation de la classe est indiquée ci-dessous pour référence :

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Cette classe est ensuite instanciée dans chaque projet spécifique à la plateforme et passée à la `LoadApplication` méthode là où le `MainPage` est chargé et affiché à l’utilisateur.
Le code pour chaque plateforme est indiqué dans les sections suivantes. Les derniers modèles de solution Xamarin.Forms déjà contient tout ce code, préconfiguré pour votre application.


### <a name="ios-project"></a>Projet iOS

IOS `AppDelegate` hérite de la classe `FormsApplicationDelegate`. Il doit :

* Appelez `LoadApplication` avec une instance de la `App` classe.

* Retourne toujours `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Projet Android

Android `MainActivity` hérite `FormsAppCompatActivity`. Dans le `OnCreate` remplacer le `LoadApplication` méthode est appelée avec une instance de la `App` classe.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Projet Windows universel (UWP) pour Windows 10

Consultez [le programme d’installation Windows projets](~/xamarin-forms/platform/windows/installation/index.md) pour plus d’informations sur la prise en charge UWP dans Xamarin.Forms.

Doit hériter de la page principale dans le projet UWP `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Construction de la c# code-behind doit appeler `LoadApplication` pour créer une instance de votre Xamarin.Forms `App`. Notez qu’il est recommandé d’utiliser explicitement l’espace de noms d’application pour qualifier le `App` , car les applications UWP également possèdent leurs propres `App` classe sans rapport avec Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Notez que `Forms.Init()` doit être appelée **App.xaml.cs** autour de la ligne 63.
