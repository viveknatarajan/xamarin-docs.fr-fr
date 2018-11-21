---
title: Page à onglets Xamarin.Forms
description: Le Xamarin.Forms TabbedPage se compose d’une liste des onglets et une plus grande zone de détails, chaque onglet du chargement du contenu dans la zone de détails. Cet article montre comment utiliser un TabbedPage pour naviguer dans une collection de pages.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 85a6bce8a1021c75064ba06f3a5daf69b7fe3e57
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171376"
---
# <a name="xamarinforms-tabbed-page"></a>Page à onglets Xamarin.Forms

_Le Xamarin.Forms TabbedPage se compose d’une liste des onglets et une plus grande zone de détails, chaque onglet du chargement du contenu dans la zone de détails. Cet article montre comment utiliser un TabbedPage pour naviguer dans une collection de pages._

## <a name="overview"></a>Vue d'ensemble

Le montrent les captures d’écran suivants un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) sur chaque plateforme :

![](tabbed-page-images/tab1.png "Exemple de TabbedPage")

Les captures d’écran suivantes vous concentrer sur le format de l’onglet sur chaque plateforme :

![](tabbed-page-images/tabbedpage-components.png "TabbedPage onglet composants")

La disposition d’un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)et ses onglets, est dépendante de la plateforme :

- Sur iOS, la liste des onglets apparaît au bas de l’écran et la zone de détails se situe au-dessus. Chaque onglet est doté d’une image d’icône qui doit être un 30 x 30 PNG avec une transparence pour la résolution normale, 60 x 60 pour la haute résolution et 90 x 90 pour iPhone 6 Plus résolution. S’il existe plus de cinq onglets, un *plus* onglet s’affiche, ce qui peut être utilisé pour accéder les onglets supplémentaires. Pour plus d’informations sur le chargement des images dans une application Xamarin.Forms, consultez [utilisation des Images](~/xamarin-forms/user-interface/images.md). Pour plus d’informations sur la configuration requise d’icône, consultez [création d’Applications avec onglet](~/ios/user-interface/controls/creating-tabbed-applications.md).

  > [!NOTE]
  > Notez que le `TabbedRenderer` pour iOS a un substituable `GetIcon` méthode qui peut être utilisée pour charger des icônes d’onglets à partir d’une source spécifiée. Ce remplacement permet d’utiliser les images SVG sous forme d’icônes sur un `TabbedPage`. En outre, les versions sélectionnées et d’une icône peuvent être fournies.

- Sur Android, la liste des onglets apparaît en haut de l’écran par défaut, et la zone de détails est ci-dessous. Toutefois, la liste d’onglets peut être déplacée vers le bas de l’écran avec un spécifique à la plateforme. Pour plus d’informations, consultez [emplacement des paramètres de barre d’outils TabbedPage et couleur](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar).

  > [!NOTE]
  > Notez que lorsque vous utilisez AppCompat sur Android, chaque onglet affiche également une icône. En outre, le `TabbedPageRenderer` pour Android AppCompat a un substituable `GetIconDrawable` méthode qui peut être utilisé pour charger des icônes d’onglets à partir d’un personnalisé `Drawable`. Ce remplacement permet d’utiliser les images SVG sous forme d’icônes sur un `TabbedPage`, et fonctionne avec les deux haut et bas des barres d’onglets. Vous pouvez également le remplaçables `SetTabIcon` méthode peut être utilisée pour charger des icônes d’onglets à partir d’un personnalisé `Drawable` pour les barres de l’onglet supérieur.

- Windows tablet facteurs de forme-, les onglets ne sont pas toujours visibles et les utilisateurs doivent Balayage vers le bas (ou avec le bouton droit, s’ils ont une souris attachée) pour afficher les onglets dans un `TabbedPage` (comme indiqué ci-dessous).

![](tabbed-page-images/windows-tabs.png "Onglets TabbedPage sur Windows")

## <a name="creating-a-tabbedpage"></a>Création d’un TabbedPage

Deux approches peuvent être utilisées pour créer un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [Remplir](#Populating_a_TabbedPage_with_a_Page_Collection) le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) avec une collection d’enfants [ `Page` ](xref:Xamarin.Forms.Page) objets, telle qu’une collection de [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instances.
- [Affecter](#Populating_a_TabbedPage_with_a_Template) une collection à le [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) propriété et affecter un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) à la [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) propriété à retourner des pages pour objets de la collection.

Avec les deux approches, le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) affiche chaque page lorsque l’utilisateur sélectionne chaque onglet.

> [!NOTE]
> Il est recommandé qu’un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) doit être rempli avec [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) et [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)instances uniquement. Cela vous aidera à garantir une expérience utilisateur cohérente sur toutes les plateformes.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Remplissage d’un TabbedPage avec une Collection de pages

L’exemple de code XAML suivant montre un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) construit en le remplissant avec une collection d’enfants [ `Page` ](xref:Xamarin.Forms.Page) objets :

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

L’exemple de code suivant montre l’équivalent [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) créé en c# :

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

Le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) est rempli avec deux enfants [ `Page` ](xref:Xamarin.Forms.Page) objets. Le premier enfant est un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance et le deuxième onglet est un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) contenant un `ContentPage` instance.

> [!NOTE]
> Le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `TabbedPage` contient trop d’éléments enfants.

L’émission des captures d’écran suivante le `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance, ce qui est affichée sur le *aujourd'hui* onglet :

![](tabbed-page-images/today-page.png "ContentPage dans un TabbedPage")

En sélectionnant le *planification* onglet affiche la `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance, ce qui est encapsulée dans un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) de l’instance et est affiché dans le capture d’écran suivante :

![](tabbed-page-images/schedule-page.png "NavigationPage dans un TabbedPage")

Pour plus d’informations sur la disposition d’un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), consultez [effectuer une Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Bien qu’il soit acceptable pour placer un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), il n’est pas recommandé de placer un `TabbedPage` dans un `NavigationPage`. C’est pourquoi, sur iOS, un `UITabBarController` toujours agit comme un wrapper pour le `UINavigationController`. Pour plus d’informations, consultez [Interfaces de contrôleur de vue combinée](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) dans la bibliothèque du développeur iOS.

#### <a name="navigation-inside-a-tab"></a>Navigation à l’intérieur d’un onglet

Navigation peut être effectuée à partir du deuxième onglet en appelant le [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) méthode sur le [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) propriété de la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance, comme illustré dans l’exemple de code suivant :

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

L’instance de `UpcomingAppointmentsPage` est ainsi envoyée dans la pile de navigation, où elle devient la page active. Ceci est illustré dans les captures d’écran suivante :

![](tabbed-page-images/navigationpage.png "Navigation à l’intérieur d’un onglet")

Pour plus d’informations sur l’exécution à l’aide de la navigation le [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) de classe, consultez [Navigation hiérarchique](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Remplissage d’un TabbedPage avec un modèle

L’exemple de code XAML suivant montre un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) construit en assignant un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) à la [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) propriété à retourner des pages pour objets de la collection :

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

Le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) est rempli avec des données en définissant le [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) propriété dans le constructeur pour le fichier code-behind :

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

L’exemple de code suivant montre l’équivalent [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) créé en c# :

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

Chaque onglet affiche une [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) qui utilise une série de [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) et [ `Label` ](xref:Xamarin.Forms.Label) instances pour afficher les données de l’onglet. Les captures d’écran suivantes affichent le contenu pour le *Tamarin* onglet :

![](tabbed-page-images/tab3.png "Remplissage d’un TabbedPage avec un modèle")

Sélectionnant un autre onglet, puis affiche le contenu de cet onglet.

> [!NOTE]
> Le [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `TabbedPage` contient trop d’éléments enfants.

Pour plus d’informations sur la [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser un TabbedPage pour naviguer dans une collection de pages. Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) se compose d’une liste des onglets et une plus grande zone de détail, avec chaque onglet du chargement du contenu dans la zone de détails.


## <a name="related-links"></a>Liens associés

- [Types de page](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
