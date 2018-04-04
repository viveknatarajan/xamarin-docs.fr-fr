---
title: Page à onglets
description: Le Xamarin.Forms TabbedPage se compose d’une liste des onglets et une plus grande zone de détails, avec chaque onglet du chargement du contenu dans la zone de détails. Cet article explique comment utiliser un TabbedPage pour naviguer dans une collection de pages.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 11287d38ec0e01e068ca385c92e6a6efdc323aeb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-page"></a>Page à onglets

_Le Xamarin.Forms TabbedPage se compose d’une liste des onglets et une plus grande zone de détails, avec chaque onglet du chargement du contenu dans la zone de détails. Cet article explique comment utiliser un TabbedPage pour naviguer dans une collection de pages._

## <a name="overview"></a>Vue d'ensemble

L’émission des captures d’écran suivante un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) sur chaque plateforme :

![](tabbed-page-images/tab1.png "Exemple de TabbedPage")

Les captures d’écran suivantes se concentrent sur le format de tabulation sur chaque plateforme :

![](tabbed-page-images/tabbedpage-components.png "TabbedPage onglet composants")

La disposition d’un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)et ses onglets, dépend de la plateforme :

- Sur iOS, la liste d’onglets apparaît au bas de l’écran, et la zone de détails est ci-dessus. Chaque onglet contient également une image d’icône qui doit être un 30 x 30 PNG avec une transparence de résolution normales, 60 x 60 pour haute résolution et 90 x 90 pour iPhone 6 Plus la résolution. S’il y a plus de cinq onglets, un *plus* onglet s’affiche, qui peut être utilisé pour accéder aux onglets supplémentaires. Pour plus d’informations sur le chargement des images dans une application de Xamarin.Forms, consultez [utilisation des Images](~/xamarin-forms/user-interface/images.md). Pour plus d’informations sur la configuration requise d’icône, consultez [création d’Applications avec onglet](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Notez que la `TabbedRenderer` pour iOS a un substituable `GetIcon` méthode qui peut être utilisé pour charger des icônes de l’onglet à partir d’une source spécifiée. Ce remplacement permet d’utiliser les images SVG sous forme d’icônes sur un `TabbedPage`. En outre, les versions sélectionnées et d’une icône peuvent être fournies.

- Sur Android, la liste des onglets s’affiche en haut de l’écran, et la zone de détail est inférieur. Les noms d’onglet sont en majuscules automatiquement et l’utilisateur peut faire défiler vers la collection des onglets si elles sont trop nombreuses pour s’ajuster à l’écran.

    > [!NOTE]
  > Notez que lorsque vous utilisez AppCompat sur Android, chaque onglet affiche également une icône. En outre, le `TabbedPageRenderer` pour Android AppCompat a un substituable `SetTabIcon` méthode qui peut être utilisé pour charger des icônes de l’onglet depuis une `Drawable`. Ce remplacement permet d’utiliser les images SVG sous forme d’icônes sur un `TabbedPage`.

- Sur Windows Phone, la liste des onglets s’affiche en haut de l’écran, et la zone de détail est inférieur. L’onglet noms sont automatiquement convertis en minuscule et l’utilisateur peut faire défiler vers la collection des onglets si elles sont trop nombreuses pour s’ajuster à l’écran.
- Sur une tablette Windows-formats, les onglets ne sont pas toujours visibles et les utilisateurs doivent faites défiler vers le bas (ou avec le bouton droit, s’ils ont une souris attachée) pour afficher les onglets dans un `TabbedPage` (comme indiqué ci-dessous).

![](tabbed-page-images/windows-tabs.png "Onglets TabbedPage sur Windows")

## <a name="creating-a-tabbedpage"></a>Création d’un TabbedPage

Deux approches peuvent être utilisés pour créer un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Remplir](#Populating_a_TabbedPage_with_a_Page_Collection) le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) avec une collection d’enfants [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objets, comme une collection de [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances.
- [Affecter](#Populating_a_TabbedPage_with_a_Template) une collection à le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) propriété et affecter un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) pour retourner les pages de propriété objets de la collection.

Avec les deux approches, le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) affiche chaque page comme l’utilisateur sélectionne chaque onglet.

> [!NOTE]
> Il est recommandé qu’un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) doit être rempli avec [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) et [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)uniquement des instances. Cela permettra de garantir une expérience utilisateur cohérente sur toutes les plateformes.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Remplissage d’un TabbedPage avec un ensemble de Page

L’exemple de code XAML suivant montre un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) construit en le remplissant avec une collection d’enfants [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objets :

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

L’exemple de code suivant montre l’équivalent [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) créé en c# :

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

Le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) est remplie avec deux enfants [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objets. Le premier enfant est un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance et le deuxième onglet est un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) contenant un `ContentPage` instance.

> [!NOTE]
> Le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `TabbedPage` contient trop d’éléments enfants.

L’émission des captures d’écran suivante le `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, ce qui est indiquée sur le *aujourd'hui* onglet :

![](tabbed-page-images/today-page.png "ContentPage dans un TabbedPage")

En sélectionnant le *planification* onglet affiche les `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, ce qui est encapsulée dans un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) d’instance et est indiqué dans le capture d’écran suivante :

![](tabbed-page-images/schedule-page.png "NavigationPage dans un TabbedPage")

Pour plus d’informations sur la disposition d’un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), consultez [effectuant la Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Bien qu’il soit possible de placer un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) dans un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), il n’est pas recommandé de placer un `TabbedPage` dans un `NavigationPage`. C’est pourquoi, sur iOS, un `UITabBarController` toujours agit comme un wrapper pour le `UINavigationController`. Pour plus d’informations, consultez [Interfaces de contrôleur de vue combinée](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) dans la bibliothèque du développeur iOS.

#### <a name="navigation-inside-a-tab"></a>Navigation à l’intérieur d’un onglet

Navigation peut être effectuée à partir du deuxième onglet en appelant le [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) méthode sur le [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) propriété de la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, comme illustré dans l’exemple de code suivant :

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

L’instance de `UpcomingAppointmentsPage` est ainsi envoyée dans la pile de navigation, où elle devient la page active. Ceci est illustré dans les captures d’écran suivants :

![](tabbed-page-images/navigationpage.png "Navigation à l’intérieur d’un onglet")

Pour plus d’informations sur l’exécution à l’aide de la navigation le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) de classe, consultez [Navigation hiérarchique](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Remplissage d’un TabbedPage avec un modèle

L’exemple de code XAML suivant montre un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) construit en assignant un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) pour retourner les pages de propriété objets de la collection :

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

Le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) est rempli avec des données en définissant le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) propriété dans le constructeur pour le fichier code-behind :

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

L’exemple de code suivant montre l’équivalent [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) créé en c# :

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

Chaque onglet affiche une [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) qui utilise une série de [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) et [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances pour afficher les données de l’onglet. Les captures d’écran suivantes affichent le contenu pour le *Tamarin* onglet :

![](tabbed-page-images/tab3.png "Remplissage d’un TabbedPage avec un modèle")

Sélectionner un autre onglet puis affiche le contenu de cet onglet.

> [!NOTE]
> Le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `TabbedPage` contient trop d’éléments enfants.

Pour plus d’informations sur la [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser un TabbedPage pour naviguer dans une collection de pages. Le Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) se compose d’une liste des onglets et une plus grande zone de détails, avec chaque onglet du chargement du contenu dans la zone de détails.


## <a name="related-links"></a>Liens associés

- [Variétés de page](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (sample)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
