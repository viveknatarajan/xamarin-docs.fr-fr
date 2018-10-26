---
redirect_url: /xamarin/tools/live-player/
title: XAML Live Aperçu avant impression
description: Ce document explique comment utiliser Xamarin Live Player pour live aperçu des pages XAML, apporter des modifications pour le XAML et voir les modifications s’affichent instantanément sur appareil.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 1602c98eceaff607c79400a37c4ace60d5bf8807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110594"
---
# <a name="xaml-live-previewing"></a>XAML Live Aperçu avant impression

Un des avantages de Xamarin Live Player est la possibilité de live pages XAML de la préversion, apporter des modifications au code dans Visual Studio et voir les modifications s’affichent instantanément sur votre appareil. L’aperçu en direct est possible sur votre appareil Android ou sur un simulateur ou un émulateur. Ce guide explique comment utiliser la fonctionnalité d’aperçu pour afficher les écrans XAML individuels.

## <a name="requirements"></a>Configuration requise

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Un ordinateur exécutant Windows 7 ou version ultérieure.
2. Visual Studio 2017 version 15.4 ou ultérieure avec le **développement Mobile en .NET** charge de travail installée.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Un Mac avec OS X 10.11, macOS 10.12 ou supérieure.
2. Visual Studio pour Mac 7.2 ou version ultérieure. Nous vous recommandons la version la plus récente.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Déploiement sur un appareil

Avant de pouvoir utiliser Xamarin Live Player avec votre appareil Android, vous devrez télécharger l’application Xamarin Live Player et l’associer à Visual Studio, comme décrit dans la [installer](~/tools/live-player/install.md) guide. Une fois que vous êtes correctement associé votre appareil pour Visual Studio, vous pouvez commencer l’aperçu instantané de votre page XAML. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ouvrez la page XAML que vous souhaitez l’aperçu instantané dans l’éditeur Visual Studio 2017 :

    ![](live-view-images/vs-image1.png)

2. Définissez la configuration de l’appareil en **déboguer** et sélectionnez l’appareil Live Player dans la liste :

    ![](live-view-images/vs-image2.png)

3. Pour exécuter cette page XAML en tant qu’un affichage en temps réel sur votre appareil, sélectionnez **Outils > Xamarin Live Player > Live affichage de l’exécution** à partir de la barre de menus :

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Ouvrez la page XAML que vous souhaitez l’aperçu instantané dans Visual Studio pour l’éditeur de Mac :

    ![](live-view-images/image1.png)

2. Définissez la configuration de l’appareil en **déboguer** et sélectionnez l’appareil Live Player dans la liste :

    ![](live-view-images/image2.png)

3. Pour exécuter cette page XAML en tant qu’un affichage en temps réel sur votre appareil, sélectionnez **exécuter > Live affichage de l’exécution** à partir de la barre de menus :

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Déploiement sur l’émulateur Android

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ouvrez la page XAML que vous souhaitez l’aperçu instantané dans l’éditeur Visual Studio 2017 :

    ![](live-view-images/vs-image1.png)

2. Définissez la configuration de l’appareil en **déboguer** pour Android et sélectionnez l’appareil Live Player dans la liste :

    ![](live-view-images/vs-image4.png)

3. Pour exécuter cette page XAML en tant qu’un affichage en temps réel sur l’émulateur Android, sélectionnez **Outils > Xamarin Live Player > Live affichage de l’exécution** à partir de la barre de menus :

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Ouvrez la page XAML que vous souhaitez l’aperçu instantané dans Visual Studio pour l’éditeur de Mac :

    ![](live-view-images/image7.png)

2. Définissez la configuration de l’appareil en **déboguer** pour Android et sélectionnez l’appareil Live Player dans la liste :

    ![](live-view-images/image6.png)

3. Pour exécuter cette page XAML en tant qu’un affichage en temps réel sur votre appareil, sélectionnez Exécuter > Live affichage de l’exécution à partir de la barre de menus :

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de Xamarin Live Player](https://xamarin.com/live)
- [Billet de blog](https://blog.xamarin.com/live-player/)
- [Exemples de Xamarin Live Player](~/tools/live-player/samples.md)
