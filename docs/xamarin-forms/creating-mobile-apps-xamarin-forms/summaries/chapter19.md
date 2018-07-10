---
title: Résumé du chapitre 19. Vues de collection
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 19. Vues de collection'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7ae6ff5bb08977ab83f95242770794b4c4363145
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935171"
---
# <a name="summary-of-chapter-19-collection-views"></a>Résumé du chapitre 19. Vues de collection

Xamarin.Forms définit trois vues qui maintiennent des collections et affichent leurs éléments :

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) est une liste relativement courte des éléments de type chaîne qui permet à l’utilisateur à choisir une
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est souvent une longue liste d’éléments généralement du même type et de mise en forme, également autoriser l’utilisateur à choisir une
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) est une collection de *cellules* (généralement de différents types et des apparences) pour afficher des données ou gérer l’entrée utilisateur

Il est courant pour les applications MVVM le `ListView` pour afficher la collection d’objets sélectionnable.

## <a name="program-options-with-picker"></a>Options du programme avec le sélecteur

Le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) est un bon choix lorsque vous avez besoin autoriser l’utilisateur à choisir une option parmi une liste relativement courte des `string` éléments.

### <a name="the-picker-and-event-handling"></a>Le sélecteur et la gestion des événements

Le [ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) exemple montre comment utiliser XAML pour définir le `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) propriété et ajoutez `string` éléments à la [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) collection. Lorsque l’utilisateur sélectionne le `Picker`, elle affiche les éléments dans le `Items` collection en fonction de la plateforme.

Le [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) événements indique quand l’utilisateur a sélectionné un élément. Base zéro [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) propriété indique ensuite l’élément sélectionné. Si aucun élément n’est sélectionné, `SelectedIndex` est égal à &ndash;1.

Vous pouvez également utiliser `SelectedIndex` pour initialiser l’élément sélectionné, mais il doit être définie après le `Items` collection est remplie. Dans XAML, cela signifie que vous allez probablement utiliser un élément de propriété pour définir `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Le sélecteur de liaison de données

Le `SelectedIndex` propriété est stockée par une propriété pouvant être liée mais `Items` n’est pas, par conséquent, à l’aide de la liaison de données avec un `Picker` est difficile. Une solution consiste à utiliser le `Picker` en association avec un [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) comme celui de la [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Le [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) montre comment cela fonctionne.

## <a name="rendering-data-with-listview"></a>Rendu des données avec ListView

Le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est la seule classe qui dérive de [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) dont elle hérite la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) et [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) propriétés.

`ItemsSource` est de type `IEnumerable` , mais il est `null` par défaut et doit être explicitement initialisés ou (généralement) une collection via une liaison de données. Les éléments de cette collection peuvent être de n’importe quel type.

`ListView` définit un [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) propriété qui est soit définie sur un des éléments dans le `ItemsSource` collection ou `null` si aucun élément n’est sélectionné. `ListView` se déclenche le [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) événement lorsqu’un nouvel élément est sélectionné.

### <a name="collections-and-selections"></a>Collections et les sélections

Le [ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) exemple remplit un `ListView` avec 17 `Color` des valeurs dans un `List<Color>` collection. Les éléments sont sélectionnables, mais par défaut, ils sont affichés avec leurs résultats peu esthétiques `ToString` représentations. Plusieurs exemples dans ce chapitre vous montrent comment corriger qui affichent et rendre aussi attractif comme vous le souhaitez.

### <a name="the-row-separator"></a>Le séparateur de ligne

Sur iOS et Android affiche, une fine ligne sépare les lignes. Vous pouvez contrôler ceci avec la [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) et [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) propriétés. `SeparatorVisibility` propriété est de type [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), une énumération avec deux membres :

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), le paramètre par défaut
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>L’élément sélectionné de liaison de données

Le `SelectedItem` propriété repose sur une propriété pouvant être liée, il peut être la source ou la cible d’une liaison de données. Sa valeur par défaut `BindingMode` est `OneWayToSource`, mais il est généralement la cible d’une liaison de données bidirectionnelle, en particulier dans les scénarios MVVM. Le [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) exemple illustre ce type de liaison.

### <a name="the-observablecollection-difference"></a>La différence de ObservableCollection

Le [ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) exemples de jeux de la `ItemsSource` propriété d’un `ListView` à un `List<DateTime>` collection puis progressivement ajoute une nouvelle `DateTime` objet à la collection chaque seconde à l’aide d’un minuteur.

Toutefois, le `ListView` n’automatiquement mettre à jour lui-même, car le `List<T>` collection n’a pas un mécanisme de notification pour indiquer quand les éléments sont ajoutés ou supprimés de la collection.

Une classe beaucoup mieux à utiliser dans de tels scénarios est [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) définies dans le `System.Collections.ObjectModel` espace de noms. Cette classe implémente le [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) interface et par conséquent se déclenche un [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) événement lorsque des éléments sont ajoutés ou supprimés de la collection, ou lorsqu’ils sont remplacés ou déplacés dans la collection. Lorsque le `ListView` en interne détecte qu’une classe qui implémente `INotifyCollectionChanged` a été défini sur sa `ItemsSource` propriété, elle joint un gestionnaire à la `CollectionChanged` événement et met à jour son affichage lorsque la collection est modifiée.

Le [ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) exemple illustre l’utilisation de `ObservableCollection`.

### <a name="templates-and-cells"></a>Modèles et des cellules

Par défaut, un `ListView` affiche les éléments dans sa collection à l’aide de chaque élément `ToString` (méthode). Une meilleure approche consiste à définir un modèle pour afficher les éléments.

Pour essayer cette fonctionnalité, vous pouvez utiliser la [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Cette classe définit un mappage statique `All` propriété de type `IList<NamedColor>` contenant 141 `NamedColor` objets correspondant aux champs publics de la `Color` structure.

Le [ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) exemples de jeux le `ItemsSource` d’un `ListView` à ce `NamedColor.All` propriété, mais uniquement les noms de classe qualifié complet de le `NamedColor` sont des objets affiché.

`ListView` a besoin d’un modèle pour afficher ces éléments. Dans le code, vous pouvez définir le [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) propriété définie par `ItemsView<TVisual>` à un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à l’aide de l’objet le [ `DataTemplate` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) qui fait référence à un dérivé de la [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) classe. `Cell` a cinq dérivés :

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; contient deux `Label` vues (point de vue conceptuel)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; Ajoute un `Image` afficher à `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; contient un `Entry` afficher avec un `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; contient un `Switch` avec un `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; peut être toute `View` (probablement avec enfants)

Puis appelez [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) et [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) sur le `DataTemplate` objet à associer les valeurs avec le `Cell` propriétés, ou pour définir des liaisons de données sur le `Cell` propriétés faisant référence à des propriétés des éléments dans le `ItemsSource` collection. Cela est illustré dans le [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) exemple.

Comme chaque élément est affiché par le `ListView`, une petite arborescence visuelle est construite à partir du modèle et des liaisons de données sont établies entre l’élément et les propriétés des éléments dans cette arborescence d’éléments visuels. Vous pouvez obtenir une idée de ce processus en installant des gestionnaires pour les [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) et [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) événements de la `ListView`, ou à l’aide d’une alternative [ `DataTemplate`constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) qui utilise une fonction qui est appelée chaque fois qu’arborescence d’éléments visuels d’un élément doit être créé.

Le [ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) montre un programme fonctionnellement identique entièrement dans XAML. Un `DataTemplate` balise est définie sur le `ItemTemplate` propriété de la `ListView`, puis le `TextCell` est défini sur le `DataTemplate`. Liaisons aux propriétés des éléments dans la collection sont définies directement sur le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) et [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) propriétés de la `TextCell`.

### <a name="custom-cells"></a>Cellules personnalisées

Dans XAML, il est possible de définir un [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) à la `DataTemplate` , puis définissez une arborescence d’éléments visuels personnalisé en tant que le [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) propriété du `ViewCell`. (`View` est la propriété de contenu de `ViewCell` la `ViewCell.View` balises ne sont pas nécessaires.) Le [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) exemple illustre cette technique :

[![Capture d’écran triple de la liste de couleurs personnalisée nommée](images/ch19fg11-small.png "nommé couleur d’une liste personnalisée")](images/ch19fg11-large.png#lightbox "nommé couleur d’une liste personnalisée")

Obtenir la taille adaptées à toutes les plateformes peut être difficile. Le [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) propriété est utile, mais dans certains cas, vous voudrez avoir recours à la [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) propriété, qui est moins efficace, mais force le `ListView` pour dimensionner les lignes. Pour iOS et Android, vous devez utiliser une de ces deux propriétés pour obtenir le dimensionnement de la ligne appropriée.

### <a name="grouping-the-listview-items"></a>Grouper les éléments de ListView

`ListView` prend en charge le regroupement d’éléments et de naviguer parmi ces groupes. Le `ItemsSource` propriété doit être définie sur une collection de collections : l’objet qui `ItemsSource` a la valeur doit implémenter `IEnumerable`, et chaque élément dans la collection doit également implémenter `IEnumerable`. Chaque groupe doit inclure deux propriétés : une description du groupe et une abréviation à trois lettres.

Le [ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque crée sept groupes de `NamedColor` objets. Le [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) exemple montre comment utiliser ces groupes avec le [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) propriété du `ListView` la valeur `true`et le [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) et [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) propriétés liées aux propriétés dans chaque groupe.

### <a name="custom-group-headers"></a>En-têtes de groupe personnalisé

Il est possible de créer des en-têtes personnalisés pour le `ListView` groupes en remplaçant le `GroupDisplayBinding` propriété avec le [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) définition d’un modèle pour les en-têtes.

### <a name="listview-and-interactivity"></a>ListView et l’interactivité

Généralement d’obtenir l’interaction utilisateur avec un `ListView` en attachant un gestionnaire à la `ItemSelected` ou [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) événement, ou en définissant une liaison de données sur le `SelectedItem` propriété. Mais certains types de cellule (`EntryCell` et `SwitchCell`) autoriser l’interaction utilisateur, et il est également possible de créer des cellules personnalisées qui elles-mêmes interagir avec l’utilisateur. Le [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) crée 100 instances de [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) et permet à l’utilisateur modifier chaque couleur à l’aide d’un trio de `Slider` éléments. Le programme utilise également le [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) dans le [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView et MVVM

`ListView` joue un rôle important dans les scénarios MVVM. Chaque fois qu’un `IEnumerable` collection existe dans un ViewModel, il est souvent lié à un `ListView`. En outre, les éléments dans la collection souvent implémentent `INotifyPropertyChanged` pour lier des propriétés dans un modèle.

### <a name="a-collection-of-viewmodels"></a>Une collection de ViewModel

Pour cela, Explorer le [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) bibliothèque crée plusieurs classes basées sur un [fichier de données XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) et des images d’étudiants fictives de l’établissement fictif.

Le [ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) dérive de la classe [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Le [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) classe est une collection de `Student` objets et dérive également de `ViewModelBase`. Le [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) télécharge le fichier XML et les assemble tous les objets.

Le [ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) programme utilise une `ImageCell` pour afficher les étudiants et leurs images dans un `ListView`:

[![Capture d’écran triple de la liste des étudiants](images/ch19fg18-small.png "liste des étudiants")](images/ch19fg18-large.png#lightbox "liste des étudiants")

Le [ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) exemple ajoute un [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) propriété mais il ne s’affiche sur Android.

### <a name="selection-and-the-binding-context"></a>Sélection et le contexte de liaison

Le [ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) programme lie le `BindingContext` d’un `StackLayout` à la `SelectedItem` propriété de la `ListView`. Cela permet au programme afficher des informations détaillées sur l’étudiant sélectionné.

### <a name="context-menus"></a>Menus contextuels

Une cellule peut définir un menu contextuel qui est implémenté de manière spécifique à la plateforme. Pour créer ce menu, ajoutez [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) des objets sur le [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) propriété de la `Cell`.

`MenuItem` définit les cinq propriétés :

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) de type `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) de type `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) de type `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) de type `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) de type `object`

Le `Command` et `CommandParameter` propriétés impliquent que le ViewModel pour chaque élément contient des méthodes pour exécuter les commandes de menu souhaitée. Dans les scénarios non MVVM, `MenuItem` définit également un [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) événement.

Le [ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) illustre cette technique. Le `Command` propriété de chaque `MenuItem` est liée à une propriété de type `ICommand` dans la `Student` classe. Définir le `IsDestructive` propriété `true` pour un `MenuItem` qui supprime ou l’objet sélectionné.

### <a name="varying-the-visuals"></a>Faire varier les éléments visuels

Parfois, vous voudrez légères variantes dans les visuels des éléments dans le `ListView` basée sur une propriété. Par exemple, lorsqu’un point de qualité moyenne se situe antérieures à 2.0, la [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) exemple affiche le nom de cette étudiant en rouge.
Cela s’effectue via l’utilisation d’un convertisseur de valeurs de liaison, [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs), dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

### <a name="refreshing-the-content"></a>L’actualisation du contenu

Le `ListView` prend en charge un mouvement déroulant pour actualiser ses données. Le programme doit définir le [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) propriété `true` pour activer cette option. Le `ListView` répond à la liste déroulante en définissant son [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) propriété `true`et en déclenchant le [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) événement et (pour les scénarios MVVM) appelant le `Execute` méthode de son [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) propriété.

Code de gestion de la `Refresh` événement ou la `RefreshCommand` éventuellement met à jour les données affichées par le `ListView` et définit `IsRefreshing` à `false`.

Le [ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) exemple montre comment utiliser un [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) qui implémente `RefreshCommand` et `IsRefreshing` propriétés pour la liaison de données.

## <a name="the-tableview-and-its-intents"></a>Le TableView et ses intentions

Bien que le `ListView` affiche généralement plusieurs instances du même type, le [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) se concentre généralement sur la fourniture d’une interface utilisateur pour plusieurs propriétés de différents types. Chaque élément est associé à sa propre [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) dérivés pour afficher la propriété ou en fournissant une interface utilisateur à ce dernier.

### <a name="properties-and-hierarchies"></a>Propriétés et des hiérarchies

`TableView` définit que quatre propriétés :

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) de type [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), énumération
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) de type [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), la propriété de contenu de `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) de type `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) de type `bool`

Le `TableIntent` énumération indique la façon dont vous prévoyez d’utiliser le `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Ces membres suggèrent également certaines utilisations pour le `TableView`.

Plusieurs autres classes sont impliqués dans la définition d’une table :

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) est une classe abstraite qui dérive de `BindableObject` et définit un [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) propriété

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) est une classe abstraite qui dérive de `TableSectionBase` et implémente `IList<T>` et `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) dérive de `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) dérive de `TableSectionBase<TableSection>`

En bref, `TableView` a un `Root` propriété que vous définissez pour un `TableRoot` objet, qui est une collection de `TableSection` objets, chacun d’eux est une collection de `Cell` objets. Une table comporte plusieurs sections, et chaque section comporte plusieurs cellules. La table elle-même peut avoir un titre, et chaque section peut contenir un titre. Bien que `TableView` utilise `Cell` dérivés, il ne fait pas utiliser de `DataTemplate`.

### <a name="a-prosaic-form"></a>Un formulaire prosaïque

Le [ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) exemple définit un [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) modèle de vue, une instance de qui devient le `BindingContext` de la `TableView`. Chaque `Cell` dérivés dans son `TableSection` peut ensuite avoir des liaisons aux propriétés de la `PersonalInformation` classe.

### <a name="custom-cells"></a>Cellules personnalisées

Le [ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) exemple s’appuie sur **EntryForm**. Le [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) classe inclut une propriété booléenne qui détermine l’applicabilité des deux propriétés supplémentaires. Pour ces deux propriétés supplémentaires, le programme utilise un personnalisé `PickerCell` selon un [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) et [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) dans le [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

Bien que le `IsEnabled` propriétés des deux `PickerCell` éléments sont liés à la propriété booléenne dans `ProgrammerInformation`, cette technique ne semble pas fonctionner, ce qui demande à l’exemple suivant.

### <a name="conditional-sections"></a>Sections conditionnelles

Le [ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) exemple place les deux éléments sont conditionnels sur la sélection de l’élément booléenne dans une fonction `TableSection`. Le fichier code-behind supprime cette section à partir de la `TableView` ou ajoute il selon la propriété booléenne.

### <a name="a-tableview-menu"></a>Un menu TableView

Une autre utilisation d’un `TableView` est un menu. Le [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) exemple illustre un menu qui vous permet de déplacer un peu `BoxView` autour de l’écran.



## <a name="related-links"></a>Liens associés

- [Chapitre 19 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Exemples de chapitre 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
