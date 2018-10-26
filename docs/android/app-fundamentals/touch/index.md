---
title: Touchers et gestes dans Xamarin.Android
description: "Les écrans tactiles sur la plupart des appareils d’aujourd'hui permettent aux utilisateurs rapidement et efficacement interagir avec les appareils de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection de tactile : il est possible d’utiliser des gestes également. Par exemple, le geste de zoom par pincement est un exemple très courant de ce par pincement une partie de l’écran avec deux doigts, que l’utilisateur peut effectuer un zoom avant ou arrière. Ce guide examine tactiles et les mouvements dans Android."
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f957c9ff5a0e7c3a0821978703860ed2f723a92
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123314"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Touchers et gestes dans Xamarin.Android

_Les écrans tactiles sur la plupart des appareils d’aujourd'hui permettent aux utilisateurs rapidement et efficacement interagir avec les appareils de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection de tactile : il est possible d’utiliser des gestes également. Par exemple, le geste de zoom par pincement est un exemple très courant de ce par pincement une partie de l’écran avec deux doigts, que l’utilisateur peut effectuer un zoom avant ou arrière. Ce guide examine tactiles et les mouvements dans Android._

## <a name="touch-overview"></a>Vue d’ensemble des fonctions tactiles

iOS et Android sont similaires dans les méthodes qu’ils gèrent tactile. À la fois peuvent prendre en charge l’interaction tactile multipoint - nombre de points de contact sur l’écran - et des mouvements complexes. Ce guide présente certaines des similarités dans les concepts, ainsi que les particularités de l’implémentation des fonctions tactiles et mouvements sur les deux plateformes.

Android utilise un `MotionEvent` objet pour encapsuler des données de toucher et des méthodes sur l’objet de vue pour écouter les contacts tactiles.

En plus de capturer des données de toucher, iOS et Android fournissent les moyens permettant d’interpréter les modèles de contacts dans des gestes. Ces modules de reconnaissance de mouvement peuvent à son tour être utilisés pour interpréter correctement les commandes spécifiques à l’application, comme une rotation d’une image ou un tour d’une page. Android fournit un certain nombre de mouvements pris en charge, ainsi que des ressources pour que d’ajouter facilement des mouvements personnalisés complexes.

Si vous travaillez sur Android ou iOS, le choix entre les contacts tactiles et les modules de reconnaissance de mouvement peut être une source de confusion. Ce guide recommande qu’en règle générale, préférence doit être émises pour les modules de reconnaissance de mouvement. Modules de reconnaissance de mouvement sont implémentées en tant que classes discrètes, qui fournissent une plus grande séparation des préoccupations et améliorer l’encapsulation. Cela rend plus facile de partager la logique entre différentes vues, en réduisant la quantité de code écrit.

Ce guide suit un format similaire pour chaque système d’exploitation : tout d’abord, tactiles de la plateforme API sont introduites et expliqué, tels qu’ils sont le fondement sur les fonctions tactiles interactions sont construites. Ensuite, nous plonger dans le monde des modules de reconnaissance de mouvement – tout d’abord en explorant certains mouvements courants et en cours d’achèvement avec la création des mouvements personnalisés pour les applications. Enfin, vous verrez comment suivre chaque doigt à l’aide du suivi tactile de bas niveau pour créer un programme finger-paint.

## <a name="sections"></a>Sections

-  [Entrées tactiles dans Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Procédure pas à pas : Utilisation des entrées tactiles dans Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Suivi tactile multipoint](touch-tracking.md)

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons examiné des entrées tactiles dans Android. Pour les deux systèmes d’exploitation, nous avons appris comment activer tactile et comment répondre aux événements tactiles. Ensuite, nous avons appris sur les mouvements et parmi les modules de reconnaissance de mouvement qu’iOS et Android fournissent pour gérer certains scénarios les plus courants. Nous avons examiné comment créer des mouvements personnalisés et de les implémenter dans les applications. Une procédure pas à pas a présenté les concepts et les API pour chaque système d’exploitation en action, et vous avez également vu comment suivre chaque doigt.



## <a name="related-links"></a>Liens associés

- [Android Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch finale (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
