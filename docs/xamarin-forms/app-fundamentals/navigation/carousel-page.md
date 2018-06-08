---
title: Page de carrousel
description: Le Xamarin.Forms CarouselPage est une page que les utilisateurs peuvent un glissement de gauche à droite pour parcourir les pages de contenu, par exemple une galerie. Cet article explique comment utiliser un CarouselPage pour naviguer dans une collection de pages.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 9259e2a85a7375106891eaae5fe22d6babfa2fcf
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846456"
---
# <a name="carousel-page"></a>Page de carrousel

_Le Xamarin.Forms CarouselPage est une page que les utilisateurs peuvent un glissement de gauche à droite pour parcourir les pages de contenu, par exemple une galerie. Cet article explique comment utiliser un CarouselPage pour naviguer dans une collection de pages._

## <a name="overview"></a>Vue d'ensemble

L’émission des captures d’écran suivante un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) sur chaque plateforme :

![](carousel-page-images/thirdpage.png "Élément de Thid CarouselPage")

La disposition d’un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) est identique sur chaque plateforme. Pages peuvent être accédées via en passant de droite à gauche pour naviguer vers l’avant dans la collection et en passant de gauche à droite pour naviguer vers l’arrière dans la collection. Les captures d’écran suivantes affichent la première page dans un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) instance :

![](carousel-page-images/firstpage.png "CarouselPage premier élément")

Le glissement de droite à gauche se déplace vers la deuxième page, comme indiqué dans les captures d’écran suivants :

![](carousel-page-images/secondpage.png "CarouselPage deuxième élément")

Le glissement à nouveau de droite à gauche déplace à la troisième page, alors que le glissement de gauche à droite retourne à la page précédente.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Création d’un CarouselPage

Deux approches peuvent être utilisés pour créer un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Remplir](#Populating_a_CarouselPage_with_a_Page_Collection) le `CarouselPage` avec une collection d’enfants [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances.
- [Affecter](#Populating_a_CarouselPage_with_a_Template) une collection à le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) propriété et affecter un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) propriété à retourner [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances des objets de la collection.

Avec les deux approches, le `CarouselPage` sera puis d’afficher chaque page à son tour, avec une interaction balayez déplacer vers la page suivante s’affiche. 

> [!NOTE]
> A [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) peut uniquement être remplie avec [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances, ou `ContentPage` dérivés.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Remplissage d’un CarouselPage avec un ensemble de Page

L’exemple de code XAML suivant montre un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) qui affiche trois [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances :

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

L’exemple de code suivant montre l’interface utilisateur équivalente dans c# :

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Chaque [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) affiche simplement un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) d’une couleur particulière et un [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) de cette couleur.

> [!NOTE]
> Le [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `CarouselPage` contient trop d’éléments enfants.

Si un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) est incorporé dans le [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) page d’un [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), le [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) propriété doit être définie sur `false` pour éviter les conflits de mouvement entre les `CarouselPage` et `MasterDetailPage`.

Pour plus d’informations sur la [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Remplissage d’un CarouselPage avec un modèle

L’exemple de code XAML suivant montre un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) construit en assignant un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) pour retourner les pages de propriété objets de la collection :

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

Le [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) est rempli avec des données en définissant le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) propriété dans le constructeur pour le fichier code-behind :

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

L’exemple de code suivant montre l’équivalent [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) créé en c# :

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Chaque [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) affiche simplement un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) d’une couleur particulière et un [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) de cette couleur.

> [!NOTE]
> Le [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `CarouselPage` contient trop d’éléments enfants.

Si un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) est incorporé dans le [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) page d’un [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), le [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) propriété doit être définie sur `false` pour éviter les conflits de mouvement entre les `CarouselPage` et `MasterDetailPage`.

Pour plus d’informations sur la [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser un [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) pour naviguer dans une collection de pages. Le `CarouselPage` est une page que les utilisateurs peuvent un glissement de gauche à droite pour parcourir les pages de contenu, à l’instar d’une galerie.


## <a name="related-links"></a>Liens associés

- [Variétés de page](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
