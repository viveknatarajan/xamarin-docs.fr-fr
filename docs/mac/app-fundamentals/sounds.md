---
title: Lecture audio avec AVAudioPlayer dans Xamarin.Mac
description: Ce document décrit la lecture audio avec AVAudioPlayer dans une application Xamarin.Mac. Il aborde AVAudioPlayer à un niveau élevé et les liens vers d’autres documents qu’il explore plus en détail.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 03a0207ce8c742f0ca98ab6c75ed3e7b2f37d09e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241351"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Lecture audio avec AVAudioPlayer dans Xamarin.Mac

## <a name="about-the-avaudioplayer"></a>Sur le AVAudioPlayer

Le `AVAudioPlayer` classe est utilisée pour les données audio de la lecture à partir de la mémoire ou un fichier. Apple recommande l’utilisation de cette classe pour la lecture audio dans votre application, sauf si vous êtes en train réseau de diffusion en continu ou nécessitent une faible latence des e/s audio.

Vous pouvez utiliser la `AVAudioPlayer` classe pour effectuer les opérations suivantes :

- Lire les sons d’une durée quelconque avec bouclage facultatif.
- Lire les sons multiples en même temps avec la synchronisation facultative.
- Contrôler le volume, la vitesse de lecture et le positionnement stéréo pour chaque lecture des sons.
- Prend en charge des fonctionnalités telles qu’avance rapide ou rembobiner.
- Obtenir les données de contrôle de niveau de la lecture.

`AVAudioPlayer` prend en charge des sons dans n’importe quel format audio fourni par iOS, tvOS et macOS comme .aif, .wav ou .mp3.

## <a name="playing-sounds-in-macos"></a>Lecture de sons dans macOS

Étant donné que macOS prend en charge les mêmes classes de boîte à outils Audio comme iOS, consultez notre iOS [audio de jouer avec AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) documentation pour tous les détails de la lecture audio dans une application Xamarin.Mac.

## <a name="related-links"></a>Liens associés

- [Référence de AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
