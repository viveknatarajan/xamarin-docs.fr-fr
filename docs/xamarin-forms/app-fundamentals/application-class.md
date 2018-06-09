---
title: Classe d’application de Xamarin.Forms
description: Cet article explique les fonctionnalités de la classe d’application par défaut, qui inclut une propriété définie à la page initiale de l’application, et un dictionnaire persistant pour stocker des valeurs simples entre les modifications d’état du cycle de vie.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 15fb866d2cde9409f401d6d021b22b8cb0468795
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240539"
---
# <a name="xamarinforms-app-class"></a>Classe d’application de Xamarin.Forms

Le `Application` classe de base offre les fonctionnalités suivantes, qui sont exposées dans la valeur par défaut des projets `App` sous-classe :

* A `MainPage` propriété, qui est l’emplacement de définition de la page initiale de l’application.
* Un persistant [ `Properties` dictionnaire](#Properties_Dictionary) pour stocker des valeurs simples entre les modifications d’état du cycle de vie.
* Statique `Current` propriété qui contient une référence à l’objet d’application actuel.

Il expose également [les méthodes de cycle de vie](~/xamarin-forms/app-fundamentals/app-lifecycle.md) comme `OnStart`, `OnSleep`, et `OnResume` , ainsi que les événements de navigation modale.

Selon le modèle que vous avez choisi la `App` classe peut être défini de deux manières :

* **C#**, ou
* **XAML ET C#**

Pour créer un **application** à l’aide de XAML, la valeur par défaut de la classe **application** classe doit être remplacé par un code XAML **application** classe et code-behind associé, comme illustré dans l’exemple de code suivant :

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

Ainsi que le paramètre de la [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) propriété, le code-behind doit également appeler le `InitializeComponent` méthode pour charger et analyser le XAML associé.

## <a name="mainpage-property"></a>Propriété de MainPage

Le `MainPage` propriété sur la `Application` classe définit la page racine de l’application.

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

Le `Application` sous-classe a statique `Properties` dictionnaire qui peut être utilisé pour stocker les données, en particulier pour une utilisation dans les `OnStart`, `OnSleep`, et `OnResume` méthodes. Vous pouvez y accéder à partir de n’importe où dans votre code Xamarin.Forms à l’aide `Application.Current.Properties`.

Le `Properties` dictionnaire utilise un `string` clé et stocke une `object` valeur.

Par exemple, vous pouvez définir un persistants `"id"` propriété n’importe où dans votre code (lorsqu’un élément est sélectionné, dans une page `OnDisappearing` (méthode), ou dans le `OnSleep` méthode) comme suit :

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
> Le `Properties` dictionnaire ne peut sérialiser des types primitifs pour le stockage. Tentative de stocker d’autres types (tels que `List<string>`) peut échouer en mode silencieux.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Persistance

Le `Properties` dictionnaire est automatiquement enregistré à l’appareil.
Les données ajoutées au dictionnaire sera disponibles lorsque l’application est retournée à partir de l’arrière-plan ou même après son redémarrage.

Xamarin.Forms 1.4 a introduit une méthode supplémentaire sur le `Application` classe - `SavePropertiesAsync()` -qui peut être appelé pour conserver de manière proactive la `Properties` dictionnaire. Cela consiste à vous permettent d’enregistrer les propriétés après les mises à jour importantes plutôt que des risques les ne pas mise en route sérialisés en raison d’une panne ou est supprimé par le système d’exploitation.

Vous pouvez trouver des références à l’aide la `Properties` dictionnaire dans le **créer des applications mobiles avec Xamarin.Forms** book chapitres [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), et [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)et le texte associé [exemples](https://github.com/xamarin/xamarin-forms-book-preview-2).



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

Cette classe est ensuite instanciée dans chaque projet spécifique à la plateforme et passée à la `LoadApplication` méthode qui est l’emplacement où le `MainPage` est chargé et affiché à l’utilisateur.
Le code pour chaque plate-forme est indiqué dans les sections suivantes. Les derniers modèles de solution Xamarin.Forms contient déjà ce code, préconfiguré pour votre application.


### <a name="ios-project"></a>iOS projet

IOS `AppDelegate` classe hérite désormais `FormsApplicationDelegate`. Il doit :

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

Le Android `MainActivity` maintenant hérite `FormsApplicationActivity`. Dans le `OnCreate` remplacer le `LoadApplication` méthode est appelée avec une instance de la `App` classe.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> Il existe une version [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) classe de base qui peut être utilisé pour mieux prendre en charge de la conception de matériel Android.
> Celui-ci deviendra le modèle Android par défaut dans les futures, mais vous pouvez suivre [ces instructions](~/xamarin-forms/platform/android/appcompat.md) pour mettre à jour vos applications Android existantes.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Projet Windows universel (UWP) pour Windows 10

Consultez [le programme d’installation Windows projets](~/xamarin-forms/platform/windows/installation/index.md) pour plus d’informations sur la prise en charge de la plateforme Windows universelle dans Xamarin.Forms.

La page principale dans le projet UWP doit hériter de `WindowsPage`. Cela signifie que le XAML et c# pour `MainPage` référence la `FormsApplicationPage` classe, comme indiqué.

Le code XAML utilise un espace de noms personnalisé pour que l’élément racine reflète la `FormsApplicationPage` classe :

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Construction de la c# code-behind doit appeler `LoadApplication` pour créer une instance de votre Xamarin.Forms `App`. Notez qu’il est conseillé d’utiliser explicitement l’espace de noms d’application pour qualifier le `App` , car les applications UWP également ont leurs propres `App` classe sans rapport avec Xamarin.Forms.

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
