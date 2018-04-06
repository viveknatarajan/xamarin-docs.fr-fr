---
title: Configuration d’application Mac
description: Ce guide présente en détail la configuration de votre application Xamarin.Mac en vue de sa publication.
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: e47ff676b4dd02d5312a74fb699ed594b5e0f944
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="mac-app-configuration"></a>Configuration d’application Mac

_Ce guide présente en détail la configuration de votre application Xamarin.Mac en vue de sa publication._


## <a name="mac-app-configuration"></a>Configuration d’application Mac

Cliquez avec le bouton droit sur le projet d’application Mac dans Visual Studio pour Mac, puis choisissez **Options**.


### <a name="application-settings"></a>Paramètres d’application

Pour modifier les paramètres d’une application Xamarin.Mac, double-cliquez sur le fichier **Info.plist** dans le **Panneau Solutions** :

![Sélection du fichier Info.plist](app-configuration-images/config04.png "Sélection du fichier Info.plist")

Cette action affiche les options disponibles pour l’application :

 [![Modification du fichier Info.plist](app-configuration-images/config01.png "Modification du fichier Info.plist")](app-configuration-images/config01-large.png#lightbox)

L’exécution d’applications créées Mac avec Xamarin.Mac a la configuration requise suivante :

- Un ordinateur Mac exécutant Mac OS X 10.7 ou ultérieur.


### <a name="signing-settings"></a>Paramètres de signature

La section **Signature Mac** de la boîte de dialogue **Options du projet** permet au développeur de signer une application Xamarin.Mac à des fins de tests, pour une auto-publication ou pour une publication par le biais de l’Apple App Store :

[![Éditeur Signature Mac](app-configuration-images/config02.png "Fenêtre Signature Mac")](app-configuration-images/config02-large.png#lightbox)

À partir de là, sélectionnez l’identité, le profil de provisionnement et tous les droits personnalisés utilisés pour signer l’application au moment de sa compilation. Le développeur peut éventuellement signer le programme d’installation utilisé pour installer l’application sur un autre Mac.


### <a name="build-settings"></a>Paramètres de build

La section **Build Mac** de la boîte de dialogue **Options du projet** permet au développeur de sélectionner l’architecture d’une application Xamarin.Mac, de contrôler la version de macOS que l’application prendra en charge et de créer éventuellement un paquet d’installation quand l’application sera compilée avec succès :

 [![Modification des paramètres de build](app-configuration-images/config03.png "Modification des paramètres de build")](app-configuration-images/config03-large.png#lightbox)


## <a name="related-links"></a>Liens associés

- [Installation](/visualstudio/mac/installation/)
- [Exemple Hello, Mac](~/mac/get-started/hello-mac.md)
- [Distribuer vos applications sur le Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [ID de développeur et GateKeeper](https://developer.apple.com/resources/developer-id/)
