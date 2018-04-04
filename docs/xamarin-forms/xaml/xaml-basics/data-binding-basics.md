---
title: Partie 4. Principes fondamentaux de liaison de données
description: Liaisons de données permettent de deux objets à lier afin qu’une modification dans une provoque une modification dans l’autre. Il s’agit d’un outil très utile, et pendant que les liaisons de données peuvent être définies entièrement dans le code, XAML fournit des raccourcis et convivialité. Par conséquent, une des extensions de balisage plus importantes dans Xamarin.Forms est obligatoire.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 2aa6fd2f54c09921621a12af9401a6f84ae37ffa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="part-4-data-binding-basics"></a>Partie 4. Principes fondamentaux de liaison de données

_Liaisons de données permettent de deux objets à lier afin qu’une modification dans une provoque une modification dans l’autre. Il s’agit d’un outil très utile, et pendant que les liaisons de données peuvent être définies entièrement dans le code, XAML fournit des raccourcis et convivialité. Par conséquent, une des extensions de balisage plus importantes dans Xamarin.Forms est obligatoire._

## <a name="data-bindings"></a>Liaisons de données

Liaisons de données connectent des propriétés de deux objets, appelées la *source* et *cible*. Dans le code, les deux étapes sont nécessaires : le `BindingContext` propriété de l’objet cible doit être définie à l’objet source et le `SetBinding` (méthode) (souvent utilisée conjointement avec la `Binding` classe) doit être appelée sur l’objet cible pour lier une propriété de ce objet à une propriété de l’objet source.

La propriété cible doit être une propriété pouvant être liée, ce qui signifie que l’objet cible doit dériver de `BindableObject`. La documentation en ligne de Xamarin.Forms indique quelles propriétés sont des propriétés pouvant être liées. Une propriété de `Label` comme `Text` est associé à la propriété pouvant être liée `TextProperty`.

Dans le balisage, vous devez également effectuer les deux mêmes étapes qui sont requises dans le code, sauf que la `Binding` remplace l’extension de balisage de la `SetBinding` appeler et `Binding` classe.

Toutefois, lorsque vous définissez des liaisons de données en XAML, il existe plusieurs méthodes pour définir le `BindingContext` de l’objet cible. Parfois, il est défini dans le fichier code-behind, parfois à l’aide un `StaticResource` ou `x:Static` extension de balisage et parfois en tant que le contenu de `BindingContext` balises d’élément de propriété.

Les liaisons sont plus souvent utilisés pour connecter les éléments visuels d’un programme avec un modèle de données sous-jacent, généralement dans une réalisation de l’architecture d’application MVVM (Model-View-ViewModel), comme indiqué dans [partie 5. À partir des liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), mais d’autres scénarios sont possibles.

## <a name="view-to-view-bindings"></a>Pour consulter les liaisons

Vous pouvez définir des liaisons de données pour lier les propriétés des deux vues sur la même page. Dans ce cas, vous définissez la `BindingContext` de l’objet cible en utilisant le `x:Reference` extension de balisage.

Est un fichier XAML qui contient un `Slider` et deux `Label` vues, un est de pivoter de la `Slider` valeur et une autre qui affiche le `Slider` valeur :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Le `Slider` contient un `x:Name` attribut qui est référencé par les deux `Label` vues à l’aide de la `x:Reference` extension de balisage.

Le `x:Reference` extension de liaison définit une propriété nommée `Name` pour définir le nom de l’élément référencé, dans ce cas `slider`. Toutefois, le `ReferenceExtension` classe qui définit le `x:Reference` extension de balisage définit également un `ContentProperty` attribut `Name`, ce qui signifie qu’il n’est pas explicitement nécessaire. Pour diverses, le premier `x:Reference` inclut « nom = « mais pas la seconde :

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

Le `Binding` extension de balisage lui-même peut avoir plusieurs propriétés, comme le `BindingBase` et `Binding` classe. Le `ContentProperty` pour `Binding` est `Path`, mais le « chemin d’accès = « partie de l’extension de balisage peut être omis si le chemin d’accès est le premier élément de la `Binding` extension de balisage. Le premier exemple a « chemin d’accès = « mais omet le deuxième exemple :

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Les propriétés peuvent être sur une seule ligne ou divisées en plusieurs lignes :

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

Faire tout ce qui est pratique.

Notez le `StringFormat` propriété dans la seconde `Binding` extension de balisage. Dans Xamarin.Forms, liaisons sans effectuer aucune conversion de type implicite, et si vous avez besoin afficher un objet autre qu’une chaîne en tant que chaîne vous devez fournir un convertisseur de type ou utiliser `StringFormat`. Les coulisses, la méthode statique `String.Format` méthode est utilisée pour mettre en œuvre `StringFormat`. Qui est potentiellement un problème, car les spécifications de mise en forme .NET impliquent des accolades, qui sont également utilisés pour délimiter des extensions de balisage. Cette opération crée un risque de confusion l’analyseur XAML. Pour éviter cela, placez la totalité de la chaîne mise en forme dans des guillemets simples :

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Voici le programme en cours d’exécution :

[![](data-binding-basics-images/sliderbinding.png "Pour consulter les liaisons")](data-binding-basics-images/sliderbinding-large.png#lightbox "pour consulter les liaisons ")

## <a name="the-binding-mode"></a>Le Mode de liaison 

Une vue unique peut avoir des liaisons de données sur plusieurs de ses propriétés. Toutefois, chaque vue peut avoir qu’un seul `BindingContext`, de sorte que plusieurs liaisons de données sur cette vue doivent toutes référencer des propriétés du même objet.

La solution à ces informations et autres problèmes implique le `Mode` propriété, qui est définie à un membre de la `BindingMode` énumération :

- `Default` 
- `OneWay` — les valeurs sont transférées de la source à la cible
- `OneWayToSource` — les valeurs sont transférés de la cible à la source
- `TwoWay` — les valeurs sont transférés les deux sens entre source et cible

Le programme suivant montre une utilisation courante de la `OneWayToSource` et `TwoWay` modes de liaison. Quatre `Slider` vues sont destinés au contrôle de la `Scale`, `Rotate`, `RotateX`, et `RotateY` propriétés d’un `Label`. Dans un premier temps, il semble que si ces quatre propriétés de la `Label` doit être des cibles de liaison de données, car chacun est définie un `Slider`. Toutefois, le `BindingContext` de `Label` peut être qu’un seul objet, et il existe quatre curseurs différents.

Pour cette raison, toutes les liaisons sont définies apparemment descendante façons : le `BindingContext` de chacun des quatre curseurs est définie sur le `Label`, et les liaisons sont définies sur le `Value` propriétés des curseurs. À l’aide de la `OneWayToSource` et `TwoWay` modes, ces `Value` propriétés peuvent définir les propriétés de la source, qui sont le `Scale`, `Rotate`, `RotateX`, et `RotateY` propriétés de la `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Les liaisons sur trois la `Slider` les vues sont `OneWayToSource`, ce qui signifie qui le `Slider` valeur provoque une modification de la propriété de son `BindingContext`, qui est la `Label` nommé `label`. Ces trois `Slider` vues entraînent des modifications à la `Rotate`, `RotateX`, et `RotateY` propriétés de la `Label`.

Toutefois, la liaison pour le `Scale` propriété est `TwoWay`. C’est parce que le `Scale` propriété a la valeur par défaut 1 et que vous utilisez un `TwoWay` liaison provoque la `Slider` initiale de la valeur à définir à 1 au lieu de 0. Si cette liaison ont été `OneWayToSource`, le `Scale` propriété est initialement définie sur 0 à partir de la `Slider` valeur par défaut. Le `Label` ne serait pas visible, et qui peut entraîner une certaine confusion pour l’utilisateur.

 [![](data-binding-basics-images/slidertransforms.png "Descendante liaisons")](data-binding-basics-images/slidertransforms-large.png#lightbox "descendante liaisons")

## <a name="bindings-and-collections"></a>Liaisons et les Collections

Nothing illustre la puissance des liaisons de XAML et les données plus un basé sur un modèle `ListView`.

`ListView` définit un `ItemsSource` propriété de type `IEnumerable`, et il affiche les éléments de la collection. Ces éléments peuvent être des objets de tout type. Par défaut, `ListView` utilise le `ToString` (méthode) de chaque élément pour afficher cet élément. Parfois, c’est ce que vous souhaitiez, mais dans de nombreux cas, `ToString` retourne uniquement le nom complet de classe de l’objet.

Toutefois, les éléments de la `ListView` collection peut être affichée comme vous le souhaitez à l’aide d’un *modèle*, qui implique une classe qui dérive de `Cell`. Le modèle a été cloné pour chaque élément dans le `ListView`, et les liaisons de données qui ont été définies sur le modèle sont transférés vers les clones individuels.

Très souvent, vous souhaiterez créer une cellule personnalisée de ces éléments à l’aide de la `ViewCell` classe. Ce processus est relativement désordonné dans le code, mais il devient très simple en XAML.

Le XamlSamples projet n’est incluse dans une classe appelée `NamedColor`. Chaque `NamedColor` objet a `Name` et `FriendlyName` propriétés de type `string`et un `Color` propriété de type `Color`. En outre, `NamedColor` a 141 en lecture seule les champs static de type `Color` correspondant aux couleurs définies dans le Xamarin.Forms `Color` classe. Un constructeur statique crée une `IEnumerable<NamedColor>` collection qui contient `NamedColor` objets correspondant à ces champs statiques et l’affecte à son statique public `All` propriété.

Définition de la méthode statique `NamedColor.All` propriété le `ItemsSource` d’un `ListView` est facile à l’aide de la `x:Static` extension de balisage :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

L’affichage résultant établit que les éléments sont réellement de type `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Liaison à une Collection")](data-binding-basics-images/listview1-large.png#lightbox "liaison à une Collection")

Il n’est pas plus d’informations, mais le `ListView` est permettant le défilement et sélectionnables.

Pour définir un modèle pour les éléments, que vous souhaitez diviser le `ItemTemplate` un élément de propriété, la propriété et affectez-lui un `DataTemplate`, qui référence puis un `ViewCell`. Pour le `View` propriété de la `ViewCell` vous pouvez définir une disposition d’une ou plusieurs vues pour afficher chaque élément. Voici un exemple simple :

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Le `Label` élément est défini sur le `View` propriété de la `ViewCell`. (Le `ViewCell.View` balises ne sont pas nécessaires, car le `View` propriété est la propriété de contenu de `ViewCell`.) Ce balisage affiche le `FriendlyName` propriété de chaque `NamedColor` objet :

[![](data-binding-basics-images/listview2.png "Liaison à une Collection avec un DataTemplate")](data-binding-basics-images/listview2-large.png#lightbox "liaison à une Collection avec un DataTemplate")

Beaucoup mieux. Maintenant, tout ce dont a besoin consiste à améliorer le modèle d’élément avec plus d’informations et la couleur réelle. Pour prendre en charge ce modèle, certaines valeurs et les objets ont été définis dans le dictionnaire de ressources de la page :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Notez l’utilisation de `OnPlatform` pour définir la taille d’un `BoxView` et la hauteur de la `ListView` lignes. Bien que les valeurs pour les trois plateformes sont identiques, le balisage peut facilement être adapté pour d’autres valeurs ajuster l’affichage. 

## <a name="binding-value-converters"></a>Convertisseurs de valeurs de liaison

Précédent **ListView démonstration** fichier XAML affiche la personne `R`, `G`, et `B` propriétés de la Xamarin.Forms `Color` structure. Ces propriétés sont de type `double` et comprise entre 0 et 1. Si vous souhaitez afficher les valeurs hexadécimales, vous ne pouvez pas simplement utiliser `StringFormat` avec une spécification de mise en forme « X2 ». Qui fonctionne uniquement pour les entiers et en outre, le `double` valeurs doivent être multiplié par 255.

Ce petit problème a été résolu avec un *convertisseur*, également appelé un *convertisseur liaison*. Il s’agit d’une classe qui implémente le `IValueConverter` interface, ce qui signifie qu’il existe deux méthodes nommées `Convert` et `ConvertBack`. Le `Convert` méthode est appelée lorsqu’une valeur est transférée à partir de la source vers la cible ; la `ConvertBack` méthode est appelée pour les transferts à partir de la cible à la source dans `OneWayToSource` ou `TwoWay` liaisons :

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

Le `ConvertBack` méthode pas ne joue un rôle dans ce programme, car les liaisons sont uniquement un moyens à partir de la source à la cible. 

Une liaison fait référence à un convertisseur de liaison avec le `Converter` propriété. Un convertisseur de liaison peut également accepter un paramètre spécifié avec le `ConverterParameter` propriété. Pour des raisons de souplesse, voici comment le multiplicateur est spécifié. Le convertisseur de liaison vérifie le paramètre de convertisseur pour valide `double` valeur.

Le convertisseur est instancié dans le dictionnaire de ressources afin qu’il peut être partagé entre plusieurs liaisons :

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Liaisons de données de trois faire référence à cette instance unique. Notez que la `Binding` extension de balisage contient incorporé `StaticResource` extension de balisage :

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Voici le résultat :

[![](data-binding-basics-images/listview3.png "Liaison à une Collection avec un DataTemplate et convertisseurs")](data-binding-basics-images/listview3-large.png#lightbox "liaison à une Collection avec un DataTemplate et convertisseurs")

Le `ListView` est sophistiquée dans la gestion des modifications qui peuvent se produire dynamiquement dans sous-jacent données, mais uniquement si prend certaines mesures. Si la collection d’éléments attribués à la `ItemsSource` propriété de la `ListView` modifications pendant l’exécution, qui est, si les éléments peuvent être ajoutés à ou supprimés de la collection : utiliser une `ObservableCollection` classe pour ces éléments. `ObservableCollection` implémente le `INotifyCollectionChanged` interface, et `ListView` installe un gestionnaire pour le `CollectionChanged` événement.

Si les propriétés des éléments eux-mêmes changent pendant l’exécution, puis les éléments de la collection doivent implémenter le `INotifyPropertyChanged` interface et signal des modifications aux valeurs de propriété à l’aide de la `PropertyChanged` événement. Cela est illustré dans la partie suivante de cette série, [partie 5. À partir de la liaison de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Récapitulatif

Liaisons de données fournissent un mécanisme puissant pour la liaison des propriétés entre deux objets dans une page, ou entre des objets visuels et les données sous-jacentes. Mais lorsque l’application commence à fonctionner avec des sources de données, un modèle d’architecture populaire application commence à émerger comme un paradigme utile. Ce sujet est abordé dans [partie 5. À partir des liaisons de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>Liens associés

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Partie 1. Mise en route avec XAML (exemple)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Partie 2. Syntaxe XAML essentielles (exemple)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Partie 3. Extensions de balisage XAML (exemple)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Partie 5. À partir de la liaison de données à MVVM (exemple)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
