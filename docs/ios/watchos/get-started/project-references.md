---
title: Références de projets
description: Explication de la relation entre l’application iOS, Espion application et extension de surveillance.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: f3573e8b578ca567ea9d7360eb132aead4c24f37
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="project-references"></a>Références de projets

_Explication de la relation entre l’application iOS, Espion application et extension de surveillance._

Les trois projets dans une solution watchOS sont *configurés automatiquement* à se référencent mutuellement de manière spécifique pour les applications watchOS 3 doivent être générés et regroupé correctement. Ces paramètres d’identificateur de groupe et les références de projet sont décrites ci-dessous pour référence.

## <a name="project-references"></a>Références de projets

Afficher les références en double-cliquant sur les nœuds de références pour chaque projet :

- **application iPhone** références **application Watch**

![](project-references-images/catalog-reference1.png "application iPhone fait référence à l’application de surveillance")

- **Surveiller l’application** références **espion Extension d’application**

![](project-references-images/catalog-reference2.png "application iPhone fait référence à l’application de surveillance")


 - Le **Extension d’application espion** ne fait pas référence à une des autres projets

![](project-references-images/catalog-reference3.png "Extension d’application espion ne fait pas référence à d’autres projets")



## <a name="bundle-identifiers"></a>Identificateurs d’offre groupée

Vous devez également vous assurer votre **identificateurs d’offre groupée** sont corrects.
Les trois projets doivent avoir le *même* préfixe d’identificateur, avec les projets de deux espion ayant prédéfinis des extensions de `watchkitextension` et `watchkitapp`, comme suit (pour le **WatchKitCatalog** exemple) :

 - Projet d’unifié Xamarin.iOS- `com.xamarin.WatchKitCatalog`

 - Projet d’Extension de WatchKit- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Projet d’application espion- `com.xamarin.WatchKitCatalog.watchkitapp`

Assurez-vous également que ces **Info.plist** paramètres sont corrects :

 - Le projet d’application Watch `WKCompanionAppBundleIdentifier` correspond aux ID d’offre groupée de l’application conteneur/parent (ie. celui qui s’exécute sur l’iPhone) ;

 - Le projet d’espion Kit Extension **ID d’offre groupée WKApp** correspond aux ID d’ensemble. du projet d’application de surveillance

Vous pouvez modifier les identificateurs en double-cliquant sur le **Info.plist** fichier dans chaque projet.

Cette capture d’écran est la **d’Extension Watch** fichier Info.plist, affichant le **espion de l’application** identificateur ainsi :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "Cette capture d’écran est le fichier Info.plist de l’Extension surveillance")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "Cette capture d’écran est le fichier Info.plist de l’Extension surveillance")

-----

Cette capture d’écran est la **l’application espion** fichier Info.plist.
En cours **système d’exploitation espion** version est 8.2, donc la **cible de déploiement** pour l’application de surveillance doit être **8.2**. Notez que si vous avez Xcode 6.3 est installé, cette valeur peut être définie à 8.3 - vous ne devez modifier 8.2.

![](project-references-images/infoplist-watchapp.png "La fichier Info.plist espion")

La cible de déploiement pour l’application de surveillance peut être différente de l’Extension de surveillance et l’application iOS.

