---
title: Création d’un Xamarin.Forms DataTemplateSelector
description: Cet article montre comment créer et consommer un DataTemplateSelector, ce qui peut être utilisé pour choisir un DataTemplate lors de l’exécution selon la valeur d’une propriété liée aux données.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994515"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Création d’un Xamarin.Forms DataTemplateSelector

_Un DataTemplateSelector peut être utilisé pour choisir un DataTemplate lors de l’exécution selon la valeur d’une propriété liée aux données. Ainsi, plusieurs modèles de données à appliquer au même type d’objet, pour personnaliser l’apparence d’objets particuliers. Cet article montre comment créer et consommer un DataTemplateSelector._

Un sélecteur de modèle de données permet des scénarios comme un [ `ListView` ](xref:Xamarin.Forms.ListView) liaison à une collection d’objets, où l’apparence de chaque objet dans le `ListView` peut être sélectionnée lors de l’exécution par le sélecteur de modèle de données retournant un particulier [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Création d’un DataTemplateSelector

Un sélecteur de modèle de données est implémenté en créant une classe qui hérite de [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). Le `OnSelectTemplate` méthode est ensuite substituée pour retourner un particulier [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), comme illustré dans l’exemple de code suivant :

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

Une instance de la classe de sélecteur de modèle de données peut ensuite être assignée aux propriétés de contrôle Xamarin.Forms façon [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1). Pour obtenir la liste des propriétés valides, consultez [création d’un DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Limitations

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) instances présentent les limitations suivantes :

- Le `DataTemplateSelector` sous-classe doit toujours retourner le même modèle pour les données mêmes si elle est interrogée plusieurs fois.
- Le `DataTemplateSelector` sous-classe ne doit pas retourner un autre `DataTemplateSelector` sous-classe.
- Le `DataTemplateSelector` sous-classe ne doit pas retourner de nouvelles instances d’un `DataTemplate` sur chaque appel. Au lieu de cela, la même instance doit être retournée. Cela créera une fuite de mémoire et désactive la virtualisation.
- Sur Android, il peut y avoir pas plus de 20 différents modèles de données par `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Utilisation d’un DataTemplateSelector dans XAML

Dans XAML, le `PersonDataTemplateSelector` peut être instancié en le déclarant en tant que ressource, comme indiqué dans l’exemple de code suivant :

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

Ce niveau de la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) définit deux [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) instances et un `PersonDataTemplateSelector` instance. Le `PersonDataTemplateSelector` instance définit son `ValidTemplate` et `InvalidTemplate` propriétés appropriés `DataTemplate` instances à l’aide de la `StaticResource` extension de balisage. Bien que les ressources sont définies dans la page [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), ils peuvent également être définies au niveau de l’application ou au niveau du contrôle.

Le `PersonDataTemplateSelector` instance est consommé en lui assignant la [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété, comme indiqué dans l’exemple de code suivant :

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Lors de l’exécution, le [ `ListView` ](xref:Xamarin.Forms.ListView) appelle le `PersonDataTemplateSelector.OnSelectTemplate` méthode pour chacun des éléments dans la collection sous-jacente, avec l’appel en passant l’objet de données en tant que le `item` paramètre. Le [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) qui est retourné par la méthode est ensuite appliquée à cet objet.

Les captures d’écran suivantes affichent le résultat de la [ `ListView` ](xref:Xamarin.Forms.ListView) appliquant le `PersonDataTemplateSelector` à chaque objet dans la collection sous-jacente :

![](selector-images/data-template-selector.png "ListView avec un sélecteur de modèle de données")

N’importe quel `Person` objet ayant un `DateOfBirth` la valeur de propriété supérieure ou égale à 1980 s’affiche en vert, avec les objets restants affichés en rouge.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Utilisation d’un DataTemplateSelector en C&num;

En c#, le `PersonDataTemplateSelector` peut être instanciée et assignée à la [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) propriété, comme indiqué dans l’exemple de code suivant :

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

Le `PersonDataTemplateSelector` instance définit son `ValidTemplate` et `InvalidTemplate` propriétés appropriés [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) instances créées par le `SetupDataTemplates` (méthode). Lors de l’exécution, le [ `ListView` ](xref:Xamarin.Forms.ListView) appelle le `PersonDataTemplateSelector.OnSelectTemplate` méthode pour chacun des éléments dans la collection sous-jacente, avec l’appel en passant l’objet de données en tant que le `item` paramètre. Le `DataTemplate` qui est retourné par la méthode est ensuite appliquée à cet objet.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer et consommer un [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). Un `DataTemplateSelector` peut être utilisé pour choisir un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) lors de l’exécution selon la valeur d’une propriété liée aux données. Cela permet à plusieurs `DataTemplate` instances à appliquer au même type d’objet, pour personnaliser l’apparence d’objets particuliers.


## <a name="related-links"></a>Liens associés

- [Sélecteur de modèle de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
