---
title: Espaces de noms XAML dans Xamarin.Forms
description: XAML utilise l’attribut XML de xmlns pour les déclarations d’espace de noms. Cet article présente la syntaxe d’espace de noms XAML et montre comment déclarer un espace de noms XAML pour accéder à un type.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: be6154631b8b51ec61feb4c713d925ff30505b7d
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831753"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Espaces de noms XAML dans Xamarin.Forms

_XAML utilise l’attribut XML de xmlns pour les déclarations d’espace de noms. Cet article présente la syntaxe d’espace de noms XAML et montre comment déclarer un espace de noms XAML pour accéder à un type._

## <a name="overview"></a>Vue d'ensemble

Il existe deux déclarations d’espace de noms XAML qui se trouvent toujours dans l’élément racine d’un fichier XAML. Le premier définit l’espace de noms par défaut, comme illustré dans l’exemple de code XAML suivant :

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

L’espace de noms par défaut Spécifie que les éléments définis dans le fichier XAML sans préfixe font référence aux classes de Xamarin.Forms, tel que [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

La seconde déclaration d’espace de noms utilise le `x` de préfixe, comme indiqué dans l’exemple de code XAML suivant :

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML utilise des préfixes pour déclarer des espaces de noms non définis par défaut, avec le préfixe utilisé lors du référencement de types au sein de l’espace de noms. Le `x` déclaration d’espace de noms Spécifie que les éléments définis dans le XAML avec le préfixe `x` sont utilisés pour les éléments et attributs qui font partie intégrante de XAML (en particulier la spécification de XAML 2009).

Le tableau suivant présente le `x` attributs d’espace de noms pris en charge par Xamarin.Forms :

|Construction|Description|
|--- |--- |
|`x:Arguments`|Spécifie les arguments de constructeur pour un constructeur non défini par défaut, ou pour une déclaration d’objet de fabrique (méthode).|
|`x:Class`|Spécifie l’espace de noms et le nom d’une classe définie dans XAML. Le nom de classe doit correspondre au nom de classe du fichier code-behind. Notez que cette construction ne peut apparaître dans l’élément racine d’un fichier XAML.|
|`x:DataType`|Spécifie le type de l’objet auquel l’élément XAML et ses enfants seront lié.|
|`x:FactoryMethod`|Spécifie une méthode de fabrique qui peut être utilisée pour initialiser un objet.|
|`x:FieldModifier`|Spécifie le niveau d’accès pour les champs générés pour les éléments XAML nommés.|
|`x:Key`|Spécifie une clé définie par l’utilisateur unique pour chaque ressource dans un `ResourceDictionary`. La valeur de clé est utilisée pour récupérer la ressource XAML et est généralement utilisée comme argument pour le `StaticResource` extension de balisage.|
|`x:Name`|Spécifie un nom d’objet de runtime pour l’élément XAML. Paramètre `x:Name` est similaire à la déclaration d’une variable dans le code.|
|`x:TypeArguments`|Spécifie les arguments de type générique au constructeur d’un type générique.|

Pour plus d’informations sur la `x:DataType` d’attribut, consultez [liaisons compilé](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md). Pour plus d’informations sur la `x:FieldModifier` d’attribut, consultez [modificateurs de champ](~/xamarin-forms/xaml/field-modifiers.md). Pour plus d’informations sur la `x:Arguments`, `x:FactoryMethod`, et `x:TypeArguments` attributs, consultez [en passant les Arguments dans XAML](~/xamarin-forms/xaml/passing-arguments.md).

Dans XAML, les déclarations d’espace de noms héritent à partir de l’élément parent à l’élément enfant. Par conséquent, lorsque vous définissez un espace de noms dans l’élément racine d’un fichier XAML, tous les éléments dans ce fichier héritent de la déclaration d’espace de noms.

## <a name="declaring-namespaces-for-types"></a>Déclaration des espaces de noms pour les Types

Types peuvent être référencés dans XAML en déclarant un espace de noms XAML avec un préfixe, avec la déclaration d’espace de noms en spécifiant le nom d’espace de noms Common Language Runtime (CLR) et éventuellement un nom d’assembly. Pour cela, vous devez définir les valeurs pour les mots clés suivants dans la déclaration d’espace de noms :

- **clr-namespace :** ou **à l’aide de :** : l’espace de noms CLR déclaré dans l’assembly qui contient les types à exposer en tant qu’éléments XAML. Ce mot clé est obligatoire.
- **assembly =** : l’assembly qui contient l’espace de noms CLR référencé. Cette valeur est le nom de l’assembly, sans l’extension de fichier. Le chemin d’accès à l’assembly doit être établie en tant que référence dans le fichier projet qui contient le fichier XAML faisant référence à l’assembly. Ce mot clé peut être omis si le **clr-namespace** valeur se trouve dans le même assembly que le code d’application qui fait référence à des types.

Notez que le caractère qui sépare le `clr-namespace` ou `using` jeton à partir de sa valeur est un signe deux-points, tandis que le caractère qui sépare le `assembly` jeton à partir de sa valeur est un signe égal. Le caractère à utiliser entre les deux jetons est un point-virgule.

L’exemple de code suivant montre une déclaration d’espace de noms XAML :

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Cela peut également être écrit en tant que :

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

Le `local` préfixe est une convention permettant d’indiquer que les types dans l’espace de noms sont locales à l’application. Ou bien, si les types sont dans un autre assembly, le nom de l’assembly doit être défini également dans la déclaration d’espace de noms, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Le préfixe d’espace de noms est ensuite spécifié lors de la déclaration d’une instance d’un type à partir d’un espace de noms importé, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Pour plus d’informations sur la définition d’un schéma de l’espace de noms personnalisé, consultez [XAML personnalisé Namespace schémas](custom-namespace-schemas.md).

## <a name="summary"></a>Récapitulatif

Cet article a introduit la syntaxe d’espace de noms XAML et vous a montré comment déclarer un espace de noms XAML pour accéder à un type. XAML utilise le `xmlns` attribut XML pour les types et les déclarations d’espace de noms peut être référencée dans XAML en déclarant un espace de noms XAML avec un préfixe.

## <a name="related-links"></a>Liens associés

- [Passage des Arguments dans XAML](~/xamarin-forms/xaml/passing-arguments.md)
