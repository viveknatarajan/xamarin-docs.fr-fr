---
title: Extensions de plateforme Xamarin.Essentials
description: Xamarin.Essentials fournit plusieurs méthodes d’extension de plateforme quand vous devez utiliser des types de plateforme comme Rect, Size et Point.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 806fcb3fa90a5ec9d39cecb743b72116b8388a03
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58176023"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials: Extensions de plateforme

Xamarin.Essentials fournit plusieurs méthodes d’extension de plateforme quand vous devez utiliser des types de plateforme comme Rect, Size et Point. Cela signifie que vous pouvez convertir la version `System` de ces types en types spécifiques iOS, Android et UWP. 

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>Utilisation d’extensions de plateforme

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Toutes les extensions de plateforme peuvent uniquement être appelées depuis le projet iOS, Android ou UWP.

### <a name="point"></a>Point

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="size"></a>Size

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>Rectangle

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

## <a name="api"></a>API

- [Code source des convertisseurs](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [Documentation sur les API de convertisseurs de points](xref:Xamarin.Essentials.PointConverters)
- [Documentation de l’API de convertisseurs de rectangles](xref:Xamarin.Essentials.RectangleConverters)
- [Documentation sur les API de convertisseurs de tailles](xref:Xamarin.Essentials.SizeConverters)
