---
title: Programme d’installation de Xamarin Player en direct
description: Modifier et tester des applications en temps réel sur votre appareil iOS ou Android
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 05/14/2018
ms.openlocfilehash: bbc935c2770fab1a853bb12fba2b7eb0283bb258
ms.sourcegitcommit: b706fbe05344f7201fbe8f82d9dbbceb66060637
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="xamarin-live-player-setup"></a>Programme d’installation de Xamarin Player en direct

Xamarin Player de Live permet d’apporter des modifications en direct à votre application et de ces modifications répercuter en temps réel sur l’appareil. Votre code s’exécute à l’intérieur de l’application Xamarin Live Player – pas nécessaire pour configurer des émulateurs ou d’utiliser des câbles pour déployer ! Cet article décrit comment configurer le lecteur Xamarin Live.

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Obtenir l’application

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le lecteur Xamarin Live est disponible pour Android depuis Google Play :

[ ![Disponible sur Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Pour les appareils Android sans Google Play Live Xamarin Player est disponible via [HockeyApp](https://aka.ms/xlp-hockeyapp) distribution. En outre, la version préliminaire génère pour Android peut être installé directement à partir de Google Play en vous inscrivant à la [programme bêta ouvert](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Nous encourageons aux utilisateurs de rejoindre Xamarin Live Player application iOS Aperçu pour bénéficier d’un accès rapide aux dernières améliorations apportées via TestFlight. En accédant à Live Xamarin Player, vous êtes acceptez Microsoft [les conditions d’utilisation](https://www.microsoft.com/en-us/legal/intellectualproperty/copyright/default.aspx) & [déclaration de confidentialité de](https://privacy.microsoft.com/en-us/privacystatement). Microsoft peut utiliser vos informations de contact pour fournir des mises à jour et des offres spéciales sur Xamarin et autres produits et services Microsoft. Vous pouvez vous désabonner à tout moment.

Pour accéder à l’iOS Xamarin Live Player version préliminaire, veuillez compléter le [informations d’inscription de TestFlight](https://fastring.xamarinliveplayer.com/), après quoi vous recevrez un message électronique de TestFlight sur l’installation de Xamarin Live Player iOS version préliminaire.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Obtenir Visual Studio 2017

Xamarin Live Player nécessite :

- 15.4 de 2017 Visual Studio ou une version ultérieure.
- Un ordinateur Visual Studio et un périphérique sur le même réseau Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Pour la première fois à l’aide de Xamarin Live Player

1. Ouvrez **Visual Studio 2017**.
2. Accédez à **Outils > Options...**  et sélectionnez le **Xamarin > autre** onglet.
3. Graduation **activer le lecteur Xamarin Live**:

  ![Cochez la case Activer le lecteur Xamarin Live dans la fenêtre des Options](install-images/vs2017-options.png)

2. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
3. Choisissez **Live le lecteur** dans la liste des appareils :

  ![Liste des périphériques inclut une option de Xamarin Live Player](install-images/devices-empty-windows.png)

  * Si vous avez déjà été associé à un appareil, il sera disponible en tant qu’option.
  * Dans le cas contraire, vous êtes invité à associer un périphérique à la demande.
4. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

  - **Exécuter sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application est redémarrée comme des modifications et enregistrer le fichier).
  - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter des variables, mais code ne peut pas être modifié.

  Vous pouvez également sélectionner **Outils > Xamarin Live Player > Live exécuter la vue actuelle**, qui vous permet de modifier l’application et voir les modifications se produisent sur l’appareil. Le mode actuel est affiché (au lieu de l’écran principal de l’application).

5. Si un périphérique est déjà associé, et l’application Xamarin Live Player s’exécute sur l’appareil, il se peut que le code s’exécute immédiatement.

  Si aucun périphérique n’est associé, un code QR s’affiche avec des instructions sur la façon d’associer un périphérique :

  ![Une fenêtre de dispositif de paire](install-images/manage-empty-windows.png)

  Si l’appareil ne peut pas être contacté pour le pairage, une erreur peut apparaître.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Obtenir Visual Studio pour Mac

Xamarin Live Player nécessite :

- OS X 10.11 macOS 10.12 ou supérieur
- Visual Studio pour Mac
- Un Mac et un périphérique sur le même réseau Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. À l’aide de Xamarin Player de dynamique pour la première fois

1. Ouvrez **Visual Studio pour Mac**.
2. Accédez à **Visual Studio > Préférences...**  et sélectionnez le **projets > Xamarin Live Player (version préliminaire)** onglet.
3. Graduation **activer le lecteur Xamarin Live**:

  [![Cochez la case Activer le lecteur Xamarin Live dans la fenêtre des Options](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
3. Choisissez **Live le lecteur** dans la liste des appareils.

  ![Liste des périphériques inclut une option de Xamarin Live Player](install-images/devices.png)

  * Si vous avez déjà été associé à un appareil, il sera disponible en tant qu’option.
  * Dans le cas contraire, vous êtes invité à associer un périphérique à la demande.
  * Choisissez **Xamarin Player dynamique périphériques...**  pour gérer les périphériques que vous souhaitez utiliser avec le lecteur Xamarin Live.

4. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

  ![Options de menu d’exécution](install-images/run-menu.png)

  - **Exécuter sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application est redémarrée comme des modifications et enregistrer le fichier).
  - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter des variables, mais code ne peut pas être modifié.
  - **Exécuter l’affichage actuel en direct** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil. Le mode actuel est affiché (au lieu de l’écran principal de l’application).

5. Si un périphérique est déjà associé, et l’application Xamarin Live Player s’exécute sur l’appareil, il se peut que le code s’exécute immédiatement.

  Si aucun périphérique n’a été couplée, un code QR s’affiche avec des instructions sur la façon d’associer un périphérique :

  ![Une fenêtre de dispositif de paire](install-images/manage-empty.png)

  Si l’appareil ne peut pas être contacté pour le pairage, une erreur s’affiche :

  ![Impossible de se connecter à un message d’erreur de périphérique](install-images/error-cannot-connect.png)


-----

Si vous rencontrez des problèmes ou que vous ne pouvez pas vous connecter, consultez [limitations et dépannage](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Liens associés

- [Limitations](~/tools/live-player/limitations.md)
- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Xamarin Player dynamique exemples](~/tools/live-player/samples.md)
