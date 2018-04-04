---
title: Résumé du chapitre 24. Navigation entre les pages
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d1a226a4532b745fddee28e943562fb51d34e65
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Résumé du chapitre 24. Navigation entre les pages

De nombreuses applications sont constituées de plusieurs pages, parmi lesquelles l’utilisateur accède. Toujours l’application dispose d’un *principal* page ou *domestique* page, et à partir de là, l’utilisateur navigue vers d’autres pages, qui sont conservées dans une pile pour naviguer vers l’arrière. Options de navigation supplémentaires sont traitées dans [ **chapitre 25. Page variétés**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Pages de modales et non modales

`VisualElement` définit un [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) propriété de type [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), qui inclut les deux méthodes suivantes pour accéder à une nouvelle page :

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Les deux méthodes acceptent une `Page` instance en tant qu’argument et retourner un `Task` objet. Les deux méthodes suivantes accédez à la page précédente :

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Si l’interface utilisateur possède son propre **précédent** bouton (comme les téléphones Android et Windows), il n’est pas nécessaire pour l’application d’appeler ces méthodes.

Bien que ces méthodes sont disponibles à partir des `VisualElement`, elles sont généralement appelées à partir de la `Navigation` propriété actuelles `Page` instance.

Applications utilisent généralement des pages modales lorsque l’utilisateur est requis pour fournir des informations sur la page avant de retourner à la page précédente. Les pages qui ne sont pas modales sont parfois appelés non modales ou *hiérarchique*. Rien dans la page elle-même fait la distinction en tant que modale ou non ; il dépend à la place de la méthode utilisée pour naviguer jusqu'à lui. Pour travailler sur toutes les plateformes, une page modale doit fournir sa propre interface utilisateur pour revenir à la page précédente.

Le [ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) exemple vous permet d’Explorer la différence entre les pages non modales et modales. Toute application qui utilise la navigation de page doit passer à sa page d’accueil pour le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) constructeur, généralement dans le programme `App` classe. Une prime est que vous n’avez plus besoin définir un `Padding` sur la page pour iOS.

Vous découvrirez que pour les non modales pages, la page [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) propriété s’affiche. IOS, Android et les plateformes de Tablet PC et du bureau Windows fournissent un élément d’interface utilisateur pour revenir à la page précédente. Des cours, Android et Windows, les téléphones ont une norme **précédent** bouton revenir en arrière.

Pour les pages modales, la page `Title` n’est pas affichée, et aucun élément d’interface utilisateur n’est fournie pour revenir à la page précédente. Bien que vous pouvez utiliser la norme Android et Windows phone **précédent** bouton pour revenir à la page précédente, la page modale sur d’autres plateformes doit fournir son propre mécanisme de revenir en arrière.

### <a name="animated-page-transitions"></a>Transitions de page animé

Autres versions des différentes méthodes de navigation sont fournies avec un deuxième argument booléen que vous définissez pour `true` si vous souhaitez que la transition de page à inclure une animation :

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Toutefois, les méthodes de navigation de page standard incluent l’animation par défaut, afin qu’ils soient uniquement utiles pour accéder à une page spécifique au démarrage (comme indiqué à la fin de ce chapitre) ou lorsque vous fournissez votre propre animation d’entrée (comme indiqué dans [ **Chapter22. Animation**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Variations visuels et fonctionnelles

`NavigationPage` inclut deux propriétés que vous pouvez définir lorsque vous instanciez la classe dans votre `App` méthode :

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` inclut également les quatre propriétés jointes pouvant être liées qui affectent la page spécifique sur lequel ils sont définis :

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) et [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) et [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) et [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) Professionnel sur iOS uniquement
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) et [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) Professionnel sur iOS et Android uniquement

### <a name="exploring-the-mechanics"></a>Explorer les mécanismes

Les méthodes de navigation de page sont asynchrones et doit être utilisés avec `await`. La saisie semi-automatique n’indique pas que navigation entre les pages est terminée, mais uniquement qu’il est sécurisé examiner la pile de navigation de page.

Lorsqu’une page accède à un autre, la première page obtienne généralement un appel à son [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) (méthode) et la deuxième page Obtient un appel à son [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) (méthode). De même, lorsqu’une page est retournée à l’autre, la première page obtienne un appel à son `OnDisappearing` (méthode) et la deuxième page généralement Obtient un appel à son `OnAppearing` (méthode). L’ordre de ces appels (et l’achèvement des méthodes asynchrones qui appelle le volet de navigation) est dépendant de la plate-forme. L’utilisation du mot « généralement » dans les deux instructions ci-dessus est en raison de la navigation sur une page modale Android, dans lequel ces appels de méthode ne se produisent.

En outre, les appels à la `OnAppearing` et `OnDisappearing` méthodes n’indiquent pas nécessairement navigation entre les pages.

Le `INavigation` interface inclut deux propriétés de collection qui vous permettent d’examiner la pile de navigation :

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) de type `IReadOnlyList<Page>` pour la pile non modale
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) de type `IReadOnlyList<Page>` pour la pile modale

Il est plus sûr accéder à ces piles à partir de la `Navigation` propriété de la `NavigationPage` (qui doit être le `App` la classe [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) propriété). Il n’est plus sûr examiner ces piles après que les méthodes de navigation de page asynchrone s’est terminé. Le [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) propriété de la `NavigationPage` n’indique pas la page active si la page actuelle est une page modale, mais indique à la place la dernière page non modale.

Le [ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) exemple vous permet d’Explorer la navigation entre les pages et les piles et les types juridiques de navigation de page :

- Une page non modale pouvez naviguer vers une autre page non modale ou d’une page modale
- Une page modale peut accéder uniquement à une autre page modale

### <a name="enforcing-modality"></a>L’application modalité

Une application utilise une page modale lorsqu’il est nécessaire obtenir des informations à partir de l’utilisateur. L’utilisateur doit être interdit de retourner à la page précédente jusqu'à ce que ces informations sont fournies. Sur iOS, il est facile fournir un **précédent** bouton et activez-la uniquement lorsque l’utilisateur a terminé avec la page. Mais pour les appareils Android et Windows phone, l’application doit substituer la [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) méthode et retournez `true` si le programme a géré la **précédent** bouton lui-même, comme illustré dans le [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) exemple.

Le [ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) exemple illustre comment cela fonctionne dans un scénario MVVM.

## <a name="navigation-variations"></a>Variantes de navigation

Si une page modale particulier peut être accédée à plusieurs fois, il doit conserver des informations afin que l’utilisateur peut modifier les informations au lieu de taper tous dans une nouvelle fois. Vous pouvez le gérer tout en conservant l’instance particulière de la page modale, mais une meilleure approche (en particulier sur iOS) conserve les informations contenues dans un modèle d’affichage.

### <a name="making-a-navigation-menu"></a>Rendre un menu de navigation

Le [ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) exemple montre comment utiliser un `TableView` pour répertorier les éléments de menu. Chaque élément est associé un `Type` objet pour une page particulière. Lorsque cet élément est sélectionné, le programme instancie la page et navigue vers elle.

[![Capture d’écran de triple du Type de la vue bibliothèque](images/ch24fg21-small.png "TableView répertoriant les éléments de Menu")](images/ch24fg21-large.png#lightbox "TableView répertoriant les éléments de Menu")

Le [ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) exemple est un peu différent dans la mesure où le menu contient des instances de chaque page plutôt que des types. Cela permet de conserver les informations à partir de chaque page, mais toutes les pages doivent être instanciées au démarrage du programme.

### <a name="manipulating-the-navigation-stack"></a>Manipulation de la pile de navigation

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) montre plusieurs fonctions définies par `INavigation` qui vous permettent de manipuler la pile de navigation de manière structurée :

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) et [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) avec animation facultatif

### <a name="dynamic-page-generation"></a>Génération de pages dynamiques

Le [ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) exemple illustre la construction d’une page lors de l’exécution en fonction de l’entrée d’utilisateur.

## <a name="patterns-of-data-transfer"></a>Modèles de transfert de données

Il est souvent nécessaire de partager des données entre les pages &mdash; pour transférer des données à une page de navigation et d’une page retourner des données à la page qui l’a appelée. Il existe plusieurs techniques pour y parvenir.

### <a name="constructor-arguments"></a>Arguments de constructeur

Lorsque vous naviguerez vers une nouvelle page, il est possible d’instancier la classe de page avec un argument de constructeur qui permet à la page pour s’initialiser. Le [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) illustre cela. Il est également possible de la page de navigation pour que ses `BindingContext` définie par la page permettant d’accéder à ce dernier.

### <a name="properties-and-method-calls"></a>Propriétés et les appels de méthode

Les autres exemples de transfert de données Explorer le problème de transfert d’informations entre les pages quand une page navigue vers une autre page et inversement. Dans ces rubriques, les *domestique* page accède à la *info* page et doit transférer des informations initialisées à la *info* page. Le *info* page obtient des informations supplémentaires à partir de l’utilisateur et transfère les informations pour le *domestique* page.

Le *domestique* page permettre accéder facilement aux méthodes publiques et les propriétés dans le *info* page dès qu’il instancie cette page. Le *info* page permettre également accéder aux méthodes publiques et les propriétés dans le *domestique* page, mais que le choix judicieux pour cela peut être difficile. Le [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) code effectue cette opération son `OnDisappearing` remplacer. L’inconvénient est que le *info* page doit connaître le type de la *domestique* page.

### <a name="messagingcenter"></a>MessagingCenter

Le Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe fournit un autre moyen pour les deux pages communiquer entre eux. Les messages sont identifiés par une chaîne de texte et peuvent être accompagnés de n’importe quel objet.

Un programme qui souhaite recevoir des messages à partir d’un type particulier doit s’abonner à ceux-ci à l’aide de [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) et spécifier une fonction de rappel. Plus tard, il peut vous désabonner en appelant [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). La fonction de rappel reçoit les messages envoyés à partir du type spécifié avec le nom spécifié est envoyé via le [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) (méthode).

Le [ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) programme montre comment transférer des données à l’aide du centre de messagerie, mais à nouveau, ceci nécessite que le *info* page connaître le type de la *accueil* page.

### <a name="events"></a>Événements

L’événement est une approche précieuses pour une classe envoyer des informations à une autre classe sans connaître le type de la classe. Dans le [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) exemple le *info* classe définit un événement qu’il se déclenche lorsque les informations sont prêt. Toutefois, il n’existe aucun emplacement pratique pour le *domestique* page pour détacher le Gestionnaire d’événements.

### <a name="the-app-class-intermediary"></a>L’intermédiaire de la classe App

Le [ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) montre comment accéder aux propriétés définies dans le `App` classe à la fois par le *domestique* page et le *info*page. Il s’agit d’une bonne solution, mais la section suivante décrit une meilleure solution.

### <a name="switching-to-a-viewmodel"></a>Passage à un ViewModel

À l’aide d’un ViewModel pour les informations de la *domestique* page et le *info* page à partager l’instance de la classe d’informations. Cela est illustré dans le [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) exemple.

### <a name="saving-and-restoring-page-state"></a>Enregistrement et restauration de l’état de la page

Le `App` intermédiaire de la classe ou l’approche ViewModel est idéale lors de l’application doit enregistrer des informations si le programme se met en veille pendant que le *info* page est active. Le [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) illustre cela.

## <a name="saving-and-restoring-the-navigation-stack"></a>Enregistrement et restauration de la pile de navigation

En règle générale, un programme multipage qui se met en veille doit accéder à la page même lorsqu’elle est restaurée. Cela signifie que ce programme doit enregistrer le contenu de la pile de navigation. Cette section montre comment automatiser ce processus dans une classe conçue à cet effet. Cette classe appelle également les pages individuelles pour les autoriser à enregistrer et restaurer l’état de la page.

Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque définit une interface nommée [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) que les classes peuvent implémenter pour enregistrer et restaurer les éléments dans le `Properties`dictionnaire.

Le [ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) classe dans le **Xamarin.FormsBook.Toolkit** dérive de la bibliothèque `Application`. Vous pouvez ensuite dériver votre `App` classe `MultiPageRestorableApp` et effectuer un peu de ménage.

Le [ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) illustre l’utilisation de `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Quelque chose comme une application réelle

Le [ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) exemple tire également parti de `MultiPageRestorableApp` et permet l’entrée et la modification de commentaires qui sont enregistrés dans le `Properties` dictionnaire.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 24 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Exemples de chapitre 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Navigation hiérarchique](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Pages modales](~/xamarin-forms/app-fundamentals/navigation/modal.md)
