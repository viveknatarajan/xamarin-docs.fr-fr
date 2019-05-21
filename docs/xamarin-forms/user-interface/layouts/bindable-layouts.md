---
title: Dispositions pouvant être liées dans Xamarin.Forms
description: Dispositions pouvant être liées permettent aux classes de mise en page générer leur contenu en le liant à une collection d’éléments, avec l’option permettant de définir l’apparence de chaque élément avec un DataTemplate.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 28846e6e9590d2adf56114fce8bc6056c0112ac1
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970969"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Dispositions pouvant être liées dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

Dispositions pouvant être liées activer n’importe quelle classe de disposition qui dérive de la [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) classe pour générer son contenu en le liant à une collection d’éléments, avec l’option permettant de définir l’apparence de chaque élément avec un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Dispositions pouvant être liées sont fournies par le `BindableLayout` (classe), qui expose les propriétés jointes suivantes :

- `ItemsSource` : Spécifie la collection de `IEnumerable` éléments devant être affichée par la mise en page.
- `ItemTemplate` – Spécifie le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) à appliquer à chaque élément dans la collection d’éléments affichés par la mise en page.
- `ItemTemplateSelector` – Spécifie le [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) qui sera utilisé pour choisir un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) pour un élément lors de l’exécution.

Ces propriétés peuvent être attachées à la [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout), [ `Grid` ](xref:Xamarin.Forms.Grid), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) , et [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) classes qui dérivent de la [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) classe.

> [!NOTE]
> Le `ItemTemplate` propriété est prioritaire lorsqu’à la fois le `ItemTemplate` et `ItemTemplateSelector` propriétés sont définies.

Le `Layout<T>` classe expose un [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) collection, à laquelle les éléments enfants d’une mise en page sont ajoutés. Lorsque le `BinableLayout.ItemsSource` propriété est définie sur une collection d’éléments et attachée à un [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-classe dérivée, chaque élément dans la collection est ajouté à la `Layout<T>.Children` collection pour l’affichage par la mise en page. Le `Layout<T>`-classe dérivée met ensuite à jour ses vues enfants lors de la collection sous-jacente est modifiée. Pour plus d’informations sur le cycle de disposition de Xamarin.Forms, consultez [création d’une disposition personnalisée](~/xamarin-forms/user-interface/layouts/custom.md).

Mises en page peut être liées doivent uniquement utiliser quand la collection d’éléments à afficher est petite et le défilement et la sélection n’est pas nécessaire. Pendant le défilement peut être fourni en encapsulant une disposition peut être liée dans un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), cela n’est pas recommandé comme pouvant être liées dispositions ne disposent pas de la virtualisation de l’interface utilisateur. Lorsque le défilement est requis, une vue de défilement qui inclut la virtualisation de l’interface utilisateur, tel que [ `ListView` ](xref:Xamarin.Forms.ListView) ou [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), doit être utilisé. Le non-respect de cette recommandation peut entraîner des problèmes de performances.

> [!IMPORTANT]
>Bien qu’il soit techniquement possible de joindre une disposition peut être liée à n’importe quelle classe de disposition qui dérive de la [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) (classe), il n’est pas toujours pratique pour ce faire, en particulier pour le [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) , [ `Grid` ](xref:Xamarin.Forms.Grid), et [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) classes. Par exemple, considérez le scénario de qui souhaite afficher une collection de données dans un [ `Grid` ](xref:Xamarin.Forms.Grid) à l’aide d’une disposition peut être liée, où chaque élément dans la collection est un objet contenant plusieurs propriétés. Chaque ligne dans le `Grid` doit afficher un objet à partir de la collection, avec chaque colonne dans la `Grid` affichant l’une des propriétés de l’objet. Étant donné que le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) pour la mise en page peut être liée peut contenir uniquement un seul objet, il est nécessaire pour cet objet à être une classe de disposition contenant plusieurs vues qui affichent chacun une des propriétés de l’objet dans un spécifique`Grid` colonne. Bien que ce scénario peut être réalisé avec des mises en page peut être liées, il en résulte un parent `Grid` contenant un enfant `Grid` pour chaque élément dans la collection liée, qui est une utilisation très inefficace et problématique de le `Grid` mise en page.

## <a name="populating-a-bindable-layout-with-data"></a>Remplissage d’une disposition peut être liée avec des données

Une disposition peut être liée est remplie avec des données en définissant son `ItemsSource` propriété à toute collection qui implémente `IEnumerable`et en l’attachant à un [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-classe dérivée :

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Le code C# équivalent est :

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Lorsque le `BindableLayout.ItemsSource` propriété jointe est définie sur une mise en page, mais la `BindableLayout.ItemTemplate` propriété jointe n’est pas définie, chaque élément le `IEnumerable` collection sera affichée par un [ `Label` ](xref:Xamarin.Forms.Label) qui est créé par le `BindableLayout` classe.

## <a name="defining-item-appearance"></a>Définir l’apparence de l’élément

L’apparence de chaque élément dans la disposition peut être liée peut être définie en configurant le `BindableLayout.ItemTemplate` propriété jointe un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Le code C# équivalent est :

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

Dans cet exemple, chaque élément dans le `TopFollowers` collection sera affichée par un `CircleImage` vue définie dans le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

![Mise en page peut être liée avec un DataTemplate](bindable-layouts-images/top-followers.png "disposition peut être liée avec un modèle de données")

Pour plus d’informations sur les modèles de données, consultez [Modèles de données Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choosing-item-appearance-at-runtime"></a>Choix d’apparence de l’élément lors de l’exécution

L’apparence de chaque élément dans la disposition peut être liée peut être choisie lors de l’exécution, selon la valeur de l’élément, en définissant le `BindableLayout.ItemTemplateSelector` propriété jointe un [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Le code C# équivalent est :

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

Le [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) utilisé dans l’exemple de l’application est illustrée dans l’exemple suivant :

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

Le `TechItemTemplateSelector` classe définit `DefaultTemplate` et `XamarinFormsTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) propriétés qui sont définies sur les différents modèles de données. Le `OnSelectTemplate` méthode retourne le `XamarinFormsTemplate`, qui affiche un élément en rouge foncé avec un cœur en regard de celle-ci, lorsque l’élément est égal à « Xamarin.Forms ». Lorsque l’élément n’est pas égal à « Xamarin.Forms », le `OnSelectTemplate` méthode retourne le `DefaultTemplate`, qui affiche un élément à l’aide de la couleur par défaut d’un [ `Label` ](xref:Xamarin.Forms.Label):

![Mise en page peut être liée avec un DataTemplateSelector](bindable-layouts-images/favorite-tech.png "disposition peut être liée avec un sélecteur de modèle de données")

Pour plus d’informations sur les sélecteurs de modèle de données, consultez [création d’un Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Liens connexes

- [Démonstration de disposition peut être liée (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [Création d’une disposition personnalisée](~/xamarin-forms/user-interface/layouts/custom.md)
- [Modèles de données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Création d’un Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
