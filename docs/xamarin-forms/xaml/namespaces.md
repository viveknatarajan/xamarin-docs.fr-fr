---
title: Espaces de noms XAML
description: "XAML utilise l’attribut XML xmlns pour les déclarations d’espace de noms. Cet article présente la syntaxe d’espace de noms XAML et montre comment déclarer un espace de noms XAML pour un type d’accès."
ms.topic: article
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2017
ms.openlocfilehash: 55b83151e9c345096aeb0bfdd686d50c5fde62fd
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="xaml-namespaces"></a>Espaces de noms XAML

_XAML utilise l’attribut XML xmlns pour les déclarations d’espace de noms. Cet article présente la syntaxe d’espace de noms XAML et montre comment déclarer un espace de noms XAML pour un type d’accès._

## <a name="overview"></a>Vue d'ensemble

Il existe deux déclarations d’espace de noms XAML qui se trouvent toujours dans l’élément racine d’un fichier XAML. Le premier définit l’espace de noms par défaut, comme indiqué dans l’exemple de code XAML suivant :

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

L’espace de noms par défaut Spécifie que les éléments définis dans le fichier XAML sans le préfixe font référence aux classes de Xamarin.Forms, tel que [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

La seconde déclaration d’espace de noms utilise le `x` de préfixe, comme indiqué dans l’exemple de code XAML suivant :

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML utilise des préfixes pour déclarer des espaces de noms par défaut, avec le préfixe utilisé lors du référencement de types dans l’espace de noms. Le `x` déclaration d’espace de noms Spécifie que les éléments définis dans le code XAML avec le préfixe `x` sont utilisés pour les éléments et attributs qui font partie intégrante XAML (en particulier la spécification de XAML 2009).

Le tableau suivant décrit les `x` attributs d’espace de noms pris en charge par Xamarin.Forms :

|Construction|Description|
|--- |--- |
|`x:Arguments`|Spécifie les arguments de constructeur pour un constructeur par défaut, ou pour une déclaration d’objet de fabrique (méthode).|
|`x:Class`|Spécifie l’espace de noms et le nom d’une classe définie en XAML. Le nom de classe doit correspondre au nom de classe du fichier code-behind. Notez que cette construction ne peut apparaître dans l’élément racine d’un fichier XAML.|
|`x:FactoryMethod`|Spécifie une méthode de fabrique qui peut être utilisée pour initialiser un objet.|
|`x:Key`|Spécifie une clé unique définie par l’utilisateur pour chaque ressource dans un `ResourceDictionary`. La valeur de clé est utilisée pour récupérer la ressource XAML et est généralement utilisée comme argument pour le `StaticResource` extension de balisage.|
|`x:Name`|Spécifie un nom d’objet de runtime pour l’élément XAML. Paramètre `x:Name` est similaire à la déclaration d’une variable dans le code.|
|`x:TypeArguments`|Spécifie les arguments de type générique au constructeur d’un type générique.|

Pour plus d’informations sur la `x:Arguments`, `x:FactoryMethod`, et `x:TypeArguments` attributs, consultez [en passant les Arguments en XAML](~/xamarin-forms/xaml/passing-arguments.md).

En XAML, les déclarations d’espace de noms héritent d’élément parent à l’élément enfant. Par conséquent, lorsque vous définissez un espace de noms dans l’élément racine d’un fichier XAML, tous les éléments dans le fichier héritent de la déclaration d’espace de noms.

## <a name="declaring-namespaces-for-types"></a>Déclarer des espaces de noms pour les Types

Types peuvent être référencés en XAML en déclarant un espace de noms XAML avec un préfixe, avec la déclaration d’espace de noms en spécifiant le nom d’espace de noms Common Language Runtime (CLR) et éventuellement un nom d’assembly. Pour cela, vous devez définir les valeurs pour les mots clés suivants dans la déclaration d’espace de noms :

- **espace de noms CLR :** ou **à l’aide de :** : l’espace de noms CLR déclaré dans l’assembly qui contient les types à exposer en tant qu’éléments XAML. Ce mot clé est requis.
- **assembly =** : l’assembly qui contient l’espace de noms CLR référencé. Cette valeur est le nom de l’assembly, sans l’extension de fichier. Le chemin d’accès à l’assembly doit être établie en tant que référence dans le fichier de projet qui contient le fichier XAML qui référence l’assembly. Ce mot clé peut être omis si le **clr-namespace** valeur se trouve dans le même assembly que le code d’application qui référence les types.

Notez que le caractère qui sépare le `clr-namespace` ou `using` jeton à partir de sa valeur est un signe deux-points, alors que le caractère qui sépare le `assembly` jeton à partir de sa valeur est un signe égal. Le caractère à utiliser entre les deux jetons est un point-virgule.

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

Le `local` préfixe est une convention utilisée pour indiquer que les types dans l’espace de noms sont locales à l’application. Vous pouvez également les types sont dans un autre assembly, le nom de l’assembly doit également être défini dans la déclaration d’espace de noms, comme illustré dans l’exemple de code XAML suivant :

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

## <a name="summary"></a>Récapitulatif

Cet article a introduit la syntaxe d’espace de noms XAML et vous a montré comment déclarer un espace de noms XAML pour un type d’accès. XAML utilise le `xmlns` attribut XML pour les types et les déclarations d’espace de noms peut être référencée dans XAML en déclarant un espace de noms XAML avec un préfixe.


## <a name="related-links"></a>Liens associés

- [Passage des Arguments en XAML](~/xamarin-forms/xaml/passing-arguments.md)
