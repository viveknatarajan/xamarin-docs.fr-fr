---
title: Création des Interfaces utilisateur avec le concepteur iOS
description: Ce document décrit comment utiliser le Concepteur de Xamarin pour iOS pour générer l’interface utilisateur de l’application avec des storyboards et des fichiers .xib. Il est lié à des documents qui traitent de disponibilité de l’outil, ses fonctionnalités de base, les contrôles concevables et fournissent des procédures pas à pas de son utilisation.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/31/2018
ms.openlocfilehash: 7c6529c539ab502fb6c13226acd18a57f8f6d58e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378669"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Création des Interfaces utilisateur avec le concepteur iOS

_Le Concepteur de Xamarin pour iOS est un concepteur visuel pour les formats d’animation et Interface Builder iOS qui est entièrement intégré à Visual Studio pour Mac et Visual Studio. Le concepteur iOS gère une compatibilité totale avec les formats et les fichiers .xib, afin que les fichiers peuvent être modifiés dans Visual Studio pour Mac ou Visual Studio en plus de l’Interface Builder de Xcode. En outre, le Concepteur de Xamarin pour iOS prend en charge les fonctionnalités avancées telles que les contrôles personnalisés qui restituent au moment du design dans l’éditeur._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Concepteur dans Visual Studio pour Mac iOS](images/designer-vsmac-sml.png "le concepteur iOS")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Concepteur dans Visual Studio iOS](images/designer-vs.png "le concepteur iOS")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Disponibilité

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac et dans Visual Studio 2017 sur Windows.

Ces guides suppose que vous connaissez le contenu couvert dans le [Xamarin.iOS mise en route Guide](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Principes de base du concepteur iOS](introduction.md)

Ce guide couvre les fonctionnalités du concepteur iOS Xamarin. Il couvre les principes de base concepteurs, en montrant comment utiliser le concepteur pour disposer les contrôles visuellement et comment modifier les propriétés.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Vue d’ensemble des contrôles concevable](ios-designable-controls-overview.md)

Ce guide aborde en profondeur à des contrôles personnalisés, comment ils sont créés et quelles spécifications qu’ils doivent satisfaire pour être restitués sur l’aire de conception. En outre, il montre comment déboguer les problèmes courants qui peuvent se produire lors de l’utilisation de contrôles concevable.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Procédure pas à pas : utilisation de contrôles personnalisés avec le concepteur iOS](ios-designable-controls-walkthrough.md)

Cet article fournit une procédure pas à pas montrant comment créer un contrôle personnalisé et l’utiliser dans le concepteur iOS. Il montre comment créer un contrôle disponible dans la boîte à outils du concepteur afin qu’il peut être glisser/déposée sur une vue. En outre, il montre comment implémenter un contrôle donc il restitue correctement au moment du design et de runtime, ainsi que comment créer des propriétés qui peuvent être définies au moment du design.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Disposition automatique avec le Concepteur de Xamarin iOS](designer-auto-layout.md)

Ce guide présente iOS disposition automatique et le nouveau workflow de contraintes disponible dans le concepteur iOS.
