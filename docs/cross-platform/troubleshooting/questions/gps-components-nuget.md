---
title: Unification de Google Play Services de composants et NuGet
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4e571f24d08fe34a2c3d689501cb2ca656aaa502
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Unification de Google Play Services de composants et NuGet

### <a name="history"></a>Historique

Il permet d’être plusieurs composants des Services Google Play et les packages NuGet :

-   Services de Google Play (Froyo)
-   Services de Google Play (personnage en pain)
-   Google Play Services (ICS)
-   Google Play Services (JellyBean)
-   Services de Google Play (KitKat)

Google en réalité est fourni avec deux .jar fichiers pour les Services Google Play :

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

L’écart existait parce que nos outils n’a pas été correctement savoir `aapt.exe` ce que le niveau d’API maximal des ressources a été à utiliser pour une application donnée. Cela signifiait, nous avons reçu des erreurs de compilation si nous avons essayé de bas niveau API comme personnage en pain à l’aide de la liaison de Services Google Play (KitKat).

### <a name="unifying-google-play-services"></a>Unifier les Services Google Play

Dans les versions plus récentes de Xamarin.Android, nous demander `aapt.exe` version maximale de ressources à utiliser, donc ce problème disparaît pour nous.

Cela ne signifie, il existe aucune raison réelle de disposer de packages distincts pour personnage en pain/ICS/JellyBean/KitKat (toutefois nous toujours doivent-elles une liaison distincte pour Froyo, car il est un fichier .jar différents entièrement).

Pour simplifier les choses pour les développeurs, nous avons maintenant nos composants et unifiés NuGet packages en deux :

-   (Froyo) des Services Google Play (lie `google-play-services-froyo.jar`)
-   Les Services Google Play (lie `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>L’application doit être utilisée ?

Dans presque tous les cas, les Services Google Play doit être utilisés. La seule raison pour laquelle utiliser le package (Froyo) est que si vous ciblez activement Froyo. La seule raison pour laquelle que ce fichier .jar distinct existe à partir de Google est étant Froyo sur un petit pourcentage de périphériques, ils eux-mêmes ont décidé d’arrêter la prise en charge, par conséquent, ce fichier .jar est un instantané figé, non pris en charge des Services Google Play.

### <a name="note-about-gingerbread"></a>Remarque sur personnage en pain

Personnage en pain n’a pas de prise en charge par défaut de Fragment, et par conséquent, certaines des classes dans la liaison ne seront pas utilisable dans une application en cours d’exécution sur un appareil personnage en pain. Les classes comme `MapFragment` ne fonctionnera pas sur personnage en pain et leur variante de prise en charge doit être utilisé à la place `SupportMapFragment`. Il incombe au développeur de connaître à utiliser. Cette incompatibilité est indiquée par Google dans leur documentation Services Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Que se passe-t-il pour l’ancien composants/de NuGet ?

Dans la mesure où ils ne sont plus nécessaires, nous avons désactivé/Delisted les composants/NuGets suivantes :

-   Services de Google Play (personnage en pain)
-   Google Play Services (JellyBean)
-   Services de Google Play (KitKat)

Existants _Google Play Services (ICS)_ composant/Nuget a été renommé en _Services Google Play_ et restent à jour à l’avenir. Tous les projets faisant référence à un des packages désactivé/Delisted doivent être mis à jour pour utiliser celle-ci.

Les composants désactivés existent encore et doivent être restaurées pour les projets, qu'ils sont toujours référencés dans, pour éviter de rompre les. De même les packages NuGet delisted existent encore et peuvent être restaurées. Ils ne seront pas être mis à jour à l’avenir.
