---
title: "Sources de données de ListView"
description: "Découvrez comment remplir votre contrôle ListView avec des données."
ms.topic: article
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 78659aff0b6b4e6401bec96a55935c141d1199f1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="listview-data-sources"></a>Sources de données de ListView

ListView est utilisé pour l’affichage des listes de données. Nous découvrirez le remplissage d’une liste avec des données, et comment nous pouvons créer de liaison à l’élément sélectionné.

- **[Paramètre ItemsSource](#ItemsSource)**  &ndash; utilise une liste simple ou un tableau.
- **[Liaison de données](#Data_Binding)**  &ndash; établit une relation entre un modèle et le ListView. Liaison est idéale pour le modèle MVVM.

## <a name="itemssource"></a>ItemsSource
ListView est remplie avec des données à l’aide du `ItemsSource` propriété, qui peut accepter toute collection implémentant `IEnumerable`. La façon la plus simple pour remplir un `ListView` implique l’utilisation d’un tableau de chaînes :

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "Liste d’affichage de ListView de chaînes")

L’approche ci-dessus remplira la `ListView` avec une liste de chaînes. Par défaut, `ListView` appellera `ToString` et afficher le résultat dans un `TextCell` pour chaque ligne. Pour personnaliser l’affichage des données, consultez [apparence de la cellule](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Étant donné que `ItemsSource` a été envoyée à un tableau, le contenu ne met pas à jour en tant que les modifications de liste ou le tableau sous-jacent. Si vous souhaitez que le ListView pour mettre à jour automatiquement que les éléments sont ajoutés, supprimés et modifiés dans la liste sous-jacente, vous devez utiliser un `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) est défini dans `System.Collections.ObjectModel` et est comparable à `List`, sauf qu’elle peut avertir `ListView` de toutes les modifications :

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Liaison de données
Liaison de données est le type « glue » qui lie les propriétés d’un objet d’interface utilisateur pour les propriétés d’un objet CLR, tel qu’une classe dans votre modèle de vues. Liaison de données est utile car elle simplifie le développement d’interfaces utilisateur en remplaçant un grand nombre de code fastidieux.

Liaison de données fonctionne en conservant les objets synchronisés que leurs valeurs liées changent. Au lieu d’avoir à écrire des gestionnaires d’événements pour chaque modification de valeur d’un contrôle, vous établissez la liaison et activez la liaison dans votre modèle de vues.

Pour plus d’informations sur la liaison de données, consultez [principes de base de données de liaison](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) qui fait partie quatre de la [Xamarin.Forms XAML notions de base de l’article série](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Liaison de cellules
Propriétés des cellules (et les enfants de cellules) peuvent être liées aux propriétés des objets dans le `ItemsSource`. Par exemple, un ListView peut servir à présenter une liste d’employés avec des images.

La classe d’employé :

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` est créée et définie comme la `ListView`de `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

La liste est remplie avec des données :

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

L’extrait suivant montre un `ListView` lié à une liste d’employés :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Notez que la liaison a été configurée dans le code par souci de simplicité, bien qu’il peut avoir été liée dans XAML.

Le bit précédent du code XAML définit un `ContentPage` qui contient un `ListView`. La source de données de la `ListView` est définie la `ItemsSource` attribut. La disposition de chaque ligne dans le `ItemsSource`est défini dans le `ListView.ItemTemplate` élément.

Voici le résultat obtenu :

![](data-and-databinding-images/bound-data.png "ListView à l’aide de la liaison de données")

### <a name="binding-selecteditem"></a>Liaison SelectedItem

Fréquence à laquelle que vous souhaitez lier à l’élément sélectionné d’un `ListView`, plutôt que d’utiliser un gestionnaire d’événements pour répondre aux modifications. Pour ce faire, en XAML, vous devez lier le `SelectedItem` propriété :

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

En supposant que `listView`de `ItemsSource` est une liste de chaînes, `SomeLabel` aura sa propriété text liée à la `SelectedItem`.



## <a name="related-links"></a>Liens associés

- [Liaison bidirectionnelle (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [notes de version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [notes de version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
