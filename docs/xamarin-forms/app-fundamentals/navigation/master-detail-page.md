---
title: Page maître / détails de Xamarin.Forms
description: 'Le Xamarin.Forms MasterDetailPage est une page qui gère deux pages connexes d’informations : une page maître qui présente les éléments et une page de détails qui présente les détails des éléments sur la page maître. Cet article explique comment utiliser un MasterDetailPage et naviguer entre ses pages d’informations.'
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 25f6cf341fcf47d5dc5320f73855bb2a4e29a9e8
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675521"
---
# <a name="xamarinforms-master-detail-page"></a>Page maître / détails de Xamarin.Forms

_Le Xamarin.Forms MasterDetailPage est une page qui gère deux pages connexes d’informations : une page maître qui présente les éléments et une page de détails qui présente les détails des éléments sur la page maître. Cet article explique comment utiliser un MasterDetailPage et naviguer entre ses pages d’informations._

## <a name="overview"></a>Vue d'ensemble

Une page maître affiche généralement une liste d’éléments, comme indiqué dans les captures d’écran suivante :

[![](master-detail-page-images/masterpage-components.png "Composants de la Page maître")](master-detail-page-images/masterpage-components-large.png#lightbox "composants de la Page maître")

L’emplacement de la liste d’éléments est identique sur chaque plateforme, et en sélectionnant un des éléments permet d’accéder à la page de détails correspondante. En outre, la page maître comporte également une barre de navigation qui contient un bouton qui peut être utilisé pour accéder à la page de détail active :

- Sur iOS, la barre de navigation est présente en haut de la page et possède un bouton qui accède à la page de détails. En outre, la page de détails active accessibles par navigation en faisant glisser vers la page maître vers la gauche.
- Sur Android, la barre de navigation est présente en haut de la page et affiche un titre, une icône et un bouton qui permet d’accéder à la page de détails. L’icône est définie dans le `[Activity]` attribut décore le `MainActivity` classe dans le projet spécifique à la plateforme Android. En outre, la page de détails active accessibles par navigation en faisant glisser vers la page maître vers la gauche, en appuyant sur la page de détails à l’extrême droite de l’écran et en appuyant sur la *retour* bouton en bas de l’écran.
- Sur la plateforme de Windows universelle (UWP), la barre de navigation est présente en haut de la page et possède un bouton qui accède à la page de détails.

Une détail page affiche les données qui correspondant à l’élément sélectionné sur le contrôleur de page, et les principaux composants de la page de détails sont affichés dans les captures d’écran suivante :

![](master-detail-page-images/detailpage-components.png "Composants de la Page de détails")

La page de détails contient une barre de navigation, dont le contenu est dépendante de la plateforme :

- Sur iOS, la barre de navigation est présente en haut de la page et affiche un titre et comporte un bouton qui renvoie à la page maître, sous réserve que l’instance de la page de détails est encapsulée dans la [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance. En outre, peut être retournée à la page maître en faisant glisser vers la page de détails à droite.
- Sur Android, une barre de navigation est présente en haut de la page et affiche un titre, une icône et un bouton qui renvoie à la page maître. L’icône est définie dans le `[Activity]` attribut décore le `MainActivity` classe dans le projet spécifique à la plateforme Android.
- Sur UWP, la barre de navigation est présente en haut de la page affiche un titre et possède un bouton qui renvoie à la page maître.

### <a name="navigation-behavior"></a>Comportement de navigation

Le comportement de l’expérience de navigation entre les pages maître et détail est dépendant de la plate-forme :

- Sur iOS, la page de détails *diapositives* vers la droite, comme les diapositives de page maître de gauche et la partie gauche du code de page est toujours visible.
- Sur Android, les pages de détail et maître sont *à superposer* sur eux.
- Sur UWP, les pages de détail et maître sont *permutées*.

Un comportement similaire est appliqué en mode paysage, sauf que la page maître sur iOS et Android a une largeur similaire en tant que la page maître en mode portrait, par conséquent, plus de la page de détails seront visibles.

Pour plus d’informations sur le contrôle le comportement de navigation, consultez [contrôler le comportement d’affichage de Page détail](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Création d’un MasterDetailPage

Un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) contient [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) et [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) propriétés qui sont tous deux de type [ `Page` ](xref:Xamarin.Forms.Page), qui sont utilisés pour obtenir et définir les pages maître et détail respectivement.

> [!IMPORTANT]
> Un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) est conçu pour être une page de la racine et l’utiliser comme page enfant dans d’autres types de page peut entraîner un comportement inattendu et incohérent. En outre, il est recommandé de la page maître un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) doit toujours être un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance, et que la page de détails doit uniquement être remplie avec [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), et `ContentPage` instances. Cela vous aidera à garantir une expérience utilisateur cohérente sur toutes les plateformes.

L’exemple de code XAML suivant montre un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) qui définit le [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) et [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) propriétés :

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

L’exemple de code suivant montre l’équivalent [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) créé en c# :

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

Le [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) propriété est définie sur une [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance. Le [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) propriété est définie sur une [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) contenant un `ContentPage` instance.

### <a name="creating-the-master-page"></a>Création de la Page maître

L’exemple de code XAML suivant illustre la déclaration de la `MasterPage` objet, qui est référencé par le biais du [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) propriété :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

La page se compose d’un [ `ListView` ](xref:Xamarin.Forms.ListView) qui est rempli avec des données dans XAML en définissant son [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) propriété vers un tableau de `MasterPageItem` instances. Chaque `MasterPageItem` définit `Title`, `IconSource`, et `TargetType` propriétés.

Un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) est affectée à la [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) propriété, pour afficher chaque `MasterPageItem`. Le `DataTemplate` contient un [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) qui se compose d’un [ `Image` ](xref:Xamarin.Forms.Image) et un [ `Label` ](xref:Xamarin.Forms.Label). Le [ `Image` ](xref:Xamarin.Forms.Image) affiche le `IconSource` valeur de propriété et le [ `Label` ](xref:Xamarin.Forms.Label) affiche le `Title` valeur de propriété, pour chaque `MasterPageItem`.

La page a son [ `Title` ](xref:Xamarin.Forms.Page.Title) et [ `Icon` ](xref:Xamarin.Forms.Page.Icon) ensemble de propriétés. L’icône s’affiche sur la page de détails, à condition que la page de détails une barre de titre. Cela doit être activé sur iOS en encapsulant l’instance de la page de détails dans un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance.

> [!NOTE]
> Le [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) page doit avoir son [ `Title` ](xref:Xamarin.Forms.Page.Title) propriété définie, ou une exception se produit.

L’exemple de code suivant montre la page équivalente créée en c# :

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

Les captures d’écran suivantes montrent la page maître sur chaque plateforme :

![](master-detail-page-images/masterpage.png "Exemple de Page maître")

### <a name="creating-and-displaying-the-detail-page"></a>Création et affichage de la Page de détails

Le `MasterPage` instance contient un `ListView` propriété qui expose ses [ `ListView` ](xref:Xamarin.Forms.ListView) instance afin que le `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) instance peut inscrire un Gestionnaire d’événements pour gérer le [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement. Cela permet la `MainPage` instance utilisée pour définir le [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) propriété à la page qui représente le texte sélectionné `ListView` élément. L’exemple de code suivant montre le Gestionnaire d’événements :

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

Le `OnItemSelected` méthode effectue les actions suivantes :

- Il récupère le [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) à partir de la [ `ListView` ](xref:Xamarin.Forms.ListView) de l’instance et fourni qu’il n’est pas `null`, définit la page de détails sur une nouvelle instance du type de page stockée dans le `TargetType`propriété de la `MasterPageItem`. Le type de page est encapsulé dans un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance pour vous assurer que l’icône référencé via le [ `Icon` ](xref:Xamarin.Forms.Page.Icon) propriété sur le `MasterPage` est affiché sur la page de détails dans iOS.
- L’élément sélectionné dans le [ `ListView` ](xref:Xamarin.Forms.ListView) a la valeur `null` pour s’assurer qu’aucune de la `ListView` éléments sont sélectionnés prochaine le `MasterPage` est présentée.
- La page de détails est présentée à l’utilisateur en définissant le [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) propriété `false`. Cette propriété détermine si la page maître ou détail est présentée. Il doit être défini sur `true` pour afficher la page maître et à `false` pour afficher la page de détails.

L’émission des captures d’écran suivante le `ContactPage` page de détails, qui est affichée une fois qu’il est sélectionné sur la page maître :

![](master-detail-page-images/detailpage.png "Exemple de Page de détail")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Contrôler le comportement d’affichage des pages de détail

Comment la [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) gère les pages maître et détail varie selon que l’application est exécutée sur un téléphone ou tablette, l’orientation de l’appareil et la valeur de la [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) propriété. Cette propriété détermine le mode d’affichage de la page de détails. Ses valeurs possibles sont :

- **Par défaut** – les pages sont affichées à l’aide de la valeur par défaut de la plateforme.
- **Menu** : la page de détails couvre ou couvre partiellement la page maître.
- **Fractionnement** : la page maître s’affiche sur la gauche et la page de détails se trouve à droite.
- **SplitOnLandscape** – un écran de fractionnement est utilisé quand l’appareil est en mode paysage.
- **SplitOnPortrait** – un écran de fractionnement est utilisé quand l’appareil est en mode portrait.

L’exemple de code XAML suivant montre comment définir le [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) propriété sur un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

L’exemple de code suivant montre l’équivalent [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) créé en c# :

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Toutefois, la valeur de la [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) propriété affecte uniquement les applications qui s’exécutent sur des tablettes ou le bureau. Applications qui s’exécutent toujours sur les téléphones ont le *menu* comportement.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) et naviguer entre ses pages d’informations. Xamarin.Forms `MasterDetailPage` est une page qui gère deux pages d’informations liées à une page maître qui présente les éléments et une page de détails qui présente les détails des éléments sur la page maître.


## <a name="related-links"></a>Liens associés

- [Types de page](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
