---
title: Application Xamarin de lecteur en direct
description: Modifier et tester des applications en temps réel sur votre appareil iOS ou Android
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 3d4d34c6945387f5878ea708faf630d2545a750e
ms.sourcegitcommit: b706fbe05344f7201fbe8f82d9dbbceb66060637
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="xamarin-live-player-app"></a>Application Xamarin de lecteur en direct

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

Une fois que vous avez installé l’application sur votre téléphone, suivez les [instructions d’installation](~/tools/live-player/install.md) pour se connecter à votre ordinateur. Essayez l’une de le [exemples d’applications](~/tools/live-player/samples.md) pour qu’elle fonctionne.

Au démarrage, l’application Xamarin Live Player ressemble à ceci (sur iOS et Android respectivement) :

![Capture d’écran de Live Player iOS application](player-images/app-iphone-sml.png) ![Capture d’écran application de lecteur Android en direct](player-images/app-android-sml.png)

Lorsque vous appuyez sur **paire à Visual Studio**, utilisez l’appareil photo pour scanner le code-barres affichant sur votre ordinateur :

![Capture d’écran du scanneur de codes-barres iOS](player-images/scan-iphone-sml.png) ![Capture d’écran du scanneur de codes-barres Android](player-images/scan-android-sml.png)

Si la connexion réussit, le code doit s’exécuter sur l’appareil presque immédiatement (telles que la [exemple Calculator](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)) :

![Exemple d’application Calculatrice en cours d’exécution sur l’appareil](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Options

Appuyez sur le bouton informations **(i)** au bas de l’application pour faire apparaître le **Options** menu :

[![Capture d’écran du menu options](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Journaux

Afficher les journaux pour diagnostiquer les problèmes.

### <a name="settings"></a>Paramètres

- Activer/désactiver l’affichage des erreurs de compilation et d’exécution.
- Informations de version.
- Envoyer des commentaires.

[![Capture d’écran des paramètres](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>La gestion des appareils

Pour vous connecter à un appareil pour la première fois, suivez les instructions de [et le programme d’installation](~/tools/live-player/install.md). Vous pouvez associer plusieurs périphériques (par exemple un iOS et une Android) et les gérer via l’IDE.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Dans Visual Studio, choisissez **Outils > Xamarin Live Player > Gérer les appareils...**

![Gérer la fenêtre de périphériques](player-images/manage-tools-menu-vs.png)

Cette fenêtre vous permet d’effectuer les opérations suivantes :

- Un nouveau périphérique en analysant le code de paire
- Vous pouvez également associer un appareil en tapant le code affiché sur l’écran
- Supprimez la liste des périphériques existants

Vous pouvez également accéder à cette fenêtre à partir de la liste des appareils.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Dans Visual Studio pour Mac, choisissez **Outils > Gestion des appareils (Xamarin Player Live)...**

![Gérer la fenêtre de périphériques](player-images/manage-tools-menu.png)

Cette fenêtre vous permet d’effectuer les opérations suivantes :

- Un nouveau périphérique en analysant le code de paire
- Vous pouvez également associer un appareil en tapant le code affiché sur l’écran
- Supprimez la liste des périphériques existants

![Gérer la fenêtre de périphériques](player-images/manage.png)

Vous pouvez également accéder à cette fenêtre à partir de la liste des appareils :

![Choisissez Xamarin Live lecteurs à partir de la liste des appareils](player-images/manage-device-menu.png)

-----

Si vous rencontrez des problèmes, consultez [limitations et dépannage](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Liens associés

- [Limitations](~/tools/live-player/limitations.md)
- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Xamarin Player dynamique exemples](samples.md)
