---
title: "Intégrations d’exemple"
ms.topic: article
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 3b07e457bd54f16826f44a3b913434fea6ebe20b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="sample-integrations"></a>Intégrations d’exemple

## <a name="sample-integrations"></a>Intégrations d’exemple

Consultez le [cuisine récepteur] [ KitchenSink] sample pour obtenir un exemple d’une intégration. Générer simplement `KitchenSink.sln` dans Visual Studio pour Mac ou Visual Studio, puis ouvrez `KitchenSink.workbook`.

[![Capture d’écran de cuisine récepteur d’intégration](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png)

Récepteur de cuisine illustre les deux ensembles de concepts :

* Les éléments de la représentation montrent comment utiliser `RepresentationManager` pour améliorer le rendu en utilisant les représentations sous forme intégrés.
* Le `Person` objet et son convertisseur de JavaScript associé illustrent l’utilisation de `ISerializableObject` sans passer par un fournisseur de représentation.

Consultez également [SkiaSharp] [ skiasharp] pour obtenir un exemple réel d’une intégration qui utilise existants [représentations](~/tools/workbooks/sdk/representations.md) fournis par les classeurs Xamarin pour afficher ses types.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks