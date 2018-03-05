---
title: "Création d’un DataTemplate"
description: "Modèles de données peuvent être créées en ligne, dans un ResourceDictionary, ou à partir d’un type personnalisé ou un type de cellule Xamarin.Forms approprié. Cet article explore chaque technique."
ms.topic: article
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 5721aee95e39b56e9732129774a03f08e0db6cc0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-datatemplate"></a>Création d’un DataTemplate

_Modèles de données peuvent être créées en ligne, dans un ResourceDictionary, ou à partir d’un type personnalisé ou un type de cellule Xamarin.Forms approprié. Cet article explore chaque technique._

Un scénario d’utilisation commun pour un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) affiche des données à partir d’une collection d’objets dans une [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). L’apparence des données pour chaque cellule dans le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) peuvent être gérés en définissant le [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Il existe un certain nombre de techniques qui peut être utilisé pour effectuer cette opération :

- [Création d’un Inline DataTemplate](#inline).
- [Création d’un DataTemplate avec un Type](#type).
- [Création d’un DataTemplate en tant que ressource](#resource).

Quelle que soit la technique utilisée, le résultat est que l’apparence de chaque cellule dans le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est défini par un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), comme indiqué dans les captures d’écran suivants :

![](creating-images/data-template-appearance.png "ListView avec un DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Création d’un Inline DataTemplate

Le [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété peut être définie sur une ligne [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Un modèle inline, qui est placé en tant qu’enfant direct d’une propriété de contrôle approprié, doit être utilisé s’il est inutile de réutiliser le modèle de données ailleurs. Les éléments spécifiés dans le `DataTemplate` définissent l’apparence de chaque cellule, comme indiqué dans l’exemple de code XAML suivant :

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

L’enfant d’une ligne [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) doit être, ou dériver à partir de, tapez [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Mise en page à l’intérieur de la `ViewCell` ici géré par un [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Le `Grid` contient trois [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances de cette liaison leurs [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriétés pour les propriétés appropriées de chaque `Person` objet dans la collection.

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

En c#, inline [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) est créé à l’aide d’une surcharge de constructeur qui spécifie un `Func` argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Création d’un DataTemplate avec un Type

Le [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété peut également être définie un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) qui est créé à partir d’un type de cellule. L’avantage de cette approche est que l’apparence définie par le type de cellule peut être réutilisée par plusieurs modèles de données dans l’ensemble de l’application. Le code XAML suivant montre un exemple de cette approche :

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

Ici, le [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) est définie sur une [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) qui est créé à partir d’un type personnalisé qui définit l’apparence de la cellule. Le type personnalisé doit dériver de type [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), comme illustré dans l’exemple de code suivant :

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

Dans le [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), disposition est ici gérée par un [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Le `Grid` contient trois [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances de cette liaison leurs [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriétés pour les propriétés appropriées de chaque `Person` objet dans la collection.

Le code c# équivalent est indiqué dans l’exemple suivant :

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

En c#, le [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) est créé à l’aide d’une surcharge de constructeur qui spécifie le type de cellule en tant qu’argument. Le type de cellule doit dériver de type [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), comme illustré dans l’exemple de code suivant :

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
> Notez que Xamarin.Forms inclut également des types de cellules qui peuvent être utilisés pour afficher des données simples dans [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cellules. Pour plus d’informations, consultez [apparence de la cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Création d’un DataTemplate en tant que ressource

Modèles de données peuvent également être créées en tant qu’objets réutilisables dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Cela est possible en donnant chaque déclaration unique `x:Key` attribut, qui lui fournit une clé Description dans la `ResourceDictionary`, comme illustré dans l’exemple de code XAML suivant :

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

Le [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) est affectée à la [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété à l’aide de la `StaticResource` extension de balisage. Notez que pendant la `DataTemplate` est défini dans la page [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), elle peut également être définie au niveau de l’application ou au niveau du contrôle.

L’exemple de code suivant montre la page équivalente dans c# :

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

Le [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) est ajouté à la [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) à l’aide de la [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) méthode, qui spécifie un `Key` chaîne qui est utilisé pour référence le `DataTemplate` lors de leur récupération.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment créer des modèles de données, en ligne, à partir d’un type personnalisé, ou dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Un modèle inline doit être utilisé s’il est inutile de réutiliser le modèle de données ailleurs. Vous pouvez également un modèle de données peut être réutilisé en le définissant comme un type personnalisé, ou comme ressource au niveau du contrôle, au niveau des pages, ou au niveau de l’application.


## <a name="related-links"></a>Liens associés

- [Apparence de cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Modèles de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
