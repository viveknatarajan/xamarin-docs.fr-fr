---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: Vs WPF. Cycle de vie de Xamarin.Forms
description: Ce document compare les similitudes et différences entre le cycle de vie de l’application pour les applications WPF et de Xamarin.Forms. Il examine également l’arborescence d’éléments visuels, les graphiques, les ressources et les styles.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: abb7773873fa181085464b5985cc8233715cc4be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781580"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>Vs WPF. Cycle de vie de Xamarin.Forms

Xamarin.Forms prend beaucoup de recommandations de conception des infrastructures basée sur XAML qui la précèdent, en particulier de WPF. Toutefois, d’autres manières il diffère considérablement qui peut être un point de pense-bête pour les personnes qui tente de migrer. Ce document tente d’identifier certains de ces problèmes et fournissent des instructions lorsque cela est possible au pont les connaissances WPF de Xamarin.Forms.

## <a name="app-lifecycle"></a>Cycle de vie

Le cycle de vie de l’application entre WPF et Xamarin.Forms est similaire. Démarrer dans le code externe (plate-forme) et lancer l’interface utilisateur via un appel de méthode. La différence est que Xamarin.Forms commence toujours dans un assembly spécifique à la plateforme, puis initialise et crée l’interface utilisateur pour l’application.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> Le `Main` méthode est, par défaut, automatiquement généré et non visible dans le code.

**Xamarin.Forms**

- **e/s** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Classe d’application

WPF et Xamarin.Forms ont un `Application` classe qui est créé comme un singleton. Dans la plupart des cas, les applications seront dériver de cette classe pour fournir une application personnalisée, bien que cela n’est pas strictement nécessaire dans WPF. Exposent toutes deux un `Application.Current` propriété pour localiser le singleton créé.

### <a name="global-properties--persistence"></a>Propriétés globales + persistance

WPF et Xamarin.Forms ont un `Application.Properties` dictionnaire disponible où vous pouvez stocker des objets globaux au niveau de l’application qui sont accessibles à n’importe où dans l’application. La principale différence est que Xamarin.Forms _conserver_ tous les types primitifs stockés dans la collection lorsque l’application est interrompue et les recharger lors de la relance. WPF ne gère pas automatiquement ce comportement, au lieu de cela, la plupart des développeurs reposait sur le stockage isolé, ou utilisé la fonction intégrée `Settings` prend en charge.

## <a name="defining-pages-and-the-visual-tree"></a>Définition des pages et l’arborescence d’éléments visuels

WPF utilise le `Window` comme élément racine de n’importe quel élément visuel de niveau supérieur. Définit un HWND dans le monde de Windows pour afficher des informations. Vous pouvez créer et afficher les fenêtres autant simultanément comme vous le souhaitez dans WPF.

Dans Xamarin.Forms, l’élément visuel de niveau supérieur est toujours défini par la plateforme - par exemple sur iOS, il s’agit d’un `UIWindow`. Il est contenu dans ces représentations de plateforme natifs à l’aide des convertisseurs de Xamarin.Forms un `Page` classe. Chaque `Page` dans Xamarin.Forms représente une « page » unique dans l’application, où un seul est visible à la fois.

Les deux WPFs `Window` et Xamarin.Forms `Page` incluent un `Title` propriété pour influencer le titre affiché et les deux avoir un `Icon` propriété pour afficher une icône spécifique de la page (**Remarque** qui le titre et l’icône ne sont pas toujours visibles dans Xamarin.Forms). En outre, vous pouvez modifier des propriétés de visual courantes sur les deux, comme la couleur d’arrière-plan ou l’image.

Il est techniquement possible d’effectuer le rendu dans les deux vues distinctes de plateforme (par exemple, définir deux `UIWindow` d’objets et que la seconde restitue à un écran ou externe AirPlay), elle nécessite que du code spécifique à la plateforme pour ce faire et n’est pas une fonctionnalité prise en charge directement de Xamarin.Forms lui-même.

### <a name="views"></a>Affichages

La hiérarchie visual pour les deux infrastructures est similaire. WPF est un peu plus approfondie en raison de sa prise en charge pour les documents de WYSIWYG.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifcycle"></a>Vue Lifcycle

Xamarin.Forms est orienté principalement sur les scénarios mobiles. Par conséquent, les applications sont _activé_, _suspendu_, et _réactivé_ lorsque l’utilisateur interagit avec eux. Cela revient à cliquer sur de le `Window` dans une application WPF et qu’il existe un ensemble de méthodes et événements correspondants, vous pouvez remplacer ou raccorder pour contrôler ce comportement.

| Objectif | WPF (méthode) | Xamarin.Forms (méthode) |
|--- |--- |--- |
|Activation initiale|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Indiqué|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Focus de suspension/perdu|Window.OnDeactivated|Page.OnSleep|
|Activé/obtenu focus|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|N/A|


La prise en charge des contrôles enfants de masquage/affichage, dans WPF il est une propriété à trois états `IsVisible` (visibles, masqués et réduit). Dans Xamarin.Forms, il est uniquement visible ou masqué via le `IsVisible` propriété.

### <a name="layout"></a>Mise en page

Mise en page se produit dans le même 2-test (mesure/réorganiser) qui se produit dans WPF. Vous pouvez vous connecter à la mise en page en substituant les méthodes suivantes dans le Xamarin.Forms `Page` classe :

| Méthode | Objectif |
|--- |--- |
|OnChildMeasureInvalidated|Taille par défaut d’un enfant a changé.|
|OnSizeAllocated|Page a été attribuée à une largeur/hauteur.|
|Événement LayoutChanged|Disposition/taille de la Page a changé.|

Il n’existe aucun événement de disposition global qui est appelée dès aujourd'hui, et n’est pas il global `CompositionTarget.Rendering` événement comme trouvé dans WPF.

#### <a name="common-layout-properties"></a>Propriétés communes de disposition

WPF et Xamarin.Forms prennent en charge `Margin` à espacement contrôle autour d’un élément, et `Padding` de contrôle espacement _à l’intérieur de_ un élément. En outre, la plupart des affichages de présentation de Xamarin.Forms ont des propriétés pour contrôler l’espacement (par exemple, ligne ou colonne).

En outre, la plupart des éléments ont des propriétés pour influencer la manière dont ils sont positionnés dans le conteneur parent :

| WPF | Xamarin.Forms | Objectif |
|--- |--- |--- |
|Alignement horizontal|HorizontalOptions|Options de gauche/centre/droite/Stretch|
|VerticalAlignment|VerticalOptions|Options de haut/centre/bas/Stretch|

> [!NOTE]
> L’interprétation réelle de ces propriétés varie selon le conteneur parent.

#### <a name="layout-views"></a>Modes d’affichage

WPF et Xamarin.Forms utilisent les contrôles de disposition pour positionner des éléments enfants. Dans la plupart des cas, ils sont très proches les uns des autres en termes de fonctionnalités.

| WPF | Xamarin.Forms | Style de disposition |
|--- |--- |--- |
|StackPanel|StackLayout|Empilement infinie de gauche à droite ou haut-bas|
|Grille|Grille|Format tabulaire (lignes et colonnes)|
|DockPanel|N/A|Ancrer les bords de la fenêtre|
|Canvas|DispositionAbsolue|Pixel/coordonnées de positionnement|
|WrapPanel|N/A|Pile de renvoi à la ligne|
|N/A|RelativeLayout|Le positionnement relatif basée sur la règle|

> [!NOTE]
> Xamarin.Forms ne prend pas en charge un `GridSplitter`.

Ces deux plateformes utilisent _propriétés jointes_ optimiser des enfants.

### <a name="rendering"></a>Rendu

Les mécanismes de rendu pour WPF et Xamarin.Forms sont radicalement différents. Dans WPF, les contrôles que vous créez directement afficher le contenu en pixels sur l’écran. WPF gère deux graphiques d’objets (_arborescences_) pour représenter cela - le _arborescence logique_ représente les contrôles tel que défini dans le code ou XAML et le _arborescence d’éléments visuels_ représente le rendu réel qui se produit sur l’écran qui est effectuée directement à l’élément visuel (via une méthode virtuelle dessin), ou via un définies en XAML `ControlTemplate` qui peuvent être remplacé ou personnalisé. En règle générale, l’arborescence d’éléments visuels est plus complexe car il contient ce tels que des bordures autour des étiquettes pour le contenu implicit, etc., de contrôles. WPF inclut un ensemble d’API (`LogicalTreeHelper` et `VisualTreeHelper`) pour examiner ces deux graphiques d’objets.

Dans Xamarin.Forms, les contrôles que vous définissez dans un `Page` sont des objets de données très simple. Ils sont similaires à la représentation sous forme d’arborescence logique, mais jamais afficher le contenu sur leurs propres. Au lieu de cela, ils sont le _modèle de données_ qui influence le rendu des éléments. Le rendu réel est effectué un [séparer l’ensemble de _convertisseurs visual_ qui sont mappé à chaque type de contrôle](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Ces convertisseurs sont enregistrés dans les projets spécifiques à la plateforme par des assemblys avec Xamarin.Forms spécifique à la plateforme. Vous pouvez voir une liste [ici](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). En plus de remplacer ou étendre le convertisseur, Xamarin.Forms prend également en charge [effets](~/xamarin-forms/app-fundamentals/effects/index.md) qui peut être utilisé pour influencer le rendu natif sur une base par-entre plates-formes.

#### <a name="the-logicalvisual-tree"></a>L’arborescence logique/Visual

Il n’existe aucune API exposée à parcourir l’arborescence logique dans Xamarin.Forms -, mais vous pouvez utiliser la réflexion pour obtenir les mêmes informations. Par exemple, [ici est une méthode qui peut énumérer les objets enfants logiques](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) avec réflexion.

## <a name="graphics"></a>Graphiques

Xamarin.Forms n’inclut pas un système de graphiques pour les primitives au-delà d’un rectangle simple (`BoxView`). Vous pouvez inclure des bibliothèques tierces 3e comme [SkiaSharp](~/graphics-games/skiasharp/index.md) pour obtenir le dessin 2D inter-plateformes ou [UrhoSharp](~/graphics-games/urhosharp/index.md) 3D.

## <a name="resources"></a>Ressources

WPF et Xamarin.Forms ont tous deux le concept de ressources et les dictionnaires de ressources. Vous pouvez placer n’importe quel type d’objet en un `ResourceDictionary` avec une clé et rechercher avec `{StaticResource}` pour les éléments qui ne changent pas, ou `{DynamicResource}` à des éléments qui peuvent changer dans le dictionnaire lors de l’exécution. L’utilisation et les mécanismes sont identiques à une différence près : Xamarin.Forms nécessite que vous définissiez le `ResourceDictionary` à affecter à la `Resources` propriété alors que WPF crée au préalable une et lui assigne automatiquement.

Par exemple, consultez la définition ci-dessous :

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

Si vous ne définissez pas le `ResourceDictionary`, une erreur d’exécution est générée.

## <a name="styles"></a>Styles

Styles sont également prises en charge dans Xamarin.Forms et peut être utilisé pour le thème Xamarin.Forms éléments qui composent l’interface utilisateur. Ils prennent en charge l’héritage (propriétés, événements et données), les déclencheurs via `BasedOn`et les recherches de ressources pour les valeurs. Les styles sont appliqués aux éléments soit explicitement par le biais du `Style` propriété, ou implicitement en ne fournissant ne pas une clé de ressource - comme WPF.

### <a name="device-styles"></a>Styles de périphérique

WPF possède un ensemble de propriétés prédéfinies (stockés en tant que valeurs statiques sur un ensemble de classes statiques telles que `SystemColors`) qui déterminent les couleurs système, les polices et les mesures sous la forme de valeurs et clés de ressources. Xamarin.Forms est similaire, mais définit un ensemble de [appareil Styles](~/xamarin-forms/user-interface/styles/device.md) pour représenter les mêmes choses. Ces styles sont fournies par le Framework et les valeurs en fonction de l’environnement d’exécution (par exemple, accessibilité).

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
