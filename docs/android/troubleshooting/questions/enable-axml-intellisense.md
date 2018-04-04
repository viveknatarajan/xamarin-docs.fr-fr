---
title: Comment faire pour activer Intellisense dans les fichiers .axml Android ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 8278576355299894eab1c7c6d3ceaadb607fe915
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Comment faire pour activer Intellisense dans les fichiers .axml Android ?

Android Intellisense dans Visual Studio a besoin de deux fichiers de schéma XML à fonctionner correctement. Pour rechercher ces fichiers, accédez à ce dossier :

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Précédemment : `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Vous trouverez dans les deux fichiers .xsd :

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio conserve tous les schémas XML dans le dossier respectif :

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Vous pouvez choisir de déplacer ces deux fichiers .xsd à l’emplacement ci-dessus, ou tout simplement ajouter ces schémas dans Visual Studio. Vous pouvez ensuite « ajouter » ces schémas à l’intérieur de Visual Studio via la **XML > schémas > ajouter** boîte de dialogue.






