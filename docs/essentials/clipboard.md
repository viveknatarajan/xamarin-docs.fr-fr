---
title: Xamarin.Essentials Presse-papiers
description: La classe Presse-papiers permet de copier et coller du texte dans le Presse-papiers système entre les applications.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 218f90746f130f02c47040a9191b1001fa80c203
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
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

- [Code source de Presse-papiers](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [Documentation de l’API du Presse-papiers](xref:Xamarin.Essentials.Clipboard)
