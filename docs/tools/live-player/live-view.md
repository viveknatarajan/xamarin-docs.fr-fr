---
redirect_url: /xamarin/tools/live-player/
title: XAML Live Aperçu avant impression
description: Tester des modifications de code d’application en temps réel sur votre appareil iOS ou Android
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: c96777666d7601412c1d2e3be1c7fe23131fc9d4
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="xaml-live-previewing"></a>XAML Live Aperçu avant impression

Un des avantages de Live Xamarin Player est la possibilité de live aperçu des pages XAML, apporter des modifications au code dans Visual Studio et voir les modifications apparaissent instantanément sur votre appareil. L’aperçu en direct est possible sur votre appareil iOS ou Android, ou sur un émulateur ou un simulateur. Ce guide montre comment utiliser la fonctionnalité d’aperçu dynamique pour afficher les écrans XAML individuels.

## <a name="requirements"></a>Spécifications

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Un ordinateur exécutant Windows 7 ou version ultérieure.
2. Visual Studio 2017 version 15.4 ou une version ultérieure avec le **développement pour appareils mobiles avec .NET** installé la charge de travail.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Un Mac avec OS X 10.11 macOS 10.12 ou supérieur.
2. Visual Studio pour Mac 7.2 ou version ultérieure. Nous vous recommandons de la version la plus récente.

-----



<a name="deploydevice" />

## <a name="deploying-to-device"></a>Déploiement sur l’appareil

Avant de pouvoir utiliser Xamarin Player Live avec votre appareil iOS ou Android, vous devez télécharger l’application Xamarin Player de Live et coupler à Visual Studio, comme décrit dans la [installer](~/tools/live-player/install.md) guide. Une fois que vous avez connectée correctement votre appareil à Visual Studio, vous pouvez commencer l’aperçu instantané de votre page XAML. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ouvrez la page XAML que vous souhaitez l’aperçu en direct dans l’éditeur Visual Studio 2017 :

    ![](live-view-images/vs-image1.png)

2. Définissez la configuration de périphérique sur **déboguer | iPhone** pour iOS ou **déboguer** pour Android, sélectionnez l’appareil Live le lecteur à partir de la liste :

    ![](live-view-images/vs-image2.png)

3. Pour exécuter cette page XAML dans un affichage en temps réel sur votre appareil, sélectionnez **Outils > Xamarin Live Player > Live exécuter la vue actuelle** à partir de la barre de menus :

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Ouvrez la page XAML que vous souhaitez l’aperçu en direct dans Visual Studio pour l’éditeur Mac :

    ![](live-view-images/image1.png)

2. Définissez la configuration de périphérique sur **déboguer | iPhone** pour iOS ou **déboguer** pour Android, sélectionnez l’appareil Live le lecteur à partir de la liste :

    ![](live-view-images/image2.png)

3. Pour exécuter cette page XAML dans un affichage en temps réel sur votre appareil, sélectionnez **exécuter > Live exécuter la vue actuelle** à partir de la barre de menus :

    ![](live-view-images/image3.png)

-----








## <a name="deploying-to-android-emulator"></a>Déploiement sur l’émulateur Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ouvrez la page XAML que vous souhaitez l’aperçu en direct dans l’éditeur Visual Studio 2017 :

    ![](live-view-images/vs-image1.png)

2. Définissez la configuration de périphérique sur **déboguer** pour Android et sélectionnez le périphérique Live le lecteur à partir de la liste :

    ![](live-view-images/vs-image4.png)

3. Pour exécuter cette page XAML dans un affichage en temps réel sur l’émulateur Android, sélectionnez **Outils > Xamarin Live Player > Live exécuter la vue actuelle** à partir de la barre de menus :

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Ouvrez la page XAML que vous souhaitez l’aperçu en direct dans Visual Studio pour l’éditeur Mac :

    ![](live-view-images/image7.png)

2. Définissez la configuration de périphérique sur **déboguer** pour Android et sélectionnez le périphérique Live le lecteur à partir de la liste :

    ![](live-view-images/image6.png)

3. Pour exécuter cette page XAML dans un affichage en temps réel sur votre appareil, sélectionnez Exécuter > exécuter actuel affichage en direct à partir de la barre de menus :

    ![](live-view-images/image3.png)

-----





## <a name="deploying-to-ios-simulator"></a>Déploiement sur iOS Simulator

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Il n’existe actuellement aucune prise en charge pour l’utilisation de l’aperçu XAML en direct sur le simulateur iOS exécutée à distance sur Windows. Vous devez à la place [déployer sur un appareil](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Ouvrez la page XAML que vous souhaitez l’aperçu en direct dans Visual Studio pour l’éditeur Mac :

    ![](live-view-images/image1.png)

2. Définissez la configuration de périphérique sur **déboguer | iPhoneSimulator** pour iOS et sélectionnez un simulateur iOS à partir de la liste :

    ![](live-view-images/image2.png)

3. Sélectionnez **exécuter > Live exécuter la vue actuelle** à partir de la barre de menus pour lancer le simulateur et afficher votre page XAML :

    ![](live-view-images/image4.png)

4. Une fois le simulateur a démarré, vous pouvez commencer à modifier le code XAML et afficher un aperçu apparaissent en direct :

    ![](live-view-images/image5.png)  

-----








## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de Xamarin Player en direct](https://xamarin.com/live)
- [billet de blog](https://blog.xamarin.com/live-player/)
- [Xamarin Player dynamique exemples](~/tools/live-player/samples.md)
