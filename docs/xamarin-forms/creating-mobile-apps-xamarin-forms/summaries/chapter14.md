---
title: "Résumé du chapitre 14. Disposition absolue"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a3980c63c31f4fdf0297fdc9b05da3590f0cac54
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Résumé du chapitre 14. Disposition absolue

Comme `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) dérive `Layout<View>` et hérite d’un `Children` propriété. `AbsoluteLayout` implémente un système de disposition qui nécessite le programmeur spécifier les positions de ses enfants et, éventuellement, de leur taille. La position est spécifiée par le coin supérieur gauche de l’enfant par rapport au coin supérieur gauche de la `AbsoluteLayout` en unités indépendantes du périphérique. `AbsoluteLayout` implémente également une fonctionnalité de dimensionnement et de positionnement proportionnel.

`AbsoluteLayout` doit être considéré comme un système de mise en forme spéciale pour être utilisé uniquement lorsque le programmeur peut imposer une taille pour les enfants (par exemple, `BoxView` éléments) ou lorsque la taille de l’élément n’affecte pas le positionnement d’autres enfants. Le `HorizontalOptions` et `VerticalOptions` propriétés n’ont aucun effet sur les enfants d’un `AbsoluteLayout`.

Ce chapitre présente également la fonctionnalité importante de *jointe propriétés pouvant être liées* qui autorisent les propriétés définies dans une classe (dans ce cas `AbsoluteLayout`) à joindre à une autre classe (un enfant de le `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>DispositionAbsolue dans le code

Vous pouvez ajouter un enfant à la `Children` collection d’un `AbsoluteLayout` à l’aide de la norme [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) (méthode), mais `AbsoluteLayout` fournit également des étendues [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) méthode qui vous permet de spécifier un [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Un autre [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) méthode nécessite uniquement un [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), auquel cas l’enfant n’est pas limitée et des tailles de lui-même.

Vous pouvez créer un `Rectangle` valeur avec un [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) qui requiert quatre valeurs &mdash; les deux premières indiquant la position de l’angle supérieur gauche de l’enfant par rapport à son parent et les deux indiquant le taille de l’enfant. Vous pouvez également utiliser un [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) qui nécessite un `Point` et un [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) valeur.

Ces `Add` méthodes sont illustrées dans [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), les positions `BoxView` à l’aide des éléments `Rectangle` valeurs et un `Label` élément à l’aide de simplement un `Point` valeur.

Le [ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) exemple utilise 32 `BoxView` éléments pour créer le modèle échiquier. Le programme donne le `BoxView` taille un codé en dur des éléments du carré de 35 unités. Le `AbsoluteLayout` a son `HorizontalOptions` et `VerticalOptions` la valeur `LayoutOptions.Center`, ce qui entraîne la `AbsoluteLayout` d’avoir une taille totale du carré de 280 unités.

## <a name="attached-bindable-properties"></a>Joint des propriétés pouvant être liées

Il est également possible de définir la position et, éventuellement, la taille d’un enfant d’un `AbsoluteLayout` une fois qu’il a été ajouté à la `Children` collection à l’aide de la méthode statique [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). Le premier argument est l’enfant ; le deuxième est un `Rectangle` objet. Vous pouvez spécifier que l’enfant se redimensionne horizontalement et verticalement en définissant les valeurs de largeur et hauteur le [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) constante.

Le [ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) exemple met le `AbsoluteLayout` dans un `ContentView` avec un `SizeChanged` gestionnaire à appeler `AbsoluteLayout.SetLayoutBounds` sur tous les enfants pour les rendre aussi grand que possible.  

La propriété pouvant être liée qui `AbsoluteLayout` définit le champ statique en lecture seule de type n’est `BindableProperty` nommé [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). La méthode statique `AbsoluteLayout.SetLayoutBounds` méthode est implémentée en appelant `SetValue` sur l’enfant avec le `AbsoluteLayout.LayoutBoundsProperty`. L’enfant contienne un dictionnaire dans lequel la propriété pouvant être liée et sa valeur sont stockées. Lors de la disposition, la `AbsoluteLayout` pouvez obtenir cette valeur en appelant [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), qui est implémentée avec un `GetValue` appeler.

## <a name="proportional-sizing-and-positioning"></a>Redimensionnement proportionnel et le positionnement

`AbsoluteLayout` implémente un redimensionnement proportionnel et la fonctionnalité de positionnement. La classe définit une deuxième propriété jointe pouvant être liée, [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), avec les méthodes statiques associées [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) et [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

L’argument `AbsoluteLayout.SetLayoutFlags` et la valeur de retour de `AbsoluteLayout.GetLayoutFlags` est une valeur de type [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), une énumération avec les membres suivants :

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (égal à 0)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

Vous pouvez combiner ces avec le langage c# opérateur de bits OR.

Avec ces indicateurs définis, certaines propriétés de la `Rectangle` structure de limites de disposition utilisée pour positionner et dimensionner les enfants sont interprétés proportionnellement.

Lorsque le `WidthProportional` indicateur est défini, un `Width` valeur de 1 signifie que l’enfant est la même largeur que le `AbsoluteLayout`. Une approche similaire est utilisée pour la hauteur.

Le positionnement proportionnel tient la taille du compte. Lorsque le `XProportional` indicateur est défini, le `X` propriété de la `Rectangle` limites de disposition est proportionnelle. Une valeur de 0 signifie que l’enfant gauche de la session est positionnée sur le bord gauche de la `AbsoluteLayout`, mais une position de 1 signifie que le bord droit de l’enfant est positionné sur le bord droit de la `AbsoluteLayout`, pas au-delà du bord droit de la `AbsoluteLayout` comme vous le feriez notre t. Un `X` l’enfant horizontalement dans les centres de propriété de 0,5 le `AbsoluteLayout`.

Le [ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) exemple illustre l’utilisation de redimensionnement proportionnel et de positionnement.

## <a name="working-with-proportional-coordinates"></a>Utilisation des coordonnées proportionnelles

Parfois, il est plus simple de considérer un positionnement proportionnel différemment de la façon dont il est implémenté dans le `AbsoluteLayout`. Vous préférerez peut-être utiliser des coordonnées proportionnelles où un `X` propriété 1 positionne bord gauche de l’enfant (plutôt que le bord droit) sur le bord droit de la `AbsoluteLayout`.

Ce schéma de positionnement de remplacement peut être appelé « coordonnées enfants fractions de seconde. » Vous pouvez convertir des coordonnées de fractions de seconde enfant pour les limites de disposition requis pour `AbsoluteLayout` via les formules suivantes :

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

Le [ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) illustre cela.

## <a name="absolutelayout-and-xaml"></a>DispositionAbsolue et XAML

Vous pouvez utiliser un `AbsoluteLayout` en XAML et définissez les propriétés de pouvant être liées attachées sur les enfants d’un `AbsoluteLayout` à l’aide des valeurs d’attribut de `AbsoluteLayout.LayoutBounds` et `AbsoluteLayout.LayoutFlags`. Cela est illustré dans le [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) et [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) exemples. Le programme de ce dernier contienne 32 `BoxView` éléments, mais utilise implicite `Style` qui inclut le `AbsoluteLayout.LayoutFlags` propriété conserver le balisage jusqu’au minimum.

Un attribut en XAML qui se compose d’un nom de classe, un point et un nom de propriété est *toujours* une propriété pouvant être liée jointe.

## <a name="overlays"></a>Superpositions

Vous pouvez utiliser `AbsoluteLayout` pour construire un *segment de recouvrement*, qui couvre la page avec d’autres contrôles, peut-être pour protéger l’utilisateur d’interagir avec les contrôles normales sur la page. 

Le [ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) exemple illustre cette technique et montre également la [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), qui affiche l’étendue à laquelle un programme a terminé une tâche.

## <a name="some-fun"></a>Certains fun

Le [ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) exemple affiche l’heure actuelle avec un affichage simulé matricielle 5 x 7. Chaque point est un `BoxView` (il y a 228 d'entre eux) dimensionné et positionné sur le `AbsoluteLayout`.

[![Capture d’écran de triple de matricielle horloge](images/ch14fg08-small.png "matricielle horloge")](images/ch14fg08-large.png#lightbox "matricielle horloge")

Le [ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) programme anime deux `Label` objets rebondit horizontalement et verticalement sur l’écran.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 14 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Exemples de chapitre 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
