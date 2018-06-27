---
title: Profilage d’applications Xamarin.iOS avec Instruments
description: Ce document décrit comment utiliser l’application Instruments d’Apple pour profiler une application Xamarin.iOS installée sur un appareil ou un simulateur.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 25129d532af0b146afedf28865649ffc9e38ee17
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785610"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profilage d’applications Xamarin.iOS avec Instruments

L’outil **Instruments** dans Xcode permet de profiler des applications Xamarin.iOS sur un appareil ou dans le simulateur. Mono utilise son modèle de juste-à-temps pour compiler le code, mais comme Instruments n’interprète pas toujours correctement ce type de données, il peut être difficile d’exploiter les sorties générées par des applications de simulateur qui utilisent Instruments.
Pour vous aider, ce guide explique comment utiliser l’application développeur pour interpréter les sorties Instruments dans ce document.

## <a name="requirements"></a>Configuration requise

L’outil Instruments dans Xcode s’exécute uniquement sur un Mac.

## <a name="opening-the-instruments-app"></a>Ouverture de l’application Instruments

Sélectionnez l’appareil et exécutez l’application Instruments :

1.  Ouvrez le projet Xamarin.iOS dans Visual Studio pour Mac.
2.  Sélectionnez la configuration **Debug|iPhone**.
3.  Connectez un appareil iOS à l’ordinateur.
4.  Dans le menu **Exécuter**, sélectionnez **Charger sur l’appareil**. L’application est ensuite générée et chargée sur l’appareil.
5.  Dans le menu **Outils**, sélectionnez **Lancer Instruments**.


Instruments s’ouvre et affiche la boîte de dialogue suivante :

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Choix d’un modèle de profilage")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Cliquez pour sélectionner le modèle **Allocations**. Les autres modèles sont valides, mais cet article traite uniquement du modèle de profil **Allocations**.

Ensuite, sélectionnez l’appareil et l’application dans le menu en haut de la fenêtre :

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Sélectionner l’appareil et l’application")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

Sélectionnez l’appareil iOS dans le menu en haut de la fenêtre et sélectionnez l’application à profiler dans le sous-menu (**MemoryDemo** dans la capture d’écran ci-dessus).

Si l’appareil n’est pas listé sous le menu, vérifiez si la **Console** dans Visual Studio pour Mac affiche des messages d’erreur ayant été générés lors du déploiement de l’application sur l’appareil. Vérifiez aussi que l’appareil a été provisionné pour le développement par le biais de l’Organizer dans Xcode.

Cliquez sur le bouton **Choose**. L’écran suivant s’affiche :

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Interface de profilage")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Cliquez sur le bouton d’enregistrement (bouton rouge en haut à gauche) pour démarrer le profilage.

La capture d’écran suivante montre un exemple de profilage dans **Instruments** :

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Exemple de profilage avec Instruments")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Récapitulatif

Ce guide vous a montré comment démarrer Instruments dans Xcode pour surveiller une application iOS depuis Visual Studio pour Mac. Consultez [Procédure pas à pas : Utilisation de l’outil Instruments d’Apple](~/ios/deploy-test/walkthrough-apples-instrument.md) pour obtenir un exemple de diagnostic d’un problème de mémoire à l’aide d’Instruments.

## <a name="related-links"></a>Liens associés

- [Procédure pas à pas pour utiliser Instruments](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Garbage Collection Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
