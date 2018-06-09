---
title: Résumé du chapitre 2. Anatomie d’une application
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 2. Anatomie d’une application'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 208cf28341ceaa43d1c56b4f5086dc98febee6be
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242745"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Résumé du chapitre 2. Anatomie d’une application

Dans une application de Xamarin.Forms, les objets qui occupent l’espace sur l’écran sont appelés *éléments visuels*, encapsulé par le [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) classe. Éléments visuels peuvent être divisées en trois catégories correspondant à ces classes :

- [Page](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Disposition](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [Affichage](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

Un `Page` dérivé occupe la totalité de l’écran ou presque tout l’écran. Souvent, l’enfant d’une page est un `Layout` dérivé pour organiser les éléments visuels enfants. Les enfants de la `Layout` peut être autres `Layout` classes ou `View` dérivés (souvent appelé *éléments*), qui sont familiers tels que texte, images bitmap, curseurs, boutons, zones de liste et ainsi de suite.

Ce chapitre explique comment créer une application en se concentrant sur la [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), qui est le `View` dérivée qui affiche du texte.

## <a name="say-hello"></a>Découvrez

Avec la plate-forme Xamarin installée, vous pouvez créer une solution Xamarin.Forms dans Visual Studio ou Visual Studio pour Mac. Le [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) solution utilise une bibliothèque de classes Portable pour le code commun. Il illustre une solution Xamarin.Forms créée dans Visual Studio sans modification. La solution se compose de six projets (la dernière dont deux ne sont pas créés avec les modèles de solution Xamarin.Forms actuels) :

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), une bibliothèque de classes Portable (PCL) partagé par d’autres projets
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), un projet d’application pour Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), un projet d’application pour iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), un projet d’application pour la plateforme Windows universelle (Windows 10 et Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), un projet d’application pour Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), un projet d’application pour Windows Phone 8.1

Vous pouvez apporter l’une de ces projets d’application du projet de démarrage, puis générer et exécuter le programme sur un appareil ou un simulateur.

Dans de nombreux programmes Xamarin.Forms, vous ne modifiez les projets d’application. Souvent, ces restent minuscules stubs pour lancer le programme. La plupart de votre focus sera la bibliothèque de classes Portable communes à toutes les applications.

## <a name="inside-the-files"></a>Dans les fichiers

Les éléments visuels affichés par le **Hello** programme sont définis dans le constructeur de la [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) classe. `App` dérive de la classe Xamarin.Forms [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

Le **références** section de la **Hello** projet de bibliothèque PCL inclut les assemblys Xamarin.Forms suivants :

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

Le **références** sections des projets de cinq application incluent des assemblys supplémentaires qui s’appliquent à chaque plate-forme :

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Les projets d’application contient un appel à la méthode statique `Forms.Init` méthode dans le `Xamarin.Forms` espace de noms. Ce code initialise la bibliothèque Xamarin.Forms. Une autre version de `Forms.Init` est définie pour chaque plateforme. Les appels à cette méthode peuvent être trouvés dans les classes suivantes :

- iOS : [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android : [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP : [ `App` (classe), `OnLaunched` (méthode)](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1 : [ `App` (classe), `OnLaunched` (méthode)](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1 : [ `App` (classe), `OnLaunched` (méthode)](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

En outre, chaque plate-forme doit instancier la `App` classe emplacement dans la bibliothèque PCL. Ceci se produit dans un appel à `LoadApplication` dans les classes suivantes :

- iOS : [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android : [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- PLATEFORME WINDOWS UNIVERSELLE : [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1 : [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1 : [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Dans le cas contraire, ces projets d’application sont des programmes de « ne rien faire » normal.

## <a name="pcl-or-sap"></a>Bibliothèque de classes portables ou SAP ?

Il est possible de créer une solution Xamarin.Forms avec le code commun dans une bibliothèque de classes Portable (PCL) ou un projet de ressource partagé (SAP). Pour créer une solution SAP, sélectionnez l’option Shared dans Visual Studio. Le [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) solution montre le modèle SAP sans modification.

Les regroupements d’approche PCL ensemble commun de code dans un projet de bibliothèque référencé par les projets d’application de plateforme. Avec l’approche SAP, le code commun efficacement existe dans tous les projets d’application de plateforme et est partagé entre eux.

La plupart des développeurs Xamarin.Forms préférez l’approche de la bibliothèque de classes portables. Dans cet ouvrage, la plupart des solutions sont PCL. Ceux qui utilisent SAP incluent un **Sap** suffixe du nom du projet.

Pour prendre en charge toutes les plateformes de Xamarin.Forms, la version de .NET utilisé par la bibliothèque PCL doit prendre en charge les plateformes suivantes :

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Classic)

Il s’agit en tant que PC profil 111.

Avec l’approche SAP, le code dans le projet partagé peut exécuter un code différent pour les différentes plateformes à l’aide de directives de préprocesseur c# (`#if`, #`elif`, et `#endif`) avec ces prédéfinies identificateurs :

- iOS : `__IOS__`
- Android : `__ANDROID__`
- PLATEFORME WINDOWS UNIVERSELLE : `WINDOWS_UWP`
- Windows 8.1 : `WINDOWS_APP`
- Windows Phone 8.1 : `WINDOWS_PHONE_APP`

Dans une bibliothèque de classes portables, vous pouvez déterminer quelle plate-forme que vous exécutez lors de l’exécution, comme vous le verrez plus loin dans ce chapitre.

## <a name="labels-for-text"></a>Étiquettes de texte

Le [ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) solution montre comment ajouter un nouveau fichier c# pour le **Greetings** projet. Ce fichier définit une classe nommée `GreetingsPage` qui dérive de `ContentPage`. Dans ce guide, la plupart des projets contiennent une seule `ContentPage` dérivé dont le nom est le nom du projet avec le suffixe `Page` ajouté.

Le `GreetingsPage` constructeur instancie un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) vue, qui est la vue Xamarin.Forms qui affiche du texte. Le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) est définie sur le texte affiché par le `Label`. Ce programme définit le `Label` à la `Content` propriété du `ContentPage`. Le constructeur de la `App` classe instancie ensuite `GreetingsPage` et lui affecte sa `MainPage` propriété.

Le texte est affiché dans le coin supérieur gauche de la page. Sur iOS, cela signifie qu’il chevauche barre d’état de la page. Il existe plusieurs solutions à ce problème :

### <a name="solution-1-include-padding-on-the-page"></a>Solution 1. Inclure la marge intérieure dans la page

Définir un [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) propriété sur la page. `Padding` est de type [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), une structure avec quatre propriétés :

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` définit une zone à l’intérieur d’une page dans laquelle le contenu est exclu. Cela permet la `Label` pour éviter le remplacement de la barre d’état iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Solution 2. Inclure la marge intérieure pour iOS (SAP uniquement)

Définir une propriété « Marge » uniquement sur iOS à l’aide d’un SAP avec une directive de préprocesseur c#. Cela est illustré dans le [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) solution.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Solution 3. Inclure la marge intérieure pour iOS (PCL ou SAP)

Dans la version de Xamarin.Forms utilisé pour le Registre, un `Padding` propriété spécifique à iOS dans une bibliothèque de classes portables ou SAP peut être sélectionnée à l’aide de la [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) ou [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) méthode statique. Ces méthodes sont désormais déconseillées.

Le `Device.OnPlatform` méthodes sont utilisées pour exécuter du code spécifique à la plateforme ou pour sélectionner des valeurs spécifiques à la plateforme. En interne, ils réalisent utilisent le [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) statique propriété en lecture seule, qui retourne un membre de la [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) énumération :

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) pour Windows 8.1, Windows Phone 8.1 et tous les périphériques de la plateforme Windows universelle.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), utilisé pour identifier le Windows Phone 8.0 précédemment, mais est désormais inutilisée
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) n’est pas utilisé

Le `Device.OnPlatform` méthodes, le `Device.OS` propriété et le `TargetPlatform` énumération sont maintenant déconseillées. Au lieu de cela, utilisez le [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) propriété et comparer le `string` retourner la valeur avec les champs statiques suivantes :

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), la chaîne « iOS »
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), la chaîne « Android »
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), la chaîne « UWP », qui fait référence à la plateforme d’exécution de Windows
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), la chaîne « Windows » pour le Windows Runtime (Windows 8.1 et Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), la chaîne « WinPhone » pour Windows Phone 8.0

Le [ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) les propriété en lecture seule statique est associée. Cela retourne un membre de la [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), qui a ces membres :

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) n’est pas utilisé

Pour iOS et Android, la limite entre `Tablet` et `Phone` est une largeur de portrait de 600 unités. Pour la plateforme Windows, `Desktop` indique une application de plateforme Windows universelle s’exécutant sous Windows 10, `Tablet` est un programme Windows 8.1, et `Phone` indique une application de plateforme Windows universelle s’exécutant sous Windows 10 ou une application Windows Phone 8.1.

## <a name="solution-3a-set-margin-on-the-label"></a>Solution 3. Définir la marge sur l’étiquette

Le [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) propriété a été introduite trop tard pour être incluses dans le Registre, mais il est également de type `Thickness` et vous pouvez le définir sur le `Label` pour définir une zone en dehors de la vue qui est incluse dans le calcul de la disposition de la vue.

Le `Padding` propriété n’est disponible sur [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) et [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) dérivés. Le `Margin` propriété n’est disponible sur tous les [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) dérivés.

## <a name="solution-4-center-the-label-within-the-page"></a>Solution de 4. Centre de l’étiquette dans la page

Vous pouvez centrer le `Label` au sein de la `Page` (ou le placer dans une des huit autres emplacements) en définissant le [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés de la `Label` une valeur de type [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). Le `LayoutOptions` structure définit deux propriétés :

- Un [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) propriété de type [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), une énumération avec quatre membres : [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), ce qui signifie que gauche ou supérieur, en fonction de la orientation, [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), ce qui signifie que le droit ou inférieur en fonction de l’orientation, et [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- Un [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) propriété de type `bool`.

En général ces propriétés ne sont pas utilisées directement. Au lieu de cela, les combinaisons de ces deux propriétés sont fournies par les huit propriétés en lecture seule statiques de type `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` et `VerticalOptions` sont les propriétés les plus importantes dans Présentation de Xamarin.Forms et sont décrits plus en détail dans [ **chapitre 4. Défilement de la pile**](chapter04.md).

Voici le résultat avec la `HorizontalOptions` et `VerticalOptions` propriétés de `Label` tous deux la valeur `LayoutOptions.Center`:

[![Capture d’écran de triple du programme de greetings](images/ch02fg05-small.png "horizontalement et verticalement centré étiquette")](images/ch02fg05-large.png#lightbox "horizontalement et verticalement centré d’étiquette")

## <a name="solution-5-center-the-text-within-the-label"></a>Solution de 5. Centre le texte dans l’étiquette

Vous pouvez également centrer le texte (ou placez-le dans huit autres emplacements sur la page) en définissant le [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) et [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) propriétés de `Label` à un membre de la [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) énumération :

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), sens gauche ou haut (selon l’orientation)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), ce qui signifie que droit ou inférieur (en fonction de l’orientation)

Ces deux propriétés sont définies uniquement par `Label`, alors que le `HorizontalAlignment` et `VerticalAlignment` propriétés sont définies par `View` et héritée par toutes les `View` dérivés. Les résultats visuels peuvent sembler similaires, mais ils sont très différents, comme le chapitre suivant montre.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 2 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Exemples de chapitre 2](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Exemples de chapitre 2 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Prise en main de Xamarin.Forms](~/xamarin-forms/get-started/index.md)
