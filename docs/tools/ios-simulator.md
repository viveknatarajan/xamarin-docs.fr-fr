---
title: Distance iOS Simulator (pour Windows)
description: Tester et déboguer des applications iOS entièrement dans Visual Studio sous Windows
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: 5a1408f6595bd1e2371cd1d0421f81a3a16a5cc3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Distance iOS Simulator (pour Windows)

_Tester et déboguer des applications iOS entièrement dans Visual Studio sous Windows_

[![](ios-simulator-images/hero-sml.png "Simulateur iOS s’exécutant sous Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>Téléchargez et installez

Téléchargez le [programme d’installation](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) et l’installer sur votre ordinateur Windows. Visual Studio Tools pour Xamarin doit déjà être installés.

> [!NOTE]
> À l’aide d’un simulateur d’iOS à distance sur Visual Studio requiert un Mac en réseau avec Xamarin est installé.

## <a name="getting-started"></a>Prise en main

Pour utiliser le simulateur iOS à distance :

1. Assurez-vous que Visual Studio s’est connecté à votre Mac au moins une fois avant de démarrer le simulateur iOS à distance.
2. Garantir une application iOS ou tvOS est la **projet de démarrage** et démarrer le débogage.

Vous pouvez désactiver le simulateur iOS à distance à partir de **Outils > Options > Xamarin > Paramètres iOS** en désactivant la case à **distant simulateur Windows** indiqué ici :

[![](ios-simulator-images/options-sml.png "case à cocher pour utiliser le simulateur")](ios-simulator-images/options.png#lightbox)

Le simulateur iOS ouvrirez ensuite l’ordinateur Mac connecté. Activez cette option pour activer le simulateur iOS à distance.

## <a name="features"></a>Fonctionnalités

Simulateur iOS à distance vous offre un moyen pour tester et déboguer des applications iOS sur le simulateur entièrement à partir de Visual Studio sur Windows.

### <a name="simulator-window"></a>Fenêtre du simulateur

La barre d’outils inclut un nombre de boutons permettant d’interagir avec le simulateur :

- **Accueil** – simule le bouton accueil sur l’appareil.
- **Verrou** – verrouille le simulateur (vous pouvez faites glisser pour déverrouiller).
- **Capture d’écran de** – enregistre une capture d’écran du simulateur sur le disque.
- [**Paramètres** ](#settings) – configurer le clavier et l’emplacement.
- Autres [ **options** ](#options) – disponible telles que de faire pivoter, secouer une variété d’options de simulateur, ou appeler d’autres États dans le simulateur. Lorsque certaines options sont masquées, ils sont accessibles à partir de l’icône de points de suspension qui apparaît dans la barre d’outils, ou en cliquant sur la fenêtre.

    [![](ios-simulator-images/maps-app-sml.png "Simulateur iOS mappe l’exemple")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>Paramètres

L’icône de « engrenage » ouvre le **paramètres** fenêtre :

[![](ios-simulator-images/settings-sml.png "paramètres du simulateur iOS")](ios-simulator-images/settings.png#lightbox)

Cela vous permet à activer le clavier physique sur le simulateur, choisissez l’emplacement est signalée à l’appareil (y compris un emplacement statique ou autres options d’emplacement de déplacement).



### <a name="other-options"></a>Autres options

Cliquez n’importe où dans la fenêtre du simulateur pour afficher toutes les options disponibles dans le simulateur, telles que la rotation, déclencher un secouez et redémarrer le simulateur :

[![](ios-simulator-images/more-sml.png "paramètres supplémentaires du simulateur iOS")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>Prise en charge de l’écran tactile

Les ordinateurs Windows les plus récents possèdent un écran tactile, et le simulateur iOS à distance vous permet de se touchent la fenêtre du simulateur pour tester les interactions de l’utilisateur dans votre application iOS.

Cela inclut le pincement le glissement et plusieurs à un doigt tactiles - choses précédemment peuvent être facilement testés uniquement sur des périphériques physiques.

Prise en charge de stylet dans Windows est également convertie en entrée Apple crayon sur le simulateur.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
