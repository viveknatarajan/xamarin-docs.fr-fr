---
title: Types natifs pour iOS et macOS
description: Ce document décrit comment Unified API de Xamarin mappe les types .NET des types natifs 32 bits et 64 bits, en fonction des besoins en fonction de l’architecture cible de compilation.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199627"
---
# <a name="native-types-for-ios-and-macos"></a>Types natifs pour iOS et macOS

Mac et iOS API utilisent les types de données spécifiques à l’architecture sont toujours 32 bits sur les plateformes 32 bits et 64 bits sur les plateformes 64 bits.

Par exemple, Objective-C mappe le `NSInteger` type de données à `int32_t` sur les systèmes 32 bits et en `int64_t` sur les systèmes 64 bits.

Pour faire correspondre ce comportement, sur notre API unifiée, nous sommes en remplaçant les utilisations précédentes de `int` (qui, dans .NET est défini comme étant toujours `System.Int32`) en un nouveau type de données : `System.nint`. Vous pouvez considérer le « n » comme signifiant « native », afin du natif type entier de la plateforme.

Avec ces nouveaux types de données, le même code source est compilé pour les architectures 32 bits et 64 bits, en fonction de vos indicateurs de compilation.

## <a name="new-data-types"></a>Nouveaux types de données

Le tableau suivant indique les modifications de nos types de données pour faire correspondre ce nouveau monde 32/64 bits :

|Type natif|type de sauvegarde de 32 bits|type de sauvegarde de 64 bits|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Nous avons choisi ces noms à autoriser votre C# code pour rechercher plus ou moins la même façon que le résultat doit être dès aujourd'hui.

### <a name="implicit-and-explicit-conversions"></a>Conversions implicites et explicites

La conception de nouveaux types de données est conçue pour permettre une seule C# fichier source à utiliser naturellement de 32 ou 64 bits le stockage en fonction de la plateforme hôte et les paramètres de compilation.

Cette opération requise permis de concevoir un ensemble de conversions implicites et explicites vers et depuis les types de données spécifiques aux types de données intégraux et à virgule flottante .NET.

Opérateurs de conversions implicites sont fournies lorsqu’il n’existe aucun risque de perte de données (stockage sur un espace de 64 bits de valeurs 32 bits).

Opérateurs de conversions explicites sont fournies lorsqu’il existe un risque potentiel de perte de données (valeur de 64 bits sont stockée sur un emplacement de stockage 32 ou potentiellement 32).

 `int`, `uint` et `float` sont toutes implicitement convertible en `nint`, `nuint` et `nfloat` comme 32 bits seront toujours tenir dans 32 ou 64 bits.

 `nint`, `nuint` et `nfloat` sont toutes implicitement convertible en `long`, `ulong` et `double` comme valeurs de bit 32 ou 64 sont toujours ajusté dans le stockage de 64 bits.

Vous devez utiliser des conversions explicites de `nint`, `nuint` et `nfloat` dans `int`, `uint` et `float` dans la mesure où les types natifs peuvent contenir 64 bits du stockage.

Vous devez utiliser des conversions explicites de `long`, `ulong` et `double` dans `nint`, `nuint` et `nfloat` étant donné que les types natifs peuvent uniquement être en mesure de contenir 32 bits de stockage.

## <a name="coregraphics-types"></a>Types de CoreGraphics

Les types de données point, la taille et rectangle qui sont utilisés avec CoreGraphics utilisent 32 ou 64 bits en fonction de l’appareil sur que lequel ils s’exécutent.  Lorsque nous lié à l’origine iOS et Mac API nous avons utilisé des structures de données existantes qui se sont produits pour faire correspondre les tailles de la plateforme hôte (les types de données dans `System.Drawing`).

Lors du déplacement vers **unifiés**, vous devez remplacer les instances de `System.Drawing` avec leurs `CoreGraphics` équivalents, comme indiqué dans le tableau suivant :

|Ancien Type de System.Drawing|Nouvelle CoreGraphics de Type de données|Description|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Blocages flottante des informations de point de rectangle.|
|`SizeF`|`CGSize`|Les informations de taille (largeur, hauteur) du point de blocages flottante|
|`PointF`|`CGPoint`|Contient des informations (X, Y) du point de virgule flottante|

Les valeurs de types utilisés données anciennes float pour stocker les éléments des structures de données, tandis que le nouvel une utilise `System.nfloat`.

## <a name="related-links"></a>Liens associés

- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
