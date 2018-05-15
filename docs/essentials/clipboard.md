---
title: Xamarin.Essentials Presse-papiers
description: La classe Presse-papiers permet de copier et coller du texte dans le Presse-papiers système entre les applications.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials Presse-papiers

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **Presse-papiers** classe vous permet de copier et coller du texte dans le Presse-papiers système entre les applications.

## <a name="using-clipboard"></a>À l’aide du Presse-papiers

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour vérifier si le **Presse-papiers** a actuellement prêt à coller du texte :

```csharp
var hasText = Clipboard.HasText;
```

Pour définir le texte la **Presse-papiers**:

```csharp
ClipBoard.SetText("Hello World");
```

Pour lire le texte à partir de la **Presse-papiers**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Code source de Presse-papiers](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Documentation de l’API du Presse-papiers](xref:Xamarin.Essentials.Clipboard)
