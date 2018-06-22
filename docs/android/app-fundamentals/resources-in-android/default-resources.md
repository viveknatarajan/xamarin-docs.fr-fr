---
title: Ressources par défaut
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 119aa8b967ace858ee56f521624f6356e08a8b80
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765946"
---
# <a name="default-resources"></a>Ressources par défaut

Ressources par défaut sont des éléments qui ne sont pas spécifiques à un périphérique particulier ou le facteur de forme et sont donc le choix par défaut par le système d’exploitation Android si aucun plus spécifique les ressources sont disponibles. Par conséquent, ils sont le type le plus courant de ressource à créer. Elles sont organisées dans les sous-répertoires de la **ressources** répertoire selon leur type de ressource :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Fichiers de ressources par défaut](default-resources-images/01-resource-files-vs.png)

Dans l’image ci-dessus, le projet possède des valeurs par défaut pour les ressources drawable, dispositions et des valeurs (fichiers XML qui contiennent des valeurs simples).

Vous trouverez ci-dessous une liste complète des types de ressources :

-  **animation** &ndash; fichiers XML décrivant les animations de propriété.
   Animations de propriété ont été introduits dans le niveau de l’API 11 (Android 3.0) et fournit de l’animation des propriétés sur un objet. Les animations de propriété sont un moyen plus souple et puissant pour décrire des animations sur n’importe quel type d’objet.

-  **chez** &ndash; fichiers XML qui décrivent *interpolation* animations. Interpolation animations sont une série d’instructions d’animation pour effectuer des transformations sur le contenu d’une rotation d’objet, ou par exemple, afficher une image ou augmenter la taille de texte. Interpolation animations sont limitées pour afficher uniquement les objets.

-  **couleur** &ndash; fichiers XML qui décrivent une liste de couleurs d’état. Pour comprendre les listes d’état de couleur, envisagez un widget de l’interface utilisateur tel qu’un bouton.
   Elle peut avoir différents états comme activé ou désactivé, et le bouton peut changer de couleur pour chaque modification de l’état. La liste est exprimée dans une liste d’état.

-  **drawable** &ndash; Drawable ressources sont un concept général pour les graphiques qui peuvent être compilés dans l’application et ensuite accessibles par les appels d’API ou référencés par d’autres ressources XML.
   Quelques exemples de drawables sont des fichiers bitmap (.png, .gif, .jpg), les bitmaps redimensionnables spéciales appelés [correctifs de neuf](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), état énumère les formes génériques définis dans XML, etc.,.
 
-  **disposition** &ndash; fichiers XML qui décrivent une disposition d’interface utilisateur, par exemple une activité ou une ligne dans une liste.

-  **menu** &ndash; fichiers XML qui décrivent les menus de l’application tel que *Options Menus*, *contextuels*, et *sous-menus*. Pour obtenir un exemple de menus, consultez le [démonstration de Menu contextuel](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) ou [contrôles Standard](https://developer.xamarin.com/samples/mobile/StandardControls/) exemple.

-  **brutes** &ndash; des fichiers arbitraires qui sont enregistrés sous leur forme binaire brut. Ces fichiers sont compilés dans une application Android dans un format binaire.

-  **valeurs** &ndash; fichiers XML qui contiennent des valeurs simples. Un fichier XML dans le répertoire de valeurs ne définit pas une seule ressource, mais il peut définir plusieurs ressources. Par exemple un fichier XML peut contenir une liste de valeurs de chaîne, tandis qu’un autre fichier XML peut contenir une liste de valeurs de couleur.

-  **XML** &ndash; les fichiers XML qui sont semblables aux fichiers de configuration .NET. Il s’agit de XML arbitraire qui permettre être lues par l’application en cours d’exécution


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Fichiers de ressources par défaut](default-resources-images/01-resource-files-xs.png)

Dans l’image ci-dessus, le projet possède des valeurs par défaut pour les ressources drawable, dispositions et des valeurs (fichiers XML qui contiennent des valeurs simples).

Vous trouverez ci-dessous une liste complète des types de ressources :

-  **animation** &ndash; fichiers XML décrivant les animations de propriété.
   Animations de propriété ont été introduits dans le niveau de l’API 11 (Android 3.0) et fournit de l’animation des propriétés sur un objet. Les animations de propriété sont un moyen plus souple et puissant pour décrire des animations sur n’importe quel type d’objet.

-  **chez** &ndash; fichiers XML qui décrivent *interpolation* animations. Interpolation animations sont une série d’instructions d’animation pour effectuer des transformations sur le contenu d’une rotation d’objet, ou par exemple, afficher une image ou augmenter la taille de texte. Interpolation animations sont limitées pour afficher uniquement les objets.

-  **couleur** &ndash; fichiers XML qui décrivent une liste de couleurs d’état. Pour comprendre les listes d’état de couleur, envisagez un widget de l’interface utilisateur tel qu’un bouton.
   Il peut être ont des états différents comme activé ou désactivé, et le bouton peut changer de couleur pour chaque modification de l’état. La liste est exprimée dans une liste d’état.

-  **police** &ndash; à partir de niveau de l’API 26, il est possible d’incorporer des polices en tant que ressource dans une application Android. Le 26 de bibliothèque de prise en charge sera polices backport au niveau de l’API 14. L’incorporation de polices permet aux applications

-  **mipmap** &ndash; Drawable ressources sont un concept général pour les graphiques qui peuvent être compilés dans l’application et ensuite accessibles par les appels d’API ou référencés par d’autres ressources XML.
   Quelques exemples de drawables sont des fichiers bitmap (.png, .gif, .jpg), les bitmaps redimensionnables spéciales appelés [correctifs de neuf](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), état énumère les formes génériques définis dans XML, etc.,.

-  **disposition** &ndash; fichiers XML qui décrivent une disposition d’interface utilisateur, par exemple une activité ou une ligne dans une liste.

-  **menu** &ndash; fichiers XML qui décrivent les menus de l’application tel que *Options Menus*, *contextuels*, et *sous-menus*. Pour obtenir un exemple de menus, consultez le [démonstration de Menu contextuel](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) ou [contrôles Standard](https://developer.xamarin.com/samples/mobile/StandardControls/) exemple.

-  **brutes** &ndash; des fichiers arbitraires qui sont enregistrés sous leur forme binaire brut. Ces fichiers sont compilés dans une application Android dans un format binaire.

-  **valeurs** &ndash; fichiers XML qui contiennent des valeurs simples. Un fichier XML dans le répertoire de valeurs ne définit pas une seule ressource, mais il peut définir plusieurs ressources. Par exemple un fichier XML peut contenir une liste de valeurs de chaîne, tandis qu’un autre fichier XML peut contenir une liste de valeurs de couleur.

-  **XML** &ndash; les fichiers XML qui sont semblables aux fichiers de configuration .NET. Il s’agit de XML arbitraire qui permettre être lues par l’application en cours d’exécution

-----
