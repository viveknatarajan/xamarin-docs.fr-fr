---
title: Utilisation de watchOS icônes dans Xamarin
description: Ce document décrit les différentes icônes nécessaires pour une application watchOS et comment configurer une solution à inclure ces icônes.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 150cca754de26edffcf97bb5d39b26166662c75b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790664"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Utilisation de watchOS icônes dans Xamarin

Solutions de l’Apple Watch nécessitent deux jeux d’icônes :

* Les icônes d’application iOS qui apparaîtront sur l’iPhone.
* Icônes de l’Apple Watch qui seront affichés dans un cercle dans le menu espion et dans les écrans de notification. L’icône d’application espion apparaît également dans le [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) application iOS.

## <a name="apple-watch-icons"></a>Icônes d’Apple Watch

| | | |
|-|-|-|
|Icône de l’application d’iOS|S’affiche sur l’iPhone et démarre l’application parente|![](icons-images/icon-ios.png)|
|Regardez l’icône de l’application|S’affiche sur l’écran d’accueil de l’Apple Watch|![](icons-images/icon-home.png)|
||S’affiche sur les notifications de surveillance|![](icons-images/notification-icon.png)|
||S’affiche dans le [Apple Watch application iOS](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Configuration de votre Solution

Pour garantir que votre application iOS et application watch affichent l’icône et le nom correct, suivez ces instructions pour chaque projet :

### <a name="ios-app"></a>Application iOS

Reportez-vous à la [guide des icônes d’Application iOS](~/ios/app-fundamentals/images-icons/app-icons.md) pour vérifier les icônes de votre application iOS sont correctement configurées.

#### <a name="infoplist"></a>Info.plist

La chaîne qui apparaît en regard de votre application de surveillance dans le [Apple Watch paramètres application](~/ios/watchos/app-fundamentals/settings.md) est configuré dans le **Info.plist de l’application iOS**.

Vérifiez que votre **Info.plist** a un `CFBundleName` clé et la valeur (Remarque : cela est différent du `CFBundleDisplayName`, peut avoir les deux) :

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Application d’Apple Watch

Une fois votre [application parente](~/ios/watchos/app-fundamentals/parent-app.md) ses icônes a configuré, vous devez ajouter un catalogue icône application à l’application de surveillance.

1. Avec le bouton droit sur le projet d’application espion et sélectionnez **fichier > Ajouter > nouveau fichier... > iOS > catalogue** pour ajouter un catalogue pour le projet.

 ![](icons-images/newasset.png "Ajouter un catalogue pour le projet")

2. Double-cliquez sur le **AppIcons.appiconset/Contents.json** fichier

  ![](icons-images/xcassets-iconset-sml.png "Le contenu de AppIcons")

3. Ajoutez toutes les images watchOS, comme indiqué dans cette capture d’écran :

  [![](icons-images/appicons-sml.png "Ajoutez toutes les images watchOS, comme indiqué dans cette capture d’écran")](icons-images/appicons.png#lightbox)

  Reportez-vous à [les instructions d’Apple icône](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) pour les tailles requises (les dimensions sont également affichées sur l’écran). N’oubliez pas que ces icônes seront automatiquement découpés pour effectuer le rendu d’un cercle.

  Votre liste d’icône doit ressembler à ceci :

  ![](icons-images/xcassets-complete-sml.png "La liste d’icône dans l’Explorateur de solutions")

4. Pour garantir le catalogue est inclus dans l’application, ajoutez la clé suivante et que la valeur pour le **Info.plist de l’application espion**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Vous pouvez vérifier les icônes sont configurés correct en vérifiant la [Apple Watch paramètres application](~/ios/watchos/app-fundamentals/settings.md) dans le simulateur, iPhone ou en générant un [notification](~/ios/watchos/platform/notifications.md) et confirmation de l’icône s’affiche sur la notification écran.

> [!NOTE]
> Icônes ne peut pas avoir un canal alpha (l’application est rejetée pendant l’envoi du magasin d’applications si un canal alpha est présent). Vous pouvez vérifier si un canal alpha existe et supprimez-le [à l’aide de l’application de la version d’évaluation sur Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Liens associés

- [Guident de l’icône d’Apple et les Images](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
