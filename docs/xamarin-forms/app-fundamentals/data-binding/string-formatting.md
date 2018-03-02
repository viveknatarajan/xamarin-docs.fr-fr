---
title: "Chaîne mise en forme"
description: "Utiliser des liaisons de données à mettre en forme et afficher les objets sous forme de chaînes"
ms.topic: article
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 3b54ed876857f0cd04d7a304ff05b9710fbd9e04
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="string-formatting"></a>Chaîne mise en forme

Il est parfois commode d’utiliser des liaisons de données pour afficher la représentation sous forme de chaîne d’un objet ou une valeur. Par exemple, vous souhaiterez peut-être utiliser un `Label` pour afficher la valeur actuelle d’un `Slider`. Dans cette liaison de données, le `Slider` est la source et la cible est la `Text` propriété de la `Label`.

Lorsque vous affichez les chaînes dans le code, l’outil le plus puissant est statique [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) (méthode). La chaîne de mise en forme inclut la mise en forme de codes spécifiques à différents types d’objets, et vous pouvez inclure tout autre texte, ainsi que les valeurs de mise en forme. Consultez le [mise en forme des Types dans .NET](/dotnet/standard/base-types/formatting-types/) article pour plus d’informations sur la mise en forme de chaîne.

## <a name="the-stringformat-property"></a>La propriété StringFormat

Cette fonctionnalité est reportée dans les liaisons de données : vous définissez le [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) propriété du `Binding` (ou le [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) propriété de la `Binding` extension de balisage) à un mise en forme de chaîne avec un espace réservé de .NET standard :

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Notez que la chaîne de mise en forme est délimitée par des caractères de guillemets simples (apostrophe) afin d’éviter l’analyseur XAML en traitant les accolades en tant qu’une autre extension de balisage XAML. Dans le cas contraire, qui permet de chaîne sans le caractère guillemet simple est la même chaîne que vous utiliseriez pour afficher une valeur à virgule flottante dans un appel à `String.Format`. Une spécification de mise en forme de `F2` provoque la valeur à afficher avec deux décimales.

Le `StringFormat` propriété convient uniquement lorsque la propriété cible est de type `string`, et le mode de liaison est `OneWay` ou `TwoWay`. Pour des liaisons bidirectionnelles, le `StringFormat` est uniquement applicable aux valeurs en passant de la source à la cible.

Comme vous allez le voir dans l’article suivant sur le [chemin de liaison](binding-path.md), liaisons de données peuvent devenir très complexe et compliquées. Lors du débogage ces liaisons de données, vous pouvez ajouter un `Label` dans le fichier XAML avec un `StringFormat` pour afficher des résultats intermédiaires. Même si vous l’utilisez que pour afficher un type d’objet, qui peut être utile.

Le **mise en forme de chaîne** page illustre plusieurs utilisations de la `StringFormat` propriété :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Les liaisons sur le `Slider` et `TimePicker` montrent l’utilisation de spécifications de format particulier à `double` et `TimeSpan` des types de données. Le `StringFormat` qui affiche le texte à partir de la `Entry` affichage montre comment spécifier des guillemets doubles dans la chaîne de mise en forme à l’aide de la `&quot;` entité HTML.

La section suivante dans le fichier XAML est un `StackLayout` avec un `BindingContext` définie sur une `x:Static` extension de balisage qui fait référence à la méthode statique `DateTime.Now` propriété. La première liaison ne possède aucune propriété :

```xaml
<Label Text="{Binding}" />
```

Cela affiche simplement la `DateTime` valeur de la `BindingContext` avec la mise en forme par défaut. La deuxième liaison affiche le `Ticks` propriété du `DateTime`, tandis que les deux liaisons affichent le `DateTime` lui-même mise en forme spécifiques. Notez cette `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Si vous avez besoin afficher la gauche ou droite entre des accolades dans votre chaîne de mise en forme, utilisez simplement une paire d’eux.

Les jeux de section dernière le `BindingContext` à la valeur de `Math.PI` et l’affiche avec la mise en forme par défaut et deux différents types de mise en forme numérique.

Voici le programme en cours d’exécution sur les trois plateformes :

[![Mise en forme de chaîne](string-formatting-images/stringformatting-small.png "mise en forme de chaîne")](string-formatting-images/stringformatting-large.png "mise en forme de chaîne")

## <a name="viewmodels-and-string-formatting"></a>ViewModel et mise en forme de chaîne

Lorsque vous utilisez `Label` et `StringFormat` pour afficher la valeur d’une vue qui est également la cible d’un modèle de vues, vous pouvez soit définir la liaison à partir de la vue à la `Label` ou à partir de ce dernier à la `Label`. En général, la deuxième approche est préférable car elle vérifie que les liaisons entre la vue et le ViewModel fonctionnent.

Cette approche est illustrée dans le **mieux sélecteur de couleurs** exemple qui utilise le même ViewModel comme le **Simple sélecteur de couleurs** programme indiqué dans le [ **en Mode de liaison** ](binding-mode.md) article :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

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

Il existe désormais trois paires de `Slider` et `Label` éléments qui sont liés à la même propriété dans la source du `HslColorViewModel` objet. La seule différence est que `Label` a un `StringFormat` à afficher chaque propriété `Slider` valeur.

[![Sélecteur de couleurs mieux](string-formatting-images/bettercolorselector-small.png "mieux sélecteur de couleurs")](string-formatting-images/bettercolorselector-large.png "mieux sélecteur de couleurs")

Vous vous demandez peut-être comment vous pouvez afficher les valeurs RVB (rouges, verts, bleus) au format de hexadécimal à deux chiffres traditionnel. Ces valeurs entières ne sont pas directement disponibles à partir de la `Color` structure. Une solution consisterait à calculer les valeurs entières des composantes de couleur dans le modèle de vues et de les exposer en tant que propriétés. Vous pouvez ensuite les mettre en forme à l’aide de la `X2` spécification de mise en forme.

Une autre approche est plus générale : vous pouvez écrire un *convertisseur de valeurs de liaison* comme indiqué dans l’article ultérieure, [ **des convertisseurs de valeurs de liaison**](converters.md).

L’article suivant, toutefois, explore le [ **chemin de liaison** ](binding-path.md) dans plus de détail et montrent comment vous pouvez l’utiliser pour référencer des sous-propriétés et les éléments des collections.


## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du carnet de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
