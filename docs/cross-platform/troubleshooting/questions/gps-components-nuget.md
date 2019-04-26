---
title: Unification des composants de Google Play Services et de NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357434"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Unification des composants de Google Play Services et de NuGet

## <a name="history"></a>Historique

Il permet d’être plusieurs composants des Services Google Play et les packages NuGet :

-   Services Google Play (Froyo)
-   Services Google Play (Gingerbread)
-   Google Play Services (ICS)
-   Services Google Play (JellyBean)
-   Services Google Play (KitKat)

Les fichiers .jar est fourni deux réellement uniquement de Google pour Google Play Services :

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

L’écart existait, car nos outils DIS correctement `aapt.exe` quel était le niveau d’API maximal des ressources à utiliser pour une application donnée. Cela signifiait, nous avons reçu des erreurs de compilation si nous avons essayé de bas niveau API comme Gingerbread à l’aide de la liaison de Services Google Play (KitKat).

## <a name="unifying-google-play-services"></a>Unification des Services Google Play

Dans les versions plus récentes de Xamarin.Android, nous indiquons désormais `aapt.exe` quelle version de ressources maximum à utiliser, donc ce problème disparaît pour nous.

Cela signifie, il est inutile réel pour avoir des packages distincts pour Gingerbread/ICS/JellyBean/KitKat (toutefois nous toujours doivent-ils une liaison distincte pour Froyo puisqu’il s’agit de complètement un fichier .jar différents).

Pour faciliter les choses pour les développeurs, nous avons maintenant nos composants et unifiées NuGet packages en deux :

-   Google Play Services (Froyo) (lie `google-play-services-froyo.jar`)
-   Services Google Play (lie `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Celle qui doit être utilisé ?

Dans presque tous les cas, Google Play Services doit être utilisés. La seule raison pour laquelle utiliser le package (Froyo) est si vous ciblez activement Froyo. La seule raison pour laquelle que ce fichier .jar distinct existe à partir de Google est dans la mesure où Froyo se trouve sur un petit pourcentage d’appareils, ils eux-mêmes ont décidé d’arrêter la prise en charge, par conséquent, ce fichier .jar est un instantané figé, non pris en charge des Services Google Play.

### <a name="note-about-gingerbread"></a>Remarque sur Gingerbread

Gingerbread n’a pas de prise en charge par défaut de Fragment, et pour cette raison, certaines des classes dans la liaison ne pourront être utilisées dans une application lors de l’exécution sur un appareil Gingerbread. Les classes comme `MapFragment` ne fonctionnera pas sur Gingerbread et leur variante de la prise en charge doit être utilisé à la place `SupportMapFragment`. Il incombe au développeur de savoir lequel utiliser. Cette incompatibilité est indiquée par Google dans leur documentation de Google Play Services.

### <a name="what-happens-to-the-old-componentsnugets"></a>Que se passe-t-il pour l’anciens composants/de NuGet ?

Dans la mesure où ils ne sont plus nécessaires, nous avons désactivé/Delisted les composants/packages NuGet suivants :

-   Services Google Play (Gingerbread)
-   Services Google Play (JellyBean)
-   Services Google Play (KitKat)

Existant _Google Play Services (ICS)_ composant/Nuget a été renommé en _Google Play Services_ et restent à jour à l’avenir. Tous les projets faisant référence à un des packages désactivé/Delisted doivent être mis à jour pour utiliser celle-ci.

Les composants désactivés existent encore et doivent être restaurées pour les projets, qu'ils sont toujours référencés dans, pour éviter de rompre les. De même les packages NuGet delisted existent encore et peuvent être restaurées. Ils ne seront pas être mis à jour à l’avenir.
