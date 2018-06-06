---
title: La gestion des tactile dans les applications Xamarin.iOS
description: Ce document liens vers des guides qui décrivent comment utiliser des fonctions tactiles, tactiles, mouvements et 3D Touch dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784160"
---
# <a name="handling-touch-in-xamarinios-apps"></a>La gestion des tactile dans les applications Xamarin.iOS

Comme d’autres plateformes mobiles, iOS a un nombre de façons de gérer l’interaction tactile. Il peut prendre en charge tactiles : nombre de points de contact sur l’écran et des mouvements complexes. Ce guide présente certains des concepts, ainsi que les particularités de l’implémentation tactiles et des mouvements sur iOS.

iOS encapsule les données de contact dans le `UITouch` (classe), qui est mise à disposition des applications via une série de `UIResponder` méthodes. Les applications peuvent remplacer ces méthodes dans les sous-classes de `UIView` et `UIViewController`, qui héritent de `UIResponder`.

En plus de capturer les données de fonctions tactiles, iOS fournit les moyens permettant d’interpréter les modèles de contacts dans mouvements. Ces modules de reconnaissance de mouvement peuvent à son tour être utilisées pour interpréter les commandes spécifiques à l’application, comme une rotation d’une image ou un tour d’une page. e/s fournit un riche ensemble de classes pour gérer les mouvements communs avec un minimum de code ajouté.

Le choix entre les fonctions tactiles et de modules de reconnaissance de mouvement peut être une source de confusion. Ce guide recommande qu’en général, préférence doit être émises pour les modules de reconnaissance de mouvement. Modules de reconnaissance de mouvement sont implémentés en tant que classes discrètes, qui fournissent la meilleure séparation des problèmes et une meilleure encapsulation. Cela rend faciles à partager la logique entre différentes vues, en réduisant la quantité de code écrit.

Toutefois, il existe lorsque vous devez utiliser le traitement de tactile de bas niveau et même effectuer le suivi de plusieurs doigts, par exemple, pour créer un programme finger-paint.

## <a name="sections"></a>Sections

-  [Touch dans iOS](touch-in-ios.md)
-  [Procédure pas à pas : Utilisation tactile dans iOS](ios-touch-walkthrough.md)
-  [Suivi tactile multipoint](touch-tracking.md)

Ce guide sert d’introduction à l’interaction tactile dans iOS. Pour plus d’informations sur l’utilisation de la 3D tactiles et commentaires tactile dans iOS, qui ont été introduit dans iOS 9 et 10 respectivement, consultez les guides spécifiques ci-dessous :

* [3D Touch](~/ios/platform/3d-touch.md)
* [Activation du retour haptique](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>Liens associés

- [iOS Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch finale (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
