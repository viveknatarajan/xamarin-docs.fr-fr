---
title: Faire défiler un élément dans la vue
description: Quand un où les balayages utilisateur pour lancer un défilement, la position de fin du défilement peut être contrôlée afin que les éléments sont affichées. En outre, CollectionView définit deux méthodes ScrollTo, défilement par programmation des éléments dans l’affichage.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: da7f379076b8e193deddc99e9004f051ba006cbb
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329947"
---
# <a name="scroll-an-item-into-view"></a>Faire défiler un élément dans la vue

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` définit deux `ScrollTo` méthodes, que faire défiler les éléments dans la vue. Une des surcharges fait défiler l’élément à l’index spécifié dans la vue, tandis que l’autre fait défiler l’élément spécifié dans la vue. Les deux surcharges ont des arguments supplémentaires qui peuvent être spécifiées pour indiquer la position exacte de l’élément après que le défilement est terminé et si vous souhaitez animer le défilement.

`CollectionView` définit un `ScrollToRequested` événement est déclenché lorsque un de la `ScrollTo` méthodes est appelée. Le `ScrollToRequestedEventArgs` objet qui accompagne le `ScrollToRequested` événement dispose de nombreuses propriétés, y compris `IsAnimated`, `Index`, `Item`, et `ScrollToPosition`. Ces propriétés sont définies à partir des arguments spécifiés dans le `ScrollTo` les appels de méthode.

Quand un où les balayages utilisateur pour lancer un défilement, la position de fin du défilement peut être contrôlée afin que les éléments sont affichées. Cette fonctionnalité est appelée l’alignement, étant donné que les éléments d’alignement pour positionner lors du défilement s’arrête. Pour plus d’informations, consultez [points d’alignement](#snap-points).

## <a name="scroll-an-item-at-an-index-into-view"></a>Faire défiler un élément à un index dans la vue

Le premier `ScrollTo` surcharge de méthode fait défiler l’élément à l’index spécifié dans la vue. Étant donné un `CollectionView` objet nommé `collectionView`, l’exemple suivant montre comment faire défiler l’élément à l’index 12 dans la vue :

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>Faire défiler un élément dans la vue

La seconde `ScrollTo` surcharge de méthode fait défiler l’élément spécifié dans la vue. Étant donné un `CollectionView` objet nommé `collectionView`, l’exemple suivant montre comment faire défiler l’élément spécifié dans la vue :

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>Position de défilement de contrôle

Lors du défilement d’un élément dans la vue, la position exacte de l’élément une fois terminé le défilement peut être spécifiée avec le `position` argument de la `ScrollTo` méthodes. Cet argument accepte un [ `ScrollToPosition` ](xref:Xamarin.Forms.ScrollToPosition) membre d’énumération.

### <a name="makevisible"></a>MakeVisible

Le [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) membre indique que l’élément doit défiler jusqu'à ce qu’il soit visible dans la vue :

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Cet exemple de code entraîne le défilement minimal requis pour faire défiler l’élément dans la vue :

[![Capture d’écran de liste verticale CollectionView avec un élément objet d’un défilement dans une vue, sur iOS et Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView de liste verticale avec élément défilé")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView de liste verticale avec élément objet d’un défilement")

> [!NOTE]
> Le [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) membre est utilisé par défaut, si le `position` argument n’est pas spécifié lors de l’appel le `ScrollTo` (méthode).

### <a name="start"></a>Start

Le [ `ScrollToPosition.Start` ](xref:Xamarin.Forms.ScrollToPosition) membre indique que l’élément doit faire défiler vers le début de la vue :

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Cet exemple de code génère l’élément en cours défile vers le début de la vue :

[![Capture d’écran de liste verticale CollectionView avec un élément objet d’un défilement dans une vue, sur iOS et Android](scrolling-images/scrolltoposition-start.png "CollectionView de liste verticale avec élément défilé")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView de liste verticale avec élément objet d’un défilement")

### <a name="center"></a>Center

Le [ `ScrollToPosition.Center` ](xref:Xamarin.Forms.ScrollToPosition) membre indique que l’élément doit faire défiler vers le centre de la vue :

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Cet exemple de code génère l’élément en cours défile vers le centre de la vue :

[![Capture d’écran de liste verticale CollectionView avec un élément objet d’un défilement dans une vue, sur iOS et Android](scrolling-images/scrolltoposition-center.png "CollectionView de liste verticale avec élément défilé")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView de liste verticale avec élément objet d’un défilement")

### <a name="end"></a>Fin

Le [ `ScrollToPosition.End` ](xref:Xamarin.Forms.ScrollToPosition) membre indique que l’élément doit être l’objet d’un défilement à la fin de la vue :

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Cet exemple de code génère l’élément en cours défile vers la fin de la vue :

[![Capture d’écran de liste verticale CollectionView avec un élément objet d’un défilement dans une vue, sur iOS et Android](scrolling-images/scrolltoposition-end.png "CollectionView de liste verticale avec élément défilé")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView de liste verticale avec élément objet d’un défilement")

## <a name="disable-scroll-animation"></a>Désactiver l’animation de défilement

Une animation de défilement s’affiche lors du défilement d’un élément dans la vue. Toutefois, cette animation peut être désactivée en définissant le `animate` argument de la `ScrollTo` méthode `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>Points d’alignement

Quand un où les balayages utilisateur pour lancer un défilement, la position de fin du défilement peut être contrôlée afin que les éléments sont affichées. Cette fonctionnalité est appelée l’alignement, étant donné que les éléments d’alignement pour positionner lorsque le défilement s’arrête et est contrôlé par les propriétés suivantes à partir de la `ItemsLayout` classe :

- `SnapPointsType`, de type `SnapPointsType`, spécifie le comportement des points d’ancrage lors du défilement.
- `SnapPointsAlignment`, de type `SnapPointsAlignment`, spécifie comment les points d’ancrage sont alignées avec les éléments.

Ces propriétés sont secondées par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

> [!NOTE]
> En cas d’alignement, il se produira dans la direction qui produit le moins de mouvement.

### <a name="snap-points-type"></a>Type de points d’alignement

Le `SnapPointsType` énumération définit les membres suivants :

- `None` Indique que le défilement ne s’aligne pas aux éléments.
- `Mandatory` Indique que le contenu s’aligne sur le composant logiciel enfichable le plus proche pointe toujours vers où le défilement naturellement arrêterait, le long de la direction de l’inertie.
- `MandatorySingle` Indique le même comportement que `Mandatory`, mais uniquement fait défiler un élément à la fois.

Par défaut, le `SnapPointsType` propriété est définie sur `SnapPointsType.None`, ce qui garantit que le défilement ne s’aligne pas les éléments, comme indiqué dans les captures d’écran suivante :

[![Capture d’écran d’une liste verticale CollectionView sans points d’ancrage, sur iOS et Android](scrolling-images/snappoints-none.png "CollectionView de liste verticale sans points d’ancrage")](scrolling-images/snappoints-none-large.png#lightbox "CollectionView de liste verticale sans le composant logiciel enfichable points")

### <a name="snap-points-alignment"></a>Alignement des points d’alignement

Le `SnapPointsAlignment` énumération définit `Start`, `Center`, et `End` membres.

> [!IMPORTANT]
> La valeur de la `SnapPointsAlignment` propriété n’est respecté lorsque le `SnapPointsType` propriété est définie sur `Mandatory`, ou `MandatorySingle`.

#### <a name="start"></a>Start

Le `SnapPointsAlignment.Start` membre indique que les points d’ancrage sont alignés avec le bord de tête d’éléments.

Par défaut, la propriété `SnapPointsAlignment` a la valeur `SnapPointsAlignment.Start`. Toutefois, par souci d’exhaustivité, l’exemple XAML suivant montre comment définir ce membre d’énumération :

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Quand un où les balayages utilisateur pour lancer un défilement, le premier élément est aligné en haut de la vue :

[![Capture d’écran de liste verticale CollectionView avec des points d’ancrage de début, sur iOS et Android](scrolling-images/snappoints-start.png "CollectionView de liste verticale avec des points d’ancrage de début")](scrolling-images/snappoints-start-large.png#lightbox "CollectionView de liste verticale avec démarrage points d’alignement")

#### <a name="center"></a>Center

Le `SnapPointsAlignment.Center` membre indique que les points d’ancrage sont alignées avec le centre d’éléments. L’exemple XAML suivant montre comment définir ce membre d’énumération :

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Quand un où les balayages utilisateur pour lancer un défilement, le premier élément sera aligné en haut de la vue de centre :

[![Capture d’écran de liste verticale CollectionView avec les points d’ancrage center sur iOS et Android](scrolling-images/snappoints-center.png "CollectionView de liste verticale avec des points d’ancrage center")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView de liste verticale avec points centraux de composant logiciel enfichable")

#### <a name="end"></a>Fin

Le `SnapPointsAlignment.End` membre indique que les points d’ancrage sont alignés avec le bord de fin des éléments. L’exemple XAML suivant montre comment définir ce membre d’énumération :

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Quand un où les balayages utilisateur pour lancer un défilement, le dernier élément est aligné sur le bas de la vue :

[![Capture d’écran de liste verticale CollectionView avec des points d’ancrage de fin, sur iOS et Android](scrolling-images/snappoints-end.png "CollectionView de liste verticale avec des points de terminaison de composant logiciel enfichable")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView de liste verticale d’alignement de fin points")

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
