---
title: Implémentation des Fragments - procédure pas à pas
description: Cet article explique comment utiliser des fragments pour développer des applications Xamarin.Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
---

# <a name="implementing-fragments---walkthrough"></a>Implémentation des fragments - procédure pas à pas

_Les fragments sont des composants autonomes et modulaires qui peuvent aider à gérer la complexité des applications Android qui ciblent les appareils avec un large éventail de tailles d’écran. Cet article décrit comment créer et utiliser des fragments lors du développement d’applications Xamarin.Android._

## <a name="overview"></a>Vue d'ensemble

Dans cette section, vous allez examiner comment créer et utiliser des fragments dans une application Xamarin.Android. Cette application affiche les titres de joue plusieurs par William Shakespeare dans une liste. Lorsque l’utilisateur appuie sur le titre d’une lecture, l’application affiche un devis à partir de ce play dans une activité séparée :

[![Application en cours d’exécution sur un téléphone Android en mode portrait](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Lorsque le téléphone pivote en mode paysage, l’apparence de l’application change : la liste de lecture et les guillemets s’affiche dans la même activité. Lorsqu’une lecture est sélectionné, le devis sera affichage dans la même activité :

[![Application en cours d’exécution sur un téléphone Android en mode paysage](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Enfin, si l’application s’exécute sur un Tablet PC :

[![Application en cours d’exécution sur une tablette Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Cet exemple d’application peut facilement s’adapter aux différents facteurs de forme et des orientations avec des modifications minimales du code à l’aide de fragments et [autres dispositions](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Les données de l’application existe dans deux tableaux de chaînes qui sont codés en dur dans l’application en tant que C# tableaux de chaîne. Chacun de ces ensembles servira de la source de données pour un fragment.  Un tableau contiendra le nom de certains joue par Shakespeare, et l’autre tableau contiendra un devis à partir de ce jeu. Lorsque l’application démarre, il affiche les noms de lecture dans un `ListFragment`. Lorsque l’utilisateur clique sur un cœur de la `ListFragment`, l’application démarre une autre activité qui affichera le devis.

L’interface utilisateur pour l’application se compose de deux dispositions, un pour portrait et l’autre pour le mode paysage. Au moment de l’exécution, Android détermine quelle mise en page à charger selon l’orientation de l’appareil et fournira cette disposition à l’activité à restituer. Toute la logique pour répondre aux clics de l’utilisateur et afficher les données se trouvera dans les fragments. Les activités dans l’application existent uniquement en tant que conteneurs qui hébergeront les fragments.

Cette procédure pas à pas est divisé en deux guides. Le [première partie](./walkthrough.md) se concentrera sur les parties essentielles de l’application. Un seul ensemble de dispositions (optimisé pour le mode portrait) sera créé, ainsi que les deux fragments et deux activités :

1. `MainActivity` &nbsp; Il s’agit de l’activité de démarrage de l’application.
1. `TitlesFragment` &nbsp; Ce fragment affichera une liste des titres de lecture qui ont été écrits par William Shakespeare. Il sera hébergé par `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` démarre le `PlayQuoteActivity` en réponse à l’utilisateur en sélectionnant un play dans `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Ce fragment affichera un devis à partir d’une lecture par William Shakespeare. Il sera hébergé par `PlayQuoteActivity`.

Le [deuxième partie de cette procédure pas à pas](./walkthrough-landscape.md) aborderons l’ajout d’une disposition différente (optimisée pour le mode paysage), qui affiche les deux fragments à l’écran. En outre, certaines modifications mineures au code sera au code afin que l’application s’adapte son comportement pour le nombre de fragments qui s’affichent simultanément sur l’écran.

## <a name="related-links"></a>Liens associés

- [FragmentsWalkthrough (sample)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Vue d’ensemble du Concepteur](~/android/user-interface/android-designer/index.md)
- [Implémentation des Fragments](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Package de support](https://developer.android.com/sdk/compatibility-library.html)
