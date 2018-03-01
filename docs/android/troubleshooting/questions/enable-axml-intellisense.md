---
title: "Comment faire pour activer Intellisense dans les fichiers .axml Android ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: ea5c4e9f7e7f1c487d4f53650b10f88afc1b41e6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
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






