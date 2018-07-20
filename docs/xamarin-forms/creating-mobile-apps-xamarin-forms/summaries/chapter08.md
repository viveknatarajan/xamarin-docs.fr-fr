---
title: Résumé du chapitre 8. Code et XAML en harmonie
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 8. Code et XAML en harmonie'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 1aa5226e1e6867f77eea4d7679650e8d62f5b981
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156988"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Résumé du chapitre 8. Code et XAML en harmonie

Ce chapitre explore XAML de façon plus approfondie, et en particulier comment code et XAML interagissent.

## <a name="passing-arguments"></a>Passage des arguments

En règle générale, une classe instanciée dans XAML doit avoir un constructeur sans paramètre public ; l’objet résultant est initialisé par le biais des paramètres de propriété. Toutefois, il existe deux autres méthodes que les objets peuvent être instanciés et initialisés.

Bien qu’il s’agit des techniques à usage général, ils sont principalement utilisés dans le cadre de modèles de vue de MVVM.

### <a name="constructors-with-arguments"></a>Constructeurs avec des arguments

Le [ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) exemple montre comment utiliser le `x:Arguments` balise pour spécifier des arguments de constructeur. Ces arguments doivent être délimités par des balises d’élément indiquant le type de l’argument. Pour les types de données .NET base, les balises suivantes sont disponibles :

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

Le [ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) exemple montre comment utiliser le `x:FactoryMethod` élément pour spécifier une méthode de fabrique qui est appelée pour créer un objet. Une telle méthode de fabrique doit être publique et statique, et il doit créer un objet du type dans lequel elle est définie. (Par exemple le [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) qualifie de méthode, car elle est publique et statique et retourne une valeur de type `Color`.) Les arguments à la méthode de fabrique sont spécifiés dans `x:Arguments` balises.

## <a name="the-xname-attribute"></a>L’attribut x : Name

Le `x:Name` attribut permet à un objet instancié dans XAML à donner un nom. Les règles pour ces noms sont les mêmes que pour les noms de variables c#. Après le retour de la `InitializeComponent` appeler le constructeur, le fichier code-behind peut faire référence à ces noms pour accéder à l’élément XAML correspondant. Les noms sont convertis en fait par l’analyseur XAML dans les champs privés de la classe partielle générée.

Le [ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) exemple illustre l’utilisation de `x:Name` pour permettre au fichier de code-behind de conserver deux `Label` éléments définis dans XAML mis à jour avec la date et heure actuelles.

Le même nom ne peut pas être utilisé pour plusieurs éléments dans la même page. Il s’agit d’un problème particulier si vous utilisez `OnPlatform` pour créer en parallèle des objets nommés pour chaque plateforme. Le [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) exemple illustre un meilleur moyen de faire quelque chose de similaire.

## <a name="custom-xaml-based-views"></a>Vues personnalisées en fonction du XAML

Il existe plusieurs façons d’éviter la répétition de balisage en XAML. Une technique courante consiste à créer une nouvelle classe XAML qui dérive de [ `ContentView` ](xref:Xamarin.Forms.ContentView). Cette technique est illustrée dans le [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) exemple. Le `ColorView` dérive de la classe `ContentView` pour afficher une couleur particulière et son nom, tandis que le `ColorViewListPage` dérive de la classe `ContentPage` comme d’habitude et explicitement crée des 17 instances de `ColorView`.

L’accès à la `ColorView` classe dans XAML nécessite une autre déclaration d’espace de noms XML, généralement nommée `local` pour les classes dans le même assembly.

## <a name="events-and-handlers"></a>Événements et gestionnaires

Événements peuvent être affectés aux gestionnaires d’événements dans XAML, mais le Gestionnaire d’événements doit être implémenté dans le fichier code-behind. Le [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) montre comment créer une interface utilisateur de clavier dans XAML et comment implémenter le `Clicked` gestionnaires dans le fichier code-behind.

## <a name="tap-gestures"></a>Appuyez sur les mouvements

N’importe quel `View` objet peut obtenir l’entrée tactile et générer des événements à partir de cette entrée. Le `View` classe définit un [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) propriété de collection qui peut contenir une ou plusieurs instances des classes qui dérivent de [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer).

Le [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) génère [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) événements. Le [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) programme montre comment associer `TapGestureRecognizer` objets à quatre `BoxView` éléments pour créer un jeu d’imitation :

[![Capture d’écran triple de collecte de monkey](images/ch08fg07-small.png "Imitation jeu")](images/ch08fg07-large.png#lightbox "Imitation jeu")

Mais le **MonkeyTap** programme doit vraiment son. (Consultez [le chapitre suivant](chapter09.md).)

## <a name="related-links"></a>Liens connexes

- [Chapitre 8 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Exemples du chapitre 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Exemple de chapitre 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Passage des Arguments dans XAML](~/xamarin-forms/xaml/passing-arguments.md)
