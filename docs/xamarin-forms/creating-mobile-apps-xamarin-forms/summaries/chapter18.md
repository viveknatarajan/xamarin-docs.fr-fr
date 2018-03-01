---
title: "Résumé du chapitre 18. MVVM"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: fa62ff952a4a8916a0c9603157d14948119d243d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>Résumé du chapitre 18. MVVM

Un des meilleurs moyens d’architecte d’une application est en séparant l’interface utilisateur à partir du code sous-jacent, qui est parfois appelé le *logique métier*. Il existe plusieurs techniques, mais celui qui est adapté aux environnements XAML est appelée Model-View-ViewModel MVVM.

## <a name="mvvm-interrelationships"></a>Relations MVVM

Une application MVVM a trois couches :

- Le modèle fournit les données sous-jacentes, parfois dans les fichiers ou web accède à
- La vue est la couche d’interface ou une présentation utilisateur, généralement implémentée dans le code XAML
- Ce dernier connecte le modèle et la vue

Le modèle connaît le ViewModel et ViewModel connaît l’existence de la vue. Généralement, ces trois couches se connectent à l’aide des mécanismes suivants :

![Vues, ViewModel et](images/ch18fg03.png "MVVM")

Dans de nombreux programmes plus petits (et ceux qui sont encore plus), fréquence à laquelle le modèle est absent ou ses fonctionnalités sont intégrée dans le modèle de vues.

## <a name="viewmodels-and-data-binding"></a>ViewModel et liaison de données

Pour prendre part aux liaisons de données, un ViewModel doit être capable de notification de la vue lorsqu’une propriété de ce dernier a changé. ViewModel met en œuvre la [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) interface dans le `System.ComponentModel` espace de noms. Cela fait partie de .NET plutôt que de Xamarin.Forms. (Généralement ViewModel tenter de maintenir l’indépendance des plates-formes.)

Le `INotifyPropertyChanged` interface déclare un événement nommé [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) qui indique la propriété qui a changé.

### <a name="a-viewmodel-clock"></a>Une horloge ViewModel

Le [ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) dans les [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) bibliothèque définit une propriété de type `DateTime` que les modifications basées sur un minuteur. La classe implémente `INotifyPropertyChanged` et déclenche une alerte le `PropertyChanged` événement chaque fois que le `DateTime` de propriété est modifiée.

Le [ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) exemple instancie ce ViewModel et utilise des liaisons de données pour le modèle de vues pour afficher les mises à jour informations de date et heure.

### <a name="interactive-properties-in-a-viewmodel"></a>Propriétés interactives dans un ViewModel

Propriétés dans un modèle de vues peuvent être plus interactives, comme illustré dans le [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) (classe), qui fait partie de la [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) exemple. Les liaisons de données fournissent des valeurs multiplicande et multiplicateur de deux `Slider` éléments et afficher le produit associé à un `Label`. Toutefois, vous pouvez modifier considérablement cette interface utilisateur en XAML sans changement consécutive ViewModel ou le fichier code-behind.

### <a name="a-color-viewmodel"></a>Un ViewModel de couleur

Le [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) dans les [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) bibliothèque intègre les modèles de couleurs RVB et TSL. Il est présenté dans le [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) exemple :

[![Capture d’écran de triple des TK](images/ch18fg08-small.png "modèle de couleurs TSL")](images/ch18fg08-large.png "modèle de couleurs TSL")

### <a name="streamlining-the-viewmodel"></a>Ce qui simplifie le ViewModel

Le code dans ViewModel peut être simplifiée en définissant un `OnPropertyChanged` à l’aide de la méthode la [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) attribut, qui obtient le nom de propriété appelant automatiquement. Le [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) bibliothèque cela et fournit une classe de base ViewModel.

## <a name="the-command-interface"></a>L’interface de commande

MVVM fonctionne avec les liaisons de données, et des liaisons de données fonctionnent avec des propriétés, donc MVVM semble en lorsqu’il s’agit de la gestion d’un `Clicked` l’événement d’un `Button` ou un `Tapped` événement d’un `TapGestureRecognizer`. Pour autoriser les ViewModel gérer ces événements, Xamarin.Forms prend en charge la *interface de commande*.

L’interface de commande se manifeste le `Button` avec deux propriétés publiques :

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) de type [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (défini dans le `System.Windows.Input` espace de noms)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) de type `Object`

Pour prendre en charge l’interface de commande, un ViewModel doit définir une propriété de type `ICommand` les données qui est ensuites liées à la `Command` propriété de la `Button`. Le `ICommand` interface déclare deux méthodes et un événement :

- Un [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) méthode avec un argument de type `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) méthode avec un argument de type `object` qui retourne `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) événement

En interne, un ViewModel définit chaque propriété de type `ICommand` à une instance d’une classe qui implémente le `ICommand` interface. Via la liaison de données, le `Button` appelle initialement la `CanExecute` (méthode) et se désactive si la méthode retourne `false`. Il définit également un gestionnaire pour le `CanExecuteChanged` événements et les appels `CanExecute` chaque fois que cet événement est déclenché. Si le `Button` est activé, il appelle la `Execute` (méthode) chaque fois que le `Button` est activé.

Vous pouvez avoir certains ViewModel antérieures à Xamarin.Forms, et ces peuvent prennent déjà en charge l’interface de commande. Pour les nouveaux ViewModel destinée à être utilisée uniquement avec Xamarin.Forms, Xamarin.Forms fournit un [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) classe et un [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) classe qui implémentent la `ICommand` interface. Le type générique est le type de l’argument de la `Execute` et `CanExecute` méthodes.

### <a name="simple-method-executions"></a>Exécutions de méthode simple

Le [ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) exemple montre comment utiliser l’interface de commande dans un modèle de vues. Le [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) classe définit deux propriétés de type `ICommand` et définit également deux propriétés privées qu’il transmet à la plus simple [ `Command` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Le programme contienne des liaisons de données à partir de ce modèle de vues pour la `Command` propriétés de deux `Button` éléments.

Le `Button` les éléments peuvent être facilement remplacés avec `TapGestureRecognizer` objets en XAML sans aucune modification de code.

### <a name="a-calculator-almost"></a>Une calculatrice, presque

Le [ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) exemple rend utilisent tous deux le `Execute` et `CanExecute` méthodes de `ICommand`. Il utilise un [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) bibliothèque. ViewModel contient six propriétés de type `ICommand`. Celles-ci sont initialisées à partir de la [ `Command` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) et [ `Command` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) de `Command` et [ `Command<T>` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) de `Command<T>`. Les touches numériques de l’ajout de la machine sont liés à la propriété qui est initialisée avec `Command<T>`et un `string` argument `Execute` et `CanExecute` identifie la clé spécifique.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModel et le cycle de vie des applications

Le `AdderViewModel` utilisé dans le **AddingMachine** exemple définit également deux méthodes nommées `SaveState` et `RestoreState`. Ces méthodes sont appelées à partir de l’application lorsqu’elle se met en veille et lorsqu’il démarre à nouveau.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 18 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Exemples de chapitre 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
