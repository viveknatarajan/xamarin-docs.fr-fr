---
title: Résumé du chapitre 16. Liaison de données
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 16. Liaison de données'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 520da1518c7b795bd1ad17cc3cfaa8d37815de53
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241508"
---
# <a name="summary-of-chapter-16-data-binding"></a>Résumé du chapitre 16. Liaison de données

Les programmeurs souvent se retrouvent l’écriture de gestionnaires d’événements qui détectent lorsque la modification d’une propriété d’un objet et l’utilisent pour modifier la valeur d’une propriété dans un autre objet. Ce processus peut être automatisé avec la technique de *liaison de données*. Liaisons de données sont généralement définis dans XAML et font partie de la définition de l’interface utilisateur.

Très souvent, ces liaisons de données connecter des objets d’interface utilisateur aux données sous-jacentes. Il s’agit d’une technique qui est abordée plus en [ **chapitre 18. MVVM**](chapter18.md). Toutefois, les liaisons de données peuvent également connecter deux ou plusieurs éléments d’interface utilisateur. La plupart des exemples de liaison de données dans ce chapitre anticipées illustrent cette technique.

## <a name="binding-basics"></a>Principes de base de liaison

Plusieurs propriétés, méthodes et classes sont impliqués dans la liaison de données :

- Le [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) dérive de la classe [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) et encapsule plusieurs caractéristiques d’une liaison de données
- Le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) propriété est définie par le [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) classe
- Le [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) méthode est également définie par le [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) classe
- Le [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) classe définit trois supplémentaires `SetBinding` méthodes

Les deux classes suivantes prennent en charge les extensions de balisage XAML pour les liaisons :

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) prend en charge le `Binding` extension de balisage
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) prend en charge le `x:Reference` extension de balisage

Deux interfaces sont impliqués dans la liaison de données :

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) dans la `System.ComponentModel` espace de noms est pour l’implémentation de notification lorsqu’une propriété est modifiée
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) permet de définir les petites classes qui convertissent les valeurs d’un type à l’autre dans les liaisons de données

Une liaison de données connecte deux propriétés de l’objet de même, ou deux objets différents (le plus souvent). Ces deux propriétés portent le *source* et *cible*. En règle générale, une modification de la propriété source provoque une modification se produise dans la propriété cible, mais parfois, la direction est inversée. Quel que soit :

- le *cible* propriété doit être soutenue par un [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- le *source* propriété est généralement un membre d’une classe qui implémente [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Une classe qui implémente `INotifyPropertyChanged` se déclenche un [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) événement lorsqu’une propriété change de valeur. `BindableObject` implémente `INotifyPropertyChanged` et déclenche automatiquement une `PropertyChanged` événement lorsqu’une propriété est soutenu par un `BindableProperty` modifie les valeurs, mais vous pouvez écrire vos propres classes qui implémentent `INotifyPropertyChanged` sans dériver `BindableObject`.

## <a name="code-and-xaml"></a>Code et XAML

Le [ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) exemple montre comment définir une liaison de données dans le code :

- La source est le `Value` propriété d’un `Slider`
- La cible est le `Opacity` propriété d’un `Label`

Les deux objets sont connectés en définissant le `BindingContext` de la `Label` de l’objet à le `Slider` objet. Les deux propriétés sont connectées en appelant un [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) méthode d’extension sur le `Label` faisant référence à la `OpacityProperty` propriété pouvant être liée et le `Value` propriété de la `Slider` exprimée sous la forme un chaîne.

Manipuler le `Slider` puis provoque le `Label` à apparaître et disparaître.

Le [ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) est le même programme avec la liaison de données définie en XAML. Le `BindingContext` de la `Label` a la valeur un `x:Reference` extension de balisage faisant référence à la `Slider`et le `Opacity` propriété de la `Label` a la valeur la `Binding` extension de balisage avec son [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) propriété faisant référence à la `Value` propriété de la `Slider`.

## <a name="source-and-bindingcontext"></a>Source et BindingContext

Le [ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) exemple illustre une approche alternative de code. A `Binding` objet est créé en définissant le [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) propriété le `Slider` objet et la [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) la propriété « Valeur ». Le [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) méthode `BindableObject` est ensuite appelée sur le `Label` objet.

Le [ `Binding` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) pourrait également avoir été utilisé pour définir le `Binding` objet.

Le [ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) montre la technique comparable en XAML. Le `Opacity` propriété de la `Label` a la valeur un `Binding` extension de balisage avec [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) définie sur le `Value` propriété et [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) définie sur un Embedded `x:Reference` extension de balisage.

En résumé, il existe deux façons de faire référence à l’objet de source de liaison :

- Via le `BindingContext` propriété de la cible
- Via le `Source` propriété de la `Binding` objet lui-même

Si les deux sont spécifiées, la seconde est prioritaire. L’avantage de le `BindingContext` est qu’il est propagé par l’arborescence d’éléments visuels. Il s’agit de *très* pratique si plusieurs propriétés de la cible sont liées au même objet source.

Le [ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) programme illustre cette technique avec le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) élément. Deux `Button` héritent des éléments de navigation et en amont un `BindingContext` de leur parent qui fait référence à la `WebView`. Le `IsEnabled` propriétés des deux boutons ont puis simple `Binding` les extensions de balisage qui ciblent le bouton `IsEnabled` propriétés basées sur les paramètres de la [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) et [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) propriétés en lecture seule de la `WebView`.

## <a name="the-binding-mode"></a>Le mode de liaison

Définir le [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) propriété du `Binding` à un membre de la [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) énumération :

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) afin que les modifications apportées à la propriété source affectent la cible
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) afin que les modifications apportées à la propriété cible affectent la source
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) afin que les modifications dans la source et cible affecte les uns des autres
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) Pour utiliser le [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) spécifié quand la cible `BindableProperty` a été créé. Si aucun n’a été spécifié, la valeur par défaut est `OneWay` pour les propriétés pouvant être liées normales, et `OneWayToSource` pour les propriétés pouvant être liées en lecture seule.

Les propriétés qui sont susceptibles d’être les cibles des liaisons de données dans les scénarios MVVM généralement avoir un `DefaultBindingMode` de `TwoWay`. Ces équivalents sont :

- `Value` propriété de `Slider` et `Stepper`
- `IsToggled` propriété de `Switch`
- `Text` propriété de `Entry`, `Editor`, et `SearchBar`
- `Date` propriété de `DatePicker`
- `Time` propriété de `TimePicker`

Le [ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) exemple illustre les modes de quatre liaison avec une liaison de données où la cible est le `FontSize` propriété d’un `Label` , la source est le `Value` propriété d’un `Slider`. Cela permet à chaque `Slider` pour contrôler la taille de police correspondantes `Label`. Mais le `Slider` éléments ne sont pas initialisés, car le `DefaultBindingMode` de la `FontSize` propriété est `OneWay`.

Le [ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) exemple définit les liaisons sur le `Value` propriété de la `Slider` faisant référence à la `FontSize` propriété de chaque `Label`. Cela semble être descendante, mais elle fonctionne mieux dans l’initialisation du `Slider` éléments, car le `Value` propriété de la `Slider` a un `DefaultBindingMode` de `TwoWay`.

[![Capture d’écran de triple d’inverser la liaison](images/ch16fg06-small.png "inverser la liaison")](images/ch16fg06-large.png#lightbox "inverser la liaison")

Cela est analogue à la façon dont les liaisons sont définies dans MVVM, et vous allez utiliser ce type de liaison fréquemment.

## <a name="string-formatting"></a>Chaîne mise en forme

Lorsque la propriété cible est de type `string`, vous pouvez utiliser la [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) propriété définie par `BindingBase` pour convertir la source à un `string`. Définir le `StringFormat` propriété .NET mise en forme de chaîne que vous utiliseriez avec la méthode statique [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) format d’affichage de l’objet. Lorsque vous utilisez cette chaîne de mise en forme au sein d’une extension de balisage, placez-le entre guillemets simples afin des accolades ne sera pas être confondue avec une extension de balisage incorporé.

Le [ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) exemple montre comment utiliser `StringFormat` en XAML.

Le [ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) exemple illustre l’affichage de la taille de la page avec des liaisons à le `Width` et `Height` propriétés de la `ContentPage`.

## <a name="why-is-it-called-path"></a>Raison pour laquelle elle est appelée « Path » ?

Le [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) propriété du `Binding` est appelé ainsi car il peut être une série de propriétés et indexeurs, séparés par des points. Le [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) montre plusieurs exemples.

## <a name="binding-value-converters"></a>Convertisseurs de valeurs de liaison

Lorsque les propriétés source et cible d’une liaison de types différents, vous pouvez convertir entre les types à l’aide d’un convertisseur de liaison. Il s’agit d’une classe qui implémente le [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) interface et contient deux méthodes : [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) pour convertir la source à la cible, et [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) convertir la cible à la source.

Le [ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque est un exemple de conversion d’un `int` à un `bool`. Est illustrée par le [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) exemple, ce qui permet uniquement le `Button` si au moins un caractère a été tapé dans un `Entry`.

Le [ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) classe convertit un `bool` à un `string` et définit deux propriétés pour spécifier le texte doit être retourné pour `false` et `true` valeurs.
Le [ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) est similaire. Le [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) exemple illustre l’utilisation de ces deux convertisseurs d’afficher du texte différents dans différentes couleurs selon un `Switch` paramètre.

Générique [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) peut remplacer le `BoolToStringConverter` et `BoolToColorConverter` et agissent comme un généralisée `bool`-à-convertisseur d’objet de tout type.

## <a name="bindings-and-custom-views"></a>Liaisons et des vues personnalisées

Vous pouvez simplifier les contrôles personnalisés à l’aide de liaisons de données. Le [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) fichier de code définit `Text`, `TextColor`, `FontSize`, `FontAttributes`, et `IsChecked` propriétés, mais n’a aucune logique pour les éléments visuels du contrôle.
À la place la [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) fichier contient toutes les marques pour les visuels du contrôle par le biais des liaisons de données sur le `Label` les éléments basés sur les propriétés définies dans le fichier code-behind.

Le [ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) exemple illustre la `NewCheckBox` contrôle personnalisé.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 16 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Exemples de chapitre 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
