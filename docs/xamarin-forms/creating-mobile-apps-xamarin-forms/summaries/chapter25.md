---
title: "Résumé du chapitre 25. Variétés de page"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bbe960357d9180df90a4423d6acfdf3f869d1b77
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>Résumé du chapitre 25. Variétés de page

Jusqu'à présent, vous avez déjà vu deux classes qui dérivent de `Page`: `ContentPage` et `NavigationPage`. Ce chapitre présente deux autres :

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) gère les deux pages, une forme de base et un détail
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) gère plusieurs pages enfants accédés via les onglets

Ces types de page fournissent des options de navigation plus sophistiquées de la `NavagationPage` abordés dans [chapitre 24. Page de Navigation](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Maître / détail

Le [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) définit deux propriétés de type `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) et [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). En règle générale, vous définissez chacune de ces propriétés pour un `ContentPage`. Le `MasterDetailPage` affiche et permet de basculer entre ces deux pages.

Il existe deux méthodes fondamentales pour basculer entre ces deux pages :

- *Fractionner* où le principal et les détails sont côte à côte
- *popover* où la page de détails couvre ou couvre partiellement le contrôleur de page

Il existe plusieurs variantes de la *popover* approche (*diapositive*, *se chevauchent*, et *swap*), mais il s’agit généralement de plateforme dépendants. Vous pouvez définir le [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) propriété du `MasterDetailPage` à un membre de la [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) énumération :

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Toutefois, cette propriété n’a aucun effet sur les téléphones. Téléphones ont toujours un comportement popover. Tablettes et bureau windows peuvent avoir un comportement de fractionnement.

### <a name="exploring-the-behaviors"></a>Explorer les comportements

Le [ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) exemple vous permet de faire des essais avec le comportement par défaut sur différents appareils. Le programme contienne deux `ContentPage` dérivés de la principale et détaillée (avec un `Title` propriété à la fois) et une autre classe qui dérive de `MasterDetailPage` qui combine les. La page de détails est placée dans un `NavigationPage` , car le programme UWP ne fonctionnera pas sans lui.

Les plateformes Windows 8.1 et Windows Phone 8.1 nécessitent qu’une image bitmap être définie pour le `Icon` propriété de la page maître.

### <a name="back-to-school"></a>Retour à l’école

Le [ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) exemple adopte une approche un peu différente à construire le programme affiche les étudiants à partir de la [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) bibliothèque.

Le `Master` et `Detail` propriétés sont définies avec des arborescences d’éléments visuels dans le [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) fichier, qui dérive de `MasterDetailPage`. Ce procédé permet de liaisons de données à définir entre les pages maître / détail.

Que le fichier XAML définit également la [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) propriété du `MasterDetailPage` à `True`. Cela provoque la page maître à afficher au démarrage. par défaut, la page de détails s’affiche. Le [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) fichier définit `IsPresented` à `false` lorsqu’un élément est sélectionné à partir de la `ListView` dans la page maître. La page de détails est ensuite affichée :

[![Capture d’écran de triple de détail et de l’école](images/ch25fg09-small.png "Page à partir d’un MasterDetailPage")](images/ch25fg09-large.png "Page à partir d’un MasterDetailPage")

### <a name="your-own-user-interface"></a>Votre propre interface utilisateur

Bien que Xamarin.Forms fournit une interface utilisateur de basculer entre les vues maître / détail, vous pouvez fournir vos propres. Pour cela :

- Définir le [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) propriété `false` pour désactiver le glissement
- Remplacer la [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) méthode et retournez `false` pour masquer les boutons de barre d’outils sur Windows 8.1 et Windows Phone 8.1.

Vous devez alors fournir un moyen de basculer entre les pages maître / détail, tel qu’illustré par la [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) exemple.

Le [ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) exemple montre comment utiliser une autre approche un `TapGestureRecognizer` sur les pages maître / détail.

## <a name="tabbedpage"></a>TabbedPage

Le [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) est une collection de pages que vous pouvez basculer entre l’utilisation des onglets. Elle est dérivée de `MultiPage<Page>` et ne définit aucun propriétés ou méthodes publiques qui lui sont propres. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), toutefois, la définit une propriété :

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) propriété de type `IList<T>`

Vous renseignez ceci `Children` collection avec les objets de la page.

Une autre approche vous permet de définir la `TabbedPage` enfants fonctionne comme un `ListView` à l’aide de ces deux propriétés qui génèrent les pages à onglets automatiquement :

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) de type `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) de type `DataTemplate`

Toutefois, cette approche ne fonctionne pas correctement sur les e/s lors de la collection contient plusieurs éléments.

`MultiPage<T>` définit les propriétés vous permettent d’effectuer le suivi de page qui est actuellement affichée :

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) de type `T`, qui fait référence à la page
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) de type `Object`, qui fait référence à l’objet dans le `ItemsSource` collection

`MultiPage<T>` définit également deux événements :

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) Lorsque le `ItemsSource` modifications de la collection
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) Lorsque la page affichée change

### <a name="discrete-tab-pages"></a>Pages d’onglets discrètes

Le [ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) exemple se compose de trois pages à onglets qui affichent les couleurs de trois façons différentes. Chaque onglet est un `ContentPage` dérivé, puis le `TabbedPage` dérivé [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) combine les trois pages.

Pour chaque page qui s’affiche dans un `TabbedPage`, le `Title` propriété est requise pour spécifier le texte dans l’onglet, et l’Apple Store requiert l’utilisation qu’une icône, donc la `Icon` propriété est définie pour iOS :

[![Capture d’écran de triple des couleurs d’onglets discrètes](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png "TabbedPage")

Le [ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) exemple possède une page d’accueil qui répertorie tous les étudiants. Lorsque l’utilisateur clique sur un étudiant, il accède à un `TabbedPage` dérivé, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), qui comprend trois `ContentPage` des objets dans son arborescence d’éléments visuels, un permet d’entrer des remarques qui le concernent.

### <a name="using-an-itemtemplate"></a>À l’aide d’un ItemTemplate

Le [ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) exemple utilise le [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Le [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) fichier définit les `DataTemplate` propriété de `TabbedPage` à un début de l’arborescence d’éléments visuels avec `ContentPage` qui contient les liaisons pour les propriétés du `NamedColor` (y compris une liaison à la `Title` propriété).

Toutefois, cela peut être problématique sur iOS. Seuls quelques-uns des éléments peuvent être affichés, et il n’existe aucun moyen de bonne à leur attribuer des icônes.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 25 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Exemples de chapitre 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Page maître / détail](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Page à onglets](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
