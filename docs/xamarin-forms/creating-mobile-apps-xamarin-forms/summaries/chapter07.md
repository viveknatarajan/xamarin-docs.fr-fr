---
title: Résumé du chapitre 7. XAML et code
description: 'Création d’applications mobiles avec Xamarin.Forms : Résumé du chapitre 7. XAML et code'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334742"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Résumé du chapitre 7. XAML et code

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> Notes sur cette page indiquent des zones où Xamarin.Forms est différente de la matière présentée dans le livre.

Xamarin.Forms prend en charge un langage de balisage basé sur XML appelé l’Extensible Application Markup Language ou le XAML (prononcé « zammel »). XAML offre une alternative à c# dans la définition de la disposition de l’interface utilisateur d’une application Xamarin.Forms et dans la définition des liaisons entre les éléments d’interface utilisateur et les données sous-jacentes.

## <a name="properties-and-attributes"></a>Propriétés et les attributs

Structures et classes de Xamarin.Forms deviennent des éléments XML dans XAML, et les propriétés de ces classes et des structures deviennent des attributs XML. Pour être instancié en XAML, une classe doit avoir généralement d’un constructeur sans paramètre public. Les propriétés définies dans XAML doivent avoir public `set` accesseurs.

Pour les propriétés des types de base de données (`string`, `double`, `bool`, et ainsi de suite), l’analyseur XAML utilise la norme `TryParse` méthodes pour convertir les paramètres de l’attribut à ces types. L’analyseur XAML peut également facilement gérer des types énumération, et il peut associer des membres de l’énumération si le type d’énumération est marqué avec le `Flags` attribut.

Pour aider l’analyseur XAML, des types plus complexes (ou propriétés de ces types) peuvent inclure un [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) qui identifie une classe qui dérive de [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) qui prend en charge la conversion à partir de valeurs de chaîne à ces types. Par exemple, le [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) convertit de noms et les chaînes, telles que « #rrggbb », couleur dans `Color` valeurs.

## <a name="property-element-syntax"></a>Syntaxe d’élément de propriété

Dans XAML, les classes et les objets créés à partir de celles-ci sont exprimées en tant qu’éléments XML. Ils sont appelés *éléments objet*. La plupart des propriétés de ces objets sont exprimées en tant qu’attributs XML. Ils sont appelés *les attributs de propriété*.

Parfois, une propriété doit être définie à un objet qui ne peuvent pas être exprimé sous forme de chaîne simple. Dans ce cas, XAML prend en charge une balise appelée un *élément property* qui se compose du nom de la classe et du nom de propriété séparés par un point. Un élément objet peut ensuite apparaître dans une paire de balises d’élément de propriété.

## <a name="adding-a-xaml-page-to-your-project"></a>Ajout d’une page XAML à votre projet

Une bibliothèque de classes Portable Xamarin.Forms peut contenir une page XAML quand il est tout d’abord créé, ou vous pouvez ajouter une page XAML à un projet existant. Dans la boîte de dialogue Ajouter un nouvel élément, choisissez l’élément qui fait référence à une page XAML, ou `ContentPage` et XAML. (Pas un `ContentView`.)

> [!NOTE]
> Options de Visual Studio ont été modifiés depuis la rédaction de ce chapitre.

Deux fichiers sont créés : un fichier XAML avec la .xaml d’extension de nom de fichier et un fichier c# avec l’extension. xaml.cs. Le fichier c# est souvent appelé le *code-behind* du fichier XAML. Le fichier code-behind est une définition de classe partielle qui dérive de `ContentPage`. Au moment de la génération, le XAML est analysé et une autre définition de classe partielle est générée pour la même classe. Cette classe générée inclut une méthode nommée `InitializeComponent` qui est appelée à partir du constructeur du fichier code-behind.

Pendant l’exécution, à la fin de la `InitializeComponent` appeler, tous les éléments du fichier XAML ont été instanciés et initialisés comme si elles avaient été créées dans le code c#.

L’élément racine dans le fichier XAML est `ContentPage`. La balise racine contient au moins deux déclarations d’espace de noms XML, un pour les éléments de Xamarin.Forms et l’autre définition d’un `x` préfixe pour les éléments et attributs intrinsèques à toutes les implémentations XAML. La balise racine contient également un `x:Class` attribut qui indique l’espace de noms et le nom de la classe qui dérive de `ContentPage`. Cela correspond à l’espace de noms et le nom dans le fichier code-behind.

La combinaison de XAML et de code est illustrée par la [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) exemple.

## <a name="the-xaml-compiler"></a>Le compilateur XAML

Xamarin.Forms a un compilateur XAML, mais son utilisation est facultative en fonction de l’utilisation d’un [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). Si le XAML n’est pas compilé, le XAML est analysé au moment de la génération et le fichier XAML est incorporé dans la bibliothèque PCL, où il est également analysé lors de l’exécution. Si le XAML est compilé, le processus de génération convertit le XAML dans un format binaire et le traitement de l’exécution est plus efficace.

## <a name="platform-specificity-in-the-xaml-file"></a>Spécificité de plateforme dans le fichier XAML

Dans XAML, le [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) classe peut être utilisée pour sélectionner le balisage de dépend de la plateforme. Ceci est une classe générique et doit être instancié avec une `x:TypeArguments` attribut qui correspond au type de cible. Le [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) classe est similaire mais utilisé beaucoup moins souvent.

L’utilisation de `OnPlatform` a changé depuis le livre a été publié. Il a été initialement utilisé conjointement avec les propriétés nommées `iOS`, `Android`, et `WinPhone`. Il est maintenant utilisé avec enfant [ `On` ](xref:Xamarin.Forms.On) objets. Définir le [ `Platform` ](xref:Xamarin.Forms.On.Platform) en une chaîne cohérente avec le public `const` champs de la [ `Device` ](xref:Xamarin.Forms.Device) classe. Définir le [ `Value` ](xref:Xamarin.Forms.On.Value) propriété une valeur cohérente avec la `x:TypeArguments` attribut de la `OnPlatform` balise.

`OnPlatform` est illustrée dans le [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) exemple, appelée ainsi car il contient des blocs de XAML quasiment identique. L’existence de ce balisage et répétitive suggère que les techniques doivent être disponibles pour la réduire.

## <a name="the-content-property-attributes"></a>Les attributs de propriété de contenu

Certains éléments de propriété se produisent très fréquemment, telles que la `<ContentPage.Content>` balise sur l’élément racine d’un `ContentPage`, ou le `<StackLayout.Children>` balise qui englobe les enfants de `StackLayout`.

Chaque classe est autorisé à identifier une propriété avec une [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) sur la classe. Pour cette propriété, les balises d’élément de propriété ne sont pas nécessaires. `ContentPage` définit sa propriété de contenu en tant que `Content`, et `Layout<T>` (la classe à partir de laquelle `StackLayout` dérive) définit sa propriété de contenu en tant que `Children`. Ces balises d’élément de propriété ne sont pas nécessaires.

L’élément de propriété de `Label` est `Text`.

## <a name="formatted-text"></a>Texte mis en forme

Le [ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) exemple contient plusieurs exemples de paramètre de la `Text` et `FormattedText` propriétés de `Label`. Dans XAML, `Span` objets apparaissent en tant qu’enfants de le `FormattedString` objet.

 Quand une chaîne multiligne est définie sur le `Text` propriété, les caractères de fin de ligne sont convertis en caractères d’espace, mais les caractères de fin de ligne sont conservées quand une chaîne multiligne apparaît en tant que contenu de la `Label` ou `Label.Text` balises :

 [![Capture d’écran de triple des variations de texte partage](images/ch07fg03-small.png "Variations de texte mis en forme")](images/ch07fg03-large.png#lightbox "Variations de texte mis en forme")

## <a name="related-links"></a>Liens connexes

- [Chapitre 7 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Exemples de chapitre 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Chapitre 7 F# exemple](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [Notions de base XAML](~/xamarin-forms/xaml/xaml-basics/index.md)
