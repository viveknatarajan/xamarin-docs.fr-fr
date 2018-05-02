---
title: Introduction à Xamarin.Forms
description: Xamarin.Forms est une abstraction du kit de ressources d’interface utilisateur en mode natif multiplateforme qui permet aux développeurs de créer facilement des interfaces utilisateur qui peuvent être partagées entre Android, iOS et la plateforme Windows universelle. Les interfaces utilisateur sont rendues à l’aide des contrôles natifs de la plateforme cible, ce qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme. Cet article présente Xamarin.Forms et décrit comment l’utiliser pour l’écriture d’applications.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 9f7c9d1b410d9d1d699644148903fdc6cfeec4fd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Introduction à Xamarin.Forms

_Xamarin.Forms est une abstraction du kit de ressources d’interface utilisateur en mode natif multiplateforme qui permet aux développeurs de créer facilement des interfaces utilisateur qui peuvent être partagées entre Android, iOS, Windows et la plateforme Windows universelle. Les interfaces utilisateur sont rendues à l’aide des contrôles natifs de la plateforme cible, ce qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme. Cet article présente Xamarin.Forms et décrit comment bien démarrer avec celui-ci pour écrire des applications._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms est un framework qui permet aux développeurs de créer rapidement des interfaces utilisateur multiplateformes. Il fournit sa propre abstraction pour l’interface utilisateur qui sera rendue à l’aide de contrôles natifs sur iOS, Android ou la plateforme Windows universelle (UWP). Cela signifie que les applications peuvent partager une grande partie de leur code d’interface utilisateur tout en conservant l’apparence native de la plateforme cible.

Xamarin.Forms tient compte du prototypage rapide d’applications qui peuvent évoluer au fil du temps vers des applications complexes. Étant donné que les applications Xamarin.Forms sont des applications natives, elles ne présentent pas les limitations d’autres kits de ressources comme le sandboxing de navigateur, des API limitées ou des performances médiocres. Les applications écrites à l’aide de Xamarin.Forms sont en mesure d’utiliser toutes les API ou fonctionnalités de la plateforme sous-jacente, notamment mais pas exclusivement CoreMotion, PassKit et StoreKit sur iOS, NFC et Google Play Services sur Android, et les vignettes sur Windows. En outre, il est possible de créer des applications avec des parties de l’interface utilisateur créées avec Xamarin.Forms et d’autres créées à l’aide du kit de ressources d’interface utilisateur native.

Les applications Xamarin.Forms sont structurées de la même façon que les applications multiplateformes classiques. L’approche la plus courante consiste à utiliser des [bibliothèques portables](~/cross-platform/app-fundamentals/pcl.md) ou des [projets partagés](~/cross-platform/app-fundamentals/shared-projects.md) pour héberger le code partagé, puis à créer des applications spécifiques à la plateforme qui consomment le code partagé.

Il existe deux techniques pour créer des interfaces utilisateur dans Xamarin.Forms. La première technique consiste à créer des interfaces utilisateur entièrement avec du code source C#. La seconde technique consiste à utiliser XAML (*Extensible Application Markup Language*), un langage de balisage déclaratif qui permet de décrire des interfaces utilisateur. Pour plus d’informations sur XAML, consultez [Notions de base XAML](~/xamarin-forms/xaml/xaml-basics/index.md).

Cet article explique les notions de base du framework Xamarin.Forms et aborde les sujets suivants :

-  [Examen d’une application Xamarin.Forms](#Examining_A_Xamarin.Forms_Application).
-  [Utilisation des contrôles et des pages Xamarin.Forms](#Views_and_Layouts).
-  [Affichage d’une liste de données](#Lists_in_Xamarin.Forms).
-  [Configuration de la liaison de données](#Data_Binding).
-  [Navigation entre les pages](#Navigation).
-  [Étapes suivantes](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Examen d’une application Xamarin.Forms

Dans Visual Studio pour Mac et Visual Studio, le modèle d’application Xamarin.Forms par défaut crée la solution Xamarin.Forms la plus simple possible, qui affiche du texte pour l’utilisateur. Si vous exécutez l’application, elle doit ressembler aux captures d’écran suivantes :

[![](introduction-to-xamarin-forms-images/image05-sml.png "Application Xamarin.Forms par défaut")](introduction-to-xamarin-forms-images/image05.png#lightbox "Application Xamarin.Forms par défaut")

Chaque écran dans les captures d’écran correspond à un élément *Page* dans Xamarin.Forms. Un élément [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) représente une *Activité* dans Android, un *Contrôleur d’affichage* dans iOS ou une *Page* dans la plateforme Windows universelle (UWP). L’exemple dans les captures d’écran ci-dessus instancie un objet [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) et l’utilise pour afficher un élément [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/).

Pour optimiser la réutilisation du code de démarrage, les applications Xamarin.Forms ont une seule classe nommée `App` qui est chargée de l’instanciation du premier élément [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) affiché. Un exemple de la classe `App` est visible dans le code suivant :

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

Ce code instancie un nouvel objet [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) qui affiche un seul élément [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) centré à la fois verticalement et horizontalement dans la page.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Lancement de la page Xamarin.Forms initiale sur chaque plateforme

Pour utiliser cet élément [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) dans une application, chaque application de plateforme doit initialiser le framework Xamarin.Forms et fournir une instance de l’objet [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) lors de son démarrage. Cette étape d’initialisation varie d’une plateforme à l’autre, et est abordée dans les sections suivantes.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Pour lancer la page Xamarin.Forms initiale dans iOS, le projet de plateforme inclut la classe `AppDelegate` qui hérite de la classe `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate`, comme illustré dans l’exemple de code suivant :

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

La valeur de remplacement `FinishedLoading` initialise le framework Xamarin.Forms en appelant la méthode `Init`. Cela entraîne le chargement de l’implémentation spécifique à iOS de Xamarin.Forms dans l’application avant que le contrôleur d’affichage racine ne soit défini par l’appel à la méthode `LoadApplication`.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Pour lancer la page Xamarin.Forms initiale dans Android, le projet de plateforme inclut du code qui crée un élément `Activity` avec l’attribut `MainLauncher`, l’activité héritant de la classe `FormsApplicationActivity`, comme illustré dans l’exemple de code suivant :

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

La valeur de remplacement `OnCreate` initialise le framework Xamarin.Forms en appelant la méthode `Init`. Cela entraîne le chargement de l’implémentation spécifique à Android de Xamarin.Forms dans l’application avant que l’application Xamarin.Forms ne soit chargée.

#### <a name="universal-windows-platform"></a>Plateforme Windows universelle

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
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

L’application Xamarin.Forms est chargée avec la méthode `LoadApplication`.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Vues et dispositions

Il existe quatre principaux groupes de contrôles permettant de créer l’interface utilisateur d’une application Xamarin.Forms.

1. **Pages** : les pages Xamarin.Forms représentent les écrans de l’application mobile multiplateforme. Pour plus d’informations sur les pages, consultez [Pages Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Dispositions** : les dispositions Xamarin.Forms sont des conteneurs utilisés pour composer des vues dans des structures logiques. Pour plus d’informations sur les dispositions, consultez [Dispositions Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Vues** : les vues Xamarin.Forms sont les contrôles affichés sur l’interface utilisateur, tels que les étiquettes, les boutons et les zones d’entrée de texte. Pour plus d’informations sur les vues, consultez [Vues Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Cellules** : les cellules Xamarin.Forms sont des éléments spécialisés utilisés pour des éléments dans une liste et décrivent comment chaque élément dans une liste doit être dessiné. Pour plus d’informations sur les cellules, consultez [Cellules Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

Lors de l’exécution, chaque contrôle est mappé à son équivalent natif, ce qui sera affiché.

Les contrôles sont hébergés à l’intérieur d’une disposition. La classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), qui implémente une disposition couramment utilisée, va maintenant être examinée.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

La classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) simplifie le développement d’applications multiplateformes en disposant automatiquement les contrôles sur l’écran, quelle que soit la taille de l’écran. Les éléments enfants sont placés les uns après les autres, horizontalement ou verticalement dans l’ordre dans lequel ils ont été ajoutés. La quantité d’espace utilisée par la classe `StackLayout` dépend de la définition des propriétés [`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/), mais `StackLayout` essaie par défaut d’utiliser la totalité de l’écran.

Le code XAML suivant montre un exemple d’utilisation d’une classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) pour disposer trois contrôles [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

Par défaut, la classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) suppose une orientation verticale, comme illustré dans les captures d’écran suivantes :

[![](introduction-to-xamarin-forms-images/image09-sml.png "StackLayout verticale")](introduction-to-xamarin-forms-images/image09.png#lightbox "StackLayout verticale")

L’orientation de la classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) peut être remplacée par une orientation horizontale, comme illustré dans l’exemple de code XAML suivant :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

Les captures d’écran suivantes illustrent la disposition obtenue :

[![](introduction-to-xamarin-forms-images/image10-sml.png "StackLayout horizontale")](introduction-to-xamarin-forms-images/image10.png#lightbox "StackLayout horizontale")

La taille des contrôles peut être définie via les propriétés `HeightRequest` et `WidthRequest`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

Les captures d’écran suivantes illustrent la disposition obtenue :

[![](introduction-to-xamarin-forms-images/image11-sml.png "StackLayout horizontale avec LayoutOptions")](introduction-to-xamarin-forms-images/image11.png#lightbox "StackLayout horizontale avec LayoutOptions")

Pour plus d’informations sur la classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), consultez [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Listes dans Xamarin.Forms

Le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est chargé de l’affichage d’une collection d’éléments à l’écran, chaque élément dans `ListView` étant contenu dans une cellule unique. Par défaut, un contrôle `ListView` utilise le modèle [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) intégré et affiche une seule ligne de texte.

L’exemple de code suivant illustre un contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) simple :

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

La capture d’écran suivante montre le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) obtenu :

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Pour plus d’informations sur le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), consultez [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Liaison à une classe personnalisée

Le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) peut également afficher des objets personnalisés à l’aide du modèle [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) par défaut.

L’exemple de code suivant illustre la classe `TodoItem` :

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

Le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) peut être rempli comme illustré dans l’exemple de code suivant :

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Une liaison peut être créée pour définir la propriété `TodoItem` qui est affichée par le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), comme illustré dans l’exemple de code suivant :

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Cette opération crée une liaison qui spécifie le chemin d’accès à la propriété `TodoItem.Name` et aboutit à la capture d’écran précédemment affichée.

Pour plus d’informations sur la liaison à une classe personnalisée, consultez [Sources de données ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Sélection d’un élément dans un contrôle ListView

Pour répondre à un utilisateur qui touche une cellule dans un contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), l’événement [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) doit être géré, comme illustré dans l’exemple de code suivant :

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Quand elle est contenue dans une classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), la méthode [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) peut être utilisée pour ouvrir une nouvelle page avec navigation vers l’arrière intégrée. L’événement [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) peut accéder à l’objet qui a été associé à la cellule via la propriété [`e.SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/), le lier à une nouvelle page et afficher cette page à l’aide de `PushAsync`, comme illustré dans l’exemple de code suivant :

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Chaque plateforme implémente la navigation vers l’arrière intégrée à sa manière. Pour plus d’informations, consultez [Navigation](#Navigation).

Pour plus d’informations sur la sélection de [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), consultez [Interactivité de ListView](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Personnalisation de l’apparence d’une cellule

L’apparence d’une cellule peut être personnalisée en sous-classant la classe [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) et en définissant le type de cette classe sur la propriété [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) du contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).

La cellule indiquée dans la capture d’écran suivante est composée d’un élément [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) et de deux contrôles [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) :

 ![](introduction-to-xamarin-forms-images/image14.png "Apparence de cellule personnalisée ListView")

Pour créer cette disposition personnalisée, la classe [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) doit être sous-classée, comme illustré dans l’exemple de code suivant :

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

Le code exécute les tâches suivantes :

-  Il ajoute un contrôle [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) et le lie à la propriété `ImageUri` de l’objet `Employee`. Pour plus d’informations sur la liaison de données, consultez [Liaison de données](#Data_Binding).
-  Il crée une classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) avec une orientation verticale pour contenir les deux contrôles [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/). Les contrôles `Label` sont liés aux propriétés `DisplayName` et `Twitter` de l’objet `Employee`.
-  Il crée une classe [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) qui va héberger les éléments [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) et `StackLayout` existants. Il dispose ses enfants à l’aide d’une orientation horizontale.

Une fois que la cellule personnalisée a été créée, elle peut être consommée par un contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) en l’enveloppant dans un élément [`DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), comme illustré dans l’exemple de code suivant :

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Ce code fournit une liste `List` d’éléments `Employee` au contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Chaque cellule est rendue à l’aide de la classe `EmployeeCell`. Le contrôle `ListView` passe l’objet `Employee` à `EmployeeCell` comme [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Pour plus d’informations sur la personnalisation de l’apparence d’une cellule, consultez [Apparence de cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Utilisation de XAML pour créer et personnaliser une liste

L’équivalent XAML du contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) dans la section précédente est illustré dans l’exemple de code suivant :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Ce code XAML définit un élément [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) qui contient un contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). La source de données du contrôle `ListView` est définie via l’attribut [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/). La disposition de chaque ligne dans l’attribut `ItemsSource` est définie dans l’élément [`ListView.ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/).

<a name="Data_Binding" />

## <a name="data-binding"></a>Liaison de données

La liaison de données connecte deux objets, appelés la *source* et la *cible*. L’objet *source* fournit les données. L’objet *cible* consomme (et affiche souvent) les données de l’objet source. Par exemple, un objet [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (objet *cible*) lie couramment sa propriété [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) à une propriété `string` publique dans un objet *source*. Le diagramme suivant illustre la relation de liaison :

![](introduction-to-xamarin-forms-images/data-binding.png "Liaison de données")

Le principal avantage de la liaison de données est que vous n’avez plus à vous soucier de la synchronisation des données entre les vues et la source de données. Les modifications apportées à l’objet *source* sont automatiquement envoyées à l’objet *cible* dans les coulisses par le framework de liaison, et les modifications apportées à l’objet cible peuvent éventuellement être renvoyées à l’objet *source*.

L’établissement de la liaison de données est un processus en deux étapes :

- La propriété [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de l’objet *cible* doit être définie sur la *source*.
- Une liaison doit être établie entre la *cible* et la *source*. En XAML, cette opération est effectuée en utilisant l’extension de balisage [`Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/). En C#, cette opération est effectuée par la méthode [`SetBinding`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/).

Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Le code suivant montre un exemple de liaison de données en XAML :

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Une liaison entre la propriété [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) et la propriété `FirstName` de l’objet *source* est établie. Les modifications apportées au contrôle `Entry` sont automatiquement propagées à l’objet `employeeToDisplay`. De même, si des modifications sont apportées à la propriété `employeeToDisplay.FirstName`, le moteur de liaison Xamarin.Forms met également à jour le contenu du contrôle `Entry`. Il s’agit là d’une *liaison bidirectionnelle*. Pour qu’une liaison bidirectionnelle fonctionne, la classe de modèle doit implémenter l’interface `INotifyPropertyChanged`.

Bien que la propriété [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de la classe `EmployeeDetailPage` puisse être définie en XAML, elle est définie ici dans le code-behind en une instance d’un objet `Employee` :

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Alors que la propriété [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de chaque objet *cible* peut être définie individuellement, cela n’est pas nécessaire. `BindingContext` est une propriété spéciale qui est héritée par tous ses enfants. Par conséquent, quand la propriété `BindingContext` sur l’élément [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) est définie sur une instance de l’objet `Employee`, tous les enfants de l’élément `ContentPage` ont la même propriété `BindingContext`et peuvent établir une liaison aux propriétés publiques de l’objet `Employee`.

### <a name="c35"></a>C&#35;

Le code suivant montre un exemple de liaison de données en C# :

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

Le constructeur [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) reçoit une instance d’un objet `Employee` et définit la propriété [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) sur l’objet auquel établir la liaison. Un contrôle [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) est instancié, et la liaison entre la propriété [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) et la propriété `FirstName` de l’objet *source* est définie. Les modifications apportées au contrôle `Entry` sont automatiquement propagées à l’objet `employeeToDisplay`. De même, si des modifications sont apportées à la propriété `employeeToDisplay.FirstName`, le moteur de liaison Xamarin.Forms met également à jour le contenu du contrôle `Entry`. Il s’agit là d’une *liaison bidirectionnelle*. Pour qu’une liaison bidirectionnelle fonctionne, la classe de modèle doit implémenter l’interface `INotifyPropertyChanged`.

La méthode `SetBinding` accepte deux paramètres. Le premier paramètre spécifie des informations sur le type de liaison. Le second paramètre est utilisé pour fournir des informations sur les éléments à lier ou le mode de liaison. Le second paramètre est, dans la plupart des cas, simplement une chaîne contenant le nom de propriété sur [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). La syntaxe suivante permet d’effectuer directement la liaison à `BindingContext` :

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

La syntaxe du point indique à Xamarin.Forms d’utiliser l’élément [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) comme source de données au lieu d’une propriété sur `BindingContext`. Cela est utile quand l’élément `BindingContext` est un type simple, comme `string` ou `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Notification de modification de propriété

Par défaut, l’objet *cible* reçoit uniquement la valeur de l’objet *source* lors de la création de la liaison. Pour que l’interface utilisateur reste synchronisée avec la source de données, il doit exister un moyen d’indiquer à l’objet *cible* quand l’objet *source* a été modifié. Ce mécanisme est fourni par l’interface `INotifyPropertyChanged`. L’implémentation de cette interface fournit des notifications à tous les contrôles liés aux données quand la valeur de la propriété sous-jacente change.

Les objets qui implémentent `INotifyPropertyChanged` doivent déclencher l’événement `PropertyChanged` quand l’une de leurs propriétés est mise à jour avec une nouvelle valeur, comme illustré dans l’exemple de code suivant :

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Quand la propriété `MyObject.FirstName` change, la méthode `OnPropertyChanged` est appelée, ce qui déclenche l’événement `PropertyChanged`. Pour éviter des déclenchements d’événements inutiles, l’événement `PropertyChanged` n’est pas déclenché si la valeur de propriété ne change pas.

Notez que, dans la méthode `OnPropertyChanged`, le paramètre `propertyName` est orné de l’attribut `CallerMemberName`. Cela garantit que, si la méthode `OnPropertyChanged` est appelée avec une valeur `null`, l’attribut `CallerMemberName` fournit le nom de la méthode qui a appelé `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Xamarin.Forms propose plusieurs expériences de navigation entre les pages différentes, selon le type de [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) utilisé. Pour les instances de [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), il existe deux expériences de navigation :

- [Navigation hiérarchique](#Hierarchical_Navigation)
- [Navigation modale](#Modal_Navigation)

Les classes [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) et [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) fournissent d’autres expériences de navigation. Pour plus d’informations, consultez [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Navigation hiérarchique

La classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) propose une expérience de navigation hiérarchique où l’utilisateur est en mesure de parcourir les pages, vers l’avant et vers l’arrière, comme il le souhaite. La classe implémente la navigation comme une pile d’objets [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) LIFO (dernier entré, premier sorti).

Dans la navigation hiérarchique, la classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est utilisée pour parcourir une pile d’objets [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Pour passer d’une page à une autre, une application envoie une nouvelle page dans la pile de navigation, où elle devient la page active. Pour revenir à la page précédente, l’application dépile la page actuelle, et la nouvelle page tout en haut devient la page active.

La première page ajoutée à une pile de navigation est appelée la page *racine* de l’application et l’exemple de code suivant illustre le déroulement de l’opération :

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Pour accéder à la page `LoginPage`, il est nécessaire d’appeler la méthode [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) sur la propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) de la page active, comme illustré dans l’exemple de code suivant :

```csharp
await Navigation.PushAsync(new LoginPage());
```

Le nouvel objet `LoginPage` est ainsi envoyé dans la pile de navigation, où il devient la page active.

La page active peut être retirée de la pile de navigation en appuyant sur le bouton *Précédent* sur l’appareil, qu’il s’agisse d’un bouton physique sur l’appareil ou d’un bouton à l’écran. Pour revenir par programmation à la page précédente, l’instance de `LoginPage` doit appeler la méthode [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/), comme illustré dans l’exemple de code suivant :

```csharp
await Navigation.PopAsync();
```

Pour plus d’informations sur la navigation hiérarchique, consultez [Navigation hiérarchique](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Navigation modale

Xamarin.Forms assure la prise en charge des pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée.

Une page modale peut être de tous les types [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) pris en charge par Xamarin.Forms. Pour afficher une page modale, l’application l’envoie dans la pile de navigation, où elle devient la page active. Pour revenir à la page précédente, l’application dépile la page actuelle, et la nouvelle page tout en haut devient la page active.

Les méthodes de navigation modale sont exposées par la propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) sur n’importe quel type dérivé [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). La propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) expose également une propriété [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) à partir de laquelle les pages modales dans la pile de navigation peuvent être obtenues. Toutefois, il n’existe aucun concept de manipulation de pile modale ni d’ajout à la page racine dans la navigation modale. En effet, ces opérations ne sont pas universellement prises en charge sur les plateformes sous-jacentes.

> [!NOTE]
> Aucune instance de [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) n’est requise pour la navigation entre les pages modales.

Pour accéder sous forme modale à la page `LoginPage`, il est nécessaire d’appeler la méthode [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) sur la propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) de la page active, comme illustré dans l’exemple de code suivant :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

L’instance de `LoginPage` est ainsi envoyée dans la pile de navigation, où elle devient la page active.

La page active peut être retirée de la pile de navigation en appuyant sur le bouton *Précédent* sur l’appareil, qu’il s’agisse d’un bouton physique sur l’appareil ou d’un bouton à l’écran. Pour revenir par programmation à la page d’origine, l’instance de `LoginPage` doit appeler la méthode [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/), comme illustré dans l’exemple de code suivant :

```csharp
await Navigation.PopModalAsync();
```

L’instance de `LoginPage` est ainsi retirée de la pile de navigation, et la nouvelle page tout en haut devient la page active.

Pour plus d’informations sur la navigation modale, consultez [Pages modales](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Étapes suivantes

Cet article d’introduction doit vous permettre de démarrer l’écriture d’applications Xamarin.Forms. Les étapes suggérées ci-dessous incluent la découverte des fonctionnalités suivantes :

- Les modèles de contrôle permettent de créer et recréer facilement des thèmes pour les pages d’application lors de l’exécution. Pour plus d’informations, consultez [Modèles de contrôle](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Les modèles de données permettent de définir la présentation des données sur les contrôles pris en charge. Pour plus d’informations, consultez [Modèles de données](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Le code partagé permet d’accéder aux fonctionnalités natives par le biais de la classe [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/). Pour plus d’informations, consultez [Accès aux fonctionnalités natives avec DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Xamarin.Forms inclut un service de messagerie simple pour envoyer et recevoir des messages, ce qui réduit le couplage entre les classes. Pour plus d’informations, consultez [Publier et s’abonner avec MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Chaque page, disposition et contrôle est restitué différemment sur chaque plateforme avec une classe `Renderer` qui crée ensuite un contrôle natif, le dispose à l’écran et ajoute le comportement spécifié dans le code partagé. Les développeurs peuvent implémenter leurs propres classes `Renderer` pour personnaliser l’apparence et/ou le comportement d’un contrôle. Pour plus d’informations, consultez [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Les effets permettent également de personnaliser les contrôles natifs sur chaque plateforme. Les effets sont créés dans des projets spécifiques à la plateforme en sous-classant le contrôle [`PlatformEffect`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/), et sont utilisés en les attachant à un contrôle Xamarin.Forms approprié. Pour plus d’informations, consultez [Effets](~/xamarin-forms/app-fundamentals/effects/index.md).

Vous pouvez également consulter « Creating Mobile Apps with Xamarin.Forms », un livre très intéressant écrit par Charles Petzold, pour découvrir plus en détail Xamarin.Forms. Pour plus d’informations, consultez [Création d’applications mobiles avec Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Récapitulatif

Cet article a présenté Xamarin.Forms et décrit comment l’utiliser pour l’écriture d’applications. Xamarin.Forms est une abstraction du kit de ressources d’interface utilisateur en mode natif multiplateforme qui permet aux développeurs de créer facilement des interfaces utilisateur qui peuvent être partagées entre Android, iOS et la plateforme Windows universelle. Les interfaces utilisateur sont rendues à l’aide des contrôles natifs de la plateforme cible, ce qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme.


## <a name="related-links"></a>Liens associés

- [Notions de base XAML](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Informations de référence sur les contrôles](~/xamarin-forms/user-interface/controls/index.md)
- [Interface utilisateur](~/xamarin-forms/user-interface/index.md)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Exemples pour bien démarrer](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [Autoformation gratuite (vidéo)](https://university.xamarin.com/self-guided)
- [Classeur iOS Hello, Xamarin.Forms](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
