---
title: Localisation d’applications et ressources de type chaîne
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: d9d90e371199c8587d61199240523cf0a23f5efd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013278"
---
# <a name="application-localization-and-string-resources"></a>Localisation d’applications et ressources de type chaîne

Localisation d’applications consiste à fournir des ressources de remplacement pour cibler une région spécifique ou des paramètres régionaux. Par exemple, vous pouvez fournir des chaînes de langue localisée pour différents pays, ou vous pouvez modifier les couleurs ou la mise en page pour faire correspondre les cultures particuliers. Android charge et utiliser les ressources correspondant aux paramètres régionaux de l’appareil au moment de l’exécution sans aucune modification au code source.

Par exemple, l’image ci-dessous montre la même application en cours d’exécution dans les trois paramètres régionaux de périphérique différent, mais le texte affiché dans chaque bouton est spécifique aux paramètres régionaux que chaque périphérique est défini sur :

[![Exemples de trois différents paramètres régionaux](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

Dans cet exemple, le contenu d’un fichier de disposition, **Main.axml** ressemble à ceci :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

Dans l’exemple ci-dessus, la chaîne pour le bouton a été chargée à partir des ressources en fournissant l’ID de ressource pour la chaîne :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Chaînes de ressources dans trois langues](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Chaînes de ressources dans trois langues](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Localisation d’applications Android

Lire le [Introduction à la localisation](~/cross-platform/app-fundamentals/localization.md) pour des conseils et des conseils sur la localisation des applications mobiles.

Le [localisation des applications Android](~/android/app-fundamentals/localization.md) guide contient des exemples plus spécifiques sur la façon de convertir les chaînes et de localiser des images à l’aide de Xamarin.Android.



## <a name="related-links"></a>Liens associés

- [Localisation d’applications Android](~/android/app-fundamentals/localization.md)
- [Vue d’ensemble de la localisation inter-plateformes](~/cross-platform/app-fundamentals/localization.md)
