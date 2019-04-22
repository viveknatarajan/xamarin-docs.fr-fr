---
title: Programme d’installation de Xamarin Live Player
description: Ce document décrit comment configurer Xamarin Live Player et utilisez-le pour apporter des modifications en direct à une application en cours d’exécution.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f9cfc69c2cd711460233e609d63bcbb8eb172ccf
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854754"
---
# <a name="xamarin-live-player-setup"></a>Programme d’installation de Xamarin Live Player

Xamarin Live Player vous permet d’apporter des modifications en direct à votre application et avaient les modifications reflétées en direct sur l’appareil. Votre code s’exécute au sein de l’application Xamarin Live Player – pas nécessaire pour configurer les émulateurs ou utiliser des câbles pour déployer ! Cet article décrit comment configurer Xamarin Live Player.

![Fonctionnalité d’évaluation](~/media/shared/preview.png)

> [!NOTE]
> Acteur de l’aperçu est uniquement disponible dans Visual Studio 2017.

## <a name="1-get-the-android-app"></a>1. Obtenir l’application Android

Xamarin Live Player est disponible pour Android à installer directement à partir de [HockeyApp](https://aka.ms/xlp-hockeyapp).

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

4. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
5. Choisissez **Live Player** dans la liste des appareils :

    ![Liste des appareils inclut une option de Xamarin Live Player](install-images/devices-empty-windows.png)

    - Si vous avez déjà associé un appareil, il sera disponible en tant qu’option.
    - Sinon, vous allez être invité à coupler un appareil si nécessaire.

6. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

    - **Démarrer sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application redémarrage que des modifications sont apportées et le fichier enregistré).
    - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter les variables, mais code ne peut pas être modifié.

    Vous pouvez également sélectionner **Outils > Xamarin Live Player > Live affichage de l’exécution**, qui vous permet de modifier l’application et voir les modifications se produisent sur l’appareil. La vue actuelle s’affiche (au lieu de l’écran principal de l’application).

7. Si un appareil est déjà associé et l’application Xamarin Live Player est en cours d’exécution sur l’appareil, il se peut que le code s’exécute immédiatement !

    Si aucun périphérique n’est associée, un code QR s’affiche avec des instructions sur la façon d’associer un appareil :

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

4. Créez ou ouvrez un projet Xamarin (ou un [exemple](~/tools/live-player/samples.md)).
5. Choisissez **Live Player** dans la liste des appareils.

    ![Liste des appareils inclut une option de Xamarin Live Player](install-images/devices.png)

    - Si vous avez déjà associé un appareil, il sera disponible en tant qu’option.
    - Sinon, vous allez être invité à coupler un appareil si nécessaire.
    - Choisissez **appareils Xamarin Live Player...**  pour gérer les appareils que vous souhaitez utiliser avec Xamarin Live Player.

6. Appuyez sur la **exécuter** bouton, ou sélectionnez une de ces options à partir de la **exécuter** ou menu contextuel :

    ![Menu options d’exécution](install-images/run-menu.png)

    - **Démarrer sans débogage** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil (application redémarrage que des modifications sont apportées et le fichier enregistré).
    - **Démarrer le débogage** : vous pouvez définir des points d’arrêt et inspecter les variables, mais code ne peut pas être modifié.
    - **Affichage de l’exécution de Live** – vous pouvez modifier l’application et voir les modifications se produisent sur l’appareil. La vue actuelle s’affiche (au lieu de l’écran principal de l’application).

7. Si un appareil est déjà associé et l’application Xamarin Live Player est en cours d’exécution sur l’appareil, il se peut que le code s’exécute immédiatement !

    Si aucun périphérique n’est associée, un code QR s’affiche avec des instructions sur la façon d’associer un appareil :

    ![Une fenêtre de dispositif de paire](install-images/manage-empty.png)

    Si l’appareil ne peut pas être contacté pour l’appairage, une erreur s’affiche :

    ![Impossible de se connecter à un message d’erreur de périphérique](install-images/error-cannot-connect.png)

-----

Si vous rencontrez des problèmes ou que vous ne pouvez pas vous connecter, consultez [résolution des problèmes et limitations](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Liens associés

- [Échantillons à utiliser avec Live Player](https://developer.xamarin.com/samples/xamarin-live-player/all/)
- [Résolution des problèmes](~/tools/live-player/troubleshooting.md)
