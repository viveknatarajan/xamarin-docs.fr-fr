---
title: "Résumé du chapitre 19. Vues de collection"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 37afa3a54fd20745a65312fb5a24d958c8ec405f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Résumé du chapitre 19. Vues de collection

Xamarin.Forms définit trois vues qui maintiennent des collections et affichent leurs éléments :

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) est une liste d’éléments de chaîne relativement courte qui permet à l’utilisateur de choisir l’une
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est souvent une longue liste d’éléments généralement du même type et la mise en forme, également autoriser l’utilisateur à choisir une
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) est une collection de *cellules* (généralement de différents types et les apparences) pour afficher des données ou gérer l’entrée d’utilisateur

Il est courant pour les applications MVVM à utiliser le `ListView` pour afficher la collection d’objets pouvant être sélectionnée.

## <a name="program-options-with-picker"></a>Options du programme avec le sélecteur

Le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) est un bon choix lorsque vous devez autoriser l’utilisateur à choisir une option parmi une liste relativement courte des `string` éléments.

### <a name="the-picker-and-event-handling"></a>Le sélecteur et la gestion des événements

Le [ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) exemple illustre l’utilisation de XAML pour définir le `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) propriété et ajoutez `string` éléments à la [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) collection. Lorsque l’utilisateur sélectionne le `Picker`, elle affiche les éléments dans le `Items` collection en fonction de la plateforme.

Le [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) événements indique quand l’utilisateur a sélectionné un élément. Base zéro [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) propriété puis indique que l’élément sélectionné. Si aucun élément n’est sélectionné, `SelectedIndex` égal  &#x2013; 1.

Vous pouvez également utiliser `SelectedIndex` pour initialiser l’élément sélectionné, mais il doit être postérieure à la `Items` collection est remplie. En XAML, cela signifie que vous allez probablement utiliser un élément de propriété pour définir `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Le sélecteur de liaison de données

Le `SelectedIndex` propriété est soutenue par une propriété pouvant être liée mais `Items` n’est pas, à l’aide de la liaison de données avec un `Picker` est difficile. Une solution consiste à utiliser le `Picker` en association avec un [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) tel que celui dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Le [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) montre comment cela fonctionne.

## <a name="rendering-data-with-listview"></a>Rendu des données avec ListView

Le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est la seule classe qui dérive de [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) dont elle hérite la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) et [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) propriétés.

`ItemsSource` est de type `IEnumerable` , mais il est `null` par défaut et doit être explicitement initialisés ou (le plus souvent) à une collection via une liaison de données. Les éléments de cette collection peuvent être de n’importe quel type.

`ListView` définit un [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) propriété qui est défini sur l’un des éléments dans le `ItemsSource` collection ou `null` si aucun élément n’est sélectionné. `ListView` se déclenche le [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) événement lorsqu’un nouvel élément est sélectionné.

### <a name="collections-and-selections"></a>Collections et les sélections

Le [ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) exemple remplit un `ListView` avec 17 `Color` des valeurs dans un `List<Color>` collection. Les éléments peuvent être sélectionnés, mais par défaut, ils sont affichés avec leur non `ToString` représentations. Plusieurs exemples dans ce chapitre montrent comment résoudre qui s’affichent et les rendre attrayantes que comme vous le souhaitez.

### <a name="the-row-separator"></a>Le séparateur de ligne

Sur iOS et Android affiche, une fine ligne sépare les lignes. Vous pouvez contrôler ce avec le [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) et [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) propriétés. `SeparatorVisibility` propriété est de type [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), une énumération avec deux membres :

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), le paramètre par défaut
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>L’élément sélectionné de liaison de données

Le `SelectedItem` propriété est soutenue par une propriété, il peut être la source ou la cible d’une liaison de données. Sa valeur par défaut `BindingMode` est `OneWayToSource`, mais il est généralement la cible d’une liaison de données bidirectionnelle, en particulier dans les scénarios MVVM. Le [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) exemple illustre ce type de liaison.

### <a name="the-observablecollection-difference"></a>La différence ObservableCollection

Le [ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) exemples de jeux de la `ItemsSource` propriété d’un `ListView` à un `List<DateTime>` collection puis progressivement ajoute une nouvelle `DateTime` objet à la collection chaque seconde à l’aide d’un minuteur.

Toutefois, le `ListView` n’automatiquement mettre à jour automatiquement, car le `List<T>` collection ne dispose d’un mécanisme de notification pour indiquer lorsque les éléments sont ajoutés ou supprimés de la collection.

Une classe beaucoup mieux à utiliser dans de tels scénarios est [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) défini dans le `System.Collections.ObjectModel` espace de noms. Cette classe implémente la [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) interface et par conséquent se déclenche un [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) événement lorsque des éléments sont ajoutés ou supprimés de la collection, ou lorsqu’ils sont remplacés ou déplacés dans la collection. Lorsque le `ListView` en interne détecte qu’une classe qui implémente `INotifyCollectionChanged` a été défini sur sa `ItemsSource` propriété, elle joint un gestionnaire à la `CollectionChanged` événement et met à jour son affichage lorsque la collection est modifiée.

Le [ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) exemple illustre l’utilisation de `ObservableCollection`.

### <a name="templates-and-cells"></a>Modèles et des cellules

Par défaut, un `ListView` affiche les éléments dans la collection à l’aide de chaque élément `ToString` (méthode). Une meilleure approche implique la définition d’un modèle pour afficher les éléments.

Pour essayer cette fonctionnalité, vous pouvez utiliser la [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Cette classe définit un mappage statique `All` propriété de type `IList<NamedColor>` contenant 141 `NamedColor` objets correspondant aux champs publics de la `Color` structure.

Le [ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) exemples de jeux de la `ItemsSource` d’un `ListView` à ce `NamedColor.All` propriété, mais uniquement les noms de classe qualifié complet de le `NamedColor` sont des objets affiché.

`ListView` a besoin d’un modèle pour afficher ces éléments. Dans le code, vous pouvez définir le [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) propriété définie par `ItemsView<TVisual>` à un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) à l’aide de l’objet le [ `DataTemplate` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) qui fait référence à un dérivé de la [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) classe. `Cell` possède cinq dérivés :

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &#x2014; contient deux `Label` vues (point de vue conceptuel)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &#x2014; Ajoute un `Image` afficher à `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &#x2014; contient une `Entry` afficher avec un `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &#x2014; contient un `Switch` avec un `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &#x2014; Il peut s’agir `View` (probablement avec enfants)

Puis appelez [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) et [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) sur la `DataTemplate` objet à associer les valeurs avec le `Cell` propriétés, ou pour définir des liaisons de données sur le `Cell` propriétés de référencer les propriétés des éléments dans le `ItemsSource` collection. Cela est illustré dans le [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) exemple.

Chaque élément est affichée par le `ListView`, une petite arborescence d’éléments visuels est construit à partir du modèle et des liaisons de données sont établies entre l’élément et les propriétés des éléments dans cette arborescence d’éléments visuels. Vous pouvez obtenir une idée de ce processus en installant des gestionnaires pour les [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) et [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) les événements de la `ListView`, ou à l’aide d’une alternative [ `DataTemplate`constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) qui utilise une fonction qui est appelée chaque fois qu’arborescence d’éléments visuels d’un élément doit être créé.

Le [ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) montre un programme fonctionnellement identique entièrement en XAML. A `DataTemplate` balise est définie sur le `ItemTemplate` propriété de la `ListView`, puis le `TextCell` est défini sur le `DataTemplate`. Les liaisons pour les propriétés des éléments dans la collection sont définies directement sur le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) et [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) propriétés de la `TextCell`.

### <a name="custom-cells"></a>Cellules personnalisées

En XAML, il est possible de définir un [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) à la `DataTemplate` , puis définissez une arborescence d’éléments visuels personnalisé en tant que le [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) propriété du `ViewCell`. (`View` est la propriété de contenu de `ViewCell` la `ViewCell.View` balises ne sont pas obligatoires.) Le [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) exemple illustre cette technique :

[![Capture d’écran de triple de la liste de couleurs personnalisée nommée](images/ch19fg11-small.png "nommé couleur d’une liste personnalisée")](images/ch19fg11-large.png "nommé couleur d’une liste personnalisée")

Obtention de la taille adaptée à toutes les plateformes peut être compliquée. Le [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) propriété est utile, mais dans certains cas que vous souhaitez recourir à la [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) propriété, qui est moins efficace mais force le `ListView` pour dimensionner les lignes. Pour iOS et Android, vous devez utiliser un de ces deux propriétés pour obtenir de dimensionner les lignes appropriées.

### <a name="grouping-the-listview-items"></a>Grouper les éléments ListView

`ListView` prend en charge le regroupement d’éléments et de naviguer parmi ces groupes. Le `ItemsSource` propriété doit être définie à une collection de collections : l’objet qui `ItemsSource` a la valeur doit implémenter `IEnumerable`, et chaque élément dans la collection doit également implémenter `IEnumerable`. Chaque groupe doit inclure deux propriétés : une description du groupe et une abréviation à trois lettres.

Le [ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque crée sept groupes de `NamedColor` objets. Le [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) montre comment utiliser ces groupes avec le [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) propriété du `ListView` la valeur `true`et le [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) et [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) propriétés liées aux propriétés dans chaque groupe.

### <a name="custom-group-headers"></a>En-têtes de groupe personnalisé

Il est possible de créer des en-têtes personnalisés pour le `ListView` groupes en remplaçant le `GroupDisplayBinding` propriété avec le [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) définition d’un modèle pour les en-têtes.

### <a name="listview-and-interactivity"></a>ListView et l’interactivité

En général une application obtient des interactions des utilisateurs avec un `ListView` en attachant un gestionnaire pour le `ItemSelected` ou [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) événement, ou en définissant une liaison de données sur le `SelectedItem` propriété. Mais certains types de cellule (`EntryCell` et `SwitchCell`) permettent l’intervention de l’utilisateur, et il est également possible de créer des cellules personnalisées qui elles-mêmes interagir avec l’utilisateur. Le [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) crée les 100 instances de [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) et permet à l’utilisateur modifier chaque couleur à l’aide de trois `Slider` éléments. Le programme tire également parti de la [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) dans les [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView et MVVM

`ListView` joue un rôle important dans les scénarios MVVM. Chaque fois qu’un `IEnumerable` collection existe dans un ViewModel, il est souvent lié à un `ListView`. En outre, les éléments de la collection souvent implémentent `INotifyPropertyChanged` pour lier des propriétés dans un modèle.

### <a name="a-collection-of-viewmodels"></a>Une collection de ViewModel

Pour cela, Explorer le [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) bibliothèque crée plusieurs classes basées sur une [fichier de données XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) et les images d’étudiants fictives de l’établissement fictif.

Le [ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) dérive de la classe [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Le [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) classe est une collection de `Student` des objets et dérive également de `ViewModelBase`. Le [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) télécharge le fichier XML et assemble tous les objets.

Le [ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) programme utilise une `ImageCell` pour afficher les participants et leurs images dans un `ListView`:

[![Capture d’écran de triple de la liste des étudiants](images/ch19fg18-small.png "liste des étudiants")](images/ch19fg18-large.png "liste des étudiants")

Le [ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) exemple ajoute un [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) propriété mais elle seulement s’affiche sur Android.

### <a name="selection-and-the-binding-context"></a>Sélection et le contexte de liaison

Le [ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) programme lie le `BindingContext` d’un `StackLayout` à la `SelectedItem` propriété de la `ListView`. Ainsi, le programme affiche des informations détaillées sur l’étudiant sélectionné.

### <a name="context-menus"></a>Menus contextuels

Une cellule peut définir un menu contextuel qui est implémenté de manière spécifique à la plateforme. Pour créer ce menu, ajoutez [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) des objets sur le [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) propriété de la `Cell`.

`MenuItem` définit les cinq propriétés :

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) de type `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) de type `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) de type `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) de type `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) de type `object`

Le `Command` et `CommandParameter` propriétés impliquent que le ViewModel pour chaque élément contient des méthodes pour exécuter les commandes de menu souhaitée. Dans les scénarios non-MVVM, `MenuItem` définit également un [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) événement.

Le [ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) illustre cette technique. Le `Command` propriété de chaque `MenuItem` est lié à une propriété de type `ICommand` dans la `Student` classe. Définir le `IsDestructive` propriété `true` pour un `MenuItem` qui supprime ou l’objet sélectionné.

### <a name="varying-the-visuals"></a>Variable d’éléments visuels

Vous avez parfois besoin légères variantes dans les éléments visuels des éléments dans le `ListView` basée sur une propriété. Par exemple, quand un point de qualité moyenne inférieure à 2.0, la [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) exemple affiche le nom de ce student en rouge.
Cela s’effectue via l’utilisation d’un convertisseur de valeurs de liaison, [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs), dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

### <a name="refreshing-the-content"></a>L’actualisation du contenu

Le `ListView` prend en charge un mouvement de la liste déroulante pour actualiser ses données. Le programme doit définir le [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) propriété `true` pour activer cette option. Le `ListView` répond à la liste déroulante en définissant son [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) propriété `true`et en déclenchant le [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) événement et (pour les scénarios MVVM) appel le `Execute` méthode de son [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) propriété.

Gestion du code la `Refresh` événement ou la `RefreshCommand` éventuellement met à jour les données affichées par le `ListView` et définit `IsRefreshing` à `false`.

Le [ **flux RSS** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) exemple montre comment utiliser un [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) qui implémente `RefreshCommand` et `IsRefreshing` propriétés de liaison de données.

## <a name="the-tableview-and-its-intents"></a>Le TableView et ses objectifs.

Alors que le `ListView` affiche généralement de plusieurs instances du même type, le [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) se concentre généralement sur la fourniture d’une interface utilisateur pour plusieurs propriétés de différents types. Chaque élément est associé à son propre [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) dérivés pour afficher la propriété ou en fournissant une interface utilisateur.

### <a name="properties-and-hierarchies"></a>Propriétés et des hiérarchies

`TableView` définit les quatre propriétés :

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) de type [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), énumération
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) de type [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), la propriété de contenu de `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) de type `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) de type `bool`

Le `TableIntent` énumération indique la manière dont vous souhaitez utiliser le `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Ces membres suggèrent également quelques utilisations pour la `TableView`.

Plusieurs autres classes sont impliqués dans la définition d’une table :

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) est une classe abstraite qui dérive de `BindableObject` et définit un [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) propriété

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) est une classe abstraite qui dérive de `TableSectionBase` et implémente `IList<T>` et `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) dérive de `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) dérive de `TableSectionBase<TableSection>`

En bref, `TableView` a un `Root` que vous avez définie pour un `TableRoot` objet, qui est une collection de `TableSection` objets, chacun d’eux est une collection de `Cell` objets. Une table contient plusieurs sections, et chaque section comporte plusieurs cellules. La table elle-même peut avoir un titre, et chaque section peut avoir un titre. Bien que `TableView` utilise `Cell` dérivés, il ne fait pas utiliser de `DataTemplate`.

### <a name="a-prosaic-form"></a>Un formulaire prosaïque

Le [ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) exemple définit un [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) modèle d’affichage, une instance de qui devient le `BindingContext` de la `TableView`. Chaque `Cell` dérivés dans son `TableSection` peut ensuite avoir des liaisons à des propriétés de la `PersonalInformation` classe.

### <a name="custom-cells"></a>Cellules personnalisées

Le [ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) exemple développe **EntryForm**. Le [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) classe inclut une propriété booléenne qui détermine l’applicabilité des deux propriétés supplémentaires. Pour ces deux propriétés supplémentaires, le programme utilise une personnalisée `PickerCell` selon un [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) et [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) dans les [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

Bien que le `IsEnabled` propriétés des deux `PickerCell` les éléments sont liés à la propriété booléenne dans `ProgrammerInformation`, cette technique ne semble pas fonctionner, qui demande à l’exemple suivant.

### <a name="conditional-sections"></a>Sections conditionnelles

Le [ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) exemple place les deux éléments qui sont soumis à la sélection de l’élément booléenne dans une fonction `TableSection`. Le fichier code-behind supprime cette section de la `TableView` ou ajoute il selon la propriété booléenne.

### <a name="a-tableview-menu"></a>Un menu TableView

Une autre utilisation d’un `TableView` est un menu. Le [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) exemple illustre un menu qui vous permet de déplacer un peu `BoxView` autour de l’écran.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 19 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Exemples du chapitre 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
