---
title: 'Xamarin.Essentials : le Presse-papiers'
description: Ce document décrit la classe du Presse-papiers dans Xamarin.Essentials, ce qui vous permet de copier et coller du texte dans le Presse-papiers système entre les applications.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782357"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials : le Presse-papiers

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
Clipboard.SetText("Hello World");
```

Pour lire le texte à partir de la **Presse-papiers**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Code source de Presse-papiers](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Documentation de l’API du Presse-papiers](xref:Xamarin.Essentials.Clipboard)
