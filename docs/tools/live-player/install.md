---
title: Programme d’installation de Xamarin Live Player
description: Ce document décrit comment configurer Xamarin Live Player et utilisez-le pour apporter des modifications en direct à une application en cours d’exécution.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 05/14/2018
ms.openlocfilehash: 40c03e978cd9ce4666089f1b2a1e2ee8f47dbd81
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831671"
---
# <a name="xamarin-live-player-setup"></a>Programme d’installation de Xamarin Live Player

Xamarin Live Player vous permet d’apporter des modifications en direct à votre application et avaient les modifications reflétées en direct sur l’appareil. Votre code s’exécute au sein de l’application Xamarin Live Player – pas nécessaire pour configurer les émulateurs ou utiliser des câbles pour déployer ! Cet article décrit comment configurer Xamarin Live Player.

![Fonctionnalité en version préliminaire](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Obtenir l’application

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player est disponible pour Android à partir de Google Play :

[ ![Disponible sur Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Pour les appareils Android sans Google Play Xamarin Live Player est disponible via [HockeyApp](https://aka.ms/xlp-hockeyapp) distribution. En outre, la version préliminaire s’appuie pour Android peut être installé directement à partir de Google Play en acceptant de la [programme ouvert Bêta](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Nous conseillons aux utilisateurs de rejoindre Xamarin Live Player application iOS version préliminaire pour bénéficier d’un accès rapide aux dernières améliorations apportées dans TestFlight. En accédant à Xamarin Live Player, vous êtes accepté de Microsoft [conditions d’utilisation](https://www.microsoft.com/en-us/legal/intellectualproperty/copyright/default.aspx) & [déclaration de confidentialité](https://privacy.microsoft.com/en-us/privacystatement). Microsoft peut utiliser vos informations de contact pour fournir des mises à jour et des offres spéciales sur Xamarin et autres produits et services Microsoft. Vous pouvez vous désabonner à tout moment.

Pour accéder à l’iOS Xamarin Live Player version préliminaire, veuillez remplir le [informations d’inscription de TestFlight](https://fastring.xamarinliveplayer.com/), après quoi vous recevrez un e-mail de TestFlight sur l’installation de Xamarin Live Player iOS version préliminaire.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Obtenir Visual Studio 2017

Xamarin Live Player requiert :

- Visual Studio 2017 15.4 ou ultérieure.
- Un ordinateur Visual Studio et un appareil sur le même réseau Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. À l’aide de Xamarin Live Player pour la première fois

1. Ouvrez **Visual Studio 2017**.
2. Accédez à **Outils > Options...**  et sélectionnez le **Xamarin > autres** onglet.
3. Graduation **activer Xamarin Live Player**:

  ![Cochez la case Activer Xamarin Live Player dans la fenêtre Options](install-images/vs2017-options.png)

2. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
3. Choisissez **Live Player** dans la liste des appareils :

  ![Liste des appareils inclut une option de Xamarin Live Player](install-images/devices-empty-windows.png)

  * Si vous avez déjà associé un appareil, il sera disponible en tant qu’option.
  * Sinon, vous allez être invité à coupler un appareil si nécessaire.
4. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

  - **Démarrer sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application redémarrage que des modifications sont apportées et le fichier enregistré).
  - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter les variables, mais code ne peut pas être modifié.

  Vous pouvez également sélectionner **Outils > Xamarin Live Player > Live affichage de l’exécution**, qui vous permet de modifier l’application et voir les modifications se produisent sur l’appareil. La vue actuelle s’affiche (au lieu de l’écran principal de l’application).

5. Si un appareil est déjà associé et l’application Xamarin Live Player est en cours d’exécution sur l’appareil, il se peut que le code s’exécute immédiatement !

  Si aucun périphérique n’est associé, un code QR s’affiche avec des instructions sur la façon d’associer un appareil :

  ![Une fenêtre de dispositif de paire](install-images/manage-empty-windows.png)

  Si l’appareil ne peut pas être contacté pour l’appairage, une erreur peut apparaître.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Obtenir Visual Studio pour Mac

Xamarin Live Player requiert :

- OS X 10.11, macOS 10.12 ou supérieure
- Visual Studio pour Mac
- Un Mac et un appareil sur le même réseau Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. À l’aide de Xamarin Live Player pour la première fois

1. Ouvrez **Visual Studio pour Mac**.
2. Accédez à **Visual Studio > Préférences...**  et sélectionnez le **projets > Xamarin Live Player (version préliminaire)** onglet.
3. Graduation **activer Xamarin Live Player**:

  [![Cochez la case Activer Xamarin Live Player dans la fenêtre Options](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
3. Choisissez **Live Player** dans la liste des appareils.

  ![Liste des appareils inclut une option de Xamarin Live Player](install-images/devices.png)

  * Si vous avez déjà associé un appareil, il sera disponible en tant qu’option.
  * Sinon, vous allez être invité à coupler un appareil si nécessaire.
  * Choisissez **appareils Xamarin Live Player...**  pour gérer les appareils que vous souhaitez utiliser avec Xamarin Live Player.

4. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

  ![Menu options d’exécution](install-images/run-menu.png)

  - **Démarrer sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application redémarrage que des modifications sont apportées et le fichier enregistré).
  - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter les variables, mais code ne peut pas être modifié.
  - **Affichage de l’exécution de Live** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil. La vue actuelle s’affiche (au lieu de l’écran principal de l’application).

5. Si un appareil est déjà associé et l’application Xamarin Live Player est en cours d’exécution sur l’appareil, il se peut que le code s’exécute immédiatement !

  Si aucun périphérique n’est associée, un code QR s’affiche avec des instructions sur la façon d’associer un appareil :

  ![Une fenêtre de dispositif de paire](install-images/manage-empty.png)

  Si l’appareil ne peut pas être contacté pour l’appairage, une erreur s’affiche :

  ![Impossible de se connecter à un message d’erreur de périphérique](install-images/error-cannot-connect.png)


-----

Si vous rencontrez des problèmes ou que vous ne pouvez pas vous connecter, consultez [résolution des problèmes et limitations](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Liens associés

- [Limitations](~/tools/live-player/limitations.md)
- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
- [Exemples de Xamarin Live Player](~/tools/live-player/samples.md)
