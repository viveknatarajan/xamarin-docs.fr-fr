---
title: XAML Namespace recommandé des préfixes dans Xamarin.Forms
description: La classe XmlnsPrefixAttribute peut être utilisée par les auteurs de contrôle pour spécifier un préfixe recommandé à associer à un espace de noms XAML pour l’utilisation XAML.
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: 33f18b3f9c9ddb6ab31ca92e2f192ffad783ec0c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075092"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML Namespace recommandé des préfixes dans Xamarin.Forms

Le `XmlnsPrefixAttribute` classe peut être utilisée par les auteurs de contrôle pour spécifier un préfixe recommandé à associer à un espace de noms XAML pour l’utilisation XAML. Le préfixe est utile lors de la prise en charge de la sérialisation arborescence des objets pour XAML, ou lors de l’interaction avec un environnement de conception qui a des fonctionnalités d’édition XAML. Exemple :

- Les éditeurs de texte XAML peuvent utiliser le `XmlnsPrefixAttribute` en tant qu’indicateur pour un espace de noms XAML initial `xmlns` mappage.
- Les environnements de conception XAML peuvent utiliser le `XmlnsPrefixAttribute` à ajouter des mappages pour le XAML quand vous faites glisser des objets en dehors d’une boîte à outils et sur une surface de conception visuelle.

Recommandé de préfixes d’espace de noms doivent être définies au niveau de l’assembly avec le `XmlnsPrefixAttribute` constructeur qui accepte deux arguments : une chaîne qui spécifie l’identificateur d’un espace de noms XAML et une chaîne qui spécifie un préfixe recommandé :

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

Préfixes doivent utiliser les chaînes courtes, étant donné que le préfixe est généralement appliqué à tous les éléments sérialisés qui proviennent de l’espace de noms XAML. Par conséquent, la longueur de chaîne de préfixe peut avoir un effet notable sur la taille de la sortie XAML sérialisée.

> [!NOTE]
> Plusieurs `XmlnsPrefixAttribute` peut être appliqué à un assembly. Par exemple, si vous avez un assembly qui définit les types pour plus d’un espace de noms XAML, vous pouvez définir différentes valeurs de préfixes pour chaque espace de noms XAML.

## <a name="related-links"></a>Liens connexes

- [Schémas d’espace de noms personnalisés XAML](custom-namespace-schemas.md)
- [Espaces de noms XAML dans Xamarin.Forms](namespaces.md)
