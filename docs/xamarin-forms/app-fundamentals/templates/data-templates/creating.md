---
title: Création d’un Xamarin.Forms DataTemplate
description: Modèles de données peuvent être créés inline, dans un ResourceDictionary, ou à partir d’un type personnalisé ou un type de cellule Xamarin.Forms approprié. Cet article explore chaque technique.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994613"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Création d’un Xamarin.Forms DataTemplate

_Modèles de données peuvent être créés inline, dans un ResourceDictionary, ou à partir d’un type personnalisé ou un type de cellule Xamarin.Forms approprié. Cet article explore chaque technique._

Un scénario d’utilisation courant pour un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) affiche des données à partir d’une collection d’objets dans un [ `ListView` ](xref:Xamarin.Forms.ListView). L’apparence des données pour chaque cellule dans le [ `ListView` ](xref:Xamarin.Forms.ListView) peuvent être gérés en définissant le [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété à un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Il existe un certain nombre de techniques qui peut être utilisé pour effectuer cette opération :

- [Création d’un Inline DataTemplate](#inline).
- [Création d’un modèle de données avec un Type](#type).
- [Création d’un DataTemplate en tant que ressource](#resource).

Quelle que soit la technique utilisée, le résultat est que l’apparence de chaque cellule dans le [ `ListView` ](xref:Xamarin.Forms.ListView) est défini par un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), comme indiqué dans les captures d’écran suivante :

![](creating-images/data-template-appearance.png "ListView avec un DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Création d’un Inline DataTemplate

Le [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété peut être définie sur une ligne [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Un modèle inline, qui est placé comme un enfant immédiat d’une propriété de contrôle approprié, doit être utilisé s’il est inutile de réutiliser le modèle de données ailleurs. Les éléments spécifiés dans le `DataTemplate` définissent l’apparence de chaque cellule, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ListView Margin="0,20,0,0">
    <ListView.ItemsSource>
        <x:Array Type="{x:Type local:Person}">
            <local:Person Name="Steve" Age="21" Location="USA" />
            <local:Person Name="John" Age="37" Location="USA" />
            <local:Person Name="Tom" Age="42" Location="UK" />
            <local:Person Name="Lucas" Age="29" Location="Germany" />
            <local:Person Name="Tariq" Age="39" Location="UK" />
            <local:Person Name="Jane" Age="30" Location="USA" />
        </x:Array>
    </ListView.ItemsSource>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

L’enfant d’un inline [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) doit être de, ou dériver à partir de, tapez [ `ViewCell` ](xref:Xamarin.Forms.ViewCell). Mise en page à l’intérieur de la `ViewCell` ici géré par un [ `Grid` ](xref:Xamarin.Forms.Grid). Le `Grid` contient trois [ `Label` ](xref:Xamarin.Forms.Label) instances que bind leurs [ `Text` ](xref:Xamarin.Forms.Label.Text) propriétés aux propriétés correspondantes de chaque `Person` objet dans la collection.

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

En c#, inline [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) est créé à l’aide d’une surcharge de constructeur qui spécifie un `Func` argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Création d’un modèle de données avec un Type

Le [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété peut également être définie sur une [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) qui est créé à partir d’un type de cellule. L’avantage de cette approche est que l’apparence définie par le type de cellule peut être réutilisée par plusieurs modèles de données tout au long de l’application. Le code XAML suivant montre un exemple de cette approche :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Ici, le [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété est définie sur une [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) qui est créé à partir d’un type personnalisé qui définit l’apparence de cellule. Le type personnalisé doit dériver de type [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), comme illustré dans l’exemple de code suivant :

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

Dans le [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), disposition est gérée ici par une [ `Grid` ](xref:Xamarin.Forms.Grid). Le `Grid` contient trois [ `Label` ](xref:Xamarin.Forms.Label) instances que bind leurs [ `Text` ](xref:Xamarin.Forms.Label.Text) propriétés aux propriétés correspondantes de chaque `Person` objet dans la collection.

Le code c# équivalent est illustré dans l’exemple suivant :

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

En c#, le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) est créé à l’aide d’une surcharge de constructeur qui spécifie le type de cellule en tant qu’argument. Le type de cellule doit dériver de type [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), comme illustré dans l’exemple de code suivant :

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Notez que Xamarin.Forms inclut également des types de cellules qui peuvent être utilisées pour afficher des données simple dans [ `ListView` ](xref:Xamarin.Forms.ListView) cellules. Pour plus d’informations, consultez [apparence de la cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Création d’un DataTemplate en tant que ressource

Modèles de données peuvent également être créés en tant qu’objets réutilisables dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Cela est obtenue en donnant chaque déclaration unique `x:Key` attribut, ce qui lui fournit une clé descriptive dans la `ResourceDictionary`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) est affectée à la [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété à l’aide de la `StaticResource` extension de balisage. Notez que pendant la `DataTemplate` est définie dans la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), il peut également être défini au niveau de l’application ou au niveau du contrôle.

L’exemple de code suivant montre la page équivalente en c# :

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

Le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) est ajouté à la [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) à l’aide de la [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) (méthode), qui spécifie un `Key` chaîne qui est utilisé pour référence le `DataTemplate` lors de leur récupération.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment créer des modèles de données, en ligne, à partir d’un type personnalisé, ou dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Un modèle inline doit être utilisé s’il est inutile de réutiliser le modèle de données ailleurs. Vous pouvez également un modèle de données peut être réutilisé en la définissant comme un type personnalisé, ou comme une ressource de niveau de contrôle, au niveau des pages, ou au niveau de l’application.


## <a name="related-links"></a>Liens associés

- [Apparence de cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Modèles de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
