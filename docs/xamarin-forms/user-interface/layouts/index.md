---
title: Mises en page
description: Mettre en forme les vues sur l’écran.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 24d9dd332275fd811c0ff60fc514ae0f84c6ee07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="layouts"></a>Mises en page

Xamarin.Forms a plusieurs dispositions et les fonctionnalités pour organiser le contenu sur l’écran. 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Les dispositions de Xamarin.Forms par [Xamarin University](https://university.xamarin.com/)**

Chaque contrôle de disposition est décrite ci-dessous, ainsi que des détails sur la gestion des modifications de l’orientation d’écran :

* **[StackLayout](stack-layout.md)**  &ndash; utilisée pour agencer des vues de manière linéaire, horizontalement ou verticalement. Vues dans un StackLayout peuvent être alignés au centre, à gauche ou à droit de la disposition.
* **[DispositionAbsolue](absolute-layout.md)**  &ndash; utilisé pour organiser les affichages en définissant les coordonnées de la & taille en termes de valeurs absolues ou des rapports. DispositionAbsolue peut servir à des vues de la couche ainsi que leur ancrage à gauche, droite ou au centre.
* **[RelativeLayout](relative-layout.md)**  &ndash; utilisée pour agencer des vues en définissant des contraintes par rapport aux dimensions et la position de leurs parents.
* **[Grille](grid.md)**  &ndash; utilisée pour agencer des vues dans une grille. Lignes et colonnes peuvent être spécifiés en termes de valeurs absolues ou des rapports.
* **[ScrollView](scroll-view.md)**  &ndash; permet de fournir une vue ne peut pas tenir entièrement dans les limites de l’écran de défilement.
* **[LayoutOptions](layout-options.md)**  &ndash; définir l’alignement et l’expansion d’une vue, par rapport à son parent.
* **[Entrée de transparence](#input_transparency)**  &ndash; Spécifie si un élément reçoit l’entrée.
* **[Marge et marge intérieure](margin-and-padding.md)**  &ndash; montre comment contrôler le comportement de disposition quand un élément est affiché dans l’interface utilisateur.
* **[Orientation du périphérique](device-orientation.md)**  &ndash; explique comment gérer les modifications de l’orientation de périphérique.
* **[Mise en page sur les périphériques Tablet PC et du bureau](tablet.md)**  &ndash; montre comment optimiser pour les écrans plus grands sur chaque plateforme.
* **[Création d’une disposition personnalisée](custom.md)**  &ndash; explique comment créer une classe de la disposition personnalisée.
* **[Compression de la mise en page](layout-compression.md)**  &ndash; supprime spécifié de mise en page à partir de l’arborescence d’éléments visuels dans le but d’améliorer les performances de rendu de page.

Contrôles de plateforme peuvent également servir directement dans les dispositions de Xamarin.Forms avec [ **incorporation Native** ](~/xamarin-forms/platform/native-views/index.md) (nouveau dans Xamarin.Forms 2.2), et vous pouvez [ **créer des dispositions personnalisées** ](custom.md) pour répondre aux besoins spécifiques.

Le graphique suivant visualise les contrôles de disposition :

[![](images/layouts-sml.png "Les dispositions de Xamarin.Forms")](images/layouts.png#lightbox "Xamarin.Forms dispositions")

## <a name="choosing-the-right-layout"></a>Choisir la disposition de droite

Les mises en page que vous choisissez dans votre application peuvent aider à ou altération potentielle de lorsque vous créez une application de Xamarin.Forms intéressante et utilisable. Prend un certain temps à considérer comment chaque works disposition peut vous aider à écrire du code de l’interface utilisateur plus claire et plus évolutif. Un écran peut avoir une combinaison des dispositions différentes pour obtenir une conception spécifique.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

Le `StackLayout` est utilisé pour afficher des vues le long d’une ligne horizontale ou verticale. Position et la taille de la mise en page est déterminée en fonction d’une vue `HeightRequest`, `WidthRequest`, `HorizontalOptions` et `VerticalOptions`. `StackLayout` est souvent utilisé en tant que la disposition de base, réorganiser les autres configurations sur l’écran.

Pour obtenir un exemple de situations dans lesquelles `StackLayout` est un bon choix, prenez une application qui a besoin pour afficher un bouton et une étiquette, avec l’étiquette aligné à gauche et le bouton aligné à droite.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

Le `AbsoluteLayout` est utilisé pour afficher des vues, avec la taille et la position spécifiés soit comme des valeurs explicites ou des valeurs par rapport à la taille de la disposition. Contrairement aux `StackLayout` et `Grid`, `AbsoluteLayout` permet l’enfant vues se chevauchent. Contrairement aux `RelativeLayout`, `AbsoluteLayout` ne vous permettent de placer des éléments hors écran.

Pour obtenir un exemple de situations dans lesquelles `AbsoluteLayout` est un bon choix, prenez une application qui doit présenter des collections d’objets en tant que piles. Cela est souvent lors de la présentation de vos albums photos ou des morceaux. Le code suivant donne l’apparence d’une pile, avec des éléments paysage pour fournir des indications sur le contenu de la pile :

En XAML :

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Notez les aspects suivants du code ci-dessus :

- Chaque `Image` s’affiche dans la même position (au milieu de l’espace horizontal)
- Le `Padding` est considéré par `AbsoluteLayout`, contrairement à `RelativeLayout`, qui l’ignore.
- `AbsoluteLayout.LayoutFlags` Spécifie l’interprétation des limites de la mise en page. Dans ce cas `PositionProportional`, signifie que les coordonnées sont un ratio de la taille de la mise en page, tandis que la taille sera interprétée comme une taille explicite.
- `AbsoluteLayout.Layoutbounds` Spécifie la position horizontale, la position verticale, la largeur et la hauteur dans l’ordre.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

Le `RelativeLayout` est utilisé pour afficher des vues, avec la taille et la position spécifiée en tant que valeurs par rapport aux valeurs de la mise en page ou un autre affichage. Valeurs relatives n’avez pas besoin de faire correspondre il correspondant de la valeur de la vue associée. Par exemple, il est possible de définir une vue `Width` propriété proportionnel à une autre vue `X` propriété.

RelativeLayout peut servir à créer des interfaces utilisateur susceptibles d’évoluer proportionnellement entre les tailles de l’appareil. Le code XAML suivant implémente une conception avec des zones dans les angles supérieur, avec un drapeau de golf avec l’indicateur dans le centre de :

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Notez les aspects suivants du code ci-dessus :

- Positions et tailles sont spécifiés sous forme de contraintes.
- Le drapeau de golf se nomme afin que l’indicateur (vert la zone) position peut être définie par rapport au drapeau de golf.
- Les expressions de contrainte ont `Factor` et `Constant` propriétés, qui peuvent être utilisées pour définir les positions et les tailles en multiples (ou fractions) des propriétés d’autres objets, ainsi qu’une constante. Constantes peuvent être négatifs.

### <a name="gridgridmd"></a>[Grid](grid.md)

Le `Grid` est utilisé pour afficher les éléments dans les lignes et colonnes. Notez que la grille n’est pas une table, afin qu’il n’a pas le concept de cellules, les lignes d’en-tête et pied de page ou les bordures séparant les lignes et colonnes. En général, la grille n’est pas appropriée pour afficher les données tabulaires. Pour cette utilisation, envisagez un [ListView](~/xamarin-forms/user-interface/listview/index.md) ou [TableView](~/xamarin-forms/user-interface/tableview.md).

Pour obtenir un exemple de situations dans lesquelles un `Grid` est la disposition de droite à utiliser, envisagez une entrée numérique pour une calculatrice. Une entrée numérique pour une calculatrice peut être constituée de quatre lignes et de trois colonnes, chacune avec un bouton. Le code suivant implémente ce modèle :

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Notez les aspects suivants du code ci-dessus :

- Grilles et colonnes sont spécifiés explicitement, ne pas déduit à partir du contenu.
- `Height` et `Width` valeurs peuvent être définies en étoile, ce qui signifie que la grille définit ces valeurs pour remplir l’espace disponible.
- Position de chaque bouton est spécifiée par `Grid.Row`  &  `Grid.Column` propriétés.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

Le [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) structure peut être utilisée pour définir l’alignement et l’expansion d’une vue, par rapport à son parent.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Marge et remplissage](margin-and-padding.md)

Le [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) et [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriétés contrôlent le comportement de disposition quand un élément est affiché dans l’interface utilisateur.

<a name="input_transparency" />

### <a name="input-transparency"></a>Transparence d’entrée

Chaque élément a un [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) propriété permet de définir si l’élément reçoit l’entrée. Sa valeur par défaut est `false`, garantissant que l’élément reçoit l’entrée.

Lorsque cette propriété est définie sur une classe de conteneur, tel qu’une classe de mise en page, sa valeur les transferts aux éléments enfants. Par conséquent, si le [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) propriété `true` sur une disposition de classe génère les éléments au sein de la mise en page ne reçoit ne pas d’entrée.

### <a name="device-orientationdevice-orientationmd"></a>[Orientation de l’appareil](device-orientation.md)

Xamarin.Forms et ses mises en page intégrés sont capables de gérer les modifications apportées à l’orientation de l’appareil. Prendre en compte les orientations que votre application prend en charge, ainsi que la manière dont vous allez rendre utilisent l’espace fourni dans les modes paysage et portrait.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Mise en page pour les applications de bureau et les Tablet PC](tablet.md)

iOS, Android et Windows prennent toutes en charge les tailles d’écran supérieure sur des plateformes tablet périphériques (ainsi que les ordinateurs portables et de bureau pour Windows). Xamarin.Forms vous permet d’optimiser votre application pour les écrans plus grands en détectant le type de périphérique et l’ajustement de la mise en page, ou à l’aide d’une page totalement différente complètement pour les écrans plus grands.

### <a name="creating-a-custom-layoutcustommd"></a>[Création d’une disposition personnalisée](custom.md)

Xamarin.Forms définit quatre classes de mise en page - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), et [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), et chaque réorganise ses enfants d’une manière différente. Cependant, parfois ses besoins pour organiser le contenu de la page à l’aide d’une disposition ne pas fournies par Xamarin.Forms. Cet article explique comment écrire une classe de disposition personnalisée et illustre une orientation sensible `WrapLayout` classe réorganise ses enfants horizontalement sur la page, puis encapsule l’affichage des enfants suivants pour les lignes supplémentaires.

### <a name="layout-compressionlayout-compressionmd"></a>[Compression de la disposition](layout-compression.md)

Compression de la mise en page supprime les mises en page spécifiées à partir de l’arborescence d’éléments visuels dans le but d’améliorer les performances de rendu de page. Les avantages en matière de performances de cette technique varient selon la complexité d’une page, la version du système d’exploitation utilisé et l’appareil sur lequel l’application est en cours d’exécution. Toutefois, les gains en termes de performances les plus importants seront visibles sur les appareils les plus anciens.

## <a name="making-your-choice"></a>Votre choix.

N’oubliez pas que dans la plupart des cas, plusieurs choix de disposition peut servir à implémenter votre conception de votre choix. Lorsqu’il existe plusieurs choix valides, prendre en compte l’approche la plus simple à votre situation.
Impossible de réaliser la plupart des conceptions avec une seule mise en page, donc imbriquer des dispositions en tant que nécessaire créer des conceptions plus complexes.


## <a name="related-links"></a>Liens associés

- [Indications de l’Interface humaine Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Site Web de conception Android](https://developer.android.com/design/index.html)
- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
