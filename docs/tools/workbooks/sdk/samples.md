---
title: Intégrations d’exemple
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 62d8cf14d6deb9c59297b708221dec10ca009f7f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="sample-integrations"></a>Intégrations d’exemple

Consultez le [cuisine récepteur] [ KitchenSink] sample pour obtenir un exemple d’une intégration. Générer simplement `KitchenSink.sln` dans Visual Studio pour Mac ou Visual Studio, puis ouvrez `KitchenSink.workbook`.

[![Capture d’écran de cuisine récepteur d’intégration](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Récepteur de cuisine illustre les deux ensembles de concepts :

* Les éléments de la représentation montrent comment utiliser `RepresentationManager` pour améliorer le rendu en utilisant les représentations sous forme intégrés.
* Le `Person` objet et son convertisseur de JavaScript associé illustrent l’utilisation de `ISerializableObject` sans passer par un fournisseur de représentation.

Consultez également [SkiaSharp] [ skiasharp] pour obtenir un exemple réel d’une intégration qui utilise existants [représentations](~/tools/workbooks/sdk/representations.md) fournis par les classeurs Xamarin pour afficher ses types.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
