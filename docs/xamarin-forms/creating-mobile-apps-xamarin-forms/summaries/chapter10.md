---
title: Résumé du chapitre 10. Extensions de balisage XAML
description: 'Création d’applications mobiles avec Xamarin.Forms : Résumé du chapitre 10. Extensions de balisage XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 076e9f5155492e5a69d906c587b24495fe39d3f1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672636"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Résumé du chapitre 10. Extensions de balisage XAML

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

Normalement, l’analyseur XAML convertit n’importe quelle chaîne définie comme valeur d’attribut sur le type de la propriété en fonction de conversion des types de données .NET de base, standard ou un [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) dérivé est attaché à la propriété ou son type avec un [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Mais il est parfois utile de définir un attribut à partir d’une autre source, par exemple, un élément dans un dictionnaire, ou la valeur d’une propriété statique ou d’un champ, ou d’un calcul quelconque.

Il s’agit du travail d’un *extension de balisage XAML*. Malgré son nom, les extensions de balisage XAML sont *pas* une extension au format XML. XAML est toujours juridique XML.

## <a name="the-code-infrastructure"></a>L’infrastructure de code

Une extension de balisage XAML est une classe qui implémente le [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) interface. Une telle classe comporte souvent le mot `Extension` à la fin de son nom, mais s’affiche généralement dans XAML sans ce suffixe.

Les extensions de balisage XAML suivantes sont prises en charge par toutes les implémentations de XAML :

- `x:Static` prise en charge par [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` prise en charge par [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` prise en charge par [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` prise en charge par [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` prise en charge par [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

Ces quatre extensions de balisage XAML sont prises en charge par de nombreuses implémentations du XAML, y compris Xamarin.Forms :

- `StaticResource` prise en charge par [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` prise en charge par [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` prise en charge par [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;abordés dans [chapitre 16. Liaison de données](chapter16.md)
- `TemplateBinding` prise en charge par [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;non traitées dans le livre

Une extension de balisage XAML supplémentaire est incluse dans Xamarin.Forms en lien avec [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;non couvertes dans le livre

## <a name="accessing-static-members"></a>L’accès aux membres statiques

Utilisez le [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) élément à définir un attribut à la valeur d’un membre de propriété, champ ou énumération statique publique. Définir le [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) propriété pour le membre statique. Il est généralement plus facile de spécifier `x:Static` et le nom du membre entre accolades. Le nom de la `Member` propriété ne doit pas être inclus, juste le membre lui-même. Cette syntaxe courante est indiquée dans le [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) exemple. Les champs statiques eux-mêmes sont définis dans le [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) classe. Cette technique vous permet d’établir des constantes utilisées via un programme.

Avec une déclaration d’espace de noms XML supplémentaire, vous pouvez référencer des propriétés statiques publiques, des champs ou des membres de l’énumération définies dans le .NET framework, en particulier comme illustré dans le [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) exemple .

## <a name="resource-dictionaries"></a>Dictionnaires de ressources

Le `VisualElement` classe définit une propriété nommée [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) que vous pouvez définir pour un objet de type [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Dans XAML, vous pouvez stocker les éléments dans ce dictionnaire et les identifier avec le `x:Key` attribut. Les éléments stockés dans le dictionnaire de ressources sont partagées entre toutes les références à l’élément.

### <a name="staticresource-for-most-purposes"></a>StaticResource pour la plupart des cas

Dans la plupart des cas, vous utiliserez le [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) extension de balisage pour référencer un élément du dictionnaire de ressources, comme illustré dans le [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) exemple . Vous pouvez utiliser un `StaticResourceExtension` élément ou `StaticResource` entre accolades :

[![Capture d’écran triple de partage des ressources](images/ch10fg03-small.png "partage des ressources")](images/ch10fg03-large.png#lightbox "partage des ressources")

Ne confondez pas la `x:Static` extension de balisage et le `StaticResource` extension de balisage.

### <a name="a-tree-of-dictionaries"></a>Une arborescence de dictionnaires

Lorsque l’analyseur XAML rencontre un `StaticResource`, il commence la recherche vers le haut l’arborescence visuelle pour une clé correspondante et recherche les `ResourceDictionary` dans l’application `App` classe. Cela permet des éléments dans un dictionnaire de ressources plus en détail l’arborescence visuelle pour remplacer un dictionnaire de ressources plus haut dans l’arborescence visuelle. Cela est illustré dans le [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) exemple.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource à des fins spéciales

Le `StaticResource` extension de balisage provoque un élément à récupérer à partir du dictionnaire lors d’une arborescence d’éléments visuels est généré pendant la `InitializeComponent` appeler. Une alternative à `StaticResource` est [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), qui maintient un lien vers la clé de dictionnaire et met à jour la cible lors de l’élément référencé par les principales modifications apportées.

La différence entre `StaticResource` et `DynamicResource` est illustrée dans le [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) exemple.

Une propriété définie par `DynamicResource` doit reposer sur une propriété pouvant être liée comme indiqué dans [chapitre 11, infrastructure bindable](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Extensions de balisage moins utilisée

Utilisez le [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) extension de balisage pour définir une propriété `null`.

Utilisez le [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) extension de balisage pour définir une propriété à un .NET `Type` objet.

Utilisez [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) pour définir un tableau. Spécifiez le type des membres du groupe en définissant le [`Type`] propriété à un `x:Type` extension de balisage.

## <a name="a-custom-markup-extension"></a>Une extension de balisage personnalisée

Vous pouvez créer vos propres extensions de balisage XAML en écrivant une classe qui implémente le [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) créent une interface avec un [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) (méthode).

Le [ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) classe satisfait ces exigences. Il crée une valeur de type `Color` en fonction des valeurs de propriétés nommées `H`, `S`, `L`, et `A`. Cette classe est le premier élément dans une bibliothèque Xamarin.Forms nommée [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) qui est développée et utilisé au cours de ce livre.

Le [ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) exemple montre comment référencer cette bibliothèque et d’utiliser l’extension de balisage personnalisée.

## <a name="related-links"></a>Liens connexes

- [Chapitre 10 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Chapitre 10 échantillons](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [Extensions de balisage XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
