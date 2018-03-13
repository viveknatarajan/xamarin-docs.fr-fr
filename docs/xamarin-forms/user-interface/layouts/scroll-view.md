---
title: ScrollView
description: "Utiliser ScrollView pour présenter des dispositions qui ne peut pas tenir sur un seul écran et le contenu libérer de l’espace pour le clavier."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 648125ca8bd2c7c8a015b4c29195dc75c0bbf0a0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) contient des mises en page et leur permet de défilement hors écran. `ScrollView` est également utilisé pour autoriser des vues déplacer automatiquement dans la partie visible de l’écran lors de l’affichage de l’aide du clavier.

[![](scroll-view-images/layouts-sml.png "Les dispositions de Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms dispositions")

Cet article décrit :

- **[Objectif](#Purpose)**  &ndash; l’objectif de `ScrollView` et lorsqu’il est utilisé.
- **[L’utilisation de](#Usage)**  &ndash; comment utiliser `ScrollView` dans la pratique.
- **[Propriétés](#Properties)**  &ndash; propriétés publiques qui peuvent être lues et modifiées.
- **[Méthodes](#Methods)**  &ndash; méthodes publiques qui peuvent être appelés pour faire défiler l’affichage.
- **[Événements](#Events)**  &ndash; les événements qui peuvent être utilisés pour écouter les changements d’état de la vue.

## <a name="purpose"></a>Objectif

`ScrollView` peut être utilisé pour vous assurer que les vues supérieure affichent correctement sur les téléphones plus petits. Par exemple, une disposition qui fonctionne sur un iPhone 6 s peut être tronquée sur un iPhone 4 s. À l’aide un `ScrollView` permettrait aux parties découpées de la disposition à afficher sur l’écran de petite taille.

## <a name="usage"></a>Utilisation

> [!NOTE]
> `ScrollView`s ne doivent pas être imbriquées. En outre, `ScrollView`s ne doit pas être imbriqué avec d’autres contrôles qui fournissent le défilement, tel que `ListView` et `WebView`.

`ScrollView` expose un `Content` propriété qui peut être définie sur une vue unique ou une disposition. Prenons l’exemple d’une disposition avec un très grand boxView, suivi d’un `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

En C# :

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,   HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Avant que l’utilisateur fait défiler vers le bas, seuls les `BoxView` est visible :

![](scroll-view-images/scroll-start.png "BoxView dans ScrollView")

Notez que lorsque l’utilisateur commence à entrer du texte dans le `Entry`, la vue défile pour laisser visibles à l’écran :

![](scroll-view-images/scroll-end.png "Entrée dans ScrollView")

## <a name="properties"></a>Propriétés

ScrollView a les propriétés suivantes :

- **Contenu** &ndash; Obtient ou définit la vue à afficher dans le `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; en lecture seule, obtient la taille du contenu, qui a une largeur et la hauteur de composant. Il s’agit d’une propriété pouvant être liée
- **[Orientation](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; il s’agit d’un [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), qui est une énumération peut être définie sur `Horizontal`, `Vertical`, ou `Both`.
- **ScrollX** &ndash; en lecture seule, obtient la position de défilement actuelle dans la dimension X.
- **ScrollY** &ndash; en lecture seule, obtient la position de défilement actuelle dans la dimension Y.

## <a name="methods"></a>Méthodes

`ScrollView` fournit un `ScrollToAsync` (méthode), qui peut être utilisé pour faire défiler l’affichage à l’aide de coordonnées ou en spécifiant une vue particulière qui doit être rendue visible.

Lorsque vous utilisez des coordonnées, spécifiez la `x` et `y` coordonnées, ainsi que d’une valeur booléenne indiquant si le défilement doit être animé :

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Lors du défilement à un élément particulier, le `ScrollToPosition` spécifie énumération où l’élément s’affiche dans la vue :

- **Centre** &ndash; fait défiler l’élément au centre de la partie visible de la vue.
- **Fin** &ndash; fait défiler l’élément à la fin de la partie visible de la vue.
- **MakeVisible** &ndash; fait défiler l’élément afin qu’il soit visible dans la vue.
- **Démarrer** &ndash; fait défiler l’élément au début de la partie visible de la vue.

Le `IsAnimated` propriété spécifie le comment de défilement de la vue. Lorsque la valeur true, une animation lisse servira, plutôt que d’instantanément déplacer le contenu dans la vue.

## <a name="events"></a>Événements

`ScrollView` expose qu’un seul événement, `Scrolled`. `Scrolled` est déclenché lorsque la vue a terminé le défilement. Le Gestionnaire d’événements pour `Scrolled` prend `ScrolledEventArgs`, qui a le `ScrollX` et `ScrollY` propriétés. Ce qui suit montre comment mettre à jour une étiquette avec la position de défilement actuelle d’un `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Notez que les positions de défilement peuvent être négatives, en raison de l’effet de rebond lors du défilement à la fin d’une liste.


## <a name="related-links"></a>Liens associés

- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
