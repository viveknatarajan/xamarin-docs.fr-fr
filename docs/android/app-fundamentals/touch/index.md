---
title: Tactiles et mouvements dans Xamarin.Android
description: Les écrans tactiles sur de nombreux périphériques actuels permettent aux utilisateurs rapidement et efficacement d’interagir avec les périphériques de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection tactile ; il est possible d’utiliser également des mouvements. Par exemple, le mouvement de pincement pour zoomer est un exemple courant de ce par pincement une partie de l’écran de l’utilisateur peut effectuer un zoom avant ou arrière de deux doigts. Ce guide examine tactile et mouvements dans Android.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3700f9c73fb58fefcdba7987c9931e385cd52d38
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Tactiles et mouvements dans Xamarin.Android

_Les écrans tactiles sur de nombreux périphériques actuels permettent aux utilisateurs rapidement et efficacement d’interagir avec les périphériques de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection tactile ; il est possible d’utiliser également des mouvements. Par exemple, le mouvement de pincement pour zoomer est un exemple courant de ce par pincement une partie de l’écran de l’utilisateur peut effectuer un zoom avant ou arrière de deux doigts. Ce guide examine tactile et mouvements dans Android._

## <a name="touch-overview"></a>Vue d’ensemble de l’interaction tactile

iOS et Android sont similaires dans manipuler les fonctions tactiles. À la fois peuvent prendre en charge - nombre de points de contact sur l’écran - tactiles et des mouvements complexes. Ce guide présente certaines des similarités dans les concepts, ainsi que les particularités de l’implémentation des fonctions tactiles et mouvements sur les deux plateformes.

Android utilise un `MotionEvent` objet pour encapsuler des données de fonctions tactiles et méthodes sur l’objet de vue pour écouter les fonctions tactiles.

En plus de capturer les données de fonctions tactiles, iOS et Android fournissent les moyens permettant d’interpréter les modèles de fonctions tactiles dans les mouvements. Ces modules de reconnaissance de mouvement peuvent à son tour être utilisées pour interpréter les commandes spécifiques à l’application, comme une rotation d’une image ou un tour d’une page. Android fournit un certain nombre de mouvements pris en charge, ainsi que des ressources afin de vous ajouter facilement des mouvements personnalisés complexes.

Si vous travaillez sur Android ou iOS, le choix entre les fonctions tactiles et de modules de reconnaissance de mouvement peut être une source de confusion. Ce guide recommande qu’en général, préférence doit être émises pour les modules de reconnaissance de mouvement. Modules de reconnaissance de mouvement sont implémentés en tant que classes discrètes, qui fournissent la meilleure séparation des problèmes et une meilleure encapsulation. Cela facilite le partage la logique entre différentes vues, en réduisant la quantité de code écrit.

Ce guide suit un format identique pour chaque système d’exploitation : tout d’abord, le contact de la plate-forme API est introduites et expliqué, car ils sont le fondement sur les fonctions tactiles des interactions. Ensuite, nous plonger dans le monde des modules de reconnaissance de mouvement : tout d’abord par l’exploration des mouvements courantes et la fin de la création des mouvements personnalisés pour les applications. Enfin, vous verrez comment assurer le suivi des doigts individuels à l’aide du suivi de tactile de bas niveau pour créer un programme finger-paint.

## <a name="sections"></a>Sections

-  [Entrées tactiles dans Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Procédure pas à pas : Utilisation tactile dans Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Suivi de tactile](touch-tracking.md)

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons examiné tactile dans Android. Pour les deux systèmes d’exploitation, nous avons appris comment activer tactile et comment répondre aux événements tactiles. Ensuite, nous avons appris mouvements et parmi les modules de reconnaissance de mouvement que Android et iOS fournissent pour gérer certains scénarios les plus courants. Nous avons examiné comment créer des mouvements personnalisés et de les implémenter dans les applications. Une procédure pas à pas a présenté les concepts et les API pour chaque système d’exploitation en action, et vous avez également appris à effectuer le suivi des doigts.



## <a name="related-links"></a>Liens associés

- [Android Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch finale (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
