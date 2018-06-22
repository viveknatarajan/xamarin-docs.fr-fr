---
title: Remoted iOS Simulator pour Windows
description: Exécutée à distance iOS Simulator pour Windows vous permet de tester vos applications sur un simulateur iOS affichés dans Windows en même temps que Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149805"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator pour Windows

Exécutée à distance iOS Simulator pour Windows vous permet de tester vos applications sur un simulateur iOS affichés dans Windows en même temps que Visual Studio 2017.

[![](ios-simulator-images/hero-sml.png "Simulateur iOS s’exécutant sous Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>Bien démarrer

Le simulateur iOS distant pour Windows est installé automatiquement dans le cadre de Xamarin dans Visual Studio 2017. Pour l’utiliser, procédez comme suit :

1. [Couplez pas Visual 2017 à un hôte de Build de Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Dans Visual Studio 2017, démarrez le débogage d’un projet iOS ou tvOS. Le simulateur iOS distant pour Windows s’affiche sur votre ordinateur Windows.

## <a name="simulator-window"></a>Fenêtre du simulateur

La barre d’outils en haut de la fenêtre du simulateur contient un nombre de boutons utiles :

- **Accueil** – simule le bouton accueil sur un appareil iOS
- **Verrou** – verrouille le simulateur (faites défiler pour déverrouiller)
- **Capture d’écran de** – enregistre une capture d’écran du simulateur
- [**Paramètres** ](#settings) – affiche le clavier, l’emplacement et autres paramètres
- [**Autres options** ](#other-options) – permet d’afficher diverses options de simulateur tels que des mouvements de rotation et secouer

    [![](ios-simulator-images/maps-app-sml.png "Simulateur iOS mappe l’exemple")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>Paramètres

Cliquez sur icône d’engrenage de la barre d’outils pour ouvrir la **paramètres** fenêtre :

[![](ios-simulator-images/settings-sml.png "paramètres du simulateur iOS")](ios-simulator-images/settings.png#lightbox)

Ces paramètres permettent d’activer le clavier physique, de choisir un emplacement de l’appareil doit activer Touch ID rapport (statiques et déplacement des emplacements sont tous deux pris en charge) et de réinitialiser le contenu et les paramètres pour le simulateur.

## <a name="other-options"></a>Autres options

Bouton de sélection de la barre d’outils, vous affichez autres options telles que la rotation, secouer mouvements et en cours de redémarrage. Ces options peuvent être affichées sous forme de liste en double-cliquant sur n’importe où dans la fenêtre du simulateur :

[![](ios-simulator-images/more-sml.png "paramètres supplémentaires du simulateur iOS")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>Prise en charge de l’écran tactile

Les ordinateurs Windows les plus récents possèdent un écran tactile. Étant donné que l’exécutée à distance iOS Simulator pour Windows prend en charge les interactions tactiles, vous pouvez tester votre application avec le même pincement, faites défiler et tactiles doigt multiples que vous utilisez avec des appareils iOS physiques.

De même, exécutée à distance iOS Simulator pour Windows traite entrée de stylet de Windows en tant qu’entrée de crayon d’Apple.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>La désactivation de distance iOS Simulator pour Windows

Pour désactiver l’exécutée à distance iOS Simulator pour Windows, accédez à **Outils > Options > Xamarin > Paramètres iOS** et désactivez **distant simulateur Windows**.

[![](ios-simulator-images/options-sml.png "case à cocher pour utiliser le simulateur")](ios-simulator-images/options.png#lightbox)

Avec cette option est désactivée, le débogage s’ouvre, le simulateur iOS sur le Mac connecté générer hôte.
