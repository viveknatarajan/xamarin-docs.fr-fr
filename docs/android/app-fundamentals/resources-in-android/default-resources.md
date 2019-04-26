---
title: Ressources par défaut
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 20865b71cce16f57b84a1c54986bd84180d3e190
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013220"
---
# <a name="default-resources"></a>Ressources par défaut

Ressources par défaut sont des éléments qui ne sont pas spécifiques à un périphérique particulier ou d’un facteur de forme et sont donc le choix par défaut par le système d’exploitation Android si non plus spécifique les ressources sont disponibles. Par conséquent, ils sont le type le plus courant de ressource à créer. Ils sont organisés dans les sous-répertoires de la **ressources** répertoire en fonction de leur type de ressource :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Fichiers de ressources par défaut](default-resources-images/01-resource-files-vs.png)

Dans l’image ci-dessus, le projet comporte des valeurs par défaut pour les ressources drawable, des dispositions et des valeurs (fichiers XML qui contiennent des valeurs simples).

Vous trouverez ci-dessous une liste complète des types de ressources :

-  **animation** &ndash; fichiers XML qui décrivent les animations de propriété.
   Animations de propriété ont été introduits dans le niveau d’API 11 (Android 3.0) et fournit de l’animation de propriétés sur un objet. Animations de propriété sont un moyen plus souple et puissant pour décrire les animations sur n’importe quel type d’objet.

-  **chez** &ndash; fichiers XML qui décrivent *interpolation* animations. Les animations d’interpolation sont une série d’instructions d’animation pour effectuer des transformations sur le contenu d’une rotation d’objet, ou par exemple, afficher une image ou augmente la taille du texte. Interpolation animations sont limitées à afficher uniquement les objets.

-  **couleur** &ndash; fichiers XML qui décrivent une liste de l’état de couleurs. Pour comprendre les listes d’état de couleur, considérez un widget d’interface utilisateur tel qu’un bouton.
   Elle peut avoir différents états tel qu’activé ou désactivé, et le bouton peut changer de couleur avec chaque modification de l’état. La liste est exprimée dans une liste d’état.

-  **drawable** &ndash; ressources Drawable sont un concept général pour les graphiques qui peut être compilé dans l’application puis accessible par les appels d’API ou référencé par d’autres ressources XML.
   Quelques exemples de drawables sont des fichiers bitmap (.png, .gif, .jpg), les bitmaps redimensionnables spéciales appelés [neuf-correctifs](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), état énumère les formes génériques définis dans XML, etc.,.
 
-  **disposition** &ndash; fichiers XML qui décrivent une disposition de l’interface utilisateur, par exemple une activité ou une ligne dans une liste.

-  **menu** &ndash; fichiers XML qui décrivent les menus de l’application tel que *Options Menus*, *Menus contextuels*, et *sous-menus*. Pour obtenir un exemple de menus, consultez le [démonstration de Menu contextuel](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) ou [contrôles Standard](https://developer.xamarin.com/samples/mobile/StandardControls/) exemple.

-  **brutes** &ndash; des fichiers arbitraires qui sont enregistrés sous leur forme binaire brut. Ces fichiers sont compilés dans une application Android dans un format binaire.

-  **valeurs** &ndash; fichiers XML qui contiennent des valeurs simples. Un fichier XML dans le répertoire de valeurs ne définit pas une seule ressource, mais au lieu de cela peut définir plusieurs ressources. Par exemple un fichier XML peut contenir une liste de valeurs de chaîne, tandis qu’un autre fichier XML peut contenir une liste de valeurs de couleur.

-  **XML** &ndash; les fichiers XML sont similaires au niveau de la fonction aux fichiers de configuration .NET. Il s’agit d’un XML arbitraire qui peut être lue par l’application en cours d’exécution.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Fichiers de ressources par défaut](default-resources-images/01-resource-files-xs.png)

Dans l’image ci-dessus, le projet comporte des valeurs par défaut pour les ressources drawable, des dispositions et des valeurs (fichiers XML qui contiennent des valeurs simples).

Vous trouverez ci-dessous une liste complète des types de ressources :

-  **animation** &ndash; fichiers XML qui décrivent les animations de propriété.
   Animations de propriété ont été introduits dans le niveau d’API 11 (Android 3.0) et fournit de l’animation de propriétés sur un objet. Animations de propriété sont un moyen plus souple et puissant pour décrire les animations sur n’importe quel type d’objet.

-  **chez** &ndash; fichiers XML qui décrivent *interpolation* animations. Les animations d’interpolation sont une série d’instructions d’animation pour effectuer des transformations sur le contenu d’une rotation d’objet, ou par exemple, afficher une image ou augmente la taille du texte. Interpolation animations sont limitées à afficher uniquement les objets.

-  **couleur** &ndash; fichiers XML qui décrivent une liste de l’état de couleurs. Pour comprendre les listes d’état de couleur, considérez un widget d’interface utilisateur tel qu’un bouton.
   Il peut être avoir différents états tel qu’activé ou désactivé, et le bouton peut changer de couleur avec chaque modification de l’état. La liste est exprimée dans une liste d’état.

-  **police** &ndash; à partir de niveau d’API 26, il est possible d’incorporer des polices en tant que ressource dans une application Android. Le 26 de bibliothèque de prise en charge sera rétroporter les polices au niveau de l’API 14. L’incorporation de polices permet aux applications charger des polices personnalisées directement à partir de dispositions XML sans avoir à les importer en tant que ressources avant utilisation.

-  **mipmap** &ndash; ressources Drawable sont un concept général pour les graphiques qui peut être compilé dans l’application puis accessible par les appels d’API ou référencé par d’autres ressources XML.
   Quelques exemples de drawables sont des fichiers bitmap (.png, .gif, .jpg), les bitmaps redimensionnables spéciales appelés [neuf-correctifs](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), état énumère les formes génériques définis dans XML, etc.,.

-  **disposition** &ndash; fichiers XML qui décrivent une disposition de l’interface utilisateur, par exemple une activité ou une ligne dans une liste.

-  **menu** &ndash; fichiers XML qui décrivent les menus de l’application tel que *Options Menus*, *Menus contextuels*, et *sous-menus*. Pour obtenir un exemple de menus, consultez le [démonstration de Menu contextuel](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) ou [contrôles Standard](https://developer.xamarin.com/samples/mobile/StandardControls/) exemple.

-  **brutes** &ndash; des fichiers arbitraires qui sont enregistrés sous leur forme binaire brut. Ces fichiers sont compilés dans une application Android dans un format binaire.

-  **valeurs** &ndash; fichiers XML qui contiennent des valeurs simples. Un fichier XML dans le répertoire de valeurs ne définit pas une seule ressource, mais au lieu de cela peut définir plusieurs ressources. Par exemple un fichier XML peut contenir une liste de valeurs de chaîne, tandis qu’un autre fichier XML peut contenir une liste de valeurs de couleur.

-  **XML** &ndash; les fichiers XML sont similaires au niveau de la fonction aux fichiers de configuration .NET. Il s’agit d’un XML arbitraire qui peut être lue par l’application en cours d’exécution

-----
