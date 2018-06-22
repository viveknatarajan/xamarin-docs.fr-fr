---
title: ToolBar
description: 'La barre d’outils est un composant de barre d’action qui offre plus de souplesse que la barre d’action par défaut : elle peut être placée n’importe où dans l’application, sa taille peut être modifiée et il peut utiliser un jeu de couleurs différent de thème de l’application. En outre, chaque écran de l’application peut avoir plusieurs barres d’outils.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 08fa00b539bd5baca4f5d61b04419a76a4a72ab1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766648"
---
# <a name="toolbar"></a>ToolBar

_La barre d’outils est un composant de barre d’action qui offre plus de souplesse que la barre d’action par défaut : elle peut être placée n’importe où dans l’application, sa taille peut être modifiée et il peut utiliser un jeu de couleurs différent de thème de l’application. En outre, chaque écran de l’application peut avoir plusieurs barres d’outils._

 
## <a name="overview"></a>Vue d'ensemble

Un élément de conception clés de toute activité Android est un *barre d’action*. La barre d’action est le composant de l’interface utilisateur qui est utilisé pour la navigation, de recherche, de menus et marque dans une application Android. Dans les versions d’Android avant Android 5.0 interface (lollipop), la barre d’action (également connu sous le *barre de l’application*) a été le composant recommandé pour fournir cette fonctionnalité. 

Le `Toolbar` widget (introduite dans Android 5.0 Lollipop) peut être considéré comme une généralisation de l’interface de barre d’action &ndash; elle est destinée à remplacer la barre d’action. Le `Toolbar` peuvent être utilisées partout dans une disposition de l’application, et il est beaucoup plus personnalisable que la barre d’action. La capture d’écran suivante illustre le personnalisé `Toolbar` exemple créé dans ce guide : 

[![Capture d’écran de l’exemple d’une barre d’outils avec modifier, enregistrer et éléments de menu de dépassement de capacité](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Il existe des différences importantes entre le `Toolbar` et la barre d’action : 

-   Un `Toolbar` peut être placée n’importe où dans l’interface utilisateur.

-   Plusieurs barres d’outils peuvent être affichés sur l’écran.

-   Si les fragments sont utilisés, chaque fragment peut avoir son propre `Toolbar`. 

-   A `Toolbar` peuvent être configurés pour couvrir uniquement une largeur partielle de l’écran. 

-   Étant donné que le `Toolbar` n’est pas lié au jeu de couleurs de décor de fenêtre de l’activité, il peut avoir un jeu de couleurs visuellement distinctes. 

-   Contrairement à la barre d’action, le `Toolbar` n’inclut pas d’une icône à gauche. Ses menus sur le droit d’utilisent moins d’espace. 

-   Le `Toolbar` hauteur est réglable. 

-   Autres vues peuvent être incluses dans le `Toolbar`. 

Un `Toolbar` peut contenir un ou plusieurs des éléments suivants : 

-   Bouton de navigation

-   Une image de logo de marque

-   Titre et le sous-titre

-   Vues personnalisées

-   Menu Action

-   Menu de dépassement de capacité

Google [la conception de matériel](https://material.google.com/) recommande tirant parti de ces éléments pour permettre à des applications un coup de œil distincte (plutôt que de compter uniquement sur une icône de l’application et le titre). 

Ce guide couvre les plus couramment utilisées `Toolbar` scénarios :

-   Remplacer la barre d’action par défaut d’une activité avec un `Toolbar`. 

-   Ajout d’un second `Toolbar` à une activité.

-   À l’aide de la **bibliothèque de prise en charge Android v7 AppCompat** bibliothèque (appelé *AppCompat* dans le reste de ce guide) pour déployer `Toolbar` dans les versions antérieures d’Android. 

 
 
## <a name="requirements"></a>Spécifications

`Toolbar` est disponible sur l’interface Lollipop 5.0 Android (API 21) et versions ultérieures. Lorsque le ciblage d’Android des versions antérieures à Android 5.0, utilisez le [Android bibliothèque prend en charge v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), qui fournit la compatibilité descendante `Toolbar` prend en charge dans un package NuGet. 
[Compatibilité de la barre d’outils](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) explique comment utiliser cette bibliothèque. 




## <a name="related-links"></a>Liens associés

- [La barre d’outils de l’interface Lollipop (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [La barre d’outils de AppCompat (exemple)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
