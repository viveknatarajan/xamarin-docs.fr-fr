---
title: Création des Interfaces utilisateur avec le concepteur iOS
description: Ce document décrit comment utiliser le Concepteur de Xamarin pour iOS pour générer des plans conceptuels et les fichiers .xib même interface utilisateur d’une application. Elle est liée à des documents qui décrivent la disponibilité de l’outil, ses fonctionnalités de base, les contrôles possible et fournissent des procédures pas à pas de son utilisation.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/31/2018
ms.openlocfilehash: eadc2147a44d6077436e394a4757d367ce42e5fa
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790001"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Création des Interfaces utilisateur avec le concepteur iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

=======
# <a name="ios-designer"></a>Concepteur iOS

_Le Concepteur de Xamarin pour iOS est un concepteur visuel pour les formats d’animation et Interface générateur iOS qui est entièrement intégré à Visual Studio pour Mac et Visual Studio. Le concepteur iOS gère une compatibilité complète avec les formats de plan conceptuel et .xib, afin que les fichiers peuvent être modifiés dans Visual Studio pour Mac ou Visual Studio en plus Interface Builder de Xcode. En outre, le Concepteur de Xamarin pour iOS prend en charge les fonctionnalités avancées telles que les contrôles personnalisés qui restituent au moment du design dans l’éditeur._
>>>>>>> maître

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![iOS concepteur dans Visual Studio pour Mac](images/designer-vsmac-sml.png "le concepteur iOS")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![iOS concepteur dans Visual Studio](images/designer-vs.png "le concepteur iOS")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Disponibilité

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac et dans Visual Studio 2017, sur Windows.

Ces guides suppose que vous connaissez le contenu traitées dans le [Xamarin.iOS Getting Started Guide](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Principes de base du concepteur iOS](introduction.md)

Ce guide couvre les fonctionnalités du concepteur Xamarin iOS. Elle couvre basics concepteur, montrant comment utiliser le concepteur pour disposer les contrôles visuellement et comment modifier les propriétés.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Vue d’ensemble de contrôles possible](ios-designable-controls-overview.md)

Ce guide se présente en détail dans les contrôles personnalisés, comment ils sont créés et les exigences, il doit remplir pour être affichées sur l’aire de conception. En outre, il montre comment déboguer les problèmes courants qui peuvent se produire lors de l’utilisation de contrôles possible.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Procédure pas à pas - à l’aide de contrôles personnalisés avec iOS Concepteur](ios-designable-controls-walkthrough.md)

Cet article fournit une procédure pas à pas montrant comment créer un contrôle personnalisé et l’utiliser dans le concepteur iOS. Il montre comment créer un contrôle disponible dans la boîte à outils du concepteur afin qu’il peut être glisser/déposé sur une vue. En outre, il montre comment implémenter un contrôle afin qu’il restitue correctement au moment de la conception et d’exécution, ainsi que comment créer des propriétés qui peuvent être définies au moment du design.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Disposition automatique avec le Concepteur de Xamarin iOS](designer-auto-layout.md)

Ce guide présente iOS disposition automatique et le nouveau workflow contraintes disponible dans le concepteur iOS.
