---
title: Définir le Mode de sélection CollectionView de Xamarin.Forms
description: Par défaut, la sélection de CollectionView est désactivée. Toutefois, la sélection unique et multiples peut être activée.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/18/2019
ms.openlocfilehash: 441afb9348a85de61d35574bb9121c7de713a897
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367575"
---
# <a name="set-collectionview-selection-mode"></a>Définir le Mode de sélection CollectionView

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> Le `CollectionView` est actuellement une version d’évaluation et ne dispose pas de certaines de ses fonctionnalités planifiée. En outre, l’API peut changer que l’implémentation est terminée.

`CollectionView` définit les propriétés suivantes qui contrôlent la sélection d’éléments :

- `SelectionMode`, de type `SelectionMode`, le mode de sélection.
- `SelectedItem`, de type `object`, l’élément sélectionné dans la liste. Cette propriété a une `null` valeur lorsque aucun élément n’est sélectionné.
- `SelectionChangedCommand`, de type `ICommand`, qui est exécuté lorsque l’élément sélectionné change.
- `SelectionChangedCommandParameter`, de type `object`, qui est le paramètre qui est passé à la `SelectionChangedCommand`.

Ces propriétés sont soutenus par [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objets, ce qui signifie que les propriétés peuvent être des cibles de liaisons de données.

Par défaut, `CollectionView` la sélection est désactivée. Toutefois, ce comportement peut être modifié en définissant le `SelectionMode` valeur de propriété à une de le `SelectionMode` membres de l’énumération :

- `None` – Indique que les éléments ne peuvent pas être sélectionnés. Valeur par défaut.
- `Single` : indique qu’un seul élément peut être sélectionné avec l’élément sélectionné est mis en surbrillance.
- `Multiple` – Indique que plusieurs éléments peuvent être sélectionnés, avec les éléments sélectionnés en cours de mise en surbrillance.

`CollectionView` définit un `SelectionChanged` événement est déclenché quand le `SelectedItem` propriété change, soit parce que l’utilisateur en sélectionnant un élément dans la liste, ou lorsqu’une application définit la propriété. Le `SelectionChangedEventArgs` objet qui accompagne le `SelectionChanged` événement a deux propriétés, tous deux de type `IReadOnlyList<object>`:

- `PreviousSelection` – la liste des éléments qui ont été sélectionnés, avant la modification de la sélection.
- `CurrentSelection` – la liste des éléments qui sont sélectionnés, après le changement de sélection.

## <a name="single-selection"></a>Sélection unique

Lorsque le `SelectionMode` propriété est définie sur `Single`, un élément unique dans le `CollectionView` peut être sélectionné. Lorsqu’un élément est sélectionné, le `SelectedItem` propriété est définie à la valeur de l’élément sélectionné. Lorsque cette propriété change, le `SelectionChangedCommand` est exécutée (avec la valeur de la `SelectionChangedCommandParameter` passé à la `ICommand`) et le `SelectionChanged` se déclenche des événements.

L’exemple XAML suivant montre un `CollectionView` qui peut répondre à la sélection d’élément unique :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

Dans cet exemple de code, le `OnCollectionViewSelectionChanged` Gestionnaire d’événements est exécuté lorsque la `SelectionChanged` se déclenche des événements, avec le Gestionnaire d’événements extrait l’élément précédemment sélectionné et l’élément actuellement sélectionné :

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (previousSelectedItems.FirstOrDefault() as Monkey)?.Name;
    string current = (currentSelectedItems.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> Le `SelectionChanged` événement peut être déclenché par les modifications qui se produisent après le passage du `SelectionMode` propriété.

Les captures d’écran suivantes montrent la sélection d’élément unique dans un `CollectionView`:

[![Capture d’écran de liste verticale CollectionView avec une sélection unique, sur iOS et Android](selection-images/single-selection.png "CollectionView de liste verticale avec une sélection unique")](selection-images/single-selection-large.png#lightbox "CollectionView de liste verticale avec unique sélection")

## <a name="pre-selection"></a>Présélection

Lorsque le `SelectionMode` propriété est définie sur `Single`, un élément unique dans le `CollectionView` peut être présélectionnée en définissant le `SelectedItem` propriété à l’élément. L’exemple XAML suivant montre un `CollectionView` qui présélectionne un seul élément :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

Le `SelectedItem` lie les données de propriété pour le `SelectedMonkey` propriété du modèle de vue connectée, ce qui est de type `Monkey`. Un `TwoWay` liaison est utilisée afin qu’en cas l’utilisateur modifie l’élément sélectionné, la valeur de la `SelectedMonkey` propriété sera définie au `Monkey` objet. Le `SelectedMonkey` propriété est définie dans le `MonkeysViewModel` classe et est défini sur le quatrième élément de la `Monkeys` collection :

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

Par conséquent, lorsque le `CollectionView` s’affiche, le quatrième élément dans la liste est sélectionné au préalable :

[![Capture d’écran d’une liste verticale CollectionView avec présélection unique, sur iOS et Android](selection-images/single-pre-selection.png "CollectionView de liste verticale avec présélection unique")](selection-images/single-pre-selection-large.png#lightbox "liste verticale CollectionView avec la présélection unique")

## <a name="change-selected-item-color"></a>Modifier la couleur de l’élément sélectionné

`CollectionView` a un `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) qui peut être utilisé pour lancer un plus grand changement pour l’élément sélectionné dans le `CollectionView`. Cas d’usage commun pour ce `VisualState` consiste à modifier la couleur d’arrière-plan de l’élément sélectionné, ce qui est illustré dans l’exemple XAML suivant :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> Le [ `Style` ](xref:Xamarin.Forms.Style) qui contient le `Selected` `VisualState` doit avoir un [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) valeur de propriété qui est le type de l’élément racine de la [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), qui est définie comme étant le `ItemTemplate` valeur de propriété.

Dans cet exemple, le [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) propriété a la valeur `Grid` , car l’élément racine de la `ItemTemplate` est un [ `Grid` ](xref:Xamarin.Forms.Grid). Le `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) Spécifie que lorsqu’un élément dans le `CollectionView` est sélectionnée, le [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) de l’élément a la valeur `LightSkyBlue`:

[![Capture d’écran de liste verticale CollectionView avec une couleur de sélection unique personnalisée sur iOS et Android](selection-images/single-selection-color.png "CollectionView de liste verticale avec une couleur de sélection unique personnalisée") ] (selection-images/single-selection-color-large.png#lightbox " Liste verticale de CollectionView avec une couleur de sélection unique personnalisée")

Pour plus d’informations sur les états visuels, consultez [Gestionnaire d’état visuel Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Désactiver la sélection

`CollectionView` sélection est désactivée par défaut. Toutefois, si un `CollectionView` a sélection activée, il peut être désactivé en définissant le `SelectionMode` propriété `None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Le code C# équivalent est :

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

Lorsque le `SelectionMode` propriété est définie sur `None`, les éléments dans le `CollectionView` ne peut pas être sélectionné, le `SelectedItem` propriété restera `null`et le `SelectionChanged` événement ne sera pas déclenché.

> [!NOTE]
> Lorsqu’un élément a été sélectionné et le `SelectionMode` propriété est modifiée à partir de `Single` à `None`, le `SelectedItem` propriété sera définie `null` et le `SelectionChanged` événement est déclenché avec vide `CurrentSelection` propriété.

## <a name="related-links"></a>Liens connexes

- [CollectionView (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Gestionnaire d’état visuel de Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
