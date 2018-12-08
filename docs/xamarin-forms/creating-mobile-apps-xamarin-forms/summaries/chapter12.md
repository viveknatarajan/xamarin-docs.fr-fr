---
title: Résumé du chapitre 12. Styles
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 12. Styles'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bb2cd1c97cc588923e0da1a8793f16445c111f0e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058845"
---
# <a name="summary-of-chapter-12-styles"></a>Résumé du chapitre 12. Styles

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

Dans Xamarin.Forms, les styles permettent plusieurs vues partager une collection de paramètres de propriété. Cela réduit le balisage et que vous permet de maintenir cohérentes thèmes visuels.

Les styles sont presque toujours définis et utilisés dans le balisage. Un objet de type [ `Style` ](xref:Xamarin.Forms.Style) est instancié dans un dictionnaire de ressources et la [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propriété d’un élément visuel en utilisant un `StaticResource` ou `DynamicResource` balisage extension.

## <a name="the-basic-style"></a>Le Style de base

Un `Style` nécessite que son [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) être définie sur le type de l’objet visuel, il s’applique à. Quand un `Style` est instancié dans un dictionnaire de ressources (comme c’est commun) il requiert également un `x:Key` attribut.

Le `Style` a une propriété de type de contenu [ `Setters` ](xref:Xamarin.Forms.Style.Setters), qui est une collection de [ `Setter` ](xref:Xamarin.Forms.Setter) objets. Chaque `Setter` associe un [ `Property` ](xref:Xamarin.Forms.Setter.Property) avec un [ `Value` ](xref:Xamarin.Forms.Setter.Value).

Dans XAML les `Property` paramètre est le nom d’une propriété CLR (comme le `Text` propriété de `Button`), mais la propriété de style doit reposer sur une propriété pouvant être liée. En outre, la propriété doit être définie dans la classe indiquée par le `TargetType` paramètre ou héritées par cette classe.

Vous pouvez spécifier le `Value` définition à l’aide de l’élément de propriété `<Setter.Value>`. Cela vous permet de définir `Value` à un objet qui ne peuvent pas être exprimé dans une chaîne de texte, ou à un `OnPlatform` de l’objet, ou à un objet instancié à l’aide de `x:Arguments` ou `x:FactoryMethod`. Le `Value` propriété peut également être définie avec un `StaticResource` expression à un autre élément dans le dictionnaire.

Le [ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) programme illustre la syntaxe de base et indique comment référencer la `Style` avec un `StaticResource` extension de balisage :

[![Capture d’écran triple du style de base](images/ch12fg01-small.png "base Styles")](images/ch12fg01-large.png#lightbox "Styles de base")

Le `Style` objet et tout objet créé dans le `Style` objet comme un `Value` paramètre sont partagés entre toutes les vues faisant référence à cette `Style`. Le `Style` ne peut pas contenir tout ce qui ne peut pas être partagée, comme un `View` dérivé.

Impossible de définir des gestionnaires d’événements dans un `Style`. Le `GestureRecognizers` propriété ne peut pas être définie dans un `Style` , car il n’est pas associé à une propriété pouvant être liée.

## <a name="styles-in-code"></a>Styles de code

Bien qu’il n’est pas courant, vous pouvez instancier et initialiser `Style` objets dans le code. Cela est illustré par la [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) exemple.

## <a name="style-inheritance"></a>Héritage de style

`Style` a un [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) propriété que vous pouvez définir pour un `StaticResource` extension de balisage faisant référence à un autre style. Ainsi, les styles hériter des styles précédentes et ajouter ou remplacer les paramètres de propriété. Le [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) illustre cela.

Si `Style2` repose sur `Style1`, le `TargetType` de `Style2` doit être le même que `Style1` ou dérivé `Style1`. Le dictionnaire de ressources dans lequel `Style1` est stocké doit être le même dictionnaire de ressources en tant que `Style2` ou un dictionnaire de ressources plus haut dans l’arborescence visuelle.

## <a name="implicit-styles"></a>Styles implicites

Si un `Style` dans une ressource dictionnaire n’a pas un `x:Key` attribut de paramètre, il est affecté automatiquement, une clé de dictionnaire et `Style` objet devient un *style implicite*. Une vue sans un `Style` paramètre et dont le type correspond à la `TargetType` exactement trouverez ce style, comme le [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) illustre.

Un style implicite peut dériver un `Style` avec un `x:Key` paramètre mais pas l’inverse. Vous ne pouvez pas référencer explicitement un style implicite.

Vous pouvez implémenter les trois types de hiérarchie avec des styles et `BasedOn`:

- À partir de styles définis sur le `Application` et `Page` jusqu'à styles définis sur les dispositions plus bas dans l’arborescence visuelle.
- À partir de styles définis pour les classes de base telles que `VisualElement` et `View` pour les styles définis pour les classes spécifiques.
- À partir de styles avec des clés de dictionnaire explicite pour les styles implicites.

Ces hiérarchies sont illustrées dans le [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) exemple.

## <a name="dynamic-styles"></a>Styles dynamiques

Un style dans un dictionnaire de ressources peut être référencé par `DynamicResource` plutôt que `StaticResource`. Cela rend le style un *style dynamique*. Si ce style est remplacé dans le dictionnaire de ressources par un autre style avec la même clé, les vues référençant ce style avec `DynamicResource` change automatiquement. En outre, l’absence d’une entrée de dictionnaire avec la clé spécifiée entraîne `StaticResource` pour lever une exception, mais pas `DynamicResource`.

Vous pouvez utiliser cette technique pour modifier dynamiquement un style ou thèmes comme le [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) montre des exemples.

Toutefois, vous ne pouvez pas définir le `BasedOn` propriété à un `DynamicResource` extension de composition, car `BasedOn` n’est pas sauvegardé par une propriété pouvant être liée. Pour dériver un style dynamiquement, ne définissez pas `BasedOn`. Au lieu de cela, définissez la [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) propriété à la clé du style que vous souhaitez dériver à partir du dictionnaire. Le [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) exemple illustre cette technique.

## <a name="device-styles"></a>Styles d’appareils

Le [ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) classe imbriquée définit 12 champs en lecture seule statiques pour les six styles avec une `TargetType` de `Label` que vous pouvez utiliser pour les types courants d’utilisations de texte.

Six de ces champs sont de type `Style` que vous pouvez définir directement à un `Style` propriété dans le code :

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Les six autres champs sont de type `string` et est utilisable comme clés de dictionnaire pour les styles dynamiques :

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) égal à « BodyStyle »
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) égal à « TitleStyle »
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) égal à « SubtitleStyle »
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) égal à « CaptionStyle »
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) égal à « ListItemTextStyle »
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) égal à « ListItemDetailTextStyle »

Ces styles sont illustrées par la [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) exemple.

## <a name="related-links"></a>Liens connexes

- [Chapitre 12 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Exemples de chapitre 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Styles](~/xamarin-forms/user-interface/styles/index.md)
