---
title: "Résumé du chapitre 8. XAML en harmonie et code"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 452a7835bcb54501edffe7a2467544c6677616ba
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Résumé du chapitre 8. XAML en harmonie et code

Ce chapitre traite plus profondément de XAML, et notamment comment XAML et code interagissent.

## <a name="passing-arguments"></a>Passer des arguments

En règle générale, une classe instanciée en XAML doit avoir un constructeur sans paramètre public ; l’objet résultant est initialisé par le biais des paramètres de propriété. Toutefois, il existe d’autres deux façons objets peuvent être instanciées et initialisées.

Bien que ces techniques à usage général, ils sont principalement utilisées conjointement avec MVVM afficher les modèles.

### <a name="constructors-with-arguments"></a>Constructeurs avec des arguments

Le [ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) exemple montre comment utiliser le `x:Arguments` balise pour spécifier des arguments de constructeur. Ces arguments doivent être délimités par des balises d’élément qui indique le type de l’argument. Pour les types de données .NET base, les balises suivantes sont disponibles :

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Puis-je appeler des méthodes à partir de XAML ?

Le [ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) exemple montre comment utiliser le `x:FactoryMethod` élément pour spécifier une méthode de fabrique qui est appelée pour créer un objet. Une telle méthode de fabrique doit être publique et statique, et il doit créer un objet du type dans lequel il est défini. (Par exemple le [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) méthode qualifie, car elle est publique et statique et retourne une valeur de type `Color`.) Les arguments de la méthode de fabrique sont spécifiées dans `x:Arguments` balises.

## <a name="the-xname-attribute"></a>L’attribut x : Name

Le `x:Name` attribut permet à un objet instancié en XAML à donner un nom. Les règles pour ces noms sont les mêmes que pour les noms de variables c#. Après le retour de la `InitializeComponent` appeler le constructeur, le fichier code-behind peut faire référence à ces noms pour accéder à l’élément XAML correspondant. Les noms sont convertis en fait par l’analyseur XAML dans les champs privés de la classe partielle générée.

Le [ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) exemple illustre l’utilisation de `x:Name` pour permettre au fichier code-behind conserver deux `Label` éléments définis dans XAML mis à jour avec la date et heure actuelles.

Le même nom ne peut pas être utilisé pour plusieurs éléments sur la même page. Il s’agit d’un problème particulier si vous utilisez `OnPlatform` pour créer en parallèle des objets nommés pour chaque plateforme. Le [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) exemple illustre un meilleur moyen de faire quelque chose de similaire.

## <a name="custom-xaml-based-views"></a>Vues personnalisées basées sur XAML

Il existe plusieurs façons d’éviter la répétition de balisage en XAML. Une technique courante consiste à créer une nouvelle classe basée sur XAML qui dérive de [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Cette technique est illustrée dans le [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) exemple. Le `ColorView` dérive de la classe `ContentView` pour afficher une couleur particulière et son nom, tandis que la `ColorViewListPage` dérive de la classe `ContentPage` comme d’habitude et explicitement crée des instances de 17 de `ColorView`.

L’accès à la `ColorView` classe dans XAML requiert une autre déclaration d’espace de noms XML, généralement nommée `local` pour les classes dans le même assembly.

## <a name="events-and-handlers"></a>Événements et gestionnaires

Les événements peuvent être affectés à des gestionnaires d’événements en XAML, mais le Gestionnaire d’événements doit être implémenté dans le fichier code-behind. Le [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) montre comment créer une interface utilisateur de clavier en XAML et comment implémenter le `Clicked` gestionnaires dans le fichier code-behind.

## <a name="tap-gestures"></a>Appuyez sur les mouvements

N’importe quel `View` objet peut obtenir l’entrée tactile et générer des événements à partir de cette entrée. Le `View` classe définit un [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) propriété de collection qui peut contenir une ou plusieurs instances de classes qui dérivent de [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

Le [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) génère [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) événements. Le [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) programme montre comment attacher `TapGestureRecognizer` quatre objets `BoxView` éléments pour créer un jeu contrefait :

[![Capture d’écran de triple de drainage de singe](images/ch08fg07-small.png "imite le jeu")](images/ch08fg07-large.png#lightbox "imite le jeu")

Mais le **MonkeyTap** programme doit réellement son. (Consultez [le chapitre suivant](chapter09.md).)



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Exemples du chapitre 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Exemple de chapitre 8) (F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
