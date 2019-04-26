---
title: Solutions de contournement et problèmes connus
description: Ce document décrit les problèmes connus et solutions de contournement pour les classeurs de Xamarin. Il aborde les problèmes de CultureInfo, les problèmes JSON et bien plus encore.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 221ed97db17da62f513448b6c85d4df205a7cbaf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61268884"
---
# <a name="known-issues--workarounds"></a>Solutions de contournement et problèmes connus

## <a name="persistence-of-cultureinfo-across-cells"></a>Persistance de CultureInfo toutes les cellules

Paramètre `System.Threading.CurrentThread.CurrentCulture` ou `System.Globalization.CultureInfo.CurrentCulture` ne sont pas conservées entre les cellules de classeur sur les cibles des classeurs Mono (Mac, iOS et Android) en raison un [bogue dans Mono `AppContext.SetSwitch` ] [ appcontext-bug] implémentation .

### <a name="workarounds"></a>Solutions

* Définir l’application-domaine local `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Ou mise à jour aux classeurs 1.2.1 ou une version ultérieure, qui sera réécrire les assignations à `System.Threading.CurrentThread.CurrentCulture` et `System.Globalization.CultureInfo.CurrentCulture` pour fournir le comportement souhaité (contourner le bogue Mono).

## <a name="unable-to-use-newtonsoftjson"></a>Impossible d’utiliser Newtonsoft.Json

### <a name="workaround"></a>Solution de contournement

* Mettre à jour vers des classeurs 1.2.1, ce qui va installer Newtonsoft.Json 9.0.1.
  Classeurs 1.3, actuellement dans le canal alpha, prend en charge les versions 10 et versions ultérieures.

### <a name="details"></a>Détails

Newtonsoft.Json 10 a été publiée qui essentiellement sa dépendance vis-à-vis Microsoft.CSharp qui est en conflit avec les classeurs de version est fourni pour prendre en charge `dynamic`. Ce problème est résolu dans la version 1.3 de classeurs, mais pour l’instant, nous avons travaillé contourner ce problème par épinglage Newtonsoft.Json spécifiquement pour la version 9.0.1.

Les packages NuGet explicitement en fonction de Newtonsoft.Json 10 ou version ultérieure sont uniquement pris en charge dans les classeurs 1.3, actuellement dans le canal alpha.

## <a name="code-tooltips-are-blank"></a>Info-bulles de code sont vides

Il existe un [bogue dans l’éditeur Monaco] [ monaco-bug] dans Safari/WebKit, qui est utilisé dans l’application Mac classeurs, qui entraîne le rendu d’info-bulles de code sans texte.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Solution de contournement

* En cliquant sur l’info-bulle quand elle apparaît forcera le texte à afficher.

* Ou mettre à jour vers des classeurs 1.2.1 ou une version ultérieure

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Convertisseurs de SkiaSharp sont manquants dans les classeurs 1.3

À partir de classeurs 1.3, nous avons supprimé les convertisseurs de SkiaSharp que nous avons livrée dans les classeurs 0.99.0, en faveur de SkiaSharp fournissant les convertisseurs lui-même, à l’aide de notre [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Solution de contournement

* Mettre à jour SkiaSharp vers la dernière version de NuGet. Au moment de l’écriture, il s’agit de 1.57.1.

## <a name="related-links"></a>Liens associés

- [Signalement des bogues](~/tools/workbooks/install.md#reporting-bugs)
