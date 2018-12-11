---
title: Remoted iOS Simulator pour Windows
description: Le simulateur Remoted iOS Simulator pour Windows vous permet de tester vos applications sur un simulateur iOS affichés dans Windows en même temps que Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: lobrien
ms.author: laobri
ms.date: 11/14/2018
ms.openlocfilehash: 4a45d4fb7aaad504c7bb5628f1e521a39463b06f
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899068"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator pour Windows

Le simulateur Remoted iOS Simulator pour Windows vous permet de tester vos applications sur un simulateur iOS affichés dans Windows en même temps que Visual Studio 2017.

[![](images/hero-sml.png "Simulateur iOS en cours d’exécution sur Windows")](images/hero.png#lightbox)

## <a name="getting-started"></a>Bien démarrer

Le simulateur iOS à distance pour Windows est installé automatiquement dans le cadre de Xamarin dans Visual Studio 2017. Pour l’utiliser, procédez comme suit :

1. [Coupler Visual 2017 vers un hôte de Build Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Dans Visual Studio 2017, démarrer le débogage d’un projet iOS ou tvOS. Le simulateur iOS à distance pour Windows s’affiche sur votre ordinateur Windows.

Espion [cette vidéo](deploy.md) pour obtenir un guide pas à pas.

## <a name="simulator-window"></a>Fenêtre du simulateur

La barre d’outils en haut de la fenêtre du simulateur contient un nombre de boutons utiles :

- **Accueil** – simule le bouton accueil sur un appareil iOS.
- **Verrou** : verrouille le simulateur (faites défiler pour déverrouiller).
- **Capture d’écran** – enregistre une capture d’écran du simulateur (stocké dans **Pictures\Xamarin\iOS simulateur\\**).
- [**Paramètres** ](#settings) – affiche le clavier, emplacement et autres paramètres.
- [**Autres options** ](#other-options) – permet d’afficher diverses options de simulateur telles que rotation, agiter mouvements et un ID tactile.

    [![](images/maps-app-sml.png "Simulateur iOS mappe exemple")](images/maps-app.png#lightbox)

## <a name="settings"></a>Paramètres

Cliquez sur icône d’engrenage de la barre d’outils pour ouvrir la **paramètres** fenêtre :

[![](images/settings-sml.png "paramètres du simulateur iOS")](images/settings.png#lightbox)

Ces paramètres permettent d’activer le clavier physique, choisissez un emplacement de l’appareil doit rapport (emplacements statiques et de déplacement sont tous deux pris en charge), activer Touch ID et réinitialiser le contenu et les paramètres pour le simulateur.

## <a name="other-options"></a>Autres options

Bouton de sélection de la barre d’outils, vous affichez les autres options telles que rotation, agiter mouvements et en redémarrant. Ces mêmes options peuvent être affichées sous forme de liste en double-cliquant sur n’importe où dans la fenêtre du simulateur :

[![](images/more-sml.png "paramètres supplémentaires du simulateur iOS")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>Prise en charge de l’écran tactile

La plupart des ordinateurs Windows disposent d’écrans tactiles. Étant donné que le simulateur Remoted iOS Simulator pour Windows prend en charge les interactions tactiles, vous pouvez tester votre application avec le même pincement, effectuez un balayage et tactiles multi doigt que vous utilisez avec les appareils iOS physiques.

De même, le simulateur Remoted iOS Simulator pour Windows traite entrée du stylet Windows en tant qu’entrée du crayon d’Apple.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>La désactivation de la Remoted iOS Simulator pour Windows

Pour désactiver le simulateur Remoted iOS Simulator pour Windows, accédez à **Outils > Options > Xamarin > Paramètres iOS** et décochez la case **Simulator à distance pour Windows**.

[![](images/options-sml.png "case à cocher pour utiliser le simulateur")](images/options.png#lightbox)

Avec cette option est désactivée, le débogage s’ouvre le simulateur iOS sur le Mac connecté hôte de build.
