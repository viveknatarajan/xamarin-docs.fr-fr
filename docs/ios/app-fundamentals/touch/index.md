---
title: Gestion des entrées tactiles dans les applications Xamarin.iOS
description: Ce document contient des liens vers des guides qui décrivent comment travailler avec touch, multipoint, mouvements et 3D Touch dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/23/2017
ms.openlocfilehash: 5aabc3a3c2ffbcffc0e12379989f7eb43b03a902
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116626"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Gestion des entrées tactiles dans les applications Xamarin.iOS

Comme les autres plateformes mobiles, iOS a un nombre de façons de gérer tactile. Il peut prendre en charge l’interaction tactile multipoint : nombre de points de contact sur l’écran et des mouvements complexes. Ce guide présente certains des concepts, ainsi que les particularités de l’implémentation de fonctions tactiles et des mouvements sur iOS.

iOS encapsule les données de contact dans le `UITouch` (classe), qui est accessible aux applications via une série de `UIResponder` méthodes. Les applications peuvent substituer ces méthodes dans les sous-classes de `UIView` et `UIViewController`, tous deux héritent `UIResponder`.

En plus de capturer des données de toucher, iOS fournit des moyens permettant d’interpréter les modèles de contacts dans des gestes. Ces modules de reconnaissance de mouvement peuvent à son tour être utilisés pour interpréter correctement les commandes spécifiques à l’application, comme une rotation d’une image ou un tour d’une page. iOS fournit une vaste collection de classes pour gérer les mouvements communs avec un minimum de code ajouté.

Le choix entre les contacts tactiles et les modules de reconnaissance de mouvement peut être une source de confusion. Ce guide recommande qu’en règle générale, préférence doit être émises pour les modules de reconnaissance de mouvement. Modules de reconnaissance de mouvement sont implémentées en tant que classes discrètes, qui fournissent une plus grande séparation des préoccupations et améliorer l’encapsulation. Cela rend faciles à partager la logique entre différentes vues, en réduisant la quantité de code écrit.

Toutefois, voici les heures lorsque vous devez utiliser le traitement tactile de bas niveau et même effectuer le suivi de plusieurs doigts, par exemple, pour créer un programme finger-paint.

## <a name="sections"></a>Sections

-  [Touch dans iOS](touch-in-ios.md)
-  [Procédure pas à pas : Utilisation de Touch dans iOS](ios-touch-walkthrough.md)
-  [Suivi tactile multipoint](touch-tracking.md)

Ce guide sert d’introduction aux entrées tactiles dans iOS. Pour plus d’informations sur l’utilisation de 3D Touch et retour haptique dans iOS, qui ont été introduit dans iOS 9 et 10 respectivement, consultez les guides spécifiques ci-dessous :

* [3D Touch](~/ios/platform/3d-touch.md)
* [Activation du retour haptique](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>Liens associés

- [iOS Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch finale (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
