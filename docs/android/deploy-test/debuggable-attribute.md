---
title: "Attribut débogable"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Attribut débogable

<a name="Overview" />


Pour permettre le débogage, Android prend en charge Java Debug Wire Protocol (JDWP). Cette technologie permet à des outils tels qu’ADB de communiquer avec une machine virtuelle Java. JDWP est important pendant le développement, mais il doit être désactivé avant la publication de l’application.

JDWP peut être la valeur de l’attribut `android:debuggable` dans une application Android. Xamarin.Android offre les possibilités suivantes pour définir cet attribut :

1.  Création d’un fichier `AndroidManifext.xml` et définition de l’attribut `android:debuggable` dans ce dernier.
1.  Inclusion de `ApplicationAttribute` dans un fichier `.CS`  comme suit : `[assembly: Application(Debuggable=false)]`.


Si `AndroidManifest.xml` et `ApplicationAttribute` sont tous deux présents, le contenu de `AndroidManifest.xml` est prioritaire devant ce qui est spécifié par le `ApplicationAttribute`.

Si ni `AndroidManifest.xml` ni `ApplicationAttribute` ne sont présents, la valeur par défaut de l’attribut `android:debuggable` dépend de la génération ou non de symboles de débogage. S’il y a des symboles de débogage, Xamarin.Android définira l’attribut `android:debuggable` sur `true`.

Notez que la valeur des attributs `android:debuggable` ne dépend PAS nécessairement de la configuration de build. Il est possible que l’attribut `android:debuggable` des versions release soit défini sur true.


## <a name="related-links"></a>Liens associés

- [Applications débogables sur le marché Android](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
