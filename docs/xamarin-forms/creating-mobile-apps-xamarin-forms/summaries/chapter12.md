---
title: "Résumé du chapitre 12. Styles"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9563bc811250038e8932067280a8e5292a379077
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-12-styles"></a>Résumé du chapitre 12. Styles

Dans Xamarin.Forms, les styles permettent plusieurs vues partager une collection de paramètres de propriété. Cela réduit le balisage et en conservant les thèmes visuels cohérents.

Les styles sont presque toujours définis et utilisés dans le balisage. Un objet de type [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) est instancié dans un dictionnaire de ressources et la [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété d’un élément visuel à l’aide un `StaticResource` ou `DyanamicResource` balisage extension.

## <a name="the-basic-style"></a>Le Style de base

A `Style` exige que son [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) être définie sur le type de l’objet visuel, elle s’applique à. Lorsqu’un `Style` est instancié dans un dictionnaire de ressources (comme c’est commun) il requiert également un `x:Key` attribut.

Le `Style` a une propriété de type de contenu [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), qui est une collection de [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) objets. Chaque `Setter` associe un [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) avec un [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

En XAML le `Property` paramètre correspond au nom d’une propriété CLR (telles que la `Text` propriété de `Button`), mais la propriété de style doit être soutenue par une propriété pouvant être liée. En outre, la propriété doit être définie dans la classe indiquée par le `TargetType` paramètre ou héritée par cette classe.

Vous pouvez spécifier le `Value` configuration à l’aide de l’élément de propriété `<Setter.Value>`. Cela vous permet de définir `Value` à un objet qui ne peut pas être exprimé dans une chaîne de texte, ou à un `OnPlatform` de l’objet ou à un objet instancié à l’aide de `x:Arguments` ou `x:FactoryMethod`. Le `Value` propriété peut également être définie avec un `StaticResource` expression à un autre élément dans le dictionnaire.

Le [ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) programme montre la syntaxe de base et indique comment référencer le `Style` avec un `StaticResource` extension de balisage :

[![Capture d’écran de triple du style de base](images/ch12fg01-small.png "Styles de base")](images/ch12fg01-large.png#lightbox "Styles de base")

Le `Style` objet et tout objet créé dans le `Style` de l’objet en un `Value` paramètre sont partagés entre toutes les vues faisant référence à cette `Style`. Le `Style` ne peut pas contenir tout ce qui ne peut pas être partagé, comme un `View` dérivé.

Impossible de définir des gestionnaires d’événements dans un `Style`. Le `GestureRecognizers` propriété ne peut pas être définie dans un `Style` , car il n’est pas sauvegardé par une propriété pouvant être liée.

## <a name="styles-in-code"></a>Styles de code

Bien qu’il n’est pas courant, vous pouvez instancier et initialiser `Style` objets dans le code. Cela est illustré par le [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) exemple.

## <a name="style-inheritance"></a>Héritage de style

`Style` a un [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) propriété que vous pouvez définir pour un `StaticResource` extension de balisage faisant référence à un autre style. Cela permet de styles hériter des styles précédentes et ajouter ou remplacer les paramètres de propriété. Le [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) illustre cela.

Si `Style2` est basée sur `Style1`, le `TargetType` de `Style2` doit être le même que `Style1` ou dérivé de `Style1`. Le dictionnaire de ressources dans lequel `Style1` est stocké doit être le même dictionnaire de ressources en tant que `Style2` ou un dictionnaire de ressources plus haut dans l’arborescence d’éléments visuels.

## <a name="implicit-styles"></a>Styles implicites

Si un `Style` dans une ressource de dictionnaire n’a pas un `x:Key` attribut de paramètre, il est attribué automatiquement, une clé de dictionnaire et `Style` objet devient un *style implicite*. Un affichage sans un `Style` paramètre et dont le type correspond à la `TargetType` exactement trouve ce style, comme le [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) illustre.

Un style implicite peut dériver un `Style` avec un `x:Key` paramètre, mais pas l’autre sens. Vous ne pouvez pas référencer explicitement un style implicite.

Vous pouvez implémenter les trois types de hiérarchie avec des styles et `BasedOn`:

- À partir de styles définis dans le `Application` et `Page` jusqu'à les styles définis sur les dispositions de niveau inférieures dans l’arborescence d’éléments visuels.
- À partir de styles définis pour les classes de base telles que `VisualElement` et `View` pour les styles définis pour les classes spécifiques.
- À partir de styles avec des clés de dictionnaire explicite pour les styles implicites.

Ces hiérarchies sont illustrées dans le [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) exemple.

## <a name="dynamic-styles"></a>Styles dynamiques

Un style dans un dictionnaire de ressources peut être référencé par `DynamicResource` plutôt que `StaticResource`. Cela rend le style d’un *style dynamique*. Si ce style est remplacé dans le dictionnaire de ressources par un autre style avec la même clé, les vues référençant ce style avec `DynamicResource` modifier automatiquement. En outre, l’absence d’une entrée de dictionnaire avec la clé spécifiée entraîne `StaticResource` pour lever une exception, mais pas `DynamicResource`.

Vous pouvez utiliser cette technique pour modifier dynamiquement les styles ou thèmes que le [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) illustre.

Toutefois, vous ne pouvez pas définir le `BasedOn` propriété un `DynamicResource` extension de composition, car `BasedOn` n’est pas sauvegardé par une propriété pouvant être liée. Pour dériver un style dynamiquement, ne définissez pas `BasedOn`. Au lieu de cela, définissez la [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété du style que vous souhaitez dériver à partir de la clé du dictionnaire. Le [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) exemple illustre cette technique.

## <a name="device-styles"></a>Styles de périphérique

Le [ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) classe imbriquée définit 12 champs en lecture seule statiques pour les six styles avec un `TargetType` de `Label` que vous pouvez utiliser pour les types courants d’utilisations de texte.

Six de ces champs sont de type `Style` que vous pouvez définir directement à un `Style` propriété dans le code :

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

Les six autres champs sont de type `string` et est utilisable comme clés de dictionnaire pour les styles dynamiques :

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) égal à « BodyStyle »
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) égal à « TitleStyle »
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) égal à « SubtitleStyle »
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) égal à « CaptionStyle »
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) égal à « ListItemTextStyle »
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) égal à « ListItemDetailTextStyle »

Ces styles sont illustrées par la [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) exemple.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Exemples de chapitre 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Styles](~/xamarin-forms/user-interface/styles/index.md)
