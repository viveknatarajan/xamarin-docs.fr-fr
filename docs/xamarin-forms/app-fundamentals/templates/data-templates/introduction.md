---
title: Présentation des modèles de données de Xamarin.Forms
description: Xamarin.Forms des modèles de données permettent de définir la présentation des données sur les contrôles pris en charge. Cet article fournit une introduction aux modèles de données, examiner les raisons pour lesquelles ils sont nécessaires.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e54c7d3ea01c59a20561b69c6e790747567d92f0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240175"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Présentation des modèles de données de Xamarin.Forms

_Xamarin.Forms des modèles de données permettent de définir la présentation des données sur les contrôles pris en charge. Cet article fournit une introduction aux modèles de données, examiner les raisons pour lesquelles ils sont nécessaires._

Considérez un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) qui affiche une collection de `Person` objets. L’exemple de code suivant montre la définition de la `Person` classe :

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

Le `Person` classe définit `Name`, `Age`, et `Location` propriétés, qui peuvent être définies lorsque un `Person` objet est créé. Le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est utilisé pour afficher la collection de `Person` des objets, comme indiqué dans l’exemple de code XAML suivant :

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
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Les éléments sont ajoutés à la [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) dans XAML en initialisant le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) propriété à partir d’un tableau de `Person` instances.

> [!NOTE]
> Notez que la `x:Array` élément requiert une `Type` attribut indiquant le type des éléments du tableau.

La page c# équivalente est indiquée dans l’exemple de code suivant, qui initialise la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) propriété un `List` de `Person` instances :

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

Le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) appels `ToString` lors de l’affichage des objets dans la collection. Étant donné qu’aucun `Person.ToString` remplacer, `ToString` retourne le nom de type de chaque objet, comme indiqué dans les captures d’écran suivants :

![](introduction-images/no-data-template.png "ListView sans un modèle de données")

Le `Person` objet peut remplacer le `ToString` méthode pour afficher des données significatives, comme indiqué dans l’exemple de code suivant :

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

Cela entraîne le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) affichant le `Person.Name` valeur de propriété pour chaque objet dans la collection, comme indiqué dans les captures d’écran suivants :

![](introduction-images/override-tostring.png "ListView avec un modèle de données")

Le `Person.ToString` remplacement peut retourner une chaîne mise en forme consistant en la `Name`, `Age`, et `Location` propriétés. Toutefois, cette approche offre uniquement un contrôle limité sur l’apparence de chaque élément de données. Pour plus de souplesse, un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) peuvent être créés qui définit l’apparence des données.

## <a name="creating-a-datatemplate"></a>Création d’un DataTemplate

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) est utilisée pour spécifier l’apparence des données et utilise généralement la liaison de données pour afficher des données. Son scénario d’utilisation courante est lors de l’affichage des données à partir d’une collection d’objets dans une [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Par exemple, lorsque un `ListView` est lié à une collection de `Person` objets, la `ListView.ItemTemplate` propriété sera définie un `DataTemplate` qui définit l’apparence de chaque `Person` de l’objet dans le `ListView`. Le `DataTemplate` contient les éléments à lier les valeurs de propriété de chaque `Person` objet. Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) peut être utilisé en tant que valeur pour les propriétés suivantes :

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), qui est hérité par [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), qui est hérité par [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), et [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Notez que bien que le [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) utilise de [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) des objets, il n’utilise pas un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Il s’agit, car les liaisons de données sont toujours définis directement sur `Cell` objets.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) qui est placé comme un enfant direct des propriétés répertoriées ci-dessus est appelé un *modèle inline*. Vous pouvez également un `DataTemplate` peut être défini comme une ressource au niveau du contrôle, au niveau de la page ou au niveau de l’application. Choix de l’emplacement définir un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) impacts où il peut être utilisé :

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) défini au contrôle au niveau peut uniquement être appliqué au contrôle.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) défini sur la page de niveau peut être appliqué à plusieurs contrôles valides sur la page.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) définis au niveau de l’application peuvent être des contrôles appliqués valide dans l’ensemble de l’application.

Modèles de données plus bas dans la hiérarchie sont prioritaires sur ceux définis plus haut quand ils partagent `x:Key` attributs. Par exemple, un modèle de données au niveau de l’application sera remplacé par un modèle de données au niveau de la page, et un modèle de données au niveau de la page sera remplacé par un modèle de données de niveau de contrôle ou un modèle de données en ligne.


## <a name="related-links"></a>Liens associés

- [Apparence de cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Modèles de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
