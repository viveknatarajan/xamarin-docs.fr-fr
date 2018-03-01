---
title: Interface utilisateur dans iOS
description: "Aborde l’utilisation de l’Interface utilisateur d’iOS dans une application Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 831ddfff7e05c391472b280095564f90528369ff
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface-in-ios"></a>Interface utilisateur dans iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Apparence API](introduction-to-the-appearance-api.md)

iOS permet de nombreux attributs visuels de contrôles d’interface utilisateur pour être à thème à l’aide de la APIs UIAppearance.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Création d’objets d’Interface utilisateur](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple regroupe connexes de fonctionnalités dans « infrastructures », qui sont équivalentes aux espaces de noms Xamarin.iOS. `UIKit` est l’espace de noms qui contient tous les contrôles d’interface utilisateur pour iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Options de disposition](~/ios/user-interface/ios-ui/layout-options.md)

Il existe deux mécanismes différents pour contrôler la disposition lorsqu’une vue est redimensionnée ou pivotée : redimensionnement automatique et mise en forme automatique.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Envoi de commentaires tactile](~/ios/user-interface/ios-ui/haptic-feedback.md)

Cet article couvre les nouveaux types de commentaires tactiles disponibles dans iOS 10 et comment les implémenter dans Xamarin.iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Utilisation avec le Thread d’interface utilisateur](~/ios/user-interface/ios-ui/ui-thread.md)

Votre code doit uniquement des threads de modifications apportées aux contrôles d’interface à partir de la main de l’utilisateur (ou l’interface utilisateur). Les mises à jour de l’interface utilisateur qui se produisent sur un autre thread (par exemple, un thread d’arrière-plan ou de rappel) ne peuvent pas s’affichent à l’écran, ou peuvent même provoquer un blocage.




