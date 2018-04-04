---
title: Pages modales
description: Xamarin.Forms assure la prise en charge des pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée. Cet article explique comment accéder aux pages modales.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 909a04398043a3c2f0c30e4da82d174a6bfaf148
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="modal-pages"></a>Pages modales

_Xamarin.Forms fournit la prise en charge pour les pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée. Cet article explique comment accéder aux pages modales._

Cet article traite des sujets suivants :

- [Exécution de navigation](#Performing_Navigation) – en exécutant un push de pages à la pile modale, à des pages de la pile modale, en désactivant le bouton Précédent et animation des transitions de page.
- [Passer des données lors de la navigation](#Passing_Data_when_Navigating) : passer des données via un constructeur de la page et un `BindingContext`.

## <a name="overview"></a>Vue d'ensemble

Une page modale peut s’agir de la [Page](~/xamarin-forms/user-interface/controls/pages.md) pris en charge par Xamarin.Forms. Pour afficher une page modale l’application transmet il dans la pile modale, où elle devient la page active, comme illustré dans le diagramme suivant :

![](modal-images/pushing.png "En exécutant un push d’une Page à la pile modale")

Pour revenir à la page précédente de l’application s’affiche à la page actuelle de la pile modale, et la nouvelle page au premier plan devient la page active, comme illustré dans le diagramme suivant :

![](modal-images/popping.png "L’affichage d’une Page à partir de la pile modale")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Exécution de Navigation

Les méthodes de navigation modale sont exposées par la propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) sur n’importe quel type dérivé [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Ces méthodes permettent de [push pages modales](#Pushing_Pages_to_the_Modal_Stack) sur la pile modale, et [pop pages modales](#Popping_Pages_from_the_Modal_Stack) à partir de la pile modale.

Le [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) propriété expose également un [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) propriété à partir de laquelle les pages modales dans la pile modale peuvent être obtenus. Toutefois, il n’existe aucun concept de manipulation de pile modale ni d’ajout à la page racine dans la navigation modale. En effet, ces opérations ne sont pas universellement prises en charge sur les plateformes sous-jacentes.

> [!NOTE]
> Aucune instance de [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) n’est requise pour la navigation entre les pages modales.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>En exécutant un push de Pages à la pile modale

Pour accéder à la `ModalPage` il n’est nécessaire appeler le [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) méthode sur le [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) propriété de la page active, comme cela est indiqué dans l’exemple de code suivant :

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Cela entraîne le `ModalPage` fourni d’instance à être placé sur la pile modale, où elle devient la page active, qu’un élément a été sélectionné dans le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) sur la `MainPage` instance. Le `ModalPage` instance est indiquée dans les captures d’écran suivants :

![](modal-images/modalpage.png "Exemple de Page modale")

Lorsque [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) est appelé, les événements suivants se produisent :

- L’appel de la page `PushModalAsync` a son [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) remplacement appelée, à condition que la plateforme sous-jacente n’est pas Android.
- La page cible de la navigation a son [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) remplacement appelé.
- Le `PushAsync` tâche se termine.

Toutefois, l’ordre précis que ces événements se produisent est dépendant de la plate-forme. Pour plus d’informations, consultez [chapitre 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) du livre de Xamarin.Forms de Petzold.

> [!NOTE]
> Les appels à la [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) et [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) remplacements ne peut pas être traités comme garanties indications de navigation de page. Par exemple, sur iOS, le `OnDisappearing` substitution est appelée sur la page active lorsque l’application se termine.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>À des Pages de la pile modale

La page active peut être dépilée modale en appuyant sur la *précédent* bouton sur l’appareil, indépendamment du s’il s’agit d’un bouton sur l’appareil ou un bouton à l’écran.

Pour revenir par programmation à la page d’origine, l’instance de `ModalPage` doit appeler la méthode [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/), comme illustré dans l’exemple de code suivant :

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

Cela entraîne le `ModalPage` instance à supprimer de la pile modale, avec la nouvelle page au premier plan devient la page active. Lorsque [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) est appelé, les événements suivants se produisent :

- L’appel de la page `PopModalAsync` a son [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) remplacement appelé.
- La page ne soit retournée à son [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) remplacement appelée, à condition que la plateforme sous-jacente n’est pas Android.
- La `PopModalAsync` retourne la tâche.

Toutefois, l’ordre précis que ces événements se produisent est dépendant de la plate-forme. Pour plus d’informations, consultez [chapitre 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) du livre de Xamarin.Forms de Petzold.

### <a name="disabling-the-back-button"></a>La désactivation du bouton précédent

Sur Android et Windows Phone, l’utilisateur peut toujours revenir à la page précédente en appuyant sur la norme *précédent* bouton sur l’appareil. Si la page modale oblige l’utilisateur terminer une tâche autonome avant de quitter la page, l’application doit désactiver le *précédent* bouton. Cela peut être accompli en substituant le [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) méthode sur la page modale. Pour plus d’informations, consultez [chapitre 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) du livre de Xamarin.Forms de Petzold.

### <a name="animating-page-transitions"></a>Animer des Transitions de Page

Le [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) propriété de chaque page fournit également substituée push et pop méthodes qui incluent un `boolean` paramètre qui détermine s’il faut afficher une animation de la page lors de la navigation, comme indiqué dans le code suivant exemple :

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Définition de la `boolean` paramètre `false` désactive l’animation de la transition de la page, tout en affectant au paramètre `true` permet à l’animation de la transition de page, si elle est prise en charge par la plateforme sous-jacente. Toutefois, les méthodes push et pop qui ne disposent pas de ce paramètre activent l’animation par défaut.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Passage de données lors de la navigation

Il est parfois nécessaire pour une page passer des données lors de la navigation vers une autre page. Deux techniques permettant d’accomplir cela sont en transférant les données via un constructeur de la page et en définissant la nouvelle page [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) aux données. Chaque maintenant nous reviendrons à son tour.

### <a name="passing-data-through-a-page-constructor"></a>Passage de données via un constructeur de la Page

La technique la plus simple pour passer des données vers une autre page pendant la navigation est via un paramètre de constructeur de page, qui est indiqué dans l’exemple de code suivant :

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Ce code crée un `MainPage` instance, en passant la date et heure actuelles au format ISO8601.

Le `MainPage` instance reçoit les données via un paramètre de constructeur, comme indiqué dans l’exemple de code suivant :

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Les données sont ensuite affichées sur la page en définissant le [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriété.

### <a name="passing-data-through-a-bindingcontext"></a>Passage de données via un objet BindingContext

Une autre approche pour passer des données vers une autre page pendant la navigation est en définissant la nouvelle page [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) aux données, comme indiqué dans l’exemple de code suivant :

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Ce code définit la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de la `DetailPage` d’instance pour le `Contact` d’instance et puis accède à la `DetailPage`.

Le `DetailPage` utilise ensuite la liaison de données pour afficher le `Contact` instance des données, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

L’exemple de code suivant montre comment la liaison de données peut être accomplie en c# :

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Les données sont ensuite affichées dans la page par une série de [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôles.

Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment accéder aux pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée.


## <a name="related-links"></a>Liens associés

- [Navigation entre les pages](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modal (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
