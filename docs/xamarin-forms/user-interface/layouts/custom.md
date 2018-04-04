---
title: Création d’une disposition personnalisée
description: Xamarin.Forms définit quatre classes de mise en page – StackLayout, DispositionAbsolue, RelativeLayout et grille, et chacune réorganise ses enfants d’une manière différente. Toutefois, il est parfois nécessaire organiser le contenu de la page à l’aide d’une disposition non fournie par Xamarin.Forms. Cet article explique comment écrire une classe de disposition personnalisée et illustre une classe de WrapLayout orientation sensibles qui réorganise ses enfants horizontalement sur la page, puis encapsule l’affichage des enfants suivants pour les lignes supplémentaires.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: f0728ac110fcf86f44a5ccb5ddd80b00af1b8d62
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-custom-layout"></a>Création d’une disposition personnalisée

_Xamarin.Forms définit quatre classes de mise en page – StackLayout, DispositionAbsolue, RelativeLayout et grille, et chacune réorganise ses enfants d’une manière différente. Toutefois, il est parfois nécessaire organiser le contenu de la page à l’aide d’une disposition non fournie par Xamarin.Forms. Cet article explique comment écrire une classe de disposition personnalisée et illustre une classe de WrapLayout orientation sensibles qui réorganise ses enfants horizontalement sur la page, puis encapsule l’affichage des enfants suivants pour les lignes supplémentaires._

## <a name="overview"></a>Vue d'ensemble

Dans Xamarin.Forms, toutes les classes de mise en page dérivent la [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) de classe et la contrainte de type générique à [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) et ses types dérivés. À son tour, le `Layout<T>` classe dérive de la [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) (classe), qui fournit le mécanisme pour les enfants de dimensionnement et de positionnement des éléments.

Chaque élément visuel est chargé de déterminer sa taille par défaut, qui est appelé le *demandé* taille. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), et [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) types dérivés sont chargés de déterminer l’emplacement et la taille de leurs enfants ou enfants, par rapport à eux-mêmes. Par conséquent, la disposition implique une relation parent-enfant, où le parent détermine ce qui doit être la taille de ses enfants, mais tente de prendre en compte la taille demandée de l’enfant.

Une bonne compréhension des cycles de mise en page et l’invalidation de Xamarin.Forms est nécessaire pour créer une disposition personnalisée. Ces cycles aborderons maintenant.

## <a name="layout"></a>Mise en page

Mise en page commence en haut de l’arborescence d’éléments visuels avec une page, et il se poursuit dans toutes les branches de l’arborescence visuelle pour englober tous les éléments visuels sur une page. Éléments parents à d’autres éléments sont chargés de dimensionnement et de positionnement de leurs enfants par rapport à eux-mêmes.

Le [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) classe définit un [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthode que les mesures d’un élément pour les opérations de mise en page, et un [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) méthode spécifie zone rectangulaire à. que l' élément sera restitué. Lorsqu’une application démarre et la première page s’affiche, un *cycle de disposition* compose la première des `Measure` des appels, puis `Layout` appelle démarre sur le [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objet :

1. Au cours du cycle de mise en page, chaque élément parent est chargée d’appeler le `Measure` méthode sur ses enfants.
1. Une fois que les enfants ont été mesurés, chaque élément parent est chargée d’appeler le `Layout` méthode sur ses enfants.

Ce cycle permet de garantir que tous les éléments visuels dans la page reçoit des appels à la `Measure` et `Layout` méthodes. Ce processus est illustré dans le diagramme suivant :

![](custom-images/layout-cycle.png "Cycle de disposition de Xamarin.Forms")

> [!NOTE]
> Notez que les cycles de mise en page peuvent également se produire sur un sous-ensemble de l’arborescence d’éléments visuels si quelque chose change pour affecter la disposition. Cela inclut les éléments soient ajoutés ou supprimés d’une collection, comme dans un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), une modification de la [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) propriété d’un élément ou une modification de la taille d’un élément.

Chaque classe Xamarin.Forms qui a un `Content` ou un `Children` propriété a une substituable [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) (méthode). Classes de disposition personnalisée qui dérivent [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) doit substituer cette méthode et vérifiez que le [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) et [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) sont des méthodes appelée sur tous les enfants de l’élément, pour fournir la disposition personnalisée souhaitée.

En outre, chaque classe qui dérive de [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) ou [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) doit remplacer le [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) (méthode), qui est une classe de mise en page Détermine la taille qu’il doit être en appelant le [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthodes de ses enfants.

> [!NOTE]
> Éléments déterminent leur taille en fonction de *contraintes*, qui indiquent la quantité d’espace est disponible pour un élément dans le parent de l’élément. Contraintes passé à la [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) et [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) méthodes peuvent aller de 0 à `Double.PositiveInfinity`. Un élément est *contraint*, ou *complètement contraint*, lorsqu’il reçoit un appel à son [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthode avec des arguments non infini - l’élément est limité pour une taille particulière. Un élément est *sans contrainte*, ou *partiellement contraint*, lorsqu’il reçoit un appel à son `Measure` méthode avec au moins un argument égal à `Double.PositiveInfinity` – la contrainte infinie peut être considéré comme indiquant le redimensionnement automatique.

## <a name="invalidation"></a>Invalidation

Invalidation est le processus par lequel une modification dans un élément sur une page déclenche un nouveau cycle de mise en page. Éléments sont considérés comme non valides quand ils n’ont plus la bonne taille ou la position. Par exemple, si le [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) propriété d’un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) modifications, le `Button` est dite non valide, car il n’a plus la taille correcte. Redimensionner le `Button` peut ensuite posséder les répercussions des modifications dans la mise en page jusqu'à la fin d’une page.

Éléments invalident eux-mêmes en appelant le [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) (méthode), généralement quand une propriété de l’élément change peuvent aboutir à une nouvelle taille de l’élément. Cette méthode déclenche la [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) événement, qui gère le parent de l’élément pour déclencher un nouveau cycle de mise en page.

Le [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) classe définit un gestionnaire pour le [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) événement sur chaque enfant ajouté à son `Content` propriété ou `Children` collection et détache le gestionnaire lors de la enfant est supprimé. Par conséquent, chaque élément dans l’arborescence d’éléments visuels qui a des enfants est averti chaque fois qu’un de ses enfants change de taille. Le diagramme suivant illustre comment une modification de la taille d’un élément dans l’arborescence d’éléments visuels peut entraîner des modifications ripple haut dans l’arborescence :

![](custom-images/invalidation.png "Invalidation de l’arborescence visuelle")

Toutefois, la `Layout` classe tente de limiter l’impact d’un changement de taille d’un enfant sur la disposition d’une page. Si la disposition est la taille de la contrainte, puis une modification de la taille enfant n’affecte pas supérieure à la mise en page parente dans l’arborescence d’éléments visuels. Toutefois, généralement une modification de la taille d’une disposition affecte comment la mise en page réorganise ses enfants. Par conséquent, toute modification de la taille d’une disposition démarrera un cycle de disposition pour la disposition et la mise en page recevra les appels à ses [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) et [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) méthodes.

Le [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) classe définit également un [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) méthode qui a un objectif similaire à la [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) (méthode). Le `InvalidateLayout` méthode doit être appelée chaque fois qu’une modification est apportée qui affecte la façon dont la disposition positionne et des tailles de ses enfants. Par exemple, le `Layout` classe appelle la `InvalidateLayout` méthode chaque fois qu’un enfant est ajouté ou supprimé à partir d’une mise en page.

Le [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) peut être substituée pour implémenter un cache pour réduire les appels de la [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthodes les enfants de la mise en page. Substitution de la `InvalidateLayout` méthode fournit une notification de lorsque les enfants sont ajoutés ou supprimés de la mise en page. De même, la [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) méthode peut être substituée pour fournir une notification lorsqu’un des enfants de la disposition taille change. Pour les substitutions de méthode, une disposition personnalisée doit répondre en effaçant le cache. Pour plus d’informations, consultez [calcul et la mise en cache des données](#caching).

## <a name="creating-a-custom-layout"></a>Création d’une disposition personnalisée

Le processus de création d’une disposition personnalisée est la suivante :

1. Créez une classe qui dérive de la classe `Layout<View>`. Pour plus d’informations, consultez [création d’un WrapLayout](#creating).
1. [*facultatif*] ajouter soutenues par des propriétés pouvant être liées pour tous les paramètres qui doivent être définies sur la classe de mise en page des propriétés. Pour plus d’informations, consultez [Ajout de propriétés soutenu par les propriétés pouvant être liées](#adding_properties).
1. Remplacer la [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) méthode à appeler le [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthode sur enfants toutes la disposition et retournent une taille demandée pour la mise en page. Pour plus d’informations, consultez [substitution de la méthode OnMeasure](#onmeasure).
1. Remplacer la [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) méthode à appeler le [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) méthode sur les enfants du tout la disposition. Échec d’appel du [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) méthode sur chaque enfant dans une disposition entraînera l’enfant jamais recevoir une bonne taille ou la position, et par conséquent, l’enfant ne sera pas visible sur la page. Pour plus d’informations, consultez [substitution de la méthode LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Lors de l’énumération des enfants dans le [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) et [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) remplacements, ignorer tout enfant dont [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) est définie sur `false`. Cela garantit que la disposition personnalisée ne sera pas laisser d’espace pour les enfants invisibles.

1. [*facultatif*] remplacer le [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) méthode à être averti lorsque des enfants sont ajoutés ou supprimés de la mise en page. Pour plus d’informations, consultez [substitution de la méthode InvalidateLayout](#invalidatelayout).
1. [*facultatif*] remplacer le [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) méthode à être averti lorsqu’un des enfants de la disposition taille change. Pour plus d’informations, consultez [substitution de la méthode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Notez que la [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) remplacement ne sera pas appelé si la taille de la disposition est régie par son parent, plutôt que de ses enfants. Toutefois, le remplacement sera appelé si une ou les deux contraintes sont infinies ou si la classe de disposition a par défaut [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ou [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) les valeurs de propriété. Pour cette raison, le [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) override ne peut pas s’appuient sur les tailles enfant obtenus pendant le [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) appel de méthode. Au lieu de cela, `LayoutChildren` doit appeler le [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) méthode sur les enfants de la disposition, avant d’appeler le [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) (méthode). Également la taille des enfants obtenu dans le `OnMeasure` remplacement peut être mis en cache pour éviter plus tard `Measure` appels dans le le `LayoutChildren` override, mais la classe de disposition doivent savoir lorsque les tailles doivent encore obtenues. Pour plus d’informations, consultez [calcul et la mise en cache des données de disposition](#caching).

La classe de disposition peut ensuite être consommée en l’ajoutant à un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)et en ajoutant des enfants de la disposition. Pour plus d’informations, consultez [consomme la WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Création d’un WrapLayout

L’exemple d’application montre une orientation sensible `WrapLayout` classe réorganise ses enfants horizontalement sur la page, puis encapsule l’affichage des enfants suivants pour les lignes supplémentaires.

Le `WrapLayout` classe alloue de la même quantité d’espace pour chaque enfant, appelé le *taille de la cellule*, en fonction de la taille maximale des enfants. Enfants plus petites que la taille de la cellule peut être positionnée au sein de la cellule en fonction de leur [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) les valeurs de propriété.

Le `WrapLayout` définition de classe est indiquée dans l’exemple de code suivant :

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Calcul et la mise en cache des données de disposition

Le `LayoutData` structure stocke les données sur une collection d’enfants dans un nombre de propriétés :

- `VisibleChildCount` – le nombre d’enfants qui sont visibles dans la disposition.
- `CellSize` – la taille maximale de tous les enfants, ajustée à la taille de la mise en page.
- `Rows` – le nombre de lignes.
- `Columns` – le nombre de colonnes.

Le `layoutDataCache` champ permet de stocker plusieurs `LayoutData` valeurs. Lorsque l’application démarre, deux `LayoutData` objets sont mises en cache dans le `layoutDataCache` dictionnaire pour l’orientation actuelle : une pour les arguments de contrainte à la `OnMeasure` override et l’autre pour le `width` et `height` arguments pour le `LayoutChildren` remplacer. Lors de la rotation de l’appareil en orientation paysage, la `OnMeasure` remplacer et `LayoutChildren` remplacement sera à nouveau appelé, ce qui entraînera deux `LayoutData` objets mis en cache dans le dictionnaire. Toutefois, lors du retour de l’appareil en orientation portrait, aucun calcul n’est nécessaire car le `layoutDataCache` possède déjà les données requises.

Le de code suivant montre l’exemple le `GetLayoutData` méthode, qui calcule les propriétés de la `LayoutData` structurée selon une taille spécifique :

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

Le `GetLayoutData` méthode effectue les opérations suivantes :

- Il détermine si un calculé `LayoutData` valeur est déjà dans le cache et la retourne, si elle est disponible.
- Sinon, elle énumère tous les enfants, appelez le `Measure` méthode sur chaque enfant avec une largeur infinie et la hauteur et détermine la taille maximale d’enfants.
- Condition qu’il y a au moins un enfant visible, elle calcule le nombre de lignes et colonnes nécessaires, puis calcule une taille de cellule pour les enfants basés sur les dimensions de la `WrapLayout`. Notez que la taille de la cellule est généralement légèrement plus large que la taille maximale d’enfants, mais qu’il peut également être plus petite si le `WrapLayout` n’est pas assez large pour les enfants ou les taller suffisamment large pour l’enfant plus haut.
- Il stocke la nouvelle `LayoutData` valeur dans le cache.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Ajout de propriétés soutenues par les propriétés pouvant être liées

Le `WrapLayout` classe définit `ColumnSpacing` et `RowSpacing` propriétés, dont les valeurs sont utilisés pour séparer les lignes et colonnes dans la disposition, et qui sont sauvegardé par les propriétés pouvant être liées. Les propriétés pouvant être liées sont affichées dans l’exemple de code suivant :

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Le Gestionnaire de modification de propriété de chaque propriété pouvant être liée appelle la `InvalidateLayout` substitution de méthode pour déclencher une nouvelle disposition passer la `WrapLayout`. Pour plus d’informations, consultez [substitution de la méthode InvalidateLayout](#invalidatelayout) et [substitution de la méthode OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Substitution de la méthode OnMeasure

Le `OnMeasure` override est indiqué dans l’exemple de code suivant :

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

La substitution appelle la `GetLayoutData` (méthode) et constructions un `SizeRequest` objet des données retournées, tout en prenant en compte la `RowSpacing` et `ColumnSpacing` les valeurs de propriété. Pour plus d’informations sur la `GetLayoutData` méthode, consultez [calcul et la mise en cache des données](#caching).

> [!IMPORTANT]
> Le [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) et [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) méthodes ne doivent jamais demander une dimension infinie en retournant un [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) valeur avec une propriété `Double.PositiveInfinity`. Toutefois, au moins un des arguments de contrainte `OnMeasure` peut être `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Substitution de la méthode LayoutChildren

Le `LayoutChildren` override est indiqué dans l’exemple de code suivant :

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Le remplacement commence par un appel à la `GetLayoutData` (méthode), puis énumère tous les enfants de la taille et de les placer dans les cellules de chaque enfant. Cela est possible en appelant le [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) (méthode), qui est utilisé pour positionner un enfant à l’intérieur d’un rectangle en fonction de son [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)valeurs de propriété. Cela équivaut à un appel à l’enfant [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) (méthode).

> [!NOTE]
> Notez que le rectangle passé à la `LayoutChildIntoBoundingRegion` méthode inclut l’ensemble du domaine dans lequel l’enfant peut résider.

Pour plus d’informations sur la `GetLayoutData` méthode, consultez [calcul et la mise en cache des données](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Substitution de la méthode InvalidateLayout

Le [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) override est appelée lorsque des enfants sont ajoutés ou supprimés à partir de la mise en page, ou lorsque l’une de la `WrapLayout` propriétés change de valeur, comme indiqué dans l’exemple de code suivant :

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Le remplacement invalide la disposition et ignore toutes les informations de présentation mis en cache.

> [!NOTE]
> Pour arrêter la [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) classe appel le [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) substitution de méthode chaque fois qu’un enfant est ajouté ou supprimé à partir d’une mise en page, le [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) et [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) méthodes et retournent `false`. La classe de mise en page permettre ensuite implémenter un processus personnalisé lorsque les enfants sont ajoutés ou supprimés.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Substitution de la méthode OnChildMeasureInvalidated

Le [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) override est appelé lorsqu’un des enfants de la disposition change de taille et est indiqué dans l’exemple de code suivant :

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Le remplacement invalide la disposition des enfants et ignore toutes les informations de présentation mis en cache.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Utilisation de la WrapLayout

Le `WrapLayout` classe peut être consommée en le plaçant sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) type dérivé, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Les enfants peuvent ensuite être ajoutés à la `WrapLayout` selon les besoins. Le code suivant montre d’exemple [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) éléments ajoutés à la `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Lorsque la page contenant le `WrapLayout` s’affiche, de façon asynchrone, l’exemple d’application accède à un fichier JSON à distance contenant une liste de photos, crée un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément pour chaque photo et l’ajoute à la `WrapLayout`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

![](custom-images/portait-screenshots.png "Captures d’écran d’exemple Application Portrait")

L’émission des captures d’écran suivante le `WrapLayout` après pivote orientation paysage :

![](custom-images/landscape-ios.png "Exemple iOS Application capture d’écran de paysage")
![](custom-images/landscape-android.png "exemple Android Application paysage capture d’écran de") 
 ![ ] (custom-images/landscape-uwp.png " Capture d’écran du paysage Application exemple UWP")

Le nombre de colonnes dans chaque ligne dépend de la taille de la photo, la largeur d’écran et le nombre de pixels par unité indépendante du périphérique. Le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) éléments chargement de façon asynchrone les photos et par conséquent la `WrapLayout` classe recevront des appels fréquents à son [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) chaque méthode `Image` élément reçoit une nouvelle taille en fonction de la photo chargée.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment écrire une classe de la disposition personnalisée et illustrées orientation sensible `WrapLayout` classe réorganise ses enfants horizontalement sur la page, puis encapsule l’affichage des enfants suivants pour les lignes supplémentaires.


## <a name="related-links"></a>Liens associés

- [WrapLayout (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Dispositions personnalisées](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Créer des dispositions personnalisées dans Xamarin.Forms (vidéo)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Disposition<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Disposition](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
