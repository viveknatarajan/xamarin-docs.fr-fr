---
title: Langage de balisage d’Application eXtensible (XAML)
description: XAML est un langage de balisage déclaratif qui peut être utilisé pour définir des interfaces utilisateur. L’interface utilisateur est défini dans un fichier XML à l’aide de la syntaxe XAML, tandis que le comportement d’exécution est défini dans un fichier code-behind distinct.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 9924e588808783fe35dbd830bbc9af288f37e7ea
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53813958"
---
# <a name="extensible-application-markup-language-xaml"></a>Langage de balisage d’Application eXtensible (XAML)

_XAML est un langage de balisage déclaratif qui peut être utilisé pour définir des interfaces utilisateur. L’interface utilisateur est défini dans un fichier XML à l’aide de la syntaxe XAML, tandis que le comportement d’exécution est défini dans un fichier code-behind distinct._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016 : Devenir un maître de XAML**

> [!NOTE]
> Essayer le [XAML Standard Preview](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[Notions de base XAML](xaml-basics/index.md)

XAML permet aux développeurs de définir des interfaces utilisateur dans les applications Xamarin.Forms à l’aide du balisage au lieu de code. XAML n’est jamais nécessaire dans un programme de Xamarin.Forms, mais il est compatible avec les outils et il est souvent plus visuellement cohérent et plus concise à code équivalent. XAML est particulièrement bien adapté pour une utilisation avec l’architecture d’application populaires Model-View-ViewModel (MVVM) : XAML définit la vue qui est liée au code du ViewModel par le biais des liaisons de données basées sur XAML.

## <a name="xaml-compilationxamlcmd"></a>[Compilation XAML](xamlc.md)

XAML peut être éventuellement compilé directement en langage intermédiaire (IL) avec le compilateur XAML (XAMLC). Cet article décrit comment utiliser XAMLC et ses avantages.

## <a name="xaml-previewerxaml-previewermd"></a>[Générateur d’aperçu XAML](xaml-previewer.md)

Le [Générateur d’aperçu XAML](~/xamarin-forms/xaml/xaml-previewer.md) affiche un aperçu en direct d’un page côte-à-côte avec le balisage XAML, ce qui vous permet de voir votre interface utilisateur de rendu en cours de frappe.

## <a name="xaml-namespacesnamespacesmd"></a>[Espaces de noms XAML](namespaces.md)

XAML utilise le `xmlns` attribut XML pour les déclarations d’espace de noms. Cet article présente la syntaxe d’espace de noms XAML et montre comment déclarer un espace de noms XAML pour accéder à un type.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Extensions de balisage XAML](markup-extensions/index.md)

XAML inclut des extensions de balisage pour la définition des attributs à des valeurs ou objets au-delà de ce qui peut être exprimé avec des chaînes simples. Ceux-ci incluent le référencement de constantes, propriétés statiques et des champs, des dictionnaires de ressources et des liaisons de données.

## <a name="field-modifiersfield-modifiersmd"></a>[Modificateurs de champ](field-modifiers.md)

Le `x:FieldModifier` namespace (attribut) spécifie le niveau d’accès pour les champs générés pour les éléments XAML nommés.

## <a name="passing-argumentspassing-argumentsmd"></a>[Passage d’arguments](passing-arguments.md)

XAML peut être utilisé pour passer des arguments à des constructeurs non définis par défaut ou aux méthodes de fabrique. Cet article montre comment utiliser des attributs XAML qui peuvent être utilisées pour passer des arguments aux constructeurs, pour appeler des méthodes de fabrique et pour spécifier le type d’un argument générique.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Propriétés pouvant être liées](bindable-properties.md)

Dans Xamarin.Forms, les fonctionnalités des common language runtime (CLR) sont étendue par les propriétés pouvant être liées. Une propriété est un type spécial de propriété, où la valeur de propriété est suivie par le système de propriétés de Xamarin.Forms. Cet article fournit une introduction aux propriétés pouvant être liées et montre comment créer et de les consommer.

## <a name="attached-propertiesattached-propertiesmd"></a>[Propriétés jointes](attached-properties.md)

Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais associé à d’autres objets et reconnaissable dans XAML en tant qu’attribut qui contient une classe et un nom de propriété séparés par un point. Cet article fournit une introduction aux propriétés jointes et montre comment créer et de les consommer.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Dictionnaires de ressources](resource-dictionaries.md)

Les ressources XAML sont des définitions d’objets qui peuvent être utilisés plusieurs fois. Un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) permet aux ressources définies dans un emplacement unique et réutilisées tout au long d’une application Xamarin.Forms. Cet article montre comment créer et consommer un `ResourceDictionary`et comment fusionner `ResourceDictionary` dans un autre.

## <a name="loading-xaml-at-runtimeruntime-loadmd"></a>[Le chargement XAML lors de l’exécution](runtime-load.md)

XAML peut être chargé et analysé lors de l’exécution avec le [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) méthodes d’extension.
