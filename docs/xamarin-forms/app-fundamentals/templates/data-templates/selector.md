---
title: Création d’un DataTemplateSelector
description: Un DataTemplateSelector peut être utilisé pour choisir un DataTemplate lors de l’exécution en fonction de la valeur d’une propriété liée aux données. Cela permet à plusieurs DataTemplates à appliquer le même type d’objet, pour personnaliser l’apparence d’objets particuliers. Cet article montre comment créer et consommer un DataTemplateSelector.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ad8cce32cb9cfe2ea5e78f11b1250440371a9851
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847190"
---
# <a name="creating-a-datatemplateselector"></a>Création d’un DataTemplateSelector

_Un DataTemplateSelector peut être utilisé pour choisir un DataTemplate lors de l’exécution en fonction de la valeur d’une propriété liée aux données. Cela permet à plusieurs DataTemplates à appliquer le même type d’objet, pour personnaliser l’apparence d’objets particuliers. Cet article montre comment créer et consommer un DataTemplateSelector._

Un sélecteur de modèle de données permet des scénarios tels qu’un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) liaison à une collection d’objets, où l’apparence de chaque objet dans le `ListView` peut être choisie lors de l’exécution par le sélecteur de modèle de données retournant un particulier [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Création d’un DataTemplateSelector

Un sélecteur de modèle de données est implémenté en créant une classe qui hérite de [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). Le `OnSelectTemplate` (méthode) est ensuite substituée pour retourner un particulier [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), comme illustré dans l’exemple de code suivant :

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

Le `OnSelectTemplate` méthode retourne le modèle approprié en fonction de la valeur de la `DateOfBirth` propriété. Le modèle à retourner est la valeur de la `ValidTemplate` propriété ou le `InvalidTemplate` propriété, qui sont définies lors de l’utilisation du `PersonDataTemplateSelector`.

Une instance de la classe de sélecteur de modèle de données peut ensuite être affectée aux propriétés de contrôle de Xamarin.Forms comme [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Pour obtenir la liste des propriétés valides, consultez [création d’un DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Limitations

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) instances présentent les limitations suivantes :

- Le `DataTemplateSelector` sous-classe doit toujours retourner le même modèle pour les données mêmes si elle est interrogée plusieurs fois.
- Le `DataTemplateSelector` sous-classe ne doit pas retourner une autre `DataTemplateSelector` sous-classe.
- Le `DataTemplateSelector` sous-classe ne doit pas retourner de nouvelles instances d’un `DataTemplate` sur chaque appel. Au lieu de cela, la même instance doit être retournée. Cela créera une fuite de mémoire et désactive la virtualisation.
- Sur Android, il peut y avoir pas plus de 20 divers modèles de données par `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Utilisation d’un DataTemplateSelector en XAML

En XAML, le `PersonDataTemplateSelector` peut être instancié en la déclarant en tant que ressource, comme indiqué dans l’exemple de code suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Ce niveau de la page [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) définit deux [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) instances et un `PersonDataTemplateSelector` instance. Le `PersonDataTemplateSelector` instance définit son `ValidTemplate` et `InvalidTemplate` propriétés approprié `DataTemplate` instances à l’aide de la `StaticResource` extension de balisage. Notez que, lorsque les ressources, sont définis dans la page [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), ils peuvent également être définies au niveau de l’application ou au niveau du contrôle.

Le `PersonDataTemplateSelector` instance est utilisée en l’assignant à la [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété, comme indiqué dans l’exemple de code suivant :

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Lors de l’exécution, le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) appelle la `PersonDataTemplateSelector.OnSelectTemplate` méthode pour chacun des éléments dans la collection sous-jacente, avec l’appel, en passant l’objet de données en tant que le `item` paramètre. Le [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) qui est retourné par la méthode est ensuite appliquée à cet objet.

Les captures d’écran suivantes affichent le résultat de la [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) appliquer le `PersonDataTemplateSelector` à chaque objet dans la collection sous-jacente :

![](selector-images/data-template-selector.png "ListView avec un sélecteur de modèle de données")

N’importe quel `Person` présentant une `DateOfBirth` la valeur de propriété supérieure ou égale à 1980 s’affiche en vert, avec les autres objets qui est affichées en rouge.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Utilisation d’un DataTemplateSelector en C&num;

En c#, le `PersonDataTemplateSelector` peut être instanciée et assignée à la [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) propriété, comme indiqué dans l’exemple de code suivant :

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

Le `PersonDataTemplateSelector` instance définit son `ValidTemplate` et `InvalidTemplate` propriétés approprié [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) instances créées par le `SetupDataTemplates` (méthode). Lors de l’exécution, le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) appelle la `PersonDataTemplateSelector.OnSelectTemplate` méthode pour chacun des éléments dans la collection sous-jacente, avec l’appel, en passant l’objet de données en tant que le `item` paramètre. Le `DataTemplate` qui est retourné par la méthode est ensuite appliquée à cet objet.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer et utiliser un [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). A `DataTemplateSelector` peut être utilisé pour choisir un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) lors de l’exécution en fonction de la valeur d’une propriété liée aux données. Cela permet à plusieurs `DataTemplate` instances à appliquer le même type d’objet, pour personnaliser l’apparence d’objets particuliers.


## <a name="related-links"></a>Liens associés

- [Sélecteur de modèle de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
