---
title: Page carrousel de Xamarin.Forms
description: Le Xamarin.Forms CarouselPage est une page que les utilisateurs peuvent balayer de gauche à droite pour parcourir les pages de contenu, comme une galerie. Cet article montre comment utiliser un CarouselPage pour naviguer dans une collection de pages.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 48c009b836ac109e0d54cd2fdb036c46e17c4387
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121501"
---
# <a name="xamarinforms-carousel-page"></a>Page carrousel de Xamarin.Forms

_Le Xamarin.Forms CarouselPage est une page que les utilisateurs peuvent balayer de gauche à droite pour parcourir les pages de contenu, comme une galerie. Cet article montre comment utiliser un CarouselPage pour naviguer dans une collection de pages._

## <a name="overview"></a>Vue d'ensemble

Le montrent les captures d’écran suivants un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) sur chaque plateforme :

![](carousel-page-images/thirdpage.png "CarouselPage troisième élément")

La disposition d’un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) est identique sur chaque plateforme. Pages peuvent être parcourues par le biais par balayage de droite à gauche pour naviguer vers l’avant via la collection et en faisant glisser vers la gauche à droite pour naviguer vers l’arrière dans la collection. Les captures d’écran suivantes montrent la première page dans un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) instance :

![](carousel-page-images/firstpage.png "CarouselPage premier élément")

Balayage de droite à gauche se déplace vers la deuxième page, comme indiqué dans les captures d’écran suivante :

![](carousel-page-images/secondpage.png "Élément de CarouselPage seconde")

Balayage de droite à gauche à nouveau actionnée à la troisième page, passant de gauche à droite retourne à la page précédente.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Création d’un CarouselPage

Deux approches peuvent être utilisées pour créer un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [Remplir](#Populating_a_CarouselPage_with_a_Page_Collection) le `CarouselPage` avec une collection d’enfants [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instances.
- [Affecter](#Populating_a_CarouselPage_with_a_Template) une collection à le [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) propriété et affecter un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) à la [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) propriété à retourner [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instances pour les objets dans la collection.

Avec les deux approches, la `CarouselPage` affiche ensuite chaque page à son tour, avec une interaction de passage vers la page suivante s’affiche.

> [!NOTE]
> Un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) peuvent uniquement être remplis avec [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instances, ou `ContentPage` dérivés.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Remplissage d’un CarouselPage avec une Collection de pages

L’exemple de code XAML suivant montre un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) qui affiche trois [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instances :

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

L’exemple de code suivant montre l’interface utilisateur équivalente en c# :

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

Chaque [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) affiche simplement un [ `Label` ](xref:Xamarin.Forms.Label) pour une couleur particulière et un [ `BoxView` ](xref:Xamarin.Forms.BoxView) de cette couleur.

> [!NOTE]
> Le [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `CarouselPage` contient trop d’éléments enfants.

Si un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) est incorporée dans le [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) page d’un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), le [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) propriété doit être définie sur `false` pour éviter les conflits de mouvement entre les `CarouselPage` et `MasterDetailPage`.

Pour plus d’informations sur la [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Remplissage d’un CarouselPage avec un modèle

L’exemple de code XAML suivant montre un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) construit en assignant un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) à la [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) propriété à retourner des pages pour objets de la collection :

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

Le [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) est rempli avec des données en définissant le [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) propriété dans le constructeur pour le fichier code-behind :

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

L’exemple de code suivant montre l’équivalent [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) créé en c# :

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

Chaque [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) affiche simplement un [ `Label` ](xref:Xamarin.Forms.Label) pour une couleur particulière et un [ `BoxView` ](xref:Xamarin.Forms.BoxView) de cette couleur.

> [!NOTE]
> Le [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) ne prend pas en charge la virtualisation de l’interface utilisateur. Par conséquent, les performances peuvent être affectées si la `CarouselPage` contient trop d’éléments enfants.

Si un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) est incorporée dans le [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) page d’un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), le [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) propriété doit être définie sur `false` pour éviter les conflits de mouvement entre les `CarouselPage` et `MasterDetailPage`.

Pour plus d’informations sur la [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), consultez [chapitre 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) du livre de Xamarin.Forms de Petzold.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser un [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) pour naviguer dans une collection de pages. Le `CarouselPage` est une page qui les utilisateurs peuvent balayer de gauche à droite pour parcourir les pages de contenu, à l’instar d’une galerie.


## <a name="related-links"></a>Liens associés

- [Types de page](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
