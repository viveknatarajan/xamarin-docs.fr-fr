---
title: Résumé du chapitre 27. Convertisseurs personnalisés
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 27. Convertisseurs personnalisés'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 844c9be72071cd307bd2330b54f8d70ddf28b9a3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240880"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Résumé du chapitre 27. Convertisseurs personnalisés

Un élément Xamarin.Forms comme `Button` est rendue avec un bouton spécifique à la plateforme encapsulé dans une classe nommée `ButtonRenderer`.  Voici le [version iOS de `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), le [Android version de `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)et le [version de Windows Runtime `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

Ce chapitre explique comment vous pouvez écrire vos propres convertisseurs pour créer des vues personnalisées qui mappent aux objets spécifiques à la plateforme.

## <a name="the-complete-class-hierarchy"></a>La hiérarchie de classe complète

Il existe sept assemblys qui contiennent le code spécifique à la plateforme de Xamarin.Forms.
Vous pouvez afficher la source sur GitHub à l’aide de ces liens :

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (petite)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (plus de trois suivants)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

Le [ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) exemple affiche une hiérarchie de classes pour les assemblys qui sont valides pour la plateforme en cours d’exécution.

Vous remarquerez une classe importante nommée `ViewRenderer`. Il s’agit de la classe de que dérivation lors de la création d’un convertisseur spécifique à la plateforme. Il existe en trois versions différentes, car elle est liée au système de vue de la plateforme cible :

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) a des arguments génériques :

- `TView` contraint à [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` contraint à [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Le Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) a des arguments génériques :

- `TView` contraint à [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` contraint à [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Le Windows Runtime [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) a nommés différemment les arguments génériques :

- `TElement` contraint à [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` contraint à [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Lorsque vous écrivez un convertisseur, vous serez dérivant une classe de `View`, puis en écrivant plusieurs `ViewRenderer` des classes, une pour chaque plateforme prise en charge. Chaque implémentation spécifique à la plateforme fait référence à une classe native qui dérive du type que vous spécifiez en tant que le `TNativeView` ou `TNativeElement` paramètre.

## <a name="hello-custom-renderers"></a>Hello, les convertisseurs personnalisés

Le [ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) programme fait référence à une vue personnalisée nommée `HelloView` dans son [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) classe.

Le [ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) classe est incluse dans le **HelloRenderers** de projet et simplement dérive `View`.

Le [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) classe dans le **HelloRenderers.iOS** dérive de projet `ViewRenderer<HelloView, UILabel>`. Dans le `OnElementChanged` override, il crée un iOS natif `UILabel` et appelle `SetNativeControl`.

Le [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) classe dans le **HelloRenderers.Droid** dérive de projet `ViewRenderer<HelloView, TextView>`. Dans le `OnElementChanged` override, il crée un Android `TextView` et appelle `SetNativeControl`.

Le [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) classe dans le **HelloRenderers.UWP** et dérive d’autres projets Windows `ViewRenderer<HelloView, TextBlock>`. Dans le `OnElementChanged` override, il crée une fenêtre `TextBlock` et appelle `SetNativeControl`.

Tous les le `ViewRenderer` dérivés contiennent un `ExportRenderer` attribut sur le niveau de l’assembly qui associe le `HelloView` classe avec particulier `HelloViewRenderer` classe. Voici comment Xamarin.Forms localise les convertisseurs dans les projets de plateforme individuels :

[![Capture d’écran de triple de vue Hello](images/ch27fg02-small.png "convertisseurs personnalisés")](images/ch27fg02-large.png#lightbox "convertisseurs personnalisés")

## <a name="renderers-and-properties"></a>Les convertisseurs et les propriétés

L’ensemble suivant de convertisseurs implémente le dessin de points de suspension et se trouve dans les différents projets de la [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) solution.

Le [ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) classe se trouve dans le **Xamarin.FormsBook.Platform** plateforme. La classe est similaire à `BoxView` et définit simplement une propriété unique : `Color` de type `Color`.

Les convertisseurs peuvent transférer des valeurs de propriété définies sur un `View` à l’objet natif en substituant la `OnElementPropertyChanged` méthode dans le convertisseur. Dans cette méthode (et, dans la plupart du convertisseur), deux propriétés sont disponibles :

- `Element`, l’élément de Xamarin.Forms
- `Control`, le mode natif ou un objet widget ou de contrôle

Les types de ces propriétés sont déterminés par les paramètres génériques de `ViewRenderer`. Dans cet exemple, `Element` est de type `EllipseView`.

Le `OnElementPropertyChanged` remplacement peut donc transférer la `Color` valeur de la `Element` à natif `Control` objet, sans doute avec un type de conversion. Les trois convertisseurs sont les suivantes :

- iOS : [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), qui utilise un [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) classe pour les points de suspension.
- Android : [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), qui utilise un [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) classe pour les points de suspension.
- Windows Runtime : [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), qui peut utiliser les fenêtres natives [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) classe.

Le [ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) classe affiche plusieurs de ces `EllipseView` objets :

[![Capture d’écran de triple de démonstration de l’Ellipse](images/ch27fg03-small.png "convertisseurs personnalisés de EllipseView")](images/ch27fg03-large.png#lightbox "convertisseurs personnalisés de EllipseView")

Le [ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) rebondit un `EllipseView` désactivé les côtés de l’écran.

## <a name="renderers-and-events"></a>Les convertisseurs et les événements

Il est également possible pour des convertisseurs générer des événements indirectement. Le [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) classe est semblable à la normale Xamarin.Forms `Slider` , mais permet de spécifier un nombre d’étapes discrètes entre le `Minimum` et `Maximum` valeurs.

Les trois convertisseurs sont les suivantes :

- iOS : [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android : [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows Runtime : [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Les convertisseurs de détecter les modifications apportées au contrôle natif, puis appelez `SetValueFromRenderer`, qui fait référence à une propriété définie dans le `StepSlider`, ce qui entraîne une modification de la `StepSlider` pour déclencher un `ValueChanged` événement.

Le [ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) exemple illustre ce nouveau curseur.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 27 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Exemples de chapitre 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
