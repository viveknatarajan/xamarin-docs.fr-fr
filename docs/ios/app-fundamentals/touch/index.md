---
title: Tactile
description: "Les écrans tactiles sur de nombreux périphériques actuels permettent aux utilisateurs rapidement et efficacement d’interagir avec les périphériques de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection tactile : il est possible d’utiliser des gestes également. Par exemple, le mouvement de pincement pour zoomer est un exemple courant de cette solution par pincement une partie de l’écran de l’utilisateur peut effectuer un zoom avant ou arrière de deux doigts. Ce guide examine tactiles et mouvements dans iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A17FD28-313F-4AAC-B82B-3847B4D64A88
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: 8f6c26048bc0ece0d64acf069151ff1d67403ccc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="touch"></a>Tactile

_Les écrans tactiles sur de nombreux périphériques actuels permettent aux utilisateurs rapidement et efficacement d’interagir avec les périphériques de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection tactile : il est possible d’utiliser des gestes également. Par exemple, le mouvement de pincement pour zoomer est un exemple courant de cette solution par pincement une partie de l’écran de l’utilisateur peut effectuer un zoom avant ou arrière de deux doigts. Ce guide examine tactiles et mouvements dans iOS._


Comme d’autres plateformes mobiles, iOS a un nombre de façons de gérer l’interaction tactile. Il peut prendre en charge tactiles : nombre de points de contact sur l’écran et des mouvements complexes. Ce guide présente certains des concepts, ainsi que les particularités de l’implémentation tactiles et des mouvements sur iOS.

iOS encapsule les données de contact dans le `UITouch` (classe), qui est mise à disposition des applications via une série de `UIResponder` méthodes. Les applications peuvent remplacer ces méthodes dans les sous-classes de `UIView` et `UIViewController`, qui héritent de `UIResponder`.

En plus de capturer les données de fonctions tactiles, iOS fournit les moyens permettant d’interpréter les modèles de contacts dans mouvements. Ces modules de reconnaissance de mouvement peuvent à son tour être utilisées pour interpréter les commandes spécifiques à l’application, comme une rotation d’une image ou un tour d’une page. e/s fournit un riche ensemble de classes pour gérer les mouvements communs avec un minimum de code ajouté.

Le choix entre les fonctions tactiles et de modules de reconnaissance de mouvement peut être une source de confusion. Ce guide recommande qu’en général, préférence doit être émises pour les modules de reconnaissance de mouvement. Modules de reconnaissance de mouvement sont implémentés en tant que classes discrètes, qui fournissent la meilleure séparation des problèmes et une meilleure encapsulation. Cela rend faciles à partager la logique entre différentes vues, en réduisant la quantité de code écrit.

Toutefois, il existe lorsque vous devez utiliser le traitement de tactile de bas niveau et même effectuer le suivi de plusieurs doigts, par exemple, pour créer un programme finger-paint.

## <a name="sections"></a>Sections

-  [Touch dans iOS](touch-in-ios.md)
-  [Procédure pas à pas : Utilisation tactile dans iOS](ios-touch-walkthrough.md)
-  [Tactiles de suivi](touch-tracking.md)

Ce guide sert d’introduction à l’interaction tactile dans iOS. Pour plus d’informations sur l’utilisation de la 3D tactiles et commentaires tactile dans iOS, qui ont été introduit dans iOS 9 et 10 respectivement, consultez les guides spécifiques ci-dessous :

* [3D Touch](~/ios/platform/3d-touch.md)
* [Envoi de commentaires tactile](~/ios/user-interface/ios-ui/haptic-feedback.md)



## <a name="related-links"></a>Liens associés

- [iOS Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch finale (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [Peinture avec les doigts (exemple)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
