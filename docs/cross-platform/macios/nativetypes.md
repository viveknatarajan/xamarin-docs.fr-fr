---
title: Types natifs
ms.topic: article
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 09858bd7902b44bbedd96f1be9c9c827131ee16f
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="native-types"></a>Types natifs

Au cœur de la différence, Mac et iOS API utiliser un type de données spécifiques à l’architecture est toujours 32 bits sur les plateformes 32 bits et 64 bits sur les plateformes 64 bits.

Par exemple, Objective-C mappe le `NSInteger` type de données à `int32_t` sur les systèmes 32 bits et en `int64_t` sur les systèmes 64 bits.

Pour correspondre à ce problème, dans notre API unifiée, nous sommes en remplaçant les utilisations précédentes de `int` (qui dans .NET est définie comme étant toujours `System.Int32`) à un type de données : `System.nint`.  Vous pouvez considérer le « n » en ce qui signifie « natifs » pour l’entier natif de type de la plateforme.

Avec ces nouveaux types de données, le même code source est compilé pour 32 bits, 32 bits et 64 bits ou 64 bits, en fonction de vos indicateurs de compilation.

## <a name="new-data-types"></a>Nouveaux types de données

Le tableau suivant indique les modifications de nos types de données pour correspondre à ce nouveau monde 32/64 bits :

|Type natif|type de stockage de 32 bits|type de stockage de 64 bits|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Nous avons choisi de ces noms pour permettre à votre code c# rechercher plus ou moins de la même façon que le résultat doit ressembler aujourd'hui.

### <a name="implicit-and-explicit-conversions"></a>Conversions implicites et explicites

La conception de nouveaux types de données est conçue pour permettre un seul fichier source c# à naturellement utilisent le stockage bits 32 ou 64 selon le plateforme hôte et les paramètres de compilation.

Cette opération requise pour concevoir un ensemble de conversions implicites et explicites vers et depuis les types de données spécifiques aux types de données .NET intégraux et à virgule flottante.

Opérateurs de conversions implicites sont fournis quand il est sans risque de perte de données (valeurs 32 bits qui est stockés sur un espace de 64 bits).

Opérateurs de conversion explicite sont fournies lorsqu’il existe un risque potentiel de perte de données (valeur de 64 bits qui est stocké sur un emplacement de stockage 32 ou potentiellement 32).

 `int`, `uint` et `float` sont toutes implicitement convertible en `nint`, `nuint` et `nfloat` que possible toujours 32 bits en 32 ou 64 bits.

 `nint`, `nuint` et `nfloat` sont toutes implicitement convertible en `long`, `ulong` et `double` comme valeurs de bit 32 ou 64 doit toujours apparaître dans le stockage de 64 bits.

Vous devez utiliser des conversions explicites de `nint`, `nuint` et `nfloat` dans `int`, `uint` et `float` comme 64 bits de stockage peuvent contenir des types natifs.

Vous devez utiliser des conversions explicites de `long`, `ulong` et `double` dans `nint`, `nuint` et `nfloat` étant donné que les types natifs peuvent uniquement être en mesure de contenir 32 bits de stockage.

## <a name="coregraphics-types"></a>Types de CoreGraphics

Les types de données de point, la taille et rectangle qui sont utilisés avec CoreGraphics utilisent 32 ou 64 bits en fonction de l’appareil qu’ils sont exécutent sur.  Lorsque nous liée à l’origine iOS et Mac API nous avons utilisé les structures de données existantes qui se sont produites pour correspondre à la taille de la plateforme hôte (les types de données dans `System.Drawing`).

Lorsque vous déplacez vers **unifié**, vous devez remplacer les instances de `System.Drawing` avec leurs `CoreGraphics` équivalents comme indiqué dans le tableau suivant :

|Ancien Type dans System.Drawing|Nouvelle CoreGraphics de Type de données|Description|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Blocages flottante les informations de point de rectangle.|
|`SizeF`|`CGSize`|Blocages flottante pointent les informations sur la taille (largeur, hauteur)|
|`PointF`|`CGPoint`|Contient une virgule flottante, des points (X, Y)|

Les anciennes données types utilisés valeurs en virgule flottante pour stocker les éléments des structures de données, alors qu’un utilise la nouvelle `System.nfloat`.

## <a name="related-links"></a>Liens associés

- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
