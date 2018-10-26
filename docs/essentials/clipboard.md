---
title: 'Xamarin.Essentials : Presse-papiers'
description: La classe Clipboard permet de copier et coller du texte dans le presse-papiers système, partagé entre les applications.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842613"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials : Presse-papiers

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Clipboard** permet de copier et coller du texte dans le presse-papiers système, partagé entre les applications.

## <a name="using-clipboard"></a>Utilisation de **Clipboard**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour vérifier si le **Presse-papiers** a actuellement prêt à coller du texte :

```csharp
var hasText = Clipboard.HasText;
```

Pour définir le texte du **Presse-papiers**:

```csharp
Clipboard.SetText("Hello World");
```

Pour lire le texte à partir du **Presse-papiers**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Code source de Presse-papiers](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Documentation de l’API du Presse-papiers](xref:Xamarin.Essentials.Clipboard)
