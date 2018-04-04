---
title: Localisation des applications et des ressources de type chaîne
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: cfb127500f919b61788087465700dfed213d5eb2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="application-localization-and-string-resources"></a>Localisation des applications et des ressources de type chaîne

Localisation des applications est le fait de fournir des ressources de remplacement pour cibler une région spécifique ou des paramètres régionaux. Par exemple, vous pouvez fournir des chaînes de langue localisée pour différents pays, ou vous pouvez modifier les couleurs ou à la disposition pour faire correspondre les cultures particuliers. Android charge et utiliser les ressources appropriées pour des paramètres régionaux de l’appareil au moment de l’exécution sans modifier le code source.

Par exemple, l’image ci-dessous montre la même application en cours d’exécution dans trois paramètres régionaux de périphérique différent, mais le texte affiché dans chaque bouton est spécifique aux paramètres régionaux qui a la valeur de chaque appareil :

[![Exemples de trois différents paramètres régionaux](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

Dans cet exemple, le contenu d’un fichier de mise en page, **Main.axml** ressemble à ceci :

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

Dans l’exemple ci-dessus, la chaîne du bouton a été chargée à partir des ressources en fournissant l’ID de ressource pour la chaîne :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Chaînes de ressources pour les trois langages](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Chaînes de ressources pour les trois langages](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Localisation d’applications Android

Lecture la [Introduction à la localisation](~/cross-platform/app-fundamentals/localization.md) pour des conseils et des conseils sur la localisation des applications mobiles.

Le [localisation des applications Android](~/android/app-fundamentals/localization.md) guide contient des exemples plus spécifiques sur la façon de convertir les chaînes et de localiser des images à l’aide de Xamarin.Android.



## <a name="related-links"></a>Liens associés

- [Localisation d’applications Android](~/android/app-fundamentals/localization.md)
- [Vue d’ensemble de la localisation d’inter-plateformes](~/cross-platform/app-fundamentals/localization.md)
