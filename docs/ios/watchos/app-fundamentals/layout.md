---
title: Utilisation de watchOS disposition dans Xamarin
description: Ce document décrit comment créer une disposition de watchOS à l’aide de Xamarin. Il traite les contrôleurs d’interface, les groupes, les séparateurs et les contrôles de contenu.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 11ff5ec2fc8fe99a780a3d728d3d84af59794cea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790612"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Utilisation de watchOS disposition dans Xamarin

La mise en page de l’Apple Watch [tailles d’écran](~/ios/watchos/app-fundamentals/screen-sizes.md) présente des défis uniques.

## <a name="design-tips"></a>Conseils de conception

Le point clé est : rendre votre interface utilisateur lisible et utilisable sur un écran de petite espion, avec un doigt volumineux. N’entrent dans l’interruption de la conception de la **iOS Simulator** (qui apparaît très volumineux) et un **pointeur** (qui fonctionne avec des cibles tactile minuscule) !

- Utiliser un arrière-plan noir - il crée l’impression d’un écran plus grand avec panneau noir de la surveillance.

- Pas de remplissage autour de votre disposition de l’écran - cadre constitue un remplissage visual naturels.

- Vous concentrer sur la lisibilité. Servez-vous des tailles de polices et couleurs judicieusement pour garantir la lisibilité. Les styles de texte intégré permet d’obtenir la prise en charge automatique de Type dynamique.

![](layout-images/type.png "Exemple de prise en charge de Type dynamique")

- Vous concentrer sur les tailles de cible tactile. Boutons/tappable lignes de la table avec les étiquettes de texte doivent couvrir la totalité de l’écran. Apple indique que « ne placez jamais plus de trois éléments côte-à-côte », et si vous n’utilisez pas les étiquettes de texte et les icônes.

- Utilisez le [ `Menu` contrôle](~/ios/watchos/user-interface/menu.md) aux fonctionnalités exposent moins souvent utilisée pour conserver la conception de votre application claire et concise.


## <a name="implementation"></a>Implémentation

Regardez que kit comprend les commandes suivantes pour vous aider à créer des dispositions d’application attrayante espion :

### <a name="interface-controller"></a>Contrôleur d’interface

Le `WKInterfaceController` est la classe de base toutes vos séquences.

L’aire de conception pour le contrôleur d’interface se comporte comme un vertical **groupe**: vous pouvez faire glisser des autres contrôles sur le contrôleur d’interface et il sera automatiquement disposé au-dessus de l’autre :

![](layout-images/controller-scene.png "Les contrôles sont automatiquement disposé au-dessus de l’autre")

Vous pouvez définir le **Position** et **taille** propriétés de chaque contrôle pour contrôler leur apparence :

![](layout-images/positionsize-attributes.png "Définir les propriétés de la Position et la taille de chaque contrôle")

Lorsque la taille est définie sur **relatif au conteneur** vous pouvez fournir une valeur proportionnelle et un réglage du décalage. Cette capture d’écran affiche un bouton qui a été défini pour utiliser 80 % de la largeur de l’écran de surveillance (**0,8**) :

![](layout-images/button-attributes.png "Fournissez une valeur proportionnelle et un réglage du décalage")


### <a name="group"></a>Regrouper

`WKInterfaceGroup` est un conteneur de disposition simple qui peut être configuré pour la pile contrôle verticalement ou horizontalement. Elle inclut l’espacement entre chaque contrôle par défaut, mais vous pouvez modifier l’espacement (et les marges) dans le **attributs** inspecteur.

![](layout-images/group-attributes.png "Modifier l’espacement et les marges dans l’inspecteur d’attributs")

Groupes peuvent eux-mêmes être dimensionnés et positionnés par rapport à des contrôles autour d’elles, et les groupes peuvent être imbriqués pour créer des dispositions complexes.

![](layout-images/group-scene.png "Les groupes peuvent être imbriqués pour créer des dispositions complexes")


### <a name="separator"></a>Séparateur

Le contrôle de séparateur destiné à fournir une aide visuelle de votre disposition. Utilisez les séparateurs de fin (ou les couleurs d’arrière-plan ou des images) pour aider l’utilisateur à comprendre le contenu est lié à l’écran.

![](layout-images/separator-scene.png "Exemple d’utilisation de séparateur")

Notez les séparateurs bleues et vertes qui n’utilisent pas la largeur totale de l’écran qui ont été configurés avec l’option **fixe** ou **relatif au conteneur** tailles.

### <a name="content-controls"></a>Contrôles de contenu

Aucune mise en page ne serait complète sans le `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, et [autres contrôles](~/ios/watchos/user-interface/index.md).
Ceux-ci peuvent être positionnées de votre disposition à l’aide de **groupes** ou les paramètres de position et la taille de chaque contrôle.



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Référence de disposition d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple couleur & Typographie de référence](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
