---
title: Les problèmes connus et les solutions de contournement
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 186faf3fc4f93d1c9a4af9e3e9f72afd569fed8b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="known-issues--workarounds"></a>Les problèmes connus et les solutions de contournement

## <a name="persistence-of-cultureinfo-across-cells"></a>Persistance de CultureInfo toutes les cellules

Paramètre `System.Threading.CurrentThread.CurrentCulture` ou `System.Globalization.CultureInfo.CurrentCulture` n’est pas conservé entre les cellules du classeur sur les cibles des classeurs Mono (Mac, iOS et Android) en raison un [bogue dans du Mono `AppContext.SetSwitch` ] [ appcontext-bug] mise en œuvre .

### <a name="workarounds"></a>Solutions

* Définir l’application-domaine local `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Ou mise à jour vers des classeurs 1.2.1 ou plus récent, qui réécrit affectations aux `System.Threading.CurrentThread.CurrentCulture` et `System.Globalization.CultureInfo.CurrentCulture` pour fournir le comportement souhaité (travail autour du bogue Mono).

## <a name="unable-to-use-newtonsoftjson"></a>Impossible d’utiliser Newtonsoft.Json

### <a name="workaround"></a>Solution de contournement

* Mettre à jour vers des classeurs 1.2.1, qui va installer Newtonsoft.Json 9.0.1.
  Classeurs 1.3, actuellement dans le canal alpha, prend en charge les versions 10 et ultérieures.

### <a name="details"></a>Détails

Newtonsoft.Json 10 a été publié qui a sa dépendance Microsoft.CSharp qui est en conflit avec les classeurs de version est fourni pour prendre en charge `dynamic`. Ce problème est résolu dans la version 1.3 de classeurs, mais pour l’instant, nous avons travaillé contourner ce problème en épingle Newtonsoft.Json spécifiquement à version9.0.1.

Les packages NuGet explicitement selon Newtonsoft.Json 10 ou une version ultérieure sont uniquement pris en charge dans les classeurs 1.3, actuellement dans le canal alpha.

## <a name="code-tooltips-are-blank"></a>Info-bulles de code sont vides

Il existe un [bogue dans l’éditeur Monaco] [ monaco-bug] dans Safari/WebKit, qui est utilisé dans l’application Mac classeurs, qui entraîne le rendu des info-bulles de code sans texte.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Solution de contournement

* En cliquant sur l’info-bulle quand elle apparaît force le texte à afficher.

* Ou mettre à jour vers des classeurs 1.2.1 ou une version ultérieure

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Convertisseurs de SkiaSharp sont manquants dans les classeurs 1.3

À partir de classeurs 1.3, nous avons supprimé les convertisseurs SkiaSharp que nous avons livrée dans les classeurs 0.99.0, en faveur de SkiaSharp fournissant les convertisseurs lui-même, à l’aide de notre [SDK] [/ guides/cross-plateforme/classeurs/sdk /].

### <a name="workaround"></a>Solution de contournement

* Mettre à jour SkiaSharp vers la dernière version dans NuGet. Au moment de l’écriture, il s’agit de 1.57.1.

## <a name="related-links"></a>Liens associés

- [Signalement des bogues](~/tools/workbooks/install.md#reporting-bugs)
