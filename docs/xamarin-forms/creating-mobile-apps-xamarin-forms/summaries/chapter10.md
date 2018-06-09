---
title: Résumé du chapitre 10. Extensions de balisage XAML
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé chapitre 10. Extensions de balisage XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cc6c3154b7e6535fa7528032fb7a91ad90a0a7f8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241098"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Résumé du chapitre 10. Extensions de balisage XAML

Normalement, l’analyseur XAML convertit une chaîne définie comme valeur d’attribut sur le type de la propriété en fonction de conversion de types de données .NET de base, standard ou un [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) dérivé attachée à la propriété ou de son type avec un [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Mais il est parfois commode définir un attribut d’une autre source, par exemple, un élément dans un dictionnaire, ou la valeur d’une propriété statique ou un champ, ou à partir d’un calcul quelconque.

Il s’agit le travail d’un *extension de balisage XAML*. Malgré son nom, les extensions de balisage XAML sont *pas* une extension au format XML. XAML est toujours conforme XML.

## <a name="the-code-infrastructure"></a>L’infrastructure de code

Une extension de balisage XAML est une classe qui implémente le [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) interface. Cette classe a souvent le mot `Extension` à la fin de son nom, mais apparaît habituellement dans XAML sans ce suffixe.

Les extensions de balisage XAML suivantes sont pris en charge par toutes les implémentations du code XAML :

- `x:Static` Prise en charge par [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` Prise en charge par [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` Prise en charge par [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` Prise en charge par [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` Prise en charge par [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Ces quatre extensions de balisage XAML sont prises en charge par de nombreuses implémentations du langage XAML, y compris Xamarin.Forms :

- `StaticResource` Prise en charge par [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` Prise en charge par [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` prise en charge par [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;abordés dans [chapitre 16. Liaison de données](#chapter16)
- `TemplateBinding` prise en charge par [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;non traitées dans l’annuaire

Une extension de balisage XAML supplémentaire est incluse dans Xamarin.Forms par [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;non traitées dans l’annuaire

## <a name="accessing-static-members"></a>L’accès aux membres statiques

Utilisez le [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) élément pour définir un attribut à la valeur d’un membre énumération, champ ou propriété statique publique. Définir le [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) propriété pour le membre statique. Il est généralement plus facile de spécifier `x:Static` et le nom du membre de celui-ci entre accolades. Le nom de la `Member` propriété ne doit pas être inclus, uniquement le membre lui-même. Cette syntaxe courante est indiquée dans le [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) exemple. Les champs statiques sont définis dans le [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) classe. Cette technique vous permet d’établir les constantes utilisées par un programme.

Avec une déclaration d’espace de noms XML supplémentaire, vous pouvez référencer le propriétés statiques publiques, des champs ou des membres de l’énumération définies dans le .NET framework, comme illustré dans le [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) exemple .

## <a name="resource-dictionaries"></a>Dictionnaires de ressources

Le `VisualElement` classe définit une propriété nommée [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) que vous pouvez définir pour un objet de type [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). En XAML, vous pouvez stocker les éléments dans ce dictionnaire et les identifier avec le `x:Key` attribut. Les éléments stockés dans le dictionnaire de ressources sont partagés entre toutes les références à l’élément.

### <a name="staticresource-for-most-purposes"></a>StaticResource pour la plupart des cas

Dans la plupart des cas, vous utiliserez le [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) extension de balisage pour faire référence à un élément du dictionnaire de ressources, comme illustré dans le [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) exemple . Vous pouvez utiliser un `StaticResourceExtension` élément ou `StaticResource` entre accolades :

[![Capture d’écran de triple de partage des ressources](images/ch10fg03-small.png "partage des ressources")](images/ch10fg03-large.png#lightbox "partage des ressources")

Ne confondez pas la `x:Static` extension de balisage et le `StaticResource` extension de balisage.

### <a name="a-tree-of-dictionaries"></a>Une arborescence des dictionnaires

Lorsque l’analyseur XAML rencontre un `StaticResource`, il commence la recherche vers le haut l’arborescence visuelle pour une clé correspondante et recherche les `ResourceDictionary` dans l’application `App` classe. Ainsi, les éléments dans un dictionnaire de ressources plus bas dans l’arborescence d’éléments visuels pour remplacer un dictionnaire de ressources plus haut dans l’arborescence d’éléments visuels. Cela est illustré dans le [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) exemple.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource à des fins spéciales

Le `StaticResource` extension de balisage provoque un élément à récupérer à partir du dictionnaire lorsqu’une arborescence d’éléments visuels est généré pendant la `InitializeComponent` appeler. Une alternative à `StaticResource` est [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), qui conserve un lien vers la clé du dictionnaire et met à jour la cible lors de l’élément référencé par les modifications de clé.

La différence entre `StaticResource` et `DynamicResource` est illustré dans le [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) exemple.

Une propriété définie par `DynamicResource` doit reposer sur une propriété comme indiqué dans [chapitre 11, l’infrastructure peut être liée](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Extensions de balisage de plus petite utilisée

Utilisez le [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) extension de balisage pour définir une propriété `null`.

Utilisez le [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) extension de balisage à une propriété .NET `Type` objet.

Utilisez [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) pour définir un tableau. Spécifiez le type des membres du groupe en définissant le [`Type`] propriété un `x:Type` extension de balisage.

## <a name="a-custom-markup-extension"></a>Une extension de balisage personnalisée

Vous pouvez créer vos propres extensions de balisage XAML en écrivant une classe qui implémente le [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) de l’interface avec un [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) (méthode).

Le [ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) classe satisfait ces exigences. Il crée une valeur de type `Color` en fonction des valeurs de propriétés nommées `H`, `S`, `L`, et `A`. Cette classe est le premier élément dans une bibliothèque de Xamarin.Forms nommée [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) qui est développé et utilisé au cours de cette documentation.

Le [ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) exemple montre comment faire référence à cette bibliothèque et d’utiliser l’extension de balisage personnalisée.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Exemples de chapitre 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
