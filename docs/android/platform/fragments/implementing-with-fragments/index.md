---
title: Implémentation de Fragments - procédure pas à pas
description: Cet article vous montre comment utiliser des fragments de développer des applications de Xamarin.Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798354"
---
# <a name="implementing-fragments---walkthrough"></a>Implémentation de fragments - procédure pas à pas

_Les fragments sont des composants autonomes et modulaires qui peuvent aider à gérer la complexité des applications Android qui ciblent les appareils avec un large éventail de tailles d’écran. Cet article vous montre comment créer et utiliser des fragments lors du développement d’applications de Xamarin.Android._

## <a name="overview"></a>Vue d'ensemble

Dans cette section, vous allez étudier comment créer et utiliser des fragments dans une application de Xamarin.Android. Cette application affiche les titres de lecture de plusieurs par William Shakespeare dans une liste. Lorsque l’utilisateur appuie sur le titre d’un jeu, puis l’application affiche un guillemet à partir de ce jeu dans une activité séparée :

[![Application en cours d’exécution sur un téléphone Android en mode portrait](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Lorsque le téléphone est pivoté en mode paysage, l’apparence de l’application change : à la fois la liste de lecture et de devis s’affiche dans la même activité. Lorsqu’une lecture est sélectionné, le guillemet seront affichées dans la même activité :

[![Application en cours d’exécution sur un téléphone Android en mode paysage](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Enfin, si l’application s’exécute sur une tablette :

[![Application en cours d’exécution sur une tablette Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Cet exemple d’application peut facilement l’adapter aux différents facteurs de forme et orientations avec des modifications minimales du code à l’aide de fragments et [dispositions autre](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Les données de l’application existera dans deux tableaux de chaînes qui sont codées en dur dans l’application en tant que tableaux de chaîne C#. Chacun de ces ensembles sera utilisée comme source de données pour un fragment.  Un tableau contiendra le nom d’une lecture par Shakespeare, et l’autre tableau contiendra un guillemet à partir de ce jeu. Lorsque l’application démarre, il affiche les noms de lecture dans un `ListFragment`. Lorsque l’utilisateur clique sur un cœur de la `ListFragment`, l’application démarre une autre activité qui affiche le devis.

L’interface utilisateur de l’application se compose de deux dispositions, un pour le mode portrait et l’autre pour le mode paysage. Au moment de l’exécution, Android détermine quelle disposition à charger en fonction de l’orientation de l’appareil et fournit cette mise en page à l’activité à restituer. Toute la logique pour répondre aux clics d’utilisateur et afficher les données se trouvera dans les fragments. Les activités dans l’application existent uniquement comme des conteneurs qui hébergeront les fragments.

Cette procédure pas à pas est divisé en deux guides. Le [première partie](./walkthrough.md) se concentrera sur les parties principales de l’application. Un seul ensemble de dispositions (optimisé pour le mode portrait) sera créé, ainsi que les deux fragments et les deux activités :

1. `MainActivity` &nbsp; Il s’agit de l’activité de démarrage de l’application.
1. `TitlesFragment` &nbsp; Ce fragment affiche une liste de titres de lecture qui ont été écrits par William Shakespeare. Il sera hébergé par `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` démarre le `PlayQuoteActivity` en réponse à l’utilisateur en sélectionnant un cœur de `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Ce fragment affichera un guillemet à partir d’une lecture par William Shakespeare. Il sera hébergé par `PlayQuoteActivity`.

Le [deuxième partie de cette procédure pas à pas](./walkthrough-landscape.md) traite l’ajout d’une disposition différente (optimisée pour le mode paysage), qui affiche les deux fragments à l’écran. En outre, certaines modifications mineures au code porteront sur le code afin que l’application s’adapte son comportement pour le nombre de fragments qui s’affichent simultanément sur l’écran.

## <a name="related-links"></a>Liens associés

- [FragmentsWalkthrough (exemple)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Vue d’ensemble du Concepteur](~/android/user-interface/android-designer/index.md)
- [Implémentation des Fragments](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Package de prise en charge](http://developer.android.com/sdk/compatibility-library.html)
