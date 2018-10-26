---
title: Interfaces utilisateur dans iOS
description: Ce document contient des liens vers des guides qui expliquent comment créer des interfaces utilisateur dans une application Xamarin.iOS. Les guides liés couvrent l’API d’apparence, création d’objets d’interface utilisateur, les options de disposition et bien plus encore.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: efb88ada8a4b4c36dd49de137eb64acd63552968
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115677"
---
# <a name="user-interfaces-in-ios"></a>Interfaces utilisateur dans iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[API d’apparence](introduction-to-the-appearance-api.md)

iOS permet de nombreux attributs visuels de contrôles d’interface utilisateur pour être à thème à l’aide de la APIs UIAppearance.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Création des objets de l’interface utilisateur](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple regroupe des connexes des fonctionnalités dans « frameworks », qui sont équivalentes aux espaces de noms Xamarin.iOS. `UIKit` est l’espace de noms qui contient tous les contrôles d’interface utilisateur pour iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Options de disposition](~/ios/user-interface/ios-ui/layout-options.md)

Il existe deux mécanismes différents pour contrôler la disposition lorsqu’une vue est redimensionnée ou pivotée : redimensionnement automatique et mise en forme automatique.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Activation du retour haptique](~/ios/user-interface/ios-ui/haptic-feedback.md)

Cet article décrit les nouveaux types de retour haptique disponible dans iOS 10 et comment les implémenter dans Xamarin.iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Utilisation du thread d’interface utilisateur](~/ios/user-interface/ios-ui/ui-thread.md)

Votre code doit faire uniquement les threads de modifications apportées aux contrôles d’interface à partir de la main de l’utilisateur (ou l’interface utilisateur). Les mises à jour de l’interface utilisateur qui se produisent sur un thread différent (par exemple, un thread de rappel ou d’arrière-plan) ne peuvent pas obtenir rendus à l’écran, ou peuvent même provoquer un blocage.




