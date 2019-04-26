---
title: Utilisation de tailles d’écran dans Xamarin de watchOS
description: Ce document décrit comment travailler avec différentes tailles d’écran de watchOS. Il aborde le Concepteur d’Interface, le simulateur, watchOS watchOS et les ressources d’image.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: b2f4cc71c1993e51ed55b51edd7c50d393e60873
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412926"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Utilisation de tailles d’écran dans Xamarin de watchOS

Apple Watch est disponible dans les deux tailles d’écran :

- **38mm**
  - pixels logiques 136 x 170 (pixels physiques 272 x 340)

- **42mm**
  - 156 x 195 pixels logiques (pixels physiques 312 x 390).

Taille de l’écran doit tenir compte lorsque vous concevez et testez vos applications.

## <a name="watchos-interface-designer"></a>Concepteur d’Interface de watchOS

Par défaut, Visual Studio pour Mac concepteur affichera regarder contrôleurs d’interface à **Any Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "Les contrôleurs d’interface concepteur affiche espion à Any Apple Watch")

Utilisez le menu de taille pour modifier et afficher un aperçu de votre table de montage séquentiel à une des tailles d’écran disponibles : **38mm** ou **42mm**:

![](screen-sizes-images/screen-menu-sml.png "Sélection de la taille de 38mm ou 42mm")

La taille d’écran plus grande parfois restituera contenu qui serait tronquée/masqué sur l’écran plus petit.
Veillez à tester sur les deux tailles.


### <a name="interface-design"></a>Conception d'interfaces

Votre application doit afficher le même contenu à l’écran, quelle que soit la taille et il doit développer ou réduire des éléments comme il convient. Dans Visual Studio pour Mac le concepteur, dans l’inspecteur d’attributs, vous devez utiliser **relatif au conteneur** ou **taille pour ajuster le contenu** plutôt que des tailles fixes.

![](screen-sizes-images/sizeattributepanel-sml.png "Utilisez Relative au conteneur ou taille pour ajuster le contenu plutôt que des tailles fixes")

Étant donné que l’écran de surveillance est entourée d’un cadre noir, fournissant la marge intérieure autour de votre interface n’est pas recommandée. Laisser les éléments reposent sur le bord de l’écran et de laisser le cadre de former une frontière naturelle autour de l’application.


## <a name="watchos-simulator"></a>Simulateur de watchOS

Lorsque test sur le simulateur, vous pouvez facilement basculer entre les tailles d’écran de deux à l’aide de la **matériel > appareil** menu.

![](screen-sizes-images/simulator.png "Le simulateur peut basculer entre les tailles d’écran de deux à l’aide du menu périphérique matériel")


## <a name="image-resources"></a>Ressources d’images

Vous devez utiliser plusieurs ressources d’image si un seul élément multimédia ne semble pas bon à des tailles différentes. Catalogues de composants d’image autoriser pour les bitmaps distincts pour chaque taille de :

![](screen-sizes-images/images-xcassets.png "Éditeur de catalogue de ressources image")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Vous pouvez également utilisez code pour déterminer la taille d’écran et charger différentes images complètement :

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

En savoir plus sur à l’aide de la [contrôle image](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>Liens associés

- [Introduction à watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
