---
title: Résumé du chapitre 7. XAML et code
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 97f1ad1f818c74a294421f223c4cea0123b83373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Résumé du chapitre 7. XAML et code

Xamarin.Forms prend en charge un langage de balisage basé sur XML appelé le Extensible Application Markup Language ou le XAML (prononcé « zammel »). XAML fournit une alternative à c# dans la définition de la disposition de l’interface utilisateur d’une application de Xamarin.Forms et définir des liaisons entre les éléments d’interface utilisateur et les données sous-jacentes.

## <a name="properties-and-attributes"></a>Propriétés et attributs

Structures et classes de Xamarin.Forms deviennent des éléments XML en XAML, et propriétés de ces classes et les structures deviennent des attributs XML. Pour être instancié en XAML, une classe doit avoir généralement d’un constructeur sans paramètre public. Les propriétés définies en XAML doivent avoir public `set` accesseurs.

Pour les propriétés des types de base de données (`string`, `double`, `bool`, et ainsi de suite), l’analyseur XAML utilise la norme `TryParse` méthodes pour convertir ces types de paramètres d’attribut. L’analyseur XAML peut gérer facilement les types énumération, et il peut combiner des membres de l’énumération si le type d’énumération est marqué avec la `Flags` attribut.

Afin de faciliter l’analyseur XAML, les types plus complexes (ou propriétés de ces types) peuvent inclure un [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) qui identifie une classe qui dérive de [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) qui prend en charge la conversion à partir de valeurs de chaîne à ces types. Par exemple, le [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) les noms et les chaînes, telles que « #rrggbb », de couleur convertit en `Color` valeurs.

## <a name="property-element-syntax"></a>Syntaxe d’élément de propriété

En XAML, les classes et les objets créés à partir de celles-ci sont exprimées en tant qu’éléments XML. Ils sont appelés *éléments objet*. La plupart des propriétés de ces objets sont exprimées en tant qu’attributs XML. Ils sont appelés *les attributs de propriété*.

Parfois, une propriété doit être définie à un objet qui ne peut pas être exprimé sous forme de chaîne simple. Dans ce cas, XAML prend en charge une balise appelée un *élément de propriété* qui se compose du nom de la classe et du nom de propriété séparés par un point. Un élément de l’objet peut ensuite apparaître dans une paire de balises d’élément de propriété.

## <a name="adding-a-xaml-page-to-your-project"></a>Ajout d’une page XAML à votre projet

Une bibliothèque de classes Portable Xamarin.Forms peut contenir une page XAML lorsqu’il est créé en premier, ou vous pouvez ajouter une page XAML à un projet existant. Dans la boîte de dialogue Ajouter un nouvel élément, sélectionnez l’élément qui fait référence à une page XAML, ou `ContentPage` et XAML. (Pas un `ContentView`.)

Deux fichiers sont créés : un fichier XAML avec le .xaml d’extension de nom de fichier et un fichier c# avec l’extension. xaml.cs. Le fichier c# est souvent appelé le *code-behind* du fichier XAML. Le fichier code-behind est une définition de classe partielle qui dérive de `ContentPage`. Au moment de la génération, le code XAML est analysé, et une autre définition de classe partielle est générée pour la même classe. Cette classe générée inclut une méthode nommée `InitializeComponent` qui est appelée à partir du constructeur du fichier code-behind.

Pendant l’exécution, à la fin de la `InitializeComponent` appeler, tous les éléments du fichier XAML ont été instanciés et initialisés comme si elles avaient été créées dans le code c#.

L’élément racine dans le fichier XAML est `ContentPage`. La balise racine contient au moins deux déclarations d’espace de noms XML, un pour les éléments Xamarin.Forms et autres définir un `x` préfixe pour les éléments et attributs intrinsèques à toutes les implémentations de XAML. La balise racine contient également un `x:Class` attribut qui indique l’espace de noms et le nom de la classe qui dérive de `ContentPage`. Cela correspond à l’espace de noms et le nom dans le fichier code-behind.

La combinaison de XAML et le code est illustrée par le [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) exemple.

## <a name="the-xaml-compiler"></a>Le compilateur XAML

Xamarin.Forms a un compilateur XAML, mais son utilisation est facultative en fonction de l’utilisation d’un [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). Si le code XAML n’est pas compilé, le code XAML est analysé au moment de la génération et le fichier XAML est incorporé dans la bibliothèque PCL, où il est également analysé lors de l’exécution. Si le code XAML est compilé, le processus de génération convertit le code XAML en une forme binaire, et le traitement de l’exécution est plus efficace.

## <a name="platform-specificity-in-the-xaml-file"></a>Spécificité de plateforme dans le fichier XAML

En XAML, le [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) classe peut être utilisée pour sélectionner le balisage de dépend de la plateforme. Ceci est une classe générique et doit être instancié avec un `x:TypeArguments` attribut qui correspond au type cible. Le [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) classe est similaire, mais utilisé beaucoup moins souvent.

L’utilisation de `OnPlatform` a changé depuis la publication de l’annuaire. Il a été initialement utilisé conjointement avec les propriétés nommées `iOS`, `Android`, et `WinPhone`. Il est maintenant utilisé avec enfant [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) objets. Définir le [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) propriété dans une chaîne cohérente avec le public `const` champs de la [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) classe. Définir le [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) propriété à une valeur cohérente avec la `x:TypeArguments` attribut de la `OnPlatform` balise.

`OnPlatform` est illustré dans le [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) exemple, appelée ainsi car il contient des blocs de code XAML quasiment identique. L’existence de ce balisage répétitive suggère que les techniques doivent être disponibles pour la réduire.

## <a name="the-content-property-attributes"></a>Les attributs de propriété de contenu

Certains éléments de propriété se produisent fréquemment, telles que la `<ContentPage.Content>` balise sur l’élément racine d’un `ContentPage`, ou le `<StackLayout.Children>` balise qui englobe les enfants de `StackLayout`.

Chaque classe est autorisée pour identifier une propriété avec un [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) sur la classe. Pour cette propriété, les balises d’élément de propriété ne sont pas requis. `ContentPage` définit la propriété de contenu en tant que `Content`, et `Layout<T>` (la classe à partir de laquelle `StackLayout` dérive) définit sa propriété de contenu en tant que `Children`. Ces balises d’élément de propriété ne sont pas nécessaires.

L’élément de propriété `Label` est `Text`.

## <a name="formatted-text"></a>Texte mis en forme

Le [ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) exemple contient plusieurs exemples de paramètre de la `Text` et `FormattedText` propriétés de `Label`. En XAML, `Span` objets s’affichent en tant qu’enfants de le `FormattedString` objet.

 Lorsqu’une chaîne multiligne est définie le `Text` propriété, les caractères de fin de ligne sont convertis en caractères d’espace, mais les caractères de fin de ligne sont conservées lorsqu’une chaîne multiligne apparaît en tant que contenu de la `Label` ou `Label.Text` balises :

 [![Capture d’écran de triple des variations de texte partage](images/ch07fg03-small.png "Variations de texte mis en forme")](images/ch07fg03-large.png#lightbox "Variations de texte mis en forme")



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 7 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Exemples de chapitre 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Exemple de chapitre 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
