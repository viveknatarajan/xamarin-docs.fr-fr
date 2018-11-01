---
title: Liaisons de base de Xamarin.Forms
description: Cet article explique comment utiliser Xamarin.Forms liaison de données, qui lie une paire de propriétés entre deux objets, au moins un d'entre eux est généralement un objet d’interface utilisateur. Ces deux objets sont appelées la cible et la source.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e31cba5c61624b0bca03443262b95d7497564750
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675196"
---
# <a name="xamarinforms-basic-bindings"></a>Liaisons de base de Xamarin.Forms

Une liaison de données Xamarin.Forms lie une paire de propriétés entre deux objets, au moins un d'entre eux est généralement un objet d’interface utilisateur. Ces deux objets sont appelés les *cible* et *source*:

- Le *cible* est l’objet (et la propriété) sur laquelle la liaison de données est définie.
- Le *source* est l’objet (et la propriété) référencé par la liaison de données.

Cette distinction peut parfois être un peu à confusion : dans le cas le plus simple, flux de données de la source à la cible, ce qui signifie que la valeur de la propriété cible est définie à partir de la valeur de la propriété source. Toutefois, dans certains cas, peuvent également de flux de données à partir de la cible à la source ou dans les deux sens. Pour éviter toute confusion, n’oubliez pas que la cible est toujours l’objet sur lequel la liaison de données est définie, même si elle est fournissant des données non réception de données.

## <a name="bindings-with-a-binding-context"></a>Liaisons avec un contexte de liaison

Bien que les liaisons de données sont généralement spécifiés entièrement dans XAML, il est intéressant de voir les liaisons de données dans le code. Le **liaison de la base Code** page contient un fichier XAML avec un `Label` et un `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Le `Slider` est défini pour une plage de 0 à 360. L’objectif de ce programme est de faire pivoter le `Label` en manipulant le `Slider`.

Sans liaisons de données, vous devez définir le `ValueChanged` événements de la `Slider` à un gestionnaire d’événements qui accède à la `Value` propriété de la `Slider` et affecte cette valeur la `Rotation` propriété de la `Label`. La liaison de données automatise ce travail ; le Gestionnaire d’événements et le code qu’il contient ne sont plus nécessaires.

Vous pouvez définir une liaison sur une instance de n’importe quelle classe qui dérive de [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), qui inclut `Element`, `VisualElement`, `View`, et `View` dérivés.  La liaison est toujours définie sur l’objet cible. La liaison fait référence à l’objet source. Pour définir la liaison de données, utilisez les deux membres suivants de la classe cible :

- Le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) propriété spécifie l’objet source.
- Le [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) méthode spécifie la propriété cible et la propriété source.

Dans cet exemple, le `Label` est la cible de liaison et le `Slider` est la source de liaison. Modifications dans le `Slider` source affectent la rotation de la `Label` cible. Flux de données à partir de la source à la cible.

Le `SetBinding` méthode définie par `BindableObject` a un argument de type [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) à partir de laquelle le [ `Binding` ](xref:Xamarin.Forms.Binding) dérive de la classe, mais il existe d’autres `SetBinding` méthodes défini par le [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) classe. Le fichier code-behind dans le **liaison de la base Code** exemple utilise une simple [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) méthode d’extension à partir de cette classe.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

Le `Label` objet étant la cible de liaison qui est l’objet sur lequel cette propriété est définie et sur lequel la méthode est appelée. Le `BindingContext` propriété indique la source de liaison, qui est le `Slider`.

Le `SetBinding` méthode est appelée sur la cible de liaison, mais spécifie la propriété cible et la propriété source. La propriété cible est spécifiée comme un `BindableProperty` objet : `Label.RotationProperty`. La propriété source est spécifiée sous forme de chaîne et indique le `Value` propriété du `Slider`.

Le `SetBinding` méthode révèle une des règles plus importantes des liaisons de données :

*La propriété cible doit reposer sur une propriété pouvant être liée.*

Cette règle implique que l’objet cible doit être une instance d’une classe qui dérive de `BindableObject`. Consultez le [ **propriétés pouvant être liées** ](~/xamarin-forms/xaml/bindable-properties.md) article pour une vue d’ensemble des objets pouvant être liés et des propriétés pouvant être liées.

Il n’est pas le cas pour la propriété source, qui est spécifié sous forme de chaîne. En interne, la réflexion est utilisée pour accéder à la propriété réelle. Dans ce cas particulier, toutefois, le `Value` propriété est également appuyée par une propriété pouvant être liée.

Le code peut être quelque peu simplifié : le `RotationProperty` propriété pouvant être liée est définie par `VisualElement`et héritée par `Label` et `ContentPage` , donc le nom de classe n’est pas nécessaire dans le `SetBinding` appeler :

```csharp
label.SetBinding(RotationProperty, "Value");
```

Toutefois, y compris le nom de classe est un rappel de l’objet cible.

Lorsque vous manipulez le `Slider`, le `Label` fait pivoter en conséquence :

[![Liaison du Code de base](basic-bindings-images/basiccodebinding-small.png "liaison du Code de base")](basic-bindings-images/basiccodebinding-large.png#lightbox "liaison du Code de base")

Le **base liaison Xaml** page est identique à **liaison de Code base** , à ceci près qu’il définit la liaison de données entier dans XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Comme dans le code, la liaison de données est définie sur l’objet cible, qui est le `Label`. Deux extensions de balisage XAML sont impliquées. Il s’agit instantanément reconnaissables par des délimiteurs d’accolade :

- Le `x:Reference` extension de balisage est nécessaire pour référencer l’objet source, qui est la `Slider` nommé `slider`.
- Le `Binding` liens d’extension de balisage la `Rotation` propriété de la `Label` à la `Value` propriété de la `Slider`.

Consultez l’article [Extensions de balisage XAML](~/xamarin-forms/xaml/markup-extensions/index.md) pour plus d’informations sur les extensions de balisage XAML. Le `x:Reference` extension de balisage est prise en charge par le [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) classe ; `Binding` est pris en charge par le [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) classe. En tant que le code XML les préfixes d’espace de noms indiquent, `x:Reference` fait partie de la spécification de XAML 2009, tandis que `Binding` fait partie de Xamarin.Forms. Notez que les guillemets n’apparaissent entre accolades.

Il est facile d’oublier le `x:Reference` extension de balisage lors de la définition du `BindingContext`. Il est courant de définir par inadvertance la propriété directement sur le nom de la source de liaison comme suit :

```xaml
BindingContext="slider"
```

Mais ce n’est pas approprié. Ce balisage définit le `BindingContext` propriété un `string` objet dont les caractères de l’orthographe « curseur » !

Notez que la propriété source est spécifiée avec la [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) propriété du `BindingExtension`, qui correspond à la [ `Path` ](xref:Xamarin.Forms.Binding.Path) propriété de la [ `Binding` ](xref:Xamarin.Forms.Binding) classe.

Le balisage affiché sur le **liaison de base XAML** page peut être simplifiée : les extensions de balisage XAML comme `x:Reference` et `Binding` peut avoir *propriété de contenu* attributs définis pour XAML extensions de balisage signifie que le nom de propriété n’a pas besoin s’affichent. Le `Name` propriété est la propriété de contenu de `x:Reference`et le `Path` propriété est la propriété de contenu de `Binding`, ce qui signifie que vous pouvez les éliminer des expressions :

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Liaisons sans un contexte de liaison

Le `BindingContext` propriété est un composant important de liaisons de données, mais il n’est pas toujours nécessaire. L’objet source à la place peut être spécifié dans le `SetBinding` appeler ou le `Binding` extension de balisage.

Cela est illustré dans le **Alternative Code liaison** exemple. Le fichier XAML est similaire à la **liaison de la base Code** échantillonner, sauf que le `Slider` est défini pour contrôler le `Scale` propriété de la `Label`. Pour cette raison, le `Slider` est défini pour une plage de &ndash;2 à 2 :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Le fichier code-behind définit la liaison avec le [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) méthode définie par `BindableObject`. L’argument est un [constructeur](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) pour le [ `Binding` ](xref:Xamarin.Forms.Binding) classe :

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

Le `Binding` constructeur a 6 de paramètres, donc la `source` paramètre est spécifié avec un argument nommé. L’argument est le `slider` objet.

Ce programme en cours d’exécution peut être un peu surprenant :

[![Liaison du Code de remplacement](basic-bindings-images/alternativecodebinding-small.png "Code alternatif liaison")](basic-bindings-images/alternativecodebinding-large.png#lightbox "liaison du Code de remplacement")

L’écran iOS sur la gauche montre comment l’écran se présente lorsque la page s’affiche tout d’abord. Où est le `Label`?

Le problème est que le `Slider` a une valeur initiale de 0. Cela entraîne le `Scale` propriété de la `Label` pour être également définie sur 0, en remplaçant sa valeur par défaut de 1. Il en résulte le `Label` initialement invisible. Comme le montrer les captures d’écran Android et Universal Windows Platform (UWP), vous pouvez manipuler la `Slider` pour rendre le `Label` apparaissent à nouveau, mais sa disparition initiale est déconcertante.

Vous allez découvrir dans le [prochain article](binding-mode.md) comment éviter ce problème en initialisant le `Slider` à partir de la valeur par défaut de la `Scale` propriété.

> [!NOTE]
> Le [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) classe définit également [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) et [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) propriétés, ce qui peuvent évoluer le `VisualElement` différemment dans le directions horizontale et verticale.

Le **Alternative XAML liaison** page montre la liaison équivalente entièrement dans XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Maintenant le `Binding` extension de balisage a deux propriétés à définir, `Source` et `Path`, séparées par une virgule. Si vous préférez, ils peuvent apparaître sur la même ligne :

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

Le `Source` propriété est définie sur un `x:Reference` extension de balisage qui comporte la même syntaxe que le paramètre le `BindingContext`. Notez que les guillemets ne s’affichent entre accolades, et que les deux propriétés doivent être séparées par une virgule.

La propriété de contenu de la `Binding` extension de balisage est `Path`, mais la `Path=` partie de l’extension de balisage peut uniquement être éliminée si c’est la première propriété dans l’expression. Pour éliminer la `Path=` partie, vous devez échanger les deux propriétés :

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Bien que les extensions de balisage XAML sont généralement délimitées par des accolades, ils peuvent également être exprimés en tant qu’éléments de l’objet :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

Maintenant le `Source` et `Path` propriétés sont des attributs XAML réguliers : les valeurs apparaissent entre guillemets et les attributs ne sont pas séparés par une virgule. Le `x:Reference` extension de balisage peut également constituer un élément objet :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Cette syntaxe n’est pas commune, mais il est parfois nécessaire lorsque des objets complexes sont impliqués.

Les exemples présentés jusqu'à présent, définissent le `BindingContext` propriété et la `Source` propriété du `Binding` à un `x:Reference` extension de balisage pour référencer une autre vue dans la page. Ces deux propriétés sont de type `Object`, et elles peuvent être définies à n’importe quel objet qui inclut les propriétés qui conviennent pour les sources de liaison.

Dans les articles à l’avance, vous découvrirez que vous pouvez définir le `BindingContext` ou `Source` propriété à un `x:Static` extension de balisage pour référencer la valeur d’une propriété statique ou d’un champ, ou un `StaticResource` extension de balisage pour référencer un objet stocké dans un dictionnaire de ressources, ou directement à un objet, qui est généralement (mais pas toujours) une instance d’un ViewModel.

Le `BindingContext` propriété peut également être définie sur une `Binding` objet afin que le `Source` et `Path` propriétés de `Binding` définir le contexte de liaison.

## <a name="binding-context-inheritance"></a>Héritage de contexte de liaison

Dans cet article, vous avez vu que vous pouvez spécifier l’objet source en utilisant le `BindingContext` propriété ou le `Source` propriété de la `Binding` objet. Si les deux sont définis, le `Source` propriété de la `Binding` est prioritaire sur la `BindingContext`.

Le `BindingContext` propriété a une caractéristique extrêmement importante :

*Le paramètre de la `BindingContext` propriété est héritée via l’arborescence visuelle.*

Comme vous le verrez, cela peut être très pratique pour simplifier les expressions de liaison et dans certains cas &mdash; en particulier dans les scénarios de Model-View-ViewModel (MVVM) &mdash; , il est essentiel.

Le **l’héritage du contexte de liaison** exemple est une simple démonstration de l’héritage du contexte de liaison :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

Le `BindingContext` propriété de la `StackLayout` est défini sur le `slider` objet. Ce contexte de liaison est hérité par les deux le `Label` et le `BoxView`, à la fois de qui ont leur `Rotation` propriétés définies sur le `Value` propriété de la `Slider`:

[![L’héritage du contexte de liaison](basic-bindings-images/bindingcontextinheritance-small.png "l’héritage du contexte de liaison")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "l’héritage du contexte de liaison")

Dans le [prochain article](binding-mode.md), vous verrez comment le *mode de liaison* peut modifier le flux de données entre les objets source et cible.


## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du livre de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
