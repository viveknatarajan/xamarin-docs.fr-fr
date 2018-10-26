---
title: Xamarin.Forms compilé des liaisons
description: Cet article explique comment utiliser des liaisons compilés pour améliorer les performances de liaison de données dans les applications Xamarin.Forms.
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2018
ms.openlocfilehash: 0b350082c834076a1d69427644259087d64bf26a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111526"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms compilé des liaisons

_Liaisons compilées sont résolus plus rapidement que les liaisons classiques, améliorant ainsi les performances de liaison de données dans les applications Xamarin.Forms._

Liaisons de données a deux problèmes principaux :

1. Il n’existe aucune validation de la compilation des expressions de liaison. Au lieu de cela, les liaisons sont résolues lors de l’exécution. Par conséquent, toutes les liaisons non valides ne sont pas détectées avant l’exécution lorsque l’application ne se comporte pas comme prévu, ou les messages d’erreur apparaissent.
1. Ils ne sont pas rentables. Les liaisons sont résolues lors de l’exécution à l’aide d’inspection d’objets à usage général (reflection) et la surcharge de procéder varie d’une plateforme.

Liaisons compilées améliorent les performances de liaison de données dans les applications Xamarin.Forms en résolvant les expressions de liaison au moment de la compilation plutôt que de runtime. En outre, cette validation au moment de la compilation des expressions de liaison permet un meilleur développeur expérience de dépannage car les liaisons non valides sont signalés comme erreurs de build.

Le processus d’utilisation de liaisons compilées consiste à :

1. Activer la compilation de XAML. Pour plus d’informations sur la compilation de XAML, consultez [XAML Compilation](~/xamarin-forms/xaml/xamlc.md).
1. Définir un `x:DataType` attribut sur un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) pour le type de l’objet qui le `VisualElement` et ses enfants seront lié. Notez que cet attribut peut être défini de nouveau à n’importe quel emplacement dans une hiérarchie d’affichage.

> [!NOTE]
> Il est recommandé de définir le `x:DataType` attribut au même niveau dans la hiérarchie d’affichage comme le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) est défini.

Au moment de la compilation XAML, les expressions de liaison non valide seront signalées comme erreurs de build. Toutefois, le compilateur XAML signalera uniquement pour la première expression de liaison non valide qu’il rencontre une erreur de build. Toutes les expressions de liaison valide qui sont définies sur le `VisualElement` ou ses enfants seront compilées, que le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) est défini en XAML ou de code. Compilation d’une expression de liaison génère du code compilé qui aura une valeur d’une propriété sur le *source*et défini sur la propriété sur le *cible* qui est spécifié dans le balisage. En outre, en fonction de l’expression de liaison, le code généré peut observer les modifications dans la valeur de la *source* propriété et actualiser le *cible* propriété et peut pousser des modifications à partir de la *cible* vers le *source*.

> [!IMPORTANT]
> Liaisons compilées sont actuellement désactivées pour toutes les expressions de liaison qui définissent le [ `Source` ](xref:Xamarin.Forms.Binding.Source) propriété. Il s’agit, car le `Source` propriété est toujours définie à l’aide de la `x:Reference` extension de balisage, qui ne peut pas être résolue au moment de la compilation.

## <a name="using-compiled-bindings"></a>À l’aide de liaisons compilées

Le **compilés un sélecteur de couleurs** page illustre l’utilisation de compilées liaisons entre les vues Xamarin.Forms et les propriétés de ViewModel :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorSelectorPage"
             Title="Compiled Color Selector">
    ...
    <StackLayout x:DataType="local:HslColorViewModel">
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>
        <BoxView Color="{Binding Color}"
                 ... />
        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />
            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>    
</ContentPage>
```

La racine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) instancie le `HslColorViewModel` et initialise le `Color` propriété dans les balises d’élément de propriété pour le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) propriété. Cette racine `StackLayout` définit également la `x:DataType` d’attribut en tant que type ViewModel, ce qui indique que les expressions de liaison à la racine `StackLayout` hiérarchie d’affichage sera compilée. Cela peut être vérifié en modifiant les expressions de liaison à lier à une propriété inexistante ViewModel, ce qui entraîne une erreur de build.

> [!IMPORTANT]
> Le `x:DataType` attribut permettre être redéfini à tout moment dans une hiérarchie d’affichage.

Le [ `BoxView` ](xref:Xamarin.Forms.BoxView), [ `Label` ](xref:Xamarin.Forms.Label) éléments, et [ `Slider` ](xref:Xamarin.Forms.Slider) vues héritent du contexte de liaison à partir de la [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Ces vues sont toutes les cibles de liaison qui font référence à des propriétés de source dans le ViewModel. Pour le [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) propriété et le [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) propriété, les liaisons de données sont `OneWay` – les propriétés dans la vue sont définies à partir des propriétés dans le ViewModel. Toutefois, le [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété utilise un `TwoWay` liaison. Cela permet à chaque `Slider` à définir à partir du ViewModel, ainsi que pour le ViewModel à définir à partir de chaque `Slider`.

Lors de la première exécution de l’application, le [ `BoxView` ](xref:Xamarin.Forms.BoxView), [ `Label` ](xref:Xamarin.Forms.Label) éléments, et [ `Slider` ](xref:Xamarin.Forms.Slider) éléments ont tous la valeur à partir du ViewModel selon initial `Color` propriété est définie lorsque le ViewModel a été instancié. Ceci est illustré dans les captures d’écran suivante :

[![Compilé le sélecteur de couleurs](compiled-bindings-images/compiledcolorselector-small.png "compilé le sélecteur de couleurs")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "compilé le sélecteur de couleurs")

Comme les curseurs sont manipulées, le [ `BoxView` ](xref:Xamarin.Forms.BoxView) et [ `Label` ](xref:Xamarin.Forms.Label) éléments sont mis à jour en conséquence.

Pour plus d’informations sur ce sélecteur de couleurs, consultez [ViewModels et les Notifications de modification de propriété](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications).

## <a name="using-compiled-bindings-in-a-datatemplate"></a>Utilisation de liaisons compilées dans un DataTemplate

Liaisons dans un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) sont interprétées dans le contexte de l’objet basé sur un modèle. Par conséquent, lorsque l’aide compilée liaisons dans un `DataTemplate`, le `DataTemplate` doit déclarer le type de son objet de données à l’aide de la `x:DataType` attribut.

Le **compilé une liste de couleurs** page illustre l’utilisation de liaisons compilées dans un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorListPage"
             Title="Compiled Color List">
    <Grid>
        ...
        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  ... >
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:NamedColor">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     ... />
                            <Label Text="{Binding FriendlyName}"
                                   ... />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <!-- The BoxView doesn't use compiled bindings -->
        <BoxView Color="{Binding Source={x:Reference colorListView}, Path=SelectedItem.Color}"
                 ... />
    </Grid>
</ContentPage>
```

Le [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ListView) propriété est définie sur la méthode statique `NamedColor.All` propriété. Le `NamedColor` classe utilise la réflexion .NET pour énumérer tous les champs publics statiques dans le [ `Color` ](xref:Xamarin.Forms.Color) structure et les stocker avec leurs noms dans une collection qui est accessible à partir de la méthode statique `All` propriété. Par conséquent, le `ListView` est rempli avec toutes les `NamedColor` instances. Pour chaque élément dans le `ListView`, le contexte de l’élément de liaison est défini un `NamedColor` objet. Le [ `BoxView` ](xref:Xamarin.Forms.BoxView) et [ `Label` ](xref:Xamarin.Forms.Label) éléments dans le [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) sont liés aux `NamedColor` propriétés.

Notez que le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) définit le `x:DataType` attribut qui sera la `NamedColor` type, indiquant que toute expressions de liaison dans le `DataTemplate` hiérarchie d’affichage sera compilée. Cela peut être vérifié en modifiant les expressions de liaison pour lier à un inexistante `NamedColor` propriété, ce qui entraîne une erreur de build.

Lors de la première exécution de l’application, le [ `ListView` ](xref:Xamarin.Forms.ListView) est rempli avec `NamedColor` instances. Lorsqu’un élément dans le `ListView` est sélectionnée, le [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) propriété est définie sur la couleur de l’élément sélectionné dans le `ListView`:

[![Compilé liste couleur](compiled-bindings-images/compiledcolorlist-small.png "compilé liste Couleur]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

Sélection d’autres éléments dans le [ `ListView` ](xref:Xamarin.Forms.BoxView) met à jour de la couleur de la [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="combining-compiled-bindings-with-classic-bindings"></a>Liaisons combinant compilé avec des liaisons classiques

Expressions de liaison sont compilées uniquement pour la hiérarchie d’affichage qui le `x:DataType` l’attribut est défini sur. À l’inverse, toutes les vues dans une hiérarchie sur lequel le `x:DataType` attribut n’est pas défini utilisera les liaisons classiques. Par conséquent, il est possible de combiner des liaisons compilés et classique sur une page. Par exemple, dans la précédente section les vues dans le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) utilisent des liaisons compilées, alors que le [ `BoxView` ](xref:Xamarin.Forms.BoxView) qui est défini sur la couleur sélectionnée dans le [ `ListView` ](xref:Xamarin.Forms.ListView) pas.

Structuration prudent de `x:DataType` attributs peuvent ainsi entraîner une page à l’aide de liaisons compilés et classiques. Vous pouvez également le `x:DataType` attribut permettre être redéfini à tout moment dans une hiérarchie d’affichage à `null` à l’aide de la `x:Null` extension de balisage. Cela indique que les expressions de liaison dans la hiérarchie d’affichage utilisent les liaisons classiques. Le *liaisons mixte* page illustre cette approche :

```xaml
<StackLayout x:DataType="local:HslColorViewModel">
    <StackLayout.BindingContext>
        <local:HslColorViewModel Color="Sienna" />
    </StackLayout.BindingContext>
    <BoxView Color="{Binding Color}"
             VerticalOptions="FillAndExpand" />
    <StackLayout x:DataType="{x:Null}"
                 Margin="10, 0">
        <Label Text="{Binding Name}" />
        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</StackLayout>   
```

La racine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) définit le `x:DataType` attribut qui sera la `HslColorViewModel` type, indiquant que toute expression de liaison à la racine `StackLayout` hiérarchie d’affichage sera compilée. Toutefois, interne `StackLayout` redéfinit le `x:DataType` attribut `null` avec la `x:Null` expression de balisage. Par conséquent, les expressions de liaison dans interne `StackLayout` utilisent des liaisons classiques. Uniquement les [ `BoxView` ](xref:Xamarin.Forms.BoxView), au sein de la racine `StackLayout` afficher la hiérarchie, les liaisons utilise compilé.

Pour plus d’informations sur la `x:Null` expression de balisage, consultez [x : Null Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#null).

## <a name="performance"></a>Performances

Liaisons compilées améliorent les performances, avec l’avantage de performances des variables de liaison de données. Tests unitaires révèle que :

- Une liaison compilée qui utilise la notification de modification de propriété (c'est-à-dire un `OneWay`, `OneWayToSource`, ou `TwoWay` liaison) est résolu plus rapides qu’une liaison classique d’environ 8 fois.
- Une liaison compilée qui n’utilise pas les notifications de modification de propriété (c'est-à-dire un `OneTime` liaison) est résolue environ de 20 fois plus rapides que d’une liaison classique.
- Définissant le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) sur une liaison compilée qui utilise la propriété notification de modification (par exemple, un `OneWay`, `OneWayToSource`, ou `TwoWay` liaison) est environ 5 fois plus rapide que de définir la `BindingContext`sur une liaison classique.
- Définition de la [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) notification de modification sur une liaison compilée qui n’utilise pas de propriété (par exemple, un `OneTime` liaison) est environ 7 fois plus rapide que paramètre le `BindingContext` sur une liaison classique.

Ces différences de performances peuvent être accrus sur les appareils mobiles, dépendantes de la plateforme utilisée, la version du système d’exploitation utilisé et l’appareil sur lequel l’application est en cours d’exécution.

## <a name="related-links"></a>Liens connexes

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
